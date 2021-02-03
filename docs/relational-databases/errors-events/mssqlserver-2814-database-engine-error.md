---
description: MSSQLSERVER_2814
title: MSSQLSERVER_2814 | Microsoft Docs
ms.custom: ''
ms.date: 07/11/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2814 (Database Engine error)
ms.assetid: 22800748-9be9-4511-9428-6b8b40e5bef9
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6d64879e4ee4bc54edc566c2109ecdd3e28598f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198195"
---
# <a name="mssqlserver_2814"></a>MSSQLSERVER_2814
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|2814|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|PR_POSSIBLE_INFINITE_RECOMPILE|  
|消息正文|检测到可能无限的重新编译: SQLHANDLE %hs，PlanHandle %hs，起始偏移量 %d，结束偏移量 %d。 上次重新编译的原因为 %d。|  
  
## <a name="explanation"></a>说明  
一个或多个语句导致查询批处理至少重新编译 50 次。 应更正指定语句以免进一步重新编译。  
  
下表列出了重新编译的原因。  
  
|原因代码|说明|  
|---------------|---------------|  
|1|架构已更改|  
|2|统计信息已更改|  
|3|编译延迟|  
|4|所设置的选项已更改|  
|5|临时表已更改|  
|6|远程行集已更改|  
|7|For Browse 权限已更改|  
|8|查询通知环境已更改|  
|9|分区视图已更改|  
|10|游标选项已更改|  
|11|已请求选项（重新编译）|  
  
## <a name="user-action"></a>用户操作  
  
1.  通过运行以下查询查看导致重新编译的语句。 将 *sql_handle*、*starting_offset*、*ending_offset* 和 *plan_handle* 占位符替换为错误消息中指定的值。 对于临时和预定义 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，database_name 和 object_name 列将为 NULL。  
  
    ```sql   
    SELECT DB_NAME(st.dbid) AS database_name,  
        OBJECT_NAME(st.objectid) AS object_name,  
        st.text  
    FROM sys.dm_exec_query_stats AS qs  
    CROSS APPLY sys.dm_exec_sql_text (*sql_handle*) AS st  
    WHERE qs.statement_start_offset = *starting_offset*  
    AND qs.statement_end_offset = *ending_offset*  
    AND qs.plan_handle = *plan_handle*;
    ```
  
2.  根据原因代码说明，修改相应语句、批处理或过程以避免重新编译。 例如，某一存储过程可能包含一个或多个 SET 语句。 应从此过程中删除这些语句。 有关重新编译原因和解决方法的其他示例，请参阅 [Batch Compilation, Recompilation, and Plan Caching Issues in SQL Server 2005](/previous-versions/sql/sql-server-2005/administrator/cc966425(v=technet.10))（SQL Server 2005 中的批编译、重新编译和计划缓存问题）。  
  
3.  如果问题仍然存在，请与 Microsoft 客户支持服务部门联系。  
  
## <a name="see-also"></a>另请参阅  
[SQL:StmtRecompile 事件类](../event-classes/sql-stmtrecompile-event-class.md)  
  
