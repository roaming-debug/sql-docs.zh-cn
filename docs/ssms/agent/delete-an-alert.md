---
title: Delete an Alert
description: Delete an Alert
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, alerts
- alerts [SQL Server], deleting
- deleting alerts
- canceling alerts
- dropping alerts
- disabling alerts
- removing alerts
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/04/2021
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: bf4cefa3d6bcd307b80ae4f4dc0944cdb18bcdba
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978779"
---
# <a name="delete-an-alert"></a>Delete an Alert

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本文演示如何使用 SQL Server Management Studio 或 Transact-SQL 删除 Microsoft SQL Server 代理警报。

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>准备工作

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限

删除警报时，将同时删除与警报相关的所有通知。

### <a name="security"></a><a name="Security"></a>安全性

#### <a name="permissions"></a><a name="Permissions"></a>权限

默认情况下，只有 **sysadmin** 固定服务器角色的成员才能删除警报。  

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio

### <a name="to-delete-an-alert"></a>删除警报

1. 在“对象资源管理器”中，单击加号以展开包含要删除的 SQL Server 代理警报的服务器。

2. 单击加号以展开“SQL Server 代理”。

3. 单击加号以展开“警报”文件夹。

4. 右键单击要删除的警报，然后选择“删除”。

5. 在“删除对象”对话框中，确认已选择正确的警报，然后选择“确定”。

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>使用 Transact-SQL

### <a name="to-delete-an-alert"></a>删除警报

1. 在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。

2. 在标准栏上，选择“新建查询”  。  

3. 将以下示例复制并粘贴到查询窗口中，然后选择“执行”。

    ```
    -- deletes the SQL Server Agent alert called 'Test Alert.'
    USE msdb ;
    GO
  
    EXEC dbo.sp_delete_alert
       @name = N'Test Alert' ;
    GO
    ```

有关详细信息，请参阅 [sp_delete_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md)。
