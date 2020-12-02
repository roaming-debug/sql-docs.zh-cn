---
description: 创建数据库邮件帐户
title: 创建数据库邮件帐户 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- Database Mail [SQL Server], accounts
- accounts [Database Mail]
ms.assetid: c07abbc6-fc6a-470b-8fa3-532f2e06b16a
author: stevestein
ms.author: sstein
ms.openlocfilehash: e80df479a6fbd13e51e69ebddff5634ce7a0c3b1
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128817"
---
# <a name="create-a-database-mail-account"></a>创建数据库邮件帐户
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  使用“数据库邮件配置向导”或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 可以创建数据库邮件帐户。  
  
-   **准备工作：**  [先决条件](#Prerequisites)  
  
-   **使用以下方法创建数据库邮件帐户：**  [数据库邮件配置向导](#SSMSProcedure)、[Transact-SQL](#TsqlProcedure)  
  
-   **Follow Up:**  [Next Steps to Configure the Database Mail](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   确定用于发送电子邮件的服务器名称和简单邮件传输协议 (SMTP) 服务器的端口号。如果 SMTP 服务器需要身份验证，请确定 SMTP 服务器的用户名和密码。  
  
-   （可选）还可以指定服务器的类型和服务器的端口号。 发送邮件的服务器类型始终为 SMTP。 默认情况下，大多数 SMTP 服务器使用端口 25。  
  
##  <a name="using-database-mail-configuration-wizard"></a><a name="SSMSProcedure"></a> 使用数据库邮件配置向导  
 **使用向导创建数据库邮件帐户**  
  
-   在对象资源管理器中，连接到您要在其上配置数据库邮件的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，然后展开服务器树。  
  
-   展开 **“管理”** 节点  
  
-   双击数据库邮件以打开数据库邮件配置向导。  
  
-   在 **“选择配置任务”** 页上，选择 **“管理数据库邮件帐户和配置文件”**，然后单击 **“下一步”**。  
  
-   在 **“管理配置文件和帐户”** 页上，选择 **“创建新帐户”** ，然后单击 **“下一步”**。  
  
-   在 **“新建帐户”** 页上，指定帐户名称、说明、邮件服务器信息和身份验证类型。 单击“下一步”   
  
-   在 **“完成该向导”** 页上，检查要执行的操作，然后单击 **“完成”** 以完成创建新帐户。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **使用 Transact-SQL 创建数据库邮件帐户**  
  
 执行存储过程 **msdb.dbo.sysmail_add_account_sp** 以创建帐户，并指定以下信息：  
  
-   要创建的帐户名。  
  
-   帐户的可选说明。  
  
-   要在发送的电子邮件上显示的电子邮件地址。  
  
-   要在发送的电子邮件上显示的显示名称。  
  
-   SMTP 服务器名称。  
  
-   用于登录到 SMTP 服务器的用户名（如果 SMTP 服务器要求身份验证）。  
  
-   用于登录到 SMTP 服务器的密码（如果 SMTP 服务器要求身份验证）。  
  
 以下示例将创建一个新数据库邮件帐户。  
  
```  
EXECUTE msdb.dbo.sysmail_add_account_sp  
    @account_name = 'AdventureWorks Administrator',  
    @description = 'Mail account for administrative e-mail.',  
    @email_address = 'dba@Adventure-Works.com',  
    @display_name = 'AdventureWorks Automated Mailer',  
    @mailserver_name = 'smtp.Adventure-Works.com' ;  
```  
  
##  <a name="follow-up-next-steps-to-configuring-the-database-mail"></a><a name="FollowUp"></a> 跟进：用于配置数据库邮件的后续步骤  
  
-   [创建数据库邮件配置文件](../../relational-databases/database-mail/create-a-database-mail-profile.md)  
  
  
