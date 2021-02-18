---
title: 查看和读取故障转移群集实例诊断日志
description: 了解如何查看和读取 SQL Server 故障转移群集实例生成的且正在运行的诊断日志。
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 68074bd5-be9d-4487-a320-5b51ef8e2b2d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0957ffc760046bfd9e3e10d018f373d94950080a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346160"
---
# <a name="view-and-read-failover-cluster-instance-diagnostics-log"></a>查看和读取故障转移群集实例诊断日志
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  将 SQL Server 资源 DLL 的所有严重错误和警告事件写入 Windows 事件日志。 正在运行的特定于 SQL Server 的诊断信息日志由 [sp_server_diagnostics (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)系统存储过程捕获，并会写入 SQL Server 故障转移群集诊断日志文件（也称为 *SQLDIAG* 日志）中。  
  
-   **开始之前：** [建议](#Recommendations)、[安全性](#Security)  
  
-   **若要查看诊断日志，请使用：** [SQL Server Management Studio](#SSMSProcedure)、[Transact-SQL](#TsqlProcedure)  
  
-   **若要配置诊断日志设置，请使用：** [Transact-SQL](#TsqlConfigure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
 默认情况下，SQLDIAG 存储在 SQL Server 实例目录的本地 LOG 文件夹下，例如“C\Program Files\Microsoft SQL Server\MSSQL13.\<InstanceName>\MSSQL\LOG”，该目录位于 Always On 故障转移群集实例 (FCI) 的自有节点上。 每个 SQLDIAG 日志文件的大小固定为 100 MB。 在被回收用于新日志之前，计算机上有十个这样的日志文件。  
  
 这些日志使用扩展事件文件格式。 **sys.fn_xe_file_target_read_file** 系统函数可用于读取由扩展事件创建的文件。 每行返回一个 XML 格式的事件。 查询系统视图以将 XML 数据作为结果集分析。 有关详细信息，请参阅 [sys.fn_xe_file_target_read_file (Transact SQL)](../../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 运行 **fn_xe_file_target_read_file** 需要 VIEW SERVER STATE 权限。  
  
 以管理员身份打开 SQL Server Management Studio  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **查看诊断日志文件：**  
  
1.  从“文件”  菜单，选择“打开文件”   ，然后选择要查看的诊断日志文件。  
  
2.  事件在右窗格中显示为行，默认情况下，仅显示出 **name** 和 **timestamp** 这两列。  
  
     这还会激活 **“扩展事件”** 菜单。  
  
3.  若要查看更多列，转到 **“扩展事件”** 菜单，然后选择 **“选择列”** 。  
  
     将打开一个显示出可用列的对话框，您可在其中选择要显示的列。  
  
4.  您可使用 **“扩展事件”** 菜单并选择 **“筛选”** 选项对事件数据进行筛选和排序。  
  
##  <a name="view-diagnostic-log-files-with-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL 查看诊断日志文件  
 **查看诊断日志文件：**  
  
 若要查看 SQLDIAG 日志文件中的所有日志项，请使用以下查询：  
  
```  
SELECT  
xml_data.value('(event/@name)[1]','varchar(max)') AS 'Name'  
,xml_data.value('(event/@package)[1]','varchar(max)') AS 'Package'  
,xml_data.value('(event/@timestamp)[1]','datetime') AS 'Time'  
,xml_data.value('(event/data[@name=''state'']/value)[1]','int') AS 'State'  
,xml_data.value('(event/data[@name=''state_desc'']/text)[1]','varchar(max)') AS 'State Description'  
,xml_data.value('(event/data[@name=''failure_condition_level'']/value)[1]','int') AS 'Failure Conditions'  
,xml_data.value('(event/data[@name=''node_name'']/value)[1]','varchar(max)') AS 'Node_Name'  
,xml_data.value('(event/data[@name=''instancename'']/value)[1]','varchar(max)') AS 'Instance Name'  
,xml_data.value('(event/data[@name=''creation time'']/value)[1]','datetime') AS 'Creation Time'  
,xml_data.value('(event/data[@name=''component'']/value)[1]','varchar(max)') AS 'Component'  
,xml_data.value('(event/data[@name=''data'']/value)[1]','varchar(max)') AS 'Data'  
,xml_data.value('(event/data[@name=''info'']/value)[1]','varchar(max)') AS 'Info'  
FROM  
 ( SELECT object_name AS 'event'  
  ,CONVERT(xml,event_data) AS 'xml_data'  
  FROM sys.fn_xe_file_target_read_file('C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Log\SQLNODE1_MSSQLSERVER_SQLDIAG_0_129936003752530000.xel',NULL,NULL,NULL)   
)   
AS XEventData  
ORDER BY Time;  
  
```  
  
> [!NOTE]  
>  您可以使用 WHERE 子句针对特定的组件或状态筛选结果。  
  
##  <a name="configure-diagnostic-log-properties-with-transact-sql"></a><a name="TsqlConfigure"></a> 使用 Transact-SQL 配置诊断日志属性  
 **配置诊断日志属性：**  
  
> [!NOTE]  
>  有关此过程的示例，请参阅本节后面的 [示例 (Transact-SQL)](#TsqlExample)。  
  
 使用数据定义语言 (DDL) 语句 **ALTER SERVER CONFIGURATION**，可启动或停止对 [sp_server_diagnostics (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) 过程捕获的诊断数据的日志记录，并设置 SQLDIAG 日志配置参数，如日志文件滚动更新计数、日志文件大小和文件位置。 有关语法详细信息，请参阅 [Setting diagnostic log options](../../../t-sql/statements/alter-server-configuration-transact-sql.md#Diagnostic)。  
  
###  <a name="examples-transact-sql"></a><a name="ConfigTsqlExample"></a> 示例 (Transact-SQL)  
  
####  <a name="setting-diagnostic-log-options"></a><a name="TsqlExample"></a> Setting diagnostic log options  
 本节中的示例说明如何设置诊断日志选项的值。  
  
##### <a name="a-starting-diagnostic-logging"></a>A. 启动诊断日志记录  
 下面的示例启动诊断数据的日志记录。  
  
```  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG ON;  
```  
  
##### <a name="b-stopping-diagnostic-logging"></a>B. 停止诊断日志记录  
 下面的示例停止诊断数据的日志记录。  
  
```  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG OFF;  
```  
  
##### <a name="c-specifying-the-location-of-the-diagnostic-logs"></a>C. 指定诊断日志的位置  
 下面的示例将诊断日志的位置设置为指定的文件路径。  
  
```  
ALTER SERVER CONFIGURATION  
SET DIAGNOSTICS LOG PATH = 'C:\logs';  
```  
  
##### <a name="d-specifying-the-maximum-size-of-each-diagnostic-log"></a>D. 指定每个诊断日志的最大大小  
 下面的示例将每个诊断日志的最大大小设置为 10 MB。  
  
```  
ALTER SERVER CONFIGURATION   
SET DIAGNOSTICS LOG MAX_SIZE = 10 MB;  
```  
  
## <a name="see-also"></a>另请参阅  
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
  
