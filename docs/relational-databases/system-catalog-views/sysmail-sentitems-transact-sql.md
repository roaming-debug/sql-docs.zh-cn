---
description: sysmail_sentitems (Transact-SQL)
title: sysmail_sentitems (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_sentitems_TSQL
- sysmail_sentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_sentitems database mail view
ms.assetid: 16eb2a44-cebb-4cec-93ac-e2498c39989f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 06dff45056c38800c0aa72ecc28e462086e8e5b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100137"
---
# <a name="sysmail_sentitems-transact-sql"></a>sysmail_sentitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  数据库邮件发送的每个消息都在视图中占一行。 若要查看哪些消息已成功发送，请使用 **sysmail_sentitems** 。  
  
 若要查看数据库邮件处理的所有消息，请使用 [sysmail_allitems &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)。 若要仅查看状态为 "失败" 的消息，请使用 " [sysmail_faileditems &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)"。 若要仅查看未发送或重试的消息，请使用 [&#40;transact-sql&#41;sysmail_unsentitems ](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md)。 若要查看电子邮件附件，请使用 [&#40;transact-sql&#41;sysmail_mailattachments ](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|邮件队列中邮件项的标识符。|  
|**profile_id**|**int**|发送消息所用配置文件的标识符。|  
|**者**|**varchar(max)**|消息收件人的电子邮件地址。|  
|**copy_recipients**|**varchar(max)**|接收消息副本的用户的电子邮件地址。|  
|**blind_copy_recipients**|**varchar(max)**|接收消息副本但其姓名未出现在消息标头中的用户的电子邮件地址。|  
|**subject**|**nvarchar (510)**|消息的主题行。|  
|**body**|**varchar(max)**|消息正文。|  
|**body_format**|**varchar (20)**|消息正文的格式。 可能的值为 **TEXT** 和 **HTML**。|  
|**importance**|**varchar (6)**|消息的 **重要性** 参数。|  
|**程度**|**varchar (12)**|消息的 **敏感度** 参数。|  
|**file_attachments**|**varchar(max)**|附加到电子邮件中的文件名列表，以分号分隔。|  
|**attachment_encoding**|**varchar (20)**|邮件附件的类型。|  
|**query**|**varchar(max)**|邮件程序所执行的查询。|  
|**execute_query_database**|**sysname**|邮件程序在其中执行查询的数据库上下文。|  
|**attach_query_result_as_file**|**bit**|如果该值为 0，则查询结果包含在电子邮件的正文中，在正文的内容之后。 如果该值为 1，则结果作为附件返回。|  
|**query_result_header**|**bit**|如果值为1，则查询结果包含列标题。 如果该值为0，则查询结果不包括列标题。|  
|**query_result_width**|**int**|消息的 **query_result_width** 参数。|  
|**query_result_separator**|**char(1)**|用于分隔查询输出中的各列的字符。|  
|**exclude_query_output**|**bit**|消息的 **exclude_query_output** 参数。 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_send_dbmail ](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md)。|  
|**append_query_error**|**bit**|消息的 **append_query_error** 参数。 0 指示如果查询中存在错误，则数据库邮件不应发送电子邮件。|  
|**send_request_date**|**datetime**|将消息放在邮件队列中的日期和时间。|  
|**send_request_user**|**sysname**|发送消息的用户。 这是数据库邮件过程的用户上下文，不是邮件的“发件人:”字段。|  
|**sent_account_id**|**int**|发送消息所用数据库邮件帐户的标识符。|  
|**sent_status**|**varchar (8)**|邮件的状态。 始终为此视图 **发送** 。|  
|**sent_date**|**datetime**|发送消息的日期和时间。|  
|**last_mod_date**|**datetime**|上次修改行的日期和时间。|  
|**last_mod_user**|**sysname**|上次修改行的用户。|  
  
## <a name="remarks"></a>备注  
 排除数据库邮件故障时，该视图通过向您显示已成功发送的消息的属性，可帮助您确定问题的本质。 如果消息成功提交到 SMTP 邮件服务器，则数据库邮件将这些消息标记为已发送。 通常可在几分钟后收到电子邮件，但也可能由于 SMTP 服务器的问题而使电子邮件的接收延迟。 如果 SMTP 邮件服务器接受了消息，则数据库邮件将该消息标记为已发送。 SMTP 邮件服务器中出现的电子邮件错误（例如，无法传递的收件人电子邮件地址）不会返回到数据库邮件。 即使这些电子邮件未被传递，也会将它们记录为已发送。 请解决 SMTP 服务器中的此类错误。 此外，SMTP 邮件服务器还可能将无法传递的消息通知发送到数据库邮件帐户的答复电子邮件地址。  
  
## <a name="permissions"></a>权限  
 授予 **sysadmin** 固定服务器角色和 **databasemailuserrole** 数据库角色。 当由 **sysadmin** 固定服务器角色的成员执行时，此视图将显示所有已发送的消息。 所有其他用户仅可查看他们已发送的消息。  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件消息处理对象](../../relational-databases/database-mail/database-mail-messaging-objects.md)  
  
  
