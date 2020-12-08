---
title: 创建和测试分类器用户定义函数 - Resource Governor
description: 了解如何创建和测试分类器用户定义的函数，该函数与在 SQL Server Management Studio 查询编辑器中运行 Transact-SQL 语句相关。
ms.custom: seo-dt-2019
ms.date: 07/11/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, classifier function create
- classifier function [SQL Server], test
- classifier function [SQL Server], create
- Resource Governor, classifier function test
ms.assetid: 7866b3c9-385b-40c6-aca5-32d3337032be
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d08ddba59acdf2692e8493a10bac58048458ae23
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506592"
---
# <a name="create-and-test-a-classifier-user-defined-function"></a>创建和测试分类器用户定义函数
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何创建和测试分类器用户定义函数 (UDF)。 这些步骤涉及在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询编辑器中执行 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 语句。  
  
 下面的过程中显示的示例说明了创建非常复杂的分类器用户定义函数的可能性。  
  
 在示例中：  
  
-   创建了 pProductionProcessing 资源池和 gProductionProcessing 工作负荷组，用于在指定时间范围内进行生产处理。  
  
-   创建了 pOffHoursProcessing 资源池和 gOffHoursProcessing 工作负荷组，用于处理不符合生产处理要求的连接。  
  
-   在 master 中创建了 TblClassificationTimeTable 表，用于保存可根据登录时间计算的开始和结束时间。 必须在 master 中创建该表，因为资源调控器对分类器函数使用了架构绑定。  
  
    > [!NOTE]  
    >  作为一种最佳做法，您不应在 master 中存储经常更新的大型表。  
  
 分类器函数可能延长登录时间。 过于复杂的函数可能会导致登录超时或减慢快速连接速度。  
  
## <a name="to-create-the-classifier-user-defined-function"></a>创建分类器用户定义函数  
  
1.  创建并配置新的资源池和工作负荷组。 将每个工作负荷组分配给相应的资源池。  
  
    ```sql  
    --- Create a resource pool for production processing  
    --- and set limits.  
    USE master;  
    GO  
    CREATE RESOURCE POOL pProductionProcessing  
    WITH  
    (  
         MAX_CPU_PERCENT = 100,  
         MIN_CPU_PERCENT = 50  
    );  
    GO  
    
    --- Create a workload group for production processing  
    --- and configure the relative importance.  
    CREATE WORKLOAD GROUP gProductionProcessing  
    WITH  
    (  
         IMPORTANCE = MEDIUM  
    );  
    
    --- Assign the workload group to the production processing  
    --- resource pool.  
    USING pProductionProcessing  
    GO  
    
    --- Create a resource pool for off-hours processing  
    --- and set limits.  
    CREATE RESOURCE POOL pOffHoursProcessing  
    WITH  
    (  
         MAX_CPU_PERCENT = 50,  
         MIN_CPU_PERCENT = 0  
    );  
    GO  
    
    --- Create a workload group for off-hours processing  
    --- and configure the relative importance.  
    CREATE WORKLOAD GROUP gOffHoursProcessing  
    WITH  
    (  
         IMPORTANCE = LOW  
    )  
    --- Assign the workload group to the off-hours processing  
    --- resource pool.  
    USING pOffHoursProcessing;  
    GO  
    ```  
  
2.  更新内存中的配置。  
  
    ```sql  
    ALTER RESOURCE GOVERNOR RECONFIGURE;  
    GO  
    ```  
  
3.  创建一个表，并定义生产处理时间范围的开始和结束时间。  
  
    ```sql  
    USE master;  
    GO  
    CREATE TABLE tblClassificationTimeTable  
    (  
         strGroupName     sysname          not null,  
         tStartTime       time              not null,  
         tEndTime         time              not null  
    );  
    GO  
    --- Add time values that the classifier will use to  
    --- determine the workload group for a session.  
    INSERT into tblClassificationTimeTable VALUES('gProductionProcessing', '6:35 AM', '6:15 PM');  
    GO  
    ```  
  
