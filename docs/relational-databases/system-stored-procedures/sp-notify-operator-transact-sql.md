---
description: sp_notify_operator (Transact-SQL)
title: sp_notify_operator (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_notify_operator_TSQL
- sp_notify_operator
dev_langs:
- TSQL
helpviewer_keywords:
- sp_notify_operator
ms.assetid: c440f5c9-9884-4196-b07c-55d87afb17c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 954a314760a22d9b00996dc460062fcc75af16aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191996"
---
# <a name="sp_notify_operator-transact-sql"></a>sp_notify_operator (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  使用数据库邮件向操作员发送电子邮件。  
  
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_notify_operator  
    [ @profile_name = ] 'profilename' ,  
    [ @id = ] id ,  
    [ @name = ] 'name' ,  
    [ @subject = ] 'subject' ,  
    [ @body = ] 'message' ,  
    [ @file_attachments = ] 'attachment'  
    [ @mail_database = ] 'mail_host_database'  
```  
  
## <a name="arguments"></a>参数  
`[ @profile_name = ] 'profilename'` 用于发送消息的数据库邮件配置文件的名称。 *profilename* 是 **nvarchar (128)**。 如果未指定 *profilename* ，则使用默认数据库邮件配置文件。  
  
`[ @id = ] id` 要向其发送消息的操作员的标识符。 *id* 为 **int**，默认值为 NULL。 必须指定 *id* 或 *名称* 之一。  
  
`[ @name = ] 'name'` 要向其发送消息的操作员的名称。 *name* 为 **nvarchar (128)**，默认值为 NULL。 必须指定 *id* 或 *名称* 之一。  
  
> **注意：** 必须先为操作员定义电子邮件地址，然后才能接收消息。  
  
`[ @subject = ] 'subject'` 电子邮件的主题。 *subject* 为 **nvarchar (256)** ，无默认值。  
  
`[ @body = ] 'message'` 电子邮件的正文。 *消息* 为 **nvarchar (max)** ，无默认值。  
  
`[ @file_attachments = ] 'attachment'` 要附加到电子邮件的文件的名称。 *附件* 是 **nvarchar (512)**，无默认值。  
  
`[ @mail_database = ] 'mail_host_database'` 指定邮件主机数据库的名称。 *mail_host_database* **(128) 为 nvarchar**。 如果未指定 *mail_host_database* ，则默认情况下使用 **msdb** 数据库。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 将指定消息发送到指定操作员的电子邮件地址。 如果操作员没有配置电子邮件地址，则返回一个错误。  
  
 必须先配置数据库邮件和邮件主机数据库才能将通知发送给操作员。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `François Ajenstat` 数据库邮件配置文件将一份通知电子邮件发送给操作员 `AdventureWorks Administrator` 电子邮件的主题是 `Test Notification`。 此电子邮件包含一句话“This is a test of notification via e-mail”。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_notify_operator  
   @profile_name = N'AdventureWorks Administrator',  
   @name = N'François Ajenstat',  
   @subject = N'Test Notification',  
   @body = N'This is a test of notification via e-mail.' ;  
GO  
```  
  
## <a name="see-also"></a>请参阅  
 [SQL Server 代理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_operator &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [sp_help_operator &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [sp_delete_operator &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)  
  
  
