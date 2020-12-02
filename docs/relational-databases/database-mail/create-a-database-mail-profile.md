---
description: 创建数据库邮件配置文件
title: 创建数据库邮件配置文件 | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- Database Mail [SQL Server], public profiles
- profiles [SQL Server], Database Mail
- public profiles [Database Mail]
ms.assetid: 58ae749d-6ada-4f9c-bf00-de7c7a992a2d
author: stevestein
ms.author: sstein
ms.openlocfilehash: f3ee012fe4bcd7fa1cd98c51f537035fc6148938
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96125247"
---
# <a name="create-a-database-mail-profile"></a>创建数据库邮件配置文件
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  使用“数据库邮件配置向导”或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 可创建数据库邮件的公共和专用配置文件。 有关邮件配置文件的详细信息，请参阅 [数据库邮件配置文件](database-mail-configuration-objects.md)。
  
-   **开始之前：** [先决条件](#Prerequisites)、[安全性](#Security)  
  
-   **使用以下方法创建数据库邮件的专用配置文件：**  [数据库邮件配置向导](#SSMSProcedure)、[Transact-SQL](#PrivateProfile)  
  
-   **使用以下方法创建数据库邮件的公共配置文件：**  [数据库邮件配置向导](#SSMSProcedure)、[Transact-SQL](#PublicProfile)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 为配置文件创建一个或多个数据库邮件帐户。 有关创建数据库邮件帐户的详细信息，请参阅 [创建数据库邮件帐户](../../relational-databases/database-mail/create-a-database-mail-account.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
 公共配置文件允许有权访问 **msdb** 数据库的任意用户使用该配置文件发送电子邮件。 专用配置文件可由用户或角色来使用。 授予角色访问配置文件的权限可创建更易维护的体系结构。 若要发送邮件，您必须是 **msdb** 数据库中的 **DatabaseMailUserRole** 的成员，并且至少有权访问一个数据库邮件配置文件。  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 创建配置文件帐户和执行存储过程的用户应是 sysadmin 固定服务器角色的成员。  
  
##  <a name="using-database-mail-configuration-wizard"></a><a name="SSMSProcedure"></a> 使用数据库邮件配置向导  
 **创建数据库邮件配置文件**  
  
-   在对象资源管理器中，连接到您要在其上配置数据库邮件的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，然后展开服务器树。  
  
-   展开 **“管理”** 节点  
  
-   双击数据库邮件以打开数据库邮件配置向导。  
  
-   在 **“选择配置任务”** 页上，选择 **“管理数据库邮件帐户和配置文件”** 选项，然后单击 **“下一步”** 。  
  
-   在 **“管理配置文件和帐户”** 页上，选择 **“创建新配置文件”** 选项，然后单击 **“下一步”**。  
  
-   在“新建配置文件”页上，指定配置文件的名称、说明并添加要包括在配置文件中的帐户，然后单击“下一步”。  
  
-   在 **“完成该向导”** 页上，检查要执行的操作，然后单击 **“完成”** 以完成创建新配置文件。  
  
-   **配置数据库邮件专用配置文件：**  
  
    -   打开数据库邮件配置向导。  
  
    -   在 **“选择配置任务”** 页上，选择 **“管理数据库邮件帐户和配置文件”** 选项，然后单击 **“下一步”**。  
  
    -   在 **“管理配置文件和帐户”** 页上，选择 **“管理配置文件安全性”** 选项，然后单击 **“下一步”**。  
  
    -   在 **“专用配置文件”** 选项卡中，选中您要配置的配置文件所对应的复选框，然后单击 **“下一步”**。  
  
    -   在 **“完成该向导”** 页上，检查要执行的操作，然后单击 **“完成”** 以完成配置该配置文件。  
  
-   **配置数据库邮件公共配置文件：**  
  
    -   打开数据库邮件配置向导。  
  
    -   在 **“选择配置任务”** 页上，选择 **“管理数据库邮件帐户和配置文件”** 选项，然后单击 **“下一步”**。  
  
    -   在 **“管理配置文件和帐户”** 页上，选择 **“管理配置文件安全性”** 选项，然后单击 **“下一步”**。  
  
    -   在 **“公共配置文件”** 选项卡中，选中您要配置的配置文件所对应的复选框，然后单击 **“下一步”**。  
  
    -   在 **“完成该向导”** 页上，检查要执行的操作，然后单击 **“完成”** 以完成配置该配置文件。  
  
## <a name="using-transact-sql"></a>“使用 Transact-SQL”  
  
###  <a name="to-create-a-database-mail-private-profile"></a><a name="PrivateProfile"></a> 创建数据库邮件专用配置文件  
  
-   连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
-   若要创建新的配置文件，请运行系统存储过程 [sysmail_add_profile_sp (Transact SQL)](../../relational-databases/system-stored-procedures/sysmail-add-profile-sp-transact-sql.md)，如下所示：  
  
     **EXECUTEmsdb.dbo.sysmail_add_profile_sp**  
  
     \@profile_name = '配置文件名称'   
  
     \@description = '说明'   
  
     其中，\@profile_name 是配置文件的名称，\@description 是配置文件的说明 。 此参数是可选的。  
  
-   对于每个帐户，运行存储过程 [sysmail_add_profileaccount_sp (Transact-SQL)](../../relational-databases/system-stored-procedures/sysmail-add-profileaccount-sp-transact-sql.md)，如下所示：  
  
     **EXECUTEmsdb.dbo.sysmail_add_profileaccount_sp**  
  
     \@profile_name = '配置文件名称'   
  
     \@account_name = '帐户名称'   
  
     \@sequence_number = '配置文件中的帐户的序列号' 。 '  
  
     其中，\@profile_name 是配置文件的名称，\@account_name 是要添加到配置文件的帐户的名称，\@sequence_number 确定在配置文件中使用帐户的顺序  。  
  
-   对于将使用此配置文件发送邮件的每个数据库角色或用户，请向他们授予对此配置文件的访问权限。 对于每个帐户，运行存储过程 [sysmail_add_principalprofile_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-principalprofile-sp-transact-sql.md)，如下所示：  
  
     **EXECUTEmsdb.sysmail_add_principalprofile_sp**  
  
     \@profile_name = '配置文件名称'   
  
     \@ principal_name = '数据库用户或角色的名称 '  
  
     \@is_default = '默认配置文件状态 '  
  
     其中，\@profile_name 是配置文件的名称，\@principal_name 是数据库用户或角色的名称，\@is_default 确定此配置文件是数据库用户还是角色的默认值  。  
  
 以下示例创建了一个数据库邮件帐户和一个数据库邮件专用配置文件，然后将帐户添加到该配置文件中，并向 **msdb** 数据库中的 **DBMailUsers** 数据库角色授予对该配置文件的访问权限。  
  
```  
-- Create a Database Mail account  
EXECUTE msdb.dbo.sysmail_add_account_sp  
    @account_name = 'AdventureWorks Administrator',  
    @description = 'Mail account for administrative e-mail.',  
    @email_address = 'dba@Adventure-Works.com',  
    @replyto_address = 'danw@Adventure-Works.com',  
    @display_name = 'AdventureWorks Automated Mailer',  
    @mailserver_name = 'smtp.Adventure-Works.com' ;  
  
-- Create a Database Mail profile  
EXECUTE msdb.dbo.sysmail_add_profile_sp  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @description = 'Profile used for administrative mail.' ;  
  
-- Add the account to the profile  
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @account_name = 'AdventureWorks Administrator',  
    @sequence_number =1 ;  
  
-- Grant access to the profile to the DBMailUsers role  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @principal_name = 'ApplicationUser',  
    @is_default = 1 ;  
```  
  
###  <a name="to-create-a-database-mail-public-profile"></a><a name="PublicProfile"></a> 创建数据库邮件公共配置文件  
  
-   连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
-   若要创建新的配置文件，请运行系统存储过程 [sysmail_add_profile_sp (Transact SQL)](../../relational-databases/system-stored-procedures/sysmail-add-profile-sp-transact-sql.md)，如下所示：  
  
     **EXECUTEmsdb.dbo.sysmail_add_profile_sp**  
  
     \@profile_name = '配置文件名称'   
  
     \@description = '说明'   
  
     其中，\@profile_name 是配置文件的名称，\@description 是配置文件的说明 。 此参数是可选的。  
  
-   对于每个帐户，运行存储过程 [sysmail_add_profileaccount_sp (Transact-SQL)](../../relational-databases/system-stored-procedures/sysmail-add-profileaccount-sp-transact-sql.md)，如下所示：  
  
     **EXECUTEmsdb.dbo.sysmail_add_profileaccount_sp**  
  
     \@profile_name = '配置文件名称'   
  
     \@account_name = '帐户名称'   
  
     \@sequence_number = '配置文件中的帐户的序列号' 。 '  
  
     其中，\@profile_name 是配置文件的名称，\@account_name 是要添加到配置文件的帐户的名称，\@sequence_number 确定在配置文件中使用帐户的顺序  。  
  
-   若要授予公共访问权限，请运行存储过程 [sysmail_add_principalprofile_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-principalprofile-sp-transact-sql.md)，如下所示：  
  
     **EXECUTEmsdb.sysmail_add_principalprofile_sp**  
  
     \@profile_name = '配置文件名称'   
  
     \@ principal_name = '公共或 0'   
  
     \@is_default = '默认配置文件状态 '  
  
     其中，\@profile_name 是配置文件的名称，\@principal_name 指示其为公共配置文件，\@is_default 确定此配置文件是数据库用户还是角色的默认值  。  
  
 以下示例创建了一个数据库邮件帐户和一个数据库邮件专用配置文件，然后将帐户添加到该配置文件中并授予对该配置文件的公共访问权限。  
  
```  
-- Create a Database Mail account  
  
EXECUTE msdb.dbo.sysmail_add_account_sp  
    @account_name = 'AdventureWorks Public Account',  
    @description = 'Mail account for use by all database users.',  
    @email_address = 'db_users@Adventure-Works.com',  
    @replyto_address = 'danw@Adventure-Works.com',  
    @display_name = 'AdventureWorks Automated Mailer',  
    @mailserver_name = 'smtp.Adventure-Works.com' ;  
  
-- Create a Database Mail profile  
  
EXECUTE msdb.dbo.sysmail_add_profile_sp  
    @profile_name = 'AdventureWorks Public Profile',  
    @description = 'Profile used for administrative mail.' ;  
  
-- Add the account to the profile  
  
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp  
    @profile_name = 'AdventureWorks Public Profile',  
    @account_name = 'AdventureWorks Public Account',  
    @sequence_number =1 ;  
  
-- Grant access to the profile to all users in the msdb database  
  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @profile_name = 'AdventureWorks Public Profile',  
    @principal_name = 'public',  
    @is_default = 1 ;  
```  
  
  