4.  创建分类器函数，它使用时间函数以及可根据查找表中的时间计算的值。 有关在分类器函数中使用查找表的信息，请参阅本主题中的“在分类器函数中使用查找表的最佳做法”。  
  
    > [!NOTE]  
    >  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 引入了一组扩展的日期和时间数据类型和函数。 有关详细信息，请参阅[日期和时间数据类型和功能 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。  
  
    ```sql  
    CREATE FUNCTION fnTimeClassifier()  
    RETURNS sysname  
    WITH SCHEMABINDING  
    AS  
    BEGIN  
    /* We recommend running the classifier function code under 
    snapshot isolation level OR using NOLOCK hint to avoid blocking on 
    lookup table. In this example, we are using NOLOCK hint. */
         DECLARE @strGroup sysname  
         DECLARE @loginTime time  
         SET @loginTime = CONVERT(time,GETDATE())  
         SELECT TOP 1 @strGroup = strGroupName  
              FROM dbo.tblClassificationTimeTable WITH(NOLOCK)
              WHERE tStartTime <= @loginTime and tEndTime >= @loginTime  
         IF(@strGroup is not null)  
         BEGIN  
              RETURN @strGroup  
         END  
    --- Use the default workload group if there is no match  
    --- on the lookup.  
         RETURN N'gOffHoursProcessing'  
    END;  
    GO  
    ```  
  
5.  注册分类器函数并更新内存中的配置。  
  
    ```sql  
    ALTER RESOURCE GOVERNOR with (CLASSIFIER_FUNCTION = dbo.fnTimeClassifier);  
    ALTER RESOURCE GOVERNOR RECONFIGURE;  
    GO  
    ```  
  
## <a name="to-verify-the-resource-pools-workload-groups-and-the-classifier-user-defined-function"></a>验证资源池、工作负荷组以及分类器用户定义函数  
  
1.  使用以下查询获取资源池和工作负荷组配置。  
  
    ```sql  
    USE master;  
    SELECT * FROM sys.resource_governor_resource_pools;  
    SELECT * FROM sys.resource_governor_workload_groups;  
    GO  
    ```  
  
2.  使用以下查询验证分类器函数是否存在以及是否启用。  
  
    ```sql  
    --- Get the classifier function Id and state (enabled).  
    SELECT * FROM sys.resource_governor_configuration;  
    GO  
    --- Get the classifer function name and the name of the schema  
    --- that it is bound to.  
    SELECT   
          object_schema_name(classifier_function_id) AS [schema_name],  
          object_name(classifier_function_id) AS [function_name]  
    FROM sys.dm_resource_governor_configuration;  
    ```  
  
3.  使用以下查询获取资源池和工作负荷组的当前运行时数据。  
  
    ```sql  
    SELECT * FROM sys.dm_resource_governor_resource_pools;  
    SELECT * FROM sys.dm_resource_governor_workload_groups;  
    GO  
    ```  
  
4.  使用以下查询确定每个组中包含的会话。  
  
    ```sql  
    SELECT s.group_id, CAST(g.name as nvarchar(20)), s.session_id, s.login_time, 
        CAST(s.host_name as nvarchar(20)), CAST(s.program_name AS nvarchar(20))  
    FROM sys.dm_exec_sessions AS s  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = s.group_id  
    ORDER BY g.name;  
    GO  
    ```  
  
5.  使用以下查询确定每个组中包含的请求。  
  
    ```sql  
    SELECT r.group_id, g.name, r.status, r.session_id, r.request_id, 
        r.start_time, r.command, r.sql_handle, t.text   
    FROM sys.dm_exec_requests AS r  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = r.group_id  
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) AS t  
    ORDER BY g.name;  
    GO  
    ```  
  
6.  使用以下查询确定分类器中运行的请求。  
  
    ```sql  
    SELECT s.group_id, g.name, s.session_id, s.login_time, s.host_name, s.program_name   
    FROM sys.dm_exec_sessions AS s  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = s.group_id  
           AND 'preconnect' = s.status  
    ORDER BY g.name;  
    GO  
  
    SELECT r.group_id, g.name, r.status, r.session_id, r.request_id, r.start_time, 
        r.command, r.sql_handle, t.text   
    FROM sys.dm_exec_requests AS r  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = r.group_id  
           AND 'preconnect' = r.status  
     CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) AS t  
    ORDER BY g.name;  
    GO  
    ```  
  
## <a name="best-practices-for-using-lookup-tables-in-a-classifier-function"></a>在分类器函数中使用查找表的最佳做法  
  
1.  除非绝对必要，否则不要使用查找表。 如果需要使用查找表，可以将其硬编码到函数本身；但是，这样做时需要权衡考虑分类器函数的复杂程度和动态变更。  
  
2.  限制为查找表执行的 I/O。  
  
    1.  使用 `TOP 1` 仅返回一行。  
  
    2.  尽量减少表中的行数。  
  
    3.  让表中所有行都位于一页中或少数几页中。  
  
    4.  确认使用索引查找操作找到的行使用了尽量多的查找列。  
  
    5.  如果考虑使用相互联接的多个表，则可以对单个表取消规范化。  
  
3.  防止阻塞查找表。  
  
    1.  使用 `NOLOCK` 提示防止阻塞，或在函数中使用最大值设置为 1,000 毫秒的 `SET LOCK_TIMEOUT`。  
  
    2.  表必须存在于 master 数据库中。 （master 数据库是在客户端计算机尝试连接时可确保恢复的唯一一个数据库）。  
  
    3.  始终使用架构对表名进行完全限定。 数据库名称不是必需的，因为该名称只能是 master 数据库的名称。  
  
    4.  表上没有触发器。  
  
    5.  如果要更新表内容，请确保在分类器函数中使用快照隔离级别事务来防止编写器阻塞读取器。 请注意，使用 `NOLOCK` 提示应该也能缓解此问题。  
  
    6.  如果可能，请在更改表内容时禁用分类器函数。  
  
        > [!WARNING]  
        >  我们强烈建议遵循如上最佳做法。 如有任何问题妨碍您遵循这些最佳做法，我们建议您与 Microsoft 支持部门联系，以求主动防止未来出现任何问题。  
  
## <a name="see-also"></a>另请参阅  
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)   
 [启用资源调控器](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [资源调控器资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [资源调控器工作负荷组](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [使用模板配置资源调控器](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)   
 [查看资源调控器属性](../../relational-databases/resource-governor/view-resource-governor-properties.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)   
 [CREATE RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
