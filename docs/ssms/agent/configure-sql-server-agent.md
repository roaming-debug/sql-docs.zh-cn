---
description: Configure SQL Server Agent
title: Configure SQL Server Agent
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, configuring
- accounts [SQL Server], SQL Server Agent
- SQL Server Agent, permissions
- security [SQL Server], SQL Server Agent
ms.assetid: 2e361a62-9e92-4fcd-80d7-d6960f127900
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 497ac3d6bd46de41ca9bd682381afca4919f6ef2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466548"
---
# <a name="configure-sql-server-agent"></a>Configure SQL Server Agent

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 目前，SQL 托管实例不支持启用和禁用 SQL Server 代理。 SQL 代理始终运行。 有关详细信息，请参阅 [SQL 数据库托管实例与 SQL Server 之间的 T-SQL 差异](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明如何在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的过程中为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]代理指定一些配置选项。 只有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]管理对象 (SMO) 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理存储过程可以使用所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理配置选项。  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限  
  
-   在 **的“对象资源管理器”中，单击** “SQL Server 代理” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 以管理作业、操作员、警报和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务。 但是，对象资源管理器仅在您拥有使用权限时才显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理节点。  
  
-   对于故障转移群集实例上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务，不应启用自动重新启动。  
  
### <a name="security"></a><a name="Security"></a>安全性  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，必须将 **代理配置为使用** sysadmin [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]固定服务器角色的成员帐户的凭据，才能执行其功能。 该帐户必须拥有以下 Windows 权限：  
  
-   以服务身份登录 (SeServiceLogonRight)  
  
-   替换进程级别标记 (SeAssignPrimaryTokenPrivilege)  
  
-   跳过遍历检查 (SeChangeNotifyPrivilege)  
  
-   调整进程的内存配额 (SeIncreaseQuotaPrivilege)  
  
有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户所需的 Windows 权限的详细信息，请参阅 [为 SQL Server 代理服务选择帐户](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) 和 [设置 Windows 服务帐户](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-configure-sql-server-agent"></a>配置 SQL Server 代理  
  
1.  单击 **“开始”** 按钮，然后在 **“开始”**  菜单上，单击 **“控制面板”**。  
  
2.  在“控制面板”中，依次单击 **“系统和安全”**、 **“管理工具”**、 **“本地安全策略”**。  
  
3.  在“本地安全策略”中，单击尖括号以展开 **“本地策略”** 文件夹，然后单击 **“用户权限指派”** 文件夹。  
  
4.  右键单击要配置用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的权限，并选择“属性”。  
  
5.  在权限的属性对话框中，验证列出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理运行的帐户。 如果没有列出，请单击 **“添加用户或组”**，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] “选择用户、计算机、服务帐户或组” **对话框中输入运行** 代理的帐户，然后单击 **“确定”**。  
  
6.  为要添加到使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理运行的每个权限重复此操作。 完成后，单击 **“确定”** 。  
