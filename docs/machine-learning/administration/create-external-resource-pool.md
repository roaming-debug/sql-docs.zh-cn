---
title: 创建资源池
description: 了解如何在 SQL Server 机器学习服务中创建和使用资源池来管理 Python 和 R 工作负载。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: da6f6817856efd9dd0310211998230d49e6f3d1b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471388"
---
# <a name="create-a-resource-pool-for-sql-server-machine-learning-services"></a>为 SQL Server 机器学习服务创建资源池
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

了解如何在 SQL Server 机器学习服务中创建和使用资源池来管理 Python 和 R 工作负载。 

此过程包括多个步骤：

1. 查看所有现有资源池的状态。 务必要了解哪些服务在使用现有资源。
2. 修改服务器资源池。
3. 为外部进程创建新资源池。
4. 创建分类函数来标识外部脚本请求。
5. 验证新的外部资源池是否正在从指定的客户端或帐户捕获 R 或 Python 作业。

<a name="bkmk_ReviewStatus"></a>

##  <a name="review-the-status-of-existing-resource-pools"></a>查看现有资源池的状态
  
1.  使用以下语句检查分配到服务器默认池的资源。
  
    ```sql
    SELECT * FROM sys.resource_governor_resource_pools WHERE name = 'default'
    ```

    示例结果 

    |pool_id|name|min_cpu_percent|max_cpu_percent|min_memory_percent|max_memory_percent|cap_cpu_percent|min_iops_per_volume|max_iops_per_volume|
    |-|-|-|-|-|-|-|-|-|
    |2|default|0|100|0|100|100|0|0|

2.  检查分配到默认外部资源池的资源。
  
    ```sql
    SELECT * FROM sys.resource_governor_external_resource_pools WHERE name = 'default'
    ```

    示例结果 

    |external_pool_id|name|max_cpu_percent|max_memory_percent|max_processes|版本|
    |-|-|-|-|-|-|
    |2|default|100|20|0|2|
 
3.  使用这些服务器默认设置时，外部运行时可能无法获得足够的资源来完成大多数任务。 若要改善资源，必须按如下所述修改服务器资源用量：
  
    -   减少数据库引擎可以使用的最大计算机内存。
  
    -   增加外部进程可以使用的最大计算机内存。

## <a name="modify-server-resource-usage"></a>修改服务器资源用量

1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中运行以下语句，将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存用量限制为“最大服务器内存”设置中的值的 **60%** 。

    ```sql
    ALTER RESOURCE POOL "default" WITH (max_memory_percent = 60);
    ```
  
2. 请运行以下语句，将外部进程的内存用量限制为计算机资源总量的 40%。
  
    ```sql
    ALTER EXTERNAL RESOURCE POOL "default" WITH (max_memory_percent = 40);
    ```
  
3.  若要实施这些更改，必须按如下所示重新配置并重新启动资源调控器：
  
    ```sql
    ALTER RESOURCE GOVERNOR RECONFIGURE;
    ```
  
    > [!NOTE]
    >  这些设置只是建议你最初使用的设置；应该根据其他服务器进程机器学习任务，以确定如何适当平衡环境和工作负载的设置。

## <a name="create-a-user-defined-external-resource-pool"></a>创建用户定义的外部资源池
  
1.  对 Resource Governor 配置的所有更改将在整个服务器上强制执行。 这些更改会影响使用服务器默认池的工作负载，以及使用外部池的工作负载。
  
     若要更细致地控制哪些工作负载优先获得资源，可以新建用户定义的外部资源池。 定义一个分类函数并将其分配给外部资源池。 “EXTERNAL”是新的关键字。
  
     创建新的用户定义的外部资源池。 在以下示例中，池命名为 **ds_ep**。
  
    ```sql
    CREATE EXTERNAL RESOURCE POOL ds_ep WITH (max_memory_percent = 40);
    ```

2.  创建名为 `ds_wg` 的工作负荷组用于管理会话请求。 对于 SQL 查询，将使用默认池；对于所有外部进程查询，将使用 `ds_ep` 池。
  
    ```sql
    CREATE WORKLOAD GROUP ds_wg WITH (importance = medium) USING "default", EXTERNAL "ds_ep";
    ```
  
     每当请求无法分类，或者发生其他任何分类失败时，请求将分配到默认组。

 
