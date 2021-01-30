---
description: sp_addmessage (Transact-SQL)
title: sp_addmessage (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addmessage
- sp_addmessage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addmessage
ms.assetid: 54746d30-f944-40e5-a707-f2d9be0fb9eb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f9bdbbac913643bd0d44ba7e45b07c8bc3e09789
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209380"
---
# <a name="sp_addmessage-transact-sql"></a>sp_addmessage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将新的用户定义错误消息存储在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]实例中。 可以使用 **sys.databases** 目录视图查看使用 **sp_addmessage** 存储的消息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addmessage [ @msgnum= ] msg_id , [ @severity= ] severity , [ @msgtext= ] 'msg'   
     [ , [ @lang= ] 'language' ]   
     [ , [ @with_log= ] { 'TRUE' | 'FALSE' } ]   
     [ , [ @replace= ] 'replace' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @msgnum = ] msg_id` 消息的 ID。 *msg_id* 的值为 **int** ，默认值为 NULL。 用户定义的错误消息 *msg_id* 可以是介于50001和2147483647之间的整数。 *Msg_id* 和 *语言* 的组合必须是唯一的;如果指定语言的 ID 已存在，则会返回错误。  
  
`[ @severity = ]severity` 错误的严重性级别。 *严重性* 为 **smallint** ，默认值为 NULL。 有效级别的范围为 1 到 25。 有关错误严重性的详细信息，请参阅 [数据库引擎错误严重性](../../relational-databases/errors-events/database-engine-error-severities.md)。  
  
`[ @msgtext = ] 'msg'` 错误消息的文本。 *msg* 为 **nvarchar (255)** ，默认值为 NULL。  
  
`[ @lang = ] 'language'` 此消息的语言。 *language* 的值为 **sysname** ，默认值为 NULL。 由于可以在同一台服务器上安装多种语言，因此 *language* 指定了每条消息的编写语言。 如果省略 *语言* ，则语言是会话的默认语言。  
  
`[ @with_log = ] { 'TRUE' | 'FALSE' }` 指示消息是否在发生时写入 Windows 应用程序日志。 **\@ with_log** 是 **varchar (5)** ，默认值为 FALSE。 如果为 TRUE，则错误始终写入 Windows 应用程序日志。 如果为 FALSE，则错误不会始终写入 Windows 应用程序日志，但仍然可以写入，具体取决于错误是如何引发的。 只有 **sysadmin** 服务器角色的成员才能使用此选项。  
  
> [!NOTE]  
>  如果消息写入了 Windows 应用程序日志，那么它也将被写入[!INCLUDE[ssDE](../../includes/ssde-md.md)]错误日志文件。  
  
`[ @replace = ] 'replace'` 如果指定为字符串 *replace*，将使用新的消息文本和严重级别覆盖现有错误消息。 *replace* 是 **varchar (7)** ，默认值为 NULL。 如果 *msg_id* 已存在，则必须指定此选项。 如果替换美国英语消息，则会为所有其他语言中具有相同 *msg_id* 的所有邮件替换严重性级别。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 对于非英语版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，必须已经存在美国英语版本的消息，然后才能使用另一种语言添加消息。 两种消息版本的严重性必须匹配。  
  
 当本地化包含参数的消息时，使用与原始消息中的参数相应的参数。 在每个参数后都插入感叹号 (!)。  
  
|原始消息|本地化的消息|  
|----------------------|-----------------------|  
|'Original message param 1: %s,<br /><br /> param 2: %d'|'Localized message param 1: %1!,<br /><br /> param 2: %2!'|  
  
 由于语言语法不同，因此，本地化消息中的参数可能不会以原始消息中相同的顺序出现。  
  
## <a name="permissions"></a>权限  
要求具有 **sysadmin** 或 **serveradmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-defining-a-custom-message"></a>A. 定义自定义的消息  
 下面的示例将自定义消息添加到 **sys.databases。**  
  
```  
USE master;  
GO  
EXEC sp_addmessage 50001, 16,   
   N'Percentage expects a value between 20 and 100.   
   Please reexecute with a more appropriate value.';  
GO  
```  
  
### <a name="b-adding-a-message-in-two-languages"></a>B. 用两种语言添加消息  
 下面的示例首先用美国英语添加一条消息，然后用法语添加同一条消息。`.`  
  
```  
USE master;  
GO  
EXEC sp_addmessage @msgnum = 60000, @severity = 16,   
   @msgtext = N'The item named %s already exists in %s.',   
   @lang = 'us_english';  
  
EXEC sp_addmessage @msgnum = 60000, @severity = 16,   
   @msgtext = N'L''élément nommé %1! existe déjà dans %2!',   
   @lang = 'French';  
GO  
```  
  
### <a name="c-changing-the-order-of-parameters"></a>C. 更改参数顺序  
 以下示例首先用美国英语添加一条消息，然后添加本地化消息，其中更改了参数顺序。  
  
```  
USE master;  
GO  
  
EXEC sp_addmessage   
    @msgnum = 60000,   
    @severity = 16,  
    @msgtext =   
        N'This is a test message with one numeric  
        parameter (%d), one string parameter (%s),   
        and another string parameter (%s).',  
    @lang = 'us_english';  
  
EXEC sp_addmessage   
    @msgnum = 60000,   
    @severity = 16,  
    @msgtext =   
        -- In the localized version of the message,  
        -- the parameter order has changed. The   
        -- string parameters are first and second  
        -- place in the message, and the numeric   
        -- parameter is third place.  
        N'Dies ist eine Testmeldung mit einem   
        Zeichenfolgenparameter (%3!),  
        einem weiteren Zeichenfolgenparameter (%2!),   
        und einem numerischen Parameter (%1!).',  
    @lang = 'German';  
GO    
  
-- Changing the session language to use the U.S. English  
-- version of the error message.  
SET LANGUAGE us_english;  
GO  
  
RAISERROR(60000,1,1,15,'param1','param2') -- error, severity, state,  
GO                                       -- parameters.  
  
-- Changing the session language to use the German  
-- version of the error message.  
SET LANGUAGE German;  
GO  
  
RAISERROR(60000,1,1,15,'param1','param2'); -- error, severity, state,   
GO                                       -- parameters.  
```  
  
## <a name="see-also"></a>另请参阅  
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_altermessage &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [sp_dropmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
