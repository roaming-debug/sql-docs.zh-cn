---
description: sysmail_add_account_sp (Transact-SQL)
title: sysmail_add_account_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_account_sp
- sysmail_add_account_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_account_sp
ms.assetid: 65e15e2e-107c-49c3-b12c-f4edf0eb1617
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e5458a79b5cb44aef8f8a931051cc8346bf4e88f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178077"
---
# <a name="sysmail_add_account_sp-transact-sql"></a>sysmail_add_account_sp (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建一个新的数据库邮件帐户，用于保存有关 SMTP 帐户的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_add_account_sp  [ @account_name = ] 'account_name',  
    [ @email_address = ] 'email_address' ,  
    [ [ @display_name = ] 'display_name' , ]  
    [ [ @replyto_address = ] 'replyto_address' , ]  
    [ [ @description = ] 'description' , ]  
    [ @mailserver_name = ] 'server_name'   
    [ , [ @mailserver_type = ] 'server_type' ]  
    [ , [ @port = ] port_number ]  
    [ , [ @username = ] 'username' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @use_default_credentials = ] use_default_credentials ]  
    [ , [ @enable_ssl = ] enable_ssl ]  
    [ , [ @account_id = ] account_id OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @account_name = ] 'account_name'` 要添加的帐户的名称。 *account_name* **sysname**，无默认值。  
  
`[ @email_address = ] 'email_address'` 要从其发送消息的电子邮件地址。 该地址必须是 Internet 电子邮件地址。 *email_address* 为 **nvarchar (128)**，无默认值。 例如，代理的帐户 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能会通过地址 **SqlAgent \@ Adventure-Works.com** 发送电子邮件。  
  
`[ @display_name = ] 'display_name'` 此帐户的电子邮件中使用的显示名称。 *display_name* 为 **nvarchar (128)**，默认值为 NULL。 例如，代理的帐户 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能会在电子邮件上显示 **SQL Server 代理自动** 发送邮件的名称。  
  
`[ @replyto_address = ] 'replyto_address'` 将此帐户中的消息响应发送到的地址。 *replyto_address* 为 **nvarchar (128)**，默认值为 NULL。 例如，对代理帐户的答复 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能会发送给数据库管理员 **danw \@ Adventure-Works.com**。  
  
`[ @description = ] 'description'` 帐户的说明。 *描述* 为 **nvarchar (256)**，默认值为 NULL。  
  
`[ @mailserver_name = ] 'server_name'` 要用于此帐户的 SMTP 邮件服务器的名称或 IP 地址。 运行的计算机 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须能够将 *server_name* 解析为 IP 地址。 *server_name* **sysname**，无默认值。  
  
`[ @mailserver_type = ] 'server_type'` 电子邮件服务器的类型。 *server_type* 的值为 **sysname**，默认值为 **"SMTP"**。  
  
`[ @port = ] port_number` 电子邮件服务器的端口号。 *port_number* 的值为 **int**，默认值为25。  
  
`[ @username = ] 'username'` 用于登录到电子邮件服务器的用户名。 *username* 为 **nvarchar (128)**，默认值为 NULL。 如果此参数为 NULL，则数据库电子邮件不对此帐户进行身份验证。 如果邮件服务器不要求身份验证，则使用 NULL 作为用户名。  
  
`[ @password = ] 'password'` 用于登录到电子邮件服务器的密码。 *密码* 为 **nvarchar (128)**，默认值为 NULL。 除非指定了用户名，否则无需提供密码。  
  
`[ @use_default_credentials = ] use_default_credentials` 指定是否使用的凭据将邮件发送到 SMTP 服务器 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 。 **use_default_credentials** 为 bit，默认值为0。 当此参数为 1 时，数据库邮件使用[!INCLUDE[ssDE](../../includes/ssde-md.md)]的凭据。 当此参数为0时，数据库邮件将发送 **\@ 用户名** 和 **\@ 密码** 参数（如果存在），否则将发送不含 **\@ 用户名** 和 **\@ 密码** 参数的邮件。  
  
`[ @enable_ssl = ] enable_ssl` 指定数据库邮件是否使用安全套接字层来加密通信。 **Enable_ssl** 为 bit，默认值为0。  
  
`[ @account_id = ] account_id OUTPUT` 返回新帐户的帐户 id。 *account_id* 的值为 **int**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 数据库邮件为 **\@ email_address**、 **\@ display_name** 和 **\@ replyto_address** 提供单独的参数。 **\@ Email_address** 参数是发送消息的地址。 **\@ Display_name** 参数是电子邮件的 "**发件人：** " 字段中显示的名称。 **\@ Replyto_address** 参数是发送电子邮件答复的地址。 例如，用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的帐户可以从仅用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的电子邮件地址发送电子邮件。 来自该地址的邮件应显示友好名称，以便收件人可以轻松确定是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理发来的邮件。 如果收件人回复该邮件，则回复应发送给数据库管理员，而不是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理使用的地址。 对于这种情况，帐户将 **SqlAgent@Adventure-Works.com** 用作电子邮件地址。 显示名称设置为 **SQL Server 代理自动邮件散播**。 帐户使用 **danw@Adventure-Works.com** 作为回复地址，因此回复此帐户发来的消息将发送给数据库管理员，而不是代理的电子邮件地址 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 通过为上述三个参数提供独立的设置，数据库邮件允许您配置邮件来适应您的需求。  
  
 **\@ Mailserver_type** 参数支持值 **"SMTP"**。  
  
 如果 **\@ use_default_credentials** 为1，则使用的凭据将邮件发送到 SMTP 服务器 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 。 如果 **\@ use_default_credentials** 为0，并且为帐户指定了 **\@ 用户名** 和 **\@ 密码**，则帐户将使用 SMTP 身份验证。 **\@ 用户名** 和 **\@ 密码** 是帐户用于 SMTP 服务器的凭据，而不是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机所在网络的凭据。  
  
 存储过程 **sysmail_add_account_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 以下示例将创建一个名为 `AdventureWorks Administrator` 的帐户。 此帐户使用电子邮件地址 `dba@Adventure-Works.com`，并将邮件发送到 SMTP 邮件服务器 `smtp.Adventure-Works.com`。 从此帐户发送的电子邮件 `AdventureWorks Automated Mailer` 将显示在消息的 " **发件人：** " 行中。 对此邮件的回复将发往 `danw@Adventure-Works.com`。  
  
```  
EXECUTE msdb.dbo.sysmail_add_account_sp  
    @account_name = 'AdventureWorks Administrator',  
    @description = 'Mail account for administrative e-mail.',  
    @email_address = 'dba@Adventure-Works.com',  
    @display_name = 'AdventureWorks Automated Mailer',  
    @mailserver_name = 'smtp.Adventure-Works.com' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [创建数据库邮件帐户](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
