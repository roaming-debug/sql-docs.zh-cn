---
description: sysmail_configure_sp (Transact-SQL)
title: sysmail_configure_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_configure_sp_TSQL
- sysmail_configure_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_configure_sp
ms.assetid: 73b33c56-2bff-446a-b495-ae198ad74db1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c150f6707a5f0ec537613617182b9da9889d85a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188657"
---
# <a name="sysmail_configure_sp-transact-sql"></a>sysmail_configure_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改数据库邮件的配置设置。 使用 **sysmail_configure_sp** 指定的配置设置适用于整个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_configure_sp [ [ @parameter_name = ] 'parameter_name' ]  
    [ , [ @parameter_value = ] 'parameter_value' ]  
    [ , [ @description = ] 'description' ]  
```  
  
## <a name="arguments"></a>参数  
 [ **@parameter_name** =] **"**_parameter_name_**"**  
 要更改的参数的名称。  
  
 [ **@parameter_value** =] **"**_parameter_value_**"**  
 参数的新值。  
  
 [ **@description** =] **"**_description_**"**  
 参数的说明。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 数据库邮件使用以下参数：  
  
| 参数名称 | 说明 | 默认值 |
| -------------- | ----------- | ------------- |
|*AccountRetryAttempts*|外部电子邮件进程尝试使用指定配置文件中的每个帐户发送电子邮件的次数。|**1**|  
|*AccountRetryDelay*|外部邮件进程在两次尝试发送邮件之间的等待时间（以秒为单位）。|**5000**|  
|*达到 databasemailexeminimumlifetime*|外部邮件进程保持活动状态的最少时间（以秒为单位）。 如果数据库邮件要发送多个邮件，增加此值可以使数据库邮件保持活动状态，避免频繁启动和停止的开销。|**600**|  
|*DefaultAttachmentEncoding*|电子邮件附件的默认编码。|MIME|  
|*MaxFileSize*|附件的最大大小（以字节为单位）。|**1000000**|  
|*ProhibitedExtensions*|一组以逗号分隔的扩展名，具有这些扩展名的文件不能作为电子邮件附件发送。|**exe,dll,vbs,js**|  
|*Logginglevel.information*|指定数据库邮件日志中要记录的消息。 以下数值之一：<br /><br /> 1 - 表示正常模式。 仅记录错误。<br /><br /> 2 - 表示扩展模式。 记录错误、警告和信息性消息。<br /><br /> 3 - 表示详细模式。 记录错误、警告、信息性消息、成功消息和其他内部消息。 该模式用于进行故障排除。|**2**|  
  
 存储过程 **sysmail_configure_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 **A. 设置数据库邮件，使其重试每个帐户 10 次**  
  
 以下示例将设置数据库邮件，使其重试每个帐户十次，然后才认为帐户不可访问。  
  
```  
EXECUTE msdb.dbo.sysmail_configure_sp  
    'AccountRetryAttempts', '10' ;  
```  
  
 **B. 将附件的最大大小设置为 2 MB**  
  
 以下示例将把附件的最大大小设置为 2 MB。  
  
```  
EXECUTE msdb.dbo.sysmail_configure_sp  
    'MaxFileSize', '2097152' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [sysmail_help_configure_sp &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sysmail-help-configure-sp-transact-sql.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
