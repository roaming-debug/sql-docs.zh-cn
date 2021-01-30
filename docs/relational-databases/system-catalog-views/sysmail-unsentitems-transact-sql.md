---
description: sysmail_unsentitems (Transact-SQL)
title: sysmail_unsentitems (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_unsentitems_TSQL
- sysmail_unsentitems
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_unsentitems database mail view
ms.assetid: 993c12da-41e5-4e53-a188-0323feb70c67
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c532febf9074456ec1806fdb9d7ad0eed9b9b26a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199691"
---
# <a name="sysmail_unsentitems-transact-sql"></a>sysmail_unsentitems (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  每个包含 "未 **发送** " 或 " **重试** " 状态的数据库邮件消息占一行。 具有“未发送”或“正在重试”状态的消息仍位于邮件队列中，并可能随时发送。 消息可能具有未 **发送** 状态，原因如下：  
  
-   消息为新消息，并且虽然该消息已放在邮件队列中，但数据库邮件正在处理其他消息，尚未处理该消息。  
  
-   数据库邮件外部程序未运行，没有发送任何邮件。  
  
 消息可能会因为以下原因而 **重试** 状态：  
  
-   数据库邮件已尝试发送邮件，但无法与 SMTP 邮件服务器联系。 数据库邮件将继续尝试使用其他数据库邮件帐户（分配给发送消息的配置文件）发送消息。 如果任何帐户都不能发送邮件，数据库邮件将等待为 **帐户重试延迟** 参数配置的时间长度，然后再次尝试发送消息。 数据库邮件使用 " **帐户重试尝试** " 参数确定尝试发送消息的次数。 只要数据库邮件尝试发送消息，消息就会保持 **重试** 状态。  
  
 如果要查看正在等待发送的消息数以及消息已在邮件队列中存在的时间，则请使用该视图。 通常，未 **发送** 的消息数将很少。 请在正常操作过程中进行基准测试，以确定消息队列中合理的消息数以便进行操作。  
  
 若要查看数据库邮件处理的所有消息，请使用 [sysmail_allitems &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)。 若要仅查看状态为 "失败" 的消息，请使用 " [sysmail_faileditems &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)"。 若要仅查看已发送的消息，请使用 [&#40;transact-sql&#41;sysmail_sentitems ](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**mailitem_id**|**int**|邮件队列中邮件项的标识符。|  
|**profile_id**|**int**|提交消息所用配置文件的标识符。|  
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
|**查询**|**varchar(max)**|邮件程序所执行的查询。|  
|**execute_query_database**|**sysname**|邮件程序在其中执行查询的数据库上下文。|  
|**attach_query_result_as_file**|**bit**|如果该值为 0，则查询结果包含在电子邮件的正文中，在正文的内容之后。 如果该值为 1，则结果作为附件返回。|  
|**query_result_header**|**bit**|如果值为1，则查询结果包含列标题。 如果该值为0，则查询结果不包括列标题。|  
|**query_result_width**|**int**|消息的 **query_result_width** 参数。|  
|**query_result_separator**|**char(1)**|用于分隔查询输出中的各列的字符。|  
|**exclude_query_output**|**bit**|消息的 **exclude_query_output** 参数。 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_send_dbmail ](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md)。|  
|**append_query_error**|**bit**|消息的 **append_query_error** 参数。 0 指示如果查询中存在错误，则数据库邮件不应发送电子邮件。|  
|**send_request_date**|**datetime**|将消息放在邮件队列中的日期和时间。|  
|**send_request_user**|**sysname**|提交消息的用户。 这是数据库邮件过程的用户上下文，不是消息的 " **发件** 字段" 字段。|  
|**sent_account_id**|**int**|发送消息所用数据库邮件帐户的标识符。 对于该视图，始终为 NULL。|  
|**sent_status**|**varchar (8)**|如果数据库邮件尚未尝试发送邮件，则将 **发送** 该邮件。 如果数据库邮件无法发送消息，但正在重试，将 **重试** 。|  
|**sent_date**|**datetime**|数据库邮件上次尝试发送邮件的日期和时间。 如果数据库邮件尚未尝试发送消息，则为 NULL。|  
|**last_mod_date**|**datetime**|上次修改行的日期和时间。|  
|**last_mod_user**|**sysname**|上次修改行的用户。|  
  
## <a name="remarks"></a>备注  
 排除数据库邮件故障时，该视图通过向您显示正在等待发送的消息数以及消息已等待的时间，可帮助您确定问题的本质。 如果没有发送任何消息，则数据库邮件外部程序可能未运行，或者可能存在网络问题阻止了数据库邮件与 SMTP 服务器的联系。 如果许多未发送的消息具有相同的 **profile_id**，则 SMTP 服务器可能存在问题。 请考虑向配置文件中添加其他帐户。 如果正在发送消息，但消息在队列中等待的时间过长，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能需要更多的资源来处理您所需的消息量。  
  
## <a name="permissions"></a>权限  
 授予 **sysadmin** 固定服务器角色和 **DatabaseMailUserRole** 数据库角色。 当由 **sysadmin** 固定服务器角色的成员执行时，此视图将显示所有未 **发送** 或正在 **重试** 的消息。 所有其他用户只能看到他们提交的未 **发送** 或 **重试** 消息。  
  
  
