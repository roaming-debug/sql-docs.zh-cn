---
description: sp_send_dbmail (Transact-SQL)
title: sp_send_dbmail (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sendmail_sp_TSQL
- sendmail_sp
- SP_SEND_DBMAIL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_send_dbmail
ms.assetid: f1d7a795-a3fd-4043-ac4b-c781e76dab47
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 72da3a1013869049b350440108dc4d4b631b5712
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190836"
---
# <a name="sp_send_dbmail-transact-sql"></a>sp_send_dbmail (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  向指定收件人发送电子邮件。 该邮件可能包含查询结果集和/或文件附件。 当邮件成功放入数据库邮件队列中时， **sp_send_dbmail** 将返回该消息的 **mailitem_id** 。 此存储过程位于 **msdb** 数据库中。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_send_dbmail [ [ @profile_name = ] 'profile_name' ]  
    [ , [ @recipients = ] 'recipients [ ; ...n ]' ]  
    [ , [ @copy_recipients = ] 'copy_recipient [ ; ...n ]' ]  
    [ , [ @blind_copy_recipients = ] 'blind_copy_recipient [ ; ...n ]' ]  
    [ , [ @from_address = ] 'from_address' ]  
    [ , [ @reply_to = ] 'reply_to' ]   
    [ , [ @subject = ] 'subject' ]   
    [ , [ @body = ] 'body' ]   
    [ , [ @body_format = ] 'body_format' ]  
    [ , [ @importance = ] 'importance' ]  
    [ , [ @sensitivity = ] 'sensitivity' ]  
    [ , [ @file_attachments = ] 'attachment [ ; ...n ]' ]  
    [ , [ @query = ] 'query' ]  
    [ , [ @execute_query_database = ] 'execute_query_database' ]  
    [ , [ @attach_query_result_as_file = ] attach_query_result_as_file ]  
    [ , [ @query_attachment_filename = ] query_attachment_filename ]  
    [ , [ @query_result_header = ] query_result_header ]  
    [ , [ @query_result_width = ] query_result_width ]  
    [ , [ @query_result_separator = ] 'query_result_separator' ]  
    [ , [ @exclude_query_output = ] exclude_query_output ]  
    [ , [ @append_query_error = ] append_query_error ]  
    [ , [ @query_no_truncate = ] query_no_truncate ]   
    [ , [ @query_result_no_padding = ] @query_result_no_padding ]   
    [ , [ @mailitem_id = ] mailitem_id ] [ OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @profile_name = ] 'profile_name'` 要从其发送消息的配置文件的名称。 *Profile_name* 的类型为 **sysname**，默认值为 NULL。 *Profile_name* 必须是现有数据库邮件配置文件的名称。 当未指定 *profile_name* 时， **sp_send_dbmail** 使用当前用户的默认专用配置文件。 如果用户没有默认的专用配置文件， **sp_send_dbmail** 将使用 **msdb** 数据库的默认公共配置文件。 如果用户没有默认的专用配置文件，并且没有数据库的默认公共配置文件，则必须指定 **\@ profile_name** 。  
  
`[ @recipients = ] 'recipients'` 要向其发送消息的电子邮件地址的列表，以分号分隔。 收件人列表的类型为 **varchar (max)**。 虽然此参数是可选参数，但至少必须指定 **\@ 收件人**、 **\@ copy_recipients** 或 **\@ blind_copy_recipients** 中的一个，否则 **sp_send_dbmail** 会返回错误。  
  
`[ @copy_recipients = ] 'copy_recipients'` 用分号分隔的电子邮件地址列表，将消息复制到该列表中。 复制收件人列表的类型为 **varchar (max)**。 虽然此参数是可选参数，但至少必须指定 **\@ 收件人**、 **\@ copy_recipients** 或 **\@ blind_copy_recipients** 中的一个，否则 **sp_send_dbmail** 会返回错误。  
  
`[ @blind_copy_recipients = ] 'blind_copy_recipients'` 用分号分隔的电子邮件地址列表，将消息复制到该列表中。 盲人复制收件人列表的类型为 **varchar (max)**。 虽然此参数是可选参数，但至少必须指定 **\@ 收件人**、 **\@ copy_recipients** 或 **\@ blind_copy_recipients** 中的一个，否则 **sp_send_dbmail** 会返回错误。  
  
`[ @from_address = ] 'from_address'` 电子邮件的 "发件人地址" 的值。 这是一个可选参数，用于覆盖邮件配置文件中的设置。 此参数的类型为 **varchar (MAX)**。 SMTP 安全设置决定是否接受这些覆盖。 如果未指定参数，则默认值为 NULL。  
  
`[ @reply_to = ] 'reply_to'` 电子邮件的 "回复地址" 的值。 它只接受一个电子邮件地址作为有效值。 这是一个可选参数，用于覆盖邮件配置文件中的设置。 此参数的类型为 **varchar (MAX)**。 SMTP 安全设置决定是否接受这些覆盖。 如果未指定参数，则默认值为 NULL。  
  
`[ @subject = ] 'subject'` 电子邮件的主题。 使用者的类型为 **nvarchar (255)**。 如果未指定主题，则默认为“SQL Server 消息”。  
  
`[ @body = ] 'body'` 电子邮件的正文。 消息正文的类型为 **nvarchar (max)**，默认值为 NULL。  
  
`[ @body_format = ] 'body_format'` 消息正文的格式。 参数的类型为 **varchar (20)**，默认值为 NULL。 如果已指定，则待发邮件的标头设置会指示邮件正文具有指定格式。 该参数可能包含下列值之一：  
  
-   TEXT  
  
-   HTML  
  
 默认为 TEXT。  
  
`[ @importance = ] 'importance'` 消息的重要性。 参数的类型为 **varchar (6)**。 该参数可能包含下列值之一：  
  
-   低  
  
-   普通  
  
-   高  
  
 默认值为 Normal。  
  
`[ @sensitivity = ] 'sensitivity'` 邮件的敏感度。 参数的类型为 **varchar (12)**。 该参数可能包含下列值之一：  
  
-   普通  
  
-   个人  
  
-   专用  
  
-   机密  
  
 默认值为 Normal。  
  
`[ @file_attachments = ] 'file_attachments'` 要附加到电子邮件的文件名称的列表，以分号分隔。 必须使用绝对路径指定列表中的文件。 附件列表的类型为 **nvarchar (max)**。 默认情况下，数据库邮件会将文件附件限制在每个文件 1 MB 大小。  
 
 > [!IMPORTANT]
 > 此参数在 Azure SQL 托管实例中不可用，因为它无法访问本地文件系统。
  
`[ @query = ] 'query'` 是要执行的查询。 查询结果可以作为文件附加，或包含在电子邮件的正文中。 查询的类型为 **nvarchar (max)**，并且可以包含任何有效的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 请注意，查询在单独的会话中执行，因此，调用 **sp_send_dbmail** 的脚本中的局部变量不能用于查询。  
  
`[ @execute_query_database = ] 'execute_query_database'` 是存储过程在其中运行查询的数据库上下文。 参数的类型为 **sysname**，默认值为当前数据库。 仅当指定了 **\@ query** 时，此参数才适用。  
  
`[ @attach_query_result_as_file = ] attach_query_result_as_file` 指定是否以附加的文件形式返回查询的结果集。 *attach_query_result_as_file* 的类型为 **bit**，默认值为0。  
  
 如果该值为0，则查询结果将包含在电子邮件的正文中，位于 **\@ 正文** 参数的内容之后。 如果值为1，则结果作为附件返回。 仅当指定了 **\@ query** 时，此参数才适用。  
  
`[ @query_attachment_filename = ] query_attachment_filename` 指定要用于查询附件的结果集的文件名。 *query_attachment_filename* 的类型为 **nvarchar (255)**，默认值为 NULL。 当 *attach_query_result* 为0时，将忽略此参数。 当 *attach_query_result* 为1且此参数为 NULL 时，数据库邮件创建任意文件名。  
  
`[ @query_result_header = ] query_result_header` 指定查询结果是否包括列标题。 Query_result_header 值为 **bit** 类型。 如果该值为 1，则查询结果包含列标题。 如果该值为 0，则查询结果不包含列标题。 此参数默认为 **1**。 仅当指定了 **\@ query** 时，此参数才适用。  
 
   >[!NOTE]
   > 将 \@ query_result_header 设置为0并将 query_no_truncate 设置为1时，可能会发生以下错误 \@ ：
   > <br> 消息22050，级别16，状态1，第12行：未能初始化 sqlcmd 库，错误编号为-2147024809。
  
`[ @query_result_width = ] query_result_width` 用于设置查询结果的格式的线条宽度（以字符为字符）。 *Query_result_width* 的类型为 **int**，默认值为256。 提供的值必须介于 10 和 32767 之间。 仅当指定了 **\@ query** 时，此参数才适用。  
  
`[ @query_result_separator = ] 'query_result_separator'` 用于分隔查询输出中的列的字符。 分隔符的类型为 **char (1)**。 默认为“ ”（空格）。  
  
`[ @exclude_query_output = ] exclude_query_output` 指定是否在电子邮件中返回查询执行的输出。 **exclude_query_output** 为 bit，默认值为0。 当此参数为0时， **sp_send_dbmail** 存储过程的执行将在控制台上打印作为查询执行结果返回的消息。 如果此参数为1，则 **sp_send_dbmail** 存储过程的执行不会在控制台上打印任何查询执行消息。  
  
`[ @append_query_error = ] append_query_error`指定在 **\@ 查询** 参数中指定的查询返回错误时是否发送电子邮件。 **append_query_error** 为 **bit**，默认值为0。 如果此参数的值为 1，则数据库邮件会发送电子邮件，并在电子邮件的正文中包括查询错误消息。 当此参数为0时，数据库邮件不发送电子邮件， **sp_send_dbmail** 以返回代码1结尾，表示失败。  
  
`[ @query_no_truncate = ] query_no_truncate` 指定是否通过选项来执行查询，以避免截断大型可变长度数据类型 (**varchar (max)**、 **nvarchar (max)**、 **varbinary (max)**、 **xml**、 **text**、 **ntext**、 **image** 和用户定义数据类型) 。 设置该选项后，查询结果将不包含列标题。 *Query_no_truncate* 值为 **bit** 类型。 当该值为 0 或未指定时，查询中的列将截断为 256 个字符。 当该值为 1 时，不截断查询中的列。 此参数的默认值为 0。  
  
> [!NOTE]  
>  与大量数据一起使用时， \@ **query_no_truncate** 选项会消耗额外的资源，从而降低服务器性能。  
  
`[ @query_result_no_padding ] @query_result_no_padding` 类型为 bit。 默认值为 0。 如果将设置为1，则不会填充查询结果，这可能会减少文件大小。如果将 \@ query_result_no_padding 设置为1，并且设置了 \@ query_result_width 参数， \@ query_result_no_padding 参数将覆盖 \@ query_result_width 参数。  
  
 在这种情况下，不发生错误。  
 
  >[!NOTE]
  > 将 \@ query_result_no_padding 设置为1并为 query_no_truncate 提供参数时，可能会出现以下错误 \@ ：
  > <br> 消息22050，级别16，状态1，第0行：执行查询失败，因为 \@ query_result_no_append 和 \@ query_no_truncate 选项是互斥的。 
  
 如果将 query_result_no_padding 设置 \@ 为1，并且设置了 \@ query_no_truncate 参数，则会引发错误。  
  
`[ @mailitem_id = ] mailitem_id [ OUTPUT ]` 可选的输出参数返回消息的 *mailitem_id* 。 *Mailitem_id* 的类型为 **int**。  
  
## <a name="return-code-values"></a>返回代码值  
 返回代码 0 表示成功。 任何其他值表示失败。 失败语句的错误代码存储在 \@ \@ 错误变量中。  
  
## <a name="result-sets"></a>结果集  
 成功时，返回消息“邮件已排队”。  
  
## <a name="remarks"></a>备注  
 使用之前，必须使用数据库邮件配置向导或 **sp_configure** 启用数据库邮件。  
  
 **sysmail_stop_sp** 通过停止外部程序使用的 Service Broker 对象停止数据库邮件。 使用 **sysmail_stop_sp** 停止数据库邮件时， **sp_send_dbmail** 仍接受邮件。 若要开始数据库邮件，请使用 **sysmail_start_sp**。  
  
 如果未指定 **\@ 配置文件**，则 **sp_send_dbmail** 使用默认配置文件。 如果发送电子邮件的用户具有默认专用配置文件，则数据库邮件使用该配置文件。 如果用户没有默认的专用配置文件， **sp_send_dbmail** 将使用默认的公共配置文件。 如果用户没有默认的专用配置文件，并且没有默认的公共配置文件， **sp_send_dbmail** 将返回错误。  
  
 **sp_send_dbmail** 不支持不含内容的电子邮件。 若要发送电子邮件，必须至少指定一个 **\@ 正文**、 **\@ 查询**、 **\@ file_attachments** 或 **\@ 使用者**。 否则， **sp_send_dbmail** 将返回错误。  
  
 数据库邮件使用当前用户的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 安全上下文控制对文件的访问。 因此，通过身份验证进行身份验证的用户 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法使用 **\@ file_attachments** 来附加文件。 Windows 不允许 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从一台远程计算机向另一台远程计算机提供凭据。 所以，如果从运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机以外的计算机运行该命令，则数据库邮件可能无法从网络共享附加文件。  
  
 如果同时指定了 **\@ query** 和 **\@ file_attachments** 但找不到该文件，则仍会执行该查询，但不会发送电子邮件。  
  
 指定查询后，结果集的格式被设置为内联文本。 使用十六进制格式发送结果中的二进制数据。  
  
 参数 **\@ 接收** 方、 **\@ copy_recipients** 和 **\@ blind_copy_recipients** 是以分号分隔的电子邮件地址列表。 必须提供至少一个参数，否则 **sp_send_dbmail** 将返回错误。  
  
 在没有事务上下文的情况下执行 **sp_send_dbmail** 时，数据库邮件启动并提交隐式事务。 在现有事务中执行 **sp_send_dbmail** 时，数据库邮件依赖于用户提交或回滚任何更改。 它不会启动内部事务。  
  
## <a name="permissions"></a>权限  
 **Sp_send_dbmail** 默认设置为 **Msdb** 数据库中 **DatabaseMailUser** 数据库角色的所有成员的权限。 但是，当发送该消息的用户无权使用该请求的配置文件时， **sp_send_dbmail** 将返回错误，并且不会发送消息。  
  
## <a name="examples"></a>示例  
  
### <a name="a-sending-an-e-mail-message"></a>A. 发送电子邮件  
 此示例使用电子邮件地址向你的朋友发送电子邮件 `myfriend@Adventure-Works.com` 。 该邮件的主题为 `Automated Success Message`。 邮件正文包含一句话 `'The stored procedure finished successfully'`。  
  
```  
EXEC msdb.dbo.sp_send_dbmail  
    @profile_name = 'Adventure Works Administrator',  
    @recipients = 'yourfriend@Adventure-Works.com',  
    @body = 'The stored procedure finished successfully.',  
    @subject = 'Automated Success Message' ;  
```  
  
### <a name="b-sending-an-e-mail-message-with-the-results-of-a-query"></a>B. 发送包含查询结果的电子邮件  
 此示例使用电子邮件地址向你的朋友发送电子邮件 `yourfriend@Adventure-Works.com` 。 该邮件的主题为 `Work Order Count`，并执行查询以显示 `DueDate` 在 2004 年 4 月 30 日后的两日内的工单数。 数据库邮件将该结果附加为文本文件。  
  
```  
EXEC msdb.dbo.sp_send_dbmail  
    @profile_name = 'Adventure Works Administrator',  
    @recipients = 'yourfriend@Adventure-Works.com',  
    @query = 'SELECT COUNT(*) FROM AdventureWorks2012.Production.WorkOrder  
                  WHERE DueDate > ''2004-04-30''  
                  AND  DATEDIFF(dd, ''2004-04-30'', DueDate) < 2' ,  
    @subject = 'Work Order Count',  
    @attach_query_result_as_file = 1 ;  
```  
  
### <a name="c-sending-an-html-e-mail-message"></a>C. 发送 HTML 电子邮件  
 此示例使用电子邮件地址向你的朋友发送电子邮件 `yourfriend@Adventure-Works.com` 。 邮件的主题为 `Work Order List`，并包含一个 HTML 文档，其中列出了 `DueDate` 在 2004 年 4 月 30 日后的二日内的工单数。 数据库邮件使用 HTML 格式发送该邮件。  
  
```  
DECLARE @tableHTML  NVARCHAR(MAX) ;  
  
SET @tableHTML =  
    N'<H1>Work Order Report</H1>' +  
    N'<table border="1">' +  
    N'<tr><th>Work Order ID</th><th>Product ID</th>' +  
    N'<th>Name</th><th>Order Qty</th><th>Due Date</th>' +  
    N'<th>Expected Revenue</th></tr>' +  
    CAST ( ( SELECT td = wo.WorkOrderID,       '',  
                    td = p.ProductID, '',  
                    td = p.Name, '',  
                    td = wo.OrderQty, '',  
                    td = wo.DueDate, '',  
                    td = (p.ListPrice - p.StandardCost) * wo.OrderQty  
              FROM AdventureWorks.Production.WorkOrder as wo  
              JOIN AdventureWorks.Production.Product AS p  
              ON wo.ProductID = p.ProductID  
              WHERE DueDate > '2004-04-30'  
                AND DATEDIFF(dd, '2004-04-30', DueDate) < 2   
              ORDER BY DueDate ASC,  
                       (p.ListPrice - p.StandardCost) * wo.OrderQty DESC  
              FOR XML PATH('tr'), TYPE   
    ) AS NVARCHAR(MAX) ) +  
    N'</table>' ;  
  
EXEC msdb.dbo.sp_send_dbmail @recipients='yourfriend@Adventure-Works.com',  
    @subject = 'Work Order List',  
    @body = @tableHTML,  
    @body_format = 'HTML' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)   
 [sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)  
  
  