有关详细信息，请参阅[资源调控器工作负荷组](../../relational-databases/resource-governor/resource-governor-workload-group.md)和 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)。
  
## <a name="create-a-classification-function-for-machine-learning"></a>为机器学习创建分类函数
  
分类函数检查传入的任务。 它确定任务是否可以使用当前资源池来运行。 不符合分类函数条件的任务将分配回到服务器的默认资源池。
  
1. 首先，指定资源调控器应使用分类器函数来确定资源池。 可将“null”指定为分类器函数的占位符。
  
    ```sql
    ALTER RESOURCE GOVERNOR WITH (classifier_function = NULL);
    ALTER RESOURCE GOVERNOR RECONFIGURE;
    ```
  
     有关详细信息，请参阅 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)。
  
2.  在每个资源池的分类器函数中，定义应分配到资源池的语句或传入请求的类型。
  
     例如，如果发送请求的应用程序是“Microsoft R Host”、“RStudio”或“Mashup”，下面的函数返回分配到用户定义的外部资源池的架构的名称；否则，它返回默认资源池。
  
    ```sql
    USE master
    GO
    CREATE FUNCTION is_ds_apps()
    RETURNS sysname
    WITH schemabinding
    AS
    BEGIN
        IF program_name() in ('Microsoft R Host', 'RStudio', 'Mashup') RETURN 'ds_wg';
        RETURN 'default'
        END;
    GO
    ```
  
3.  创建该函数后，请重新配置资源组，将新的分类器函数分配到前面定义的外部资源组。
  
    ```sql
    ALTER RESOURCE GOVERNOR WITH  (classifier_function = dbo.is_ds_apps);
    ALTER RESOURCE GOVERNOR RECONFIGURE;
    GO
    ```

## <a name="verify-new-resource-pools-and-affinity"></a>验证新资源池和相关性

检查每个工作负载组的服务器内存配置和 CPU。 查看以下内容，验证是否已进行实例资源更改：

+ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务器的默认池
+ 外部进程的默认资源池
+ 外部进程的用户定义池

1. 运行以下语句以查看所有工作负载组：

    ```sql
    SELECT * FROM sys.resource_governor_workload_groups;
    ```

    **示例结果**

    |group_id|name|importance|request_max_memory_grant_percent|request_max_cpu_time_sec|request_memory_grant_timeout_sec|max_dop|group_max_requests pool_id|pool_idd|external_pool_id|
    |-|-|-|-|-|-|-|-|-|-|
    |1|内部|中型|25|0|0|0|0|1|2|
    |2|默认值|中型|25|0|0|0|0|2|2|
    |256|ds_wg|中型|25|0|0|0|0|2|256|
  
2.  使用新的目录视图 [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) 查看所有外部资源池。
  
    ```sql
    SELECT * FROM sys.resource_governor_external_resource_pools;
    ```

    **示例结果**
    
    |external_pool_id|name|max_cpu_percent|max_memory_percent|max_processes|版本|
    |-|-|-|-|-|-|
    |2|默认值|100|20|0|2|
    |256|ds_ep|100|40|0|1|
  
     有关详细信息，请参阅[资源调控器目录视图 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)。
  
3.  运行以下语句返回有关已关联到外部资源池的计算机资源的信息（如果适用）：
  
    ```sql
    SELECT * FROM sys.resource_governor_external_resource_pool_affinity;
    ```
  
     由于池是使用 AUTO 相关性创建的，因此不会显示任何信息。 有关详细信息，请参阅 [sys.dm_resource_governor_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pool-affinity-transact-sql.md)。

## <a name="next-steps"></a>后续步骤

有关管理服务器资源的详细信息，请参阅：

+ [资源调控器](../../relational-databases/resource-governor/resource-governor.md) 
+ [与资源调控器相关的动态管理视图 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)

有关机器学习资源管理的概述，请参阅：

+ [在 SQL Server 机器学习服务中使用 Resource Governor 管理 Python 和 R 工作负载](resource-governor.md)
