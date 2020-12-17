---
description: 警报属性 - 新建警报（“常规”页）
title: 警报属性 - 新建警报（“常规”页）
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.alert.general.f1
ms.assetid: f5c11610-62e3-44df-9800-a5dc35be4a09
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 18cbd660ed24526331aed570415eccd314127c73
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472438"
---
# <a name="alert-properties---new-alert-general-page"></a>警报属性 - 新建警报（“常规”页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此页可以查看和修改 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理警报的常规属性。  

## <a name="options"></a>选项  
**名称**  
更改警报的名称。  
  
**启用**  
启用警报。 如果未启用警报，则警报中指定的操作将不会发生。  
  
**类型**  
选择警报的类型：  
  
-   **SQL Server 事件警报** ，该警报用于响应 [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows 事件日志中的消息。  
  
-   **SQL Server 性能条件警报** ，该警报用于响应性能计数器中的特定条件。  
  
-   **WMI 事件警报** ，该警报用于响应 Windows Management Instrumentation (WMI) 事件。  
  
## <a name="sql-server-event-alert-options"></a>SQL Server 事件警报选项  
**数据库名称**  
为该事件指定一个数据库，或者指定“所有数据库”，这样不管在哪一个数据库中发生该事件，都会对消息作出响应。  
  
**错误号**  
指定此事件将用于响应错误，并指定错误号。  
  
**严重性**  
指定此事件将用于响应特定严重级别的所有消息，并指定严重级别。  
  
**当消息包含以下内容时触发警报**  
按特定字符串筛选事件。 选中此选项时，该警报只对包含特定字符串的事件作出响应。  
  
**消息正文**  
指定要用于筛选事件的字符串。  
  
## <a name="sql-server-performance-condition-alerts"></a>SQL Server 性能条件警报  
**Object**  
指定要监视的性能对象。  
  
**计数器**  
指定位于要监视的性能对象内的计数器。  
  
**实例**  
指定要监视的计数器实例。  
  
**计数器满足以下条件时触发警报**  
指定警报响应的计数器的行为。 例如，可能希望警报响应 **Free space in tempdb (KB)** 计数器的值低于特定值的条件，或者希望警报响应 **SQL Compilations/sec** 高于特定值的条件。  
  
**值**  
指定计数器的值。  
  
## <a name="wmi-event-alert-options"></a>WMI 事件警报选项  
**Namespace**  
指定用于 WMI 查询语言 (WQL) 语句的命名空间。 仅支持运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的计算机上的命名空间。  
  
**查询**  
指定用于标识该警报所响应事件的 WQL 语句。  
  
## <a name="see-also"></a>另请参阅  
[警报](../../ssms/agent/alerts.md)  
[将 WQL 与 WMI Provider for Server Events 结合使用](../../relational-databases/wmi-provider-server-events/using-wql-with-the-wmi-provider-for-server-events.md)  
[使用错误号创建警报](../../ssms/agent/create-an-alert-using-an-error-number.md)  
[Create an Alert Using Severity Level](../../ssms/agent/create-an-alert-using-severity-level.md)  
