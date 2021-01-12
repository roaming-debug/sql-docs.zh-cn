---
description: sysmail_allitems (Transact-SQL)
title: sysmail_allitems (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_allitems_TSQL
- sysmail_allitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_allitems database mail view
ms.assetid: 21fb8432-7677-4435-902f-64a58bba4cbb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6bc9b58549bbc20fe66caaf2adc61edd4345acd7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096651"
---
# <a name="sysmail_allitems-transact-sql"></a>sysmail_allitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  数据库邮件处理的每个消息都在视图中占一行。 如果要查看所有消息的状态，则请使用该视图。  
  
 若要仅查看状态为 "失败" 的消息，请使用 " [sysmail_faileditems &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)"。 若要仅查看未发送的消息，请使用 [&#40;transact-sql&#41;sysmail_unsentitems ](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md)。 若要仅查看已发送的消息，请使用 [&#40;transact-sql&#41;sysmail_sentitems ](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|邮件队列中邮件项的标识符。|  
|**profile_id**|**int**|发送消息所用配置文件的标识符。|  
|**者**|**varchar(max)**|消息收件人的电子邮件地址。|  
|**copy_recipients**|**varchar(max)**|接收消息副本的用户的电子邮件地址。|  
|**blind_copy_recipients**|**varchar(max)**|接收消息副本但其姓名未出现在消息标头中的用户的电子邮件地址。|  
|**subject**|**nvarchar (510)**|消息的主题行。|  
|**body**|**varchar(max)**|消息正文。|  
|**body_format**|**varchar (20)**|消息正文的格式。 可能值为 TEXT 和 HTML。|  
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
|**send_request_user**|**sysname**|提交消息的用户。 这是数据库邮件过程的用户上下文，不是邮件的“发件人:”字段。|  
|**sent_account_id**|**int**|发送消息所用数据库邮件帐户的标识符。|  
|**sent_status**|**varchar (8)**|邮件的状态。 可能的值为：<br /><br /> **已发送** -邮件已发送。<br /><br /> 未 **发送**-数据库邮件仍在尝试发送消息。<br /><br /> 正在 **重试**-数据库邮件无法发送消息，但正在尝试再次发送。<br /><br /> **失败** -数据库邮件无法发送消息。|  
|**sent_date**|**datetime**|发送消息的日期和时间。|  
|**last_mod_date**|**datetime**|上次修改行的日期和时间。|  
|**last_mod_user**|**sysname**|上次修改行的用户。|  
  
## <a name="remarks"></a>备注  
 使用 " **sysmail_allitems** " 视图可以查看数据库邮件处理的所有消息的状态。 排除数据库邮件故障时，该视图通过向您显示已发送的消息的属性（与未发送的消息的属性进行比较），可帮助您确定问题的本质。  
  
 此视图公开的系统表包含所有消息，并且可能会导致 **msdb** 数据库增长。 应定期从视图中删除旧的消息，以减小表的大小。 有关详细信息，请参阅 [创建 SQL Server 代理作业来存档数据库邮件消息和事件日志](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)。  
  
## <a name="permissions"></a>权限  
 授予 **sysadmin** 固定服务器角色和 **DatabaseMailUserRole** 数据库角色。 当由 **sysadmin** 固定服务器角色的成员执行时，此视图将显示所有消息。 所有其他用户仅可查看他们已提交的消息。  
  
  
