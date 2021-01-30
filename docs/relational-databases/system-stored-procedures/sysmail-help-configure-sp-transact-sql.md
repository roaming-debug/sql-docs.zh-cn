---
description: sysmail_help_configure_sp (Transact-SQL)
title: sysmail_help_configure_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_configure_sp
- sysmail_help_configure_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_configure_sp
ms.assetid: e598d4c8-3041-4965-b046-dce3a8e3d3e0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 21ce11d37659561e172c11fb7b5a899193141166
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181964"
---
# <a name="sysmail_help_configure_sp-transact-sql"></a>sysmail_help_configure_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  显示数据库邮件的配置设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_help_configure_sp  [ [ @parameter_name = ] 'parameter_name' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @parameter_name = ] 'parameter_name'` 要检索的配置设置的名称。 如果指定此参数，则将在 **\@ parameter_value** 输出参数中返回配置设置的值。 当未指定 **\@ parameter_name** 时，此存储过程将返回一个结果集，其中包含实例中的所有数据库邮件配置设置。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 当未指定 **\@ parameter_name** 时，将返回包含以下列的结果集。  
  
| 列名称 | 数据类型 | 说明 |
| ----------- | --------- | ----------- |
|**paramname**|**nvarchar(256)**|配置参数的名称。|  
|**paramvalue**|**nvarchar(256)**|配置参数的值。|  
|description|**nvarchar(256)**|配置参数的说明。|  
  
## <a name="remarks"></a>备注  
 存储过程 **sysmail_help_configure_sp** 列出实例的当前数据库邮件配置设置。  
  
 如果指定了 **\@ parameter_name** ，但没有为 **\@ parameter_value** 提供输出参数，则此存储过程不会生成任何输出。  
  
 存储过程 **sysmail_help_configure_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来调用该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 以下示例显示如何列出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的数据库邮件配置设置。  
  
```  
EXECUTE msdb.dbo.sysmail_help_configure_sp ;  
```  
  
 下面是示例结果集，由于行的长度原因而进行了编辑：  
  
```  
paramname                       paramvalue      description  
------------------------------- --------------- -----------------------------------------------------------------------------  
AccountRetryAttempts            1               Number of retry attempts for a mail server  
AccountRetryDelay               5000            Delay between each retry attempt to mail server  
DatabaseMailExeMinimumLifeTime  600             Minimum process lifetime in seconds  
DefaultAttachmentEncoding       MIME            Default attachment encoding  
LoggingLevel                    2               Database Mail logging level: normal - 1, extended - 2 (default), verbose - 3  
MaxFileSize                     1000000         Default maximum file size  
ProhibitedExtensions            exe,dll,vbs,js  Extensions not allowed in outgoing mails  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
