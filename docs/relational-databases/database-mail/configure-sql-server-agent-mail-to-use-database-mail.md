---
description: 配置 SQL Server 代理邮件以使用数据库邮件
title: 配置 SQL Server 代理邮件以使用数据库邮件
ms.date: 08/05/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- Database Mail [SQL Server], SQL Server Agent Mail
- SQL Server Agent Mail
ms.assetid: 4b8b61bd-4bd1-43cd-b6e5-c6ed2e101dce
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
ms.openlocfilehash: d3740fd3c1e6030eda7428bc73a11f6c65fec68f
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250326"
---
# <a name="configure-sql-server-agent-mail-to-use-database-mail"></a>配置 SQL Server 代理邮件以使用数据库邮件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理以使用数据库邮件通过 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 发送 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的通知和警报。  有关如何启用和配置数据库邮件的信息，请参阅 [配置数据库邮件](../../relational-databases/database-mail/configure-database-mail.md)。  有关使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]的示例，请参阅 [创建数据库邮件配置文件](../../relational-databases/database-mail/create-a-database-mail-profile.md)。
  
-   **开始之前：**  
  
-   [先决条件](#Prerequisites)  
  
-   [安全性](#Security)  
  
-   [使用 SQL Server Management Studio 配置 SQL Server 代理以使用数据库邮件](#SSMSProcedure)  
  
-   [后续任务](#Follow_Up)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
  > [!NOTE]
  > 托管实例上的 SQL 代理始终配置为使用数据库邮件，因此在托管实例上此内容不适用。 在托管实例中，需要具有必须调用的配置文件 [AzureManagedInstance_dbmail_profile](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)，才能将 SQL 代理与数据库邮件绑定。 
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   [启用数据库邮件](../../relational-databases/database-mail/configure-database-mail.md)。  
  
-    创建供[代理服务帐户使用的](../../relational-databases/database-mail/create-a-database-mail-account.md) 数据库邮件帐户 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   创建供 [代理服务帐户使用的](../../relational-databases/database-mail/create-a-database-mail-profile.md) 数据库邮件配置文件 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，并将用户添加到 **DatabaseMailUserRole** 数据库的 **DatabaseMailUserRole** 中。
  
-   将该配置文件设置为 **msdb** 数据库的默认配置文件。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 创建配置文件帐户和执行存储过程的用户应是 sysadmin 固定服务器角色的成员。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **配置 SQL Server 代理邮件以使用数据库邮件**  
  
-   在对象资源管理器中，展开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
-   右键单击“SQL Server 代理”，然后单击“属性”。  
  
-   单击 **“警报系统”**。  
  
-   选择 **“启用邮件配置文件”**。  
  
-   在 **“邮件系统”** 列表中，选择 **“数据库邮件”**。  
  
-   在 **“邮件配置文件列表”** 中，为数据库邮件选择一个邮件配置文件。 
  
-   重启 SQL Server 代理。  
  
##  <a name="follow-up-tasks"></a><a name="Follow_Up"></a> 后续任务  
 需要执行下列任务以完成对发送警报和通知的代理配置。  
  
-   [警报](../../ssms/agent/alerts.md)  
  
     可以配置警报，将特定数据库事件或操作系统情况通知操作员。  
  
-   [运算符](../../ssms/agent/operators.md)  
  
     操作员是可以接收电子通知的人员或组的别名  
  
