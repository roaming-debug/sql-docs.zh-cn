---
description: 发送邮件任务
title: 发送邮件任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sendmailtask.f1
- sql13.dts.designer.sendmailtask.general.f1
- sql13.dts.designer.sendmailtask.mail.f1
helpviewer_keywords:
- mail [Integration Services]
- Send Mail task
- e-mail [Integration Services]
- messages [Integration Services]
- sending messages
ms.assetid: fe0b7cbc-fe8e-4fe2-95b4-2953efff5869
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 362da93d1048564812826fceb3c0f2e76a47977a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348287"
---
# <a name="send-mail-task"></a>发送邮件任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  发送邮件任务可以发送电子邮件。 通过使用发送邮件任务，包可以在包工作流中的任务成功或失败时发送邮件，也可为响应运行时包引发的事件而发送邮件。 例如，该任务可以通知数据库管理员，告知备份数据库任务是成功还是失败。  
  
 可以采用下列方法配置发送邮件任务：  
  
-   提供电子邮件的消息正文。  
  
-   提供电子邮件的主题行。  
  
-   设置邮件的优先级别。 该任务支持三种优先级别：正常、低和高。  
  
-   在 To、Cc 和 Bcc 行中指定收件人。 如果任务指定多个收件人，则收件人之间用分号分隔。  
  
    > [!NOTE]  
    >  根据 Internet 标准，每个 To、Cc 和 Bcc 行最多包含 256 个字符。  
  
-   包含附件。 如果任务指定多个附件，则附件之间用管道符 (|) 分隔。  
  
    > [!NOTE]  
    >  如果包时运行时找不到附件文件，则将产生错误。  
  
-   指定要使用的 SMTP 连接管理器。  
  
    > [!IMPORTANT]  
    >  SMTP 连接管理器仅支持匿名身份验证和 Windows 身份验证， 而不支持基本身份验证。  
  
 消息正文可以是提供的字符串、包含文本的文件连接或包含文本的变量名。 该任务使用文件连接管理器来连接文件。 有关详细信息，请参阅 [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md)。  
  
 该任务使用 SMTP 连接管理器与邮件服务器建立连接。 有关详细信息，请参阅 [SMTP Connection Manager](../../integration-services/connection-manager/smtp-connection-manager.md)。  
  
## <a name="custom-logging-messages-available-on-the-send-mail-task"></a>发送邮件任务可用的自定义日志记录消息  
 下表列出了发送邮件任务的自定义日志项。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
|日志项|说明|  
|---------------|-----------------|  
|**SendMailTaskBegin**|指示任务开始发送电子邮件。|  
|**SendMailTaskEnd**|指示任务已发送完电子邮件。|  
|**SendMailTaskInfo**|提供有关任务的说明性信息。|  
  
## <a name="configuring-the-send-mail-task"></a>配置发送邮件任务  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的信息，请单击以下主题：  
  
-   [“表达式”页](../../integration-services/expressions/expressions-page.md)  
  
 有关以编程方式设置这些属性的信息，请单击以下主题：  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.SendMailTask.SendMailTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的信息，单击 [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)。  
  
## <a name="related-content"></a>相关内容  
  
-   shareourideas.com 上的技术文章 [如何在 C# 中发送具有传递通知的电子邮件](https://go.microsoft.com/fwlink/?LinkId=237625)（如何在 C# 中发送具有传递通知的电子邮件）  
  
## <a name="send-mail-task-editor-general-page"></a>发送邮件任务编辑器（“常规”页）
  可以使用 **“发送邮件任务编辑器”** 对话框的 **“常规”** 页，对发送邮件任务进行命名和说明。  
  
### <a name="options"></a>选项  
 **名称**  
 为发送邮件任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
 **注意** 任务名称在包中必须是唯一的。  
  
 **说明**  
 键入发送邮件任务的说明。  
  
## <a name="send-mail-task-editor-mail-page"></a>发送邮件任务编辑器（“邮件”页）
  使用 **“发送邮件任务编辑器”** 对话框中的 **“邮件”** 页，可以指定收件人、邮件类型和邮件的优先级。 您还可以在邮件中附加文件。 邮件正文可以是您提供的字符串，也可以是指向包含文本的文件连接，还可以是包含文本的变量的名称。  
  
### <a name="options"></a>选项  
 **SMTPConnection**  
 从列表中选择一个 SMTP 连接管理器，或单击“\<New connection...>”创建新的连接管理器。  
  
> [!IMPORTANT]  
>  SMTP 连接管理器仅支持匿名身份验证和 Windows 身份验证， 而不支持基本身份验证。  
  
 **相关主题：** [SMTP 连接管理器](../../integration-services/connection-manager/smtp-connection-manager.md)  
  
 **From**  
 指定发件人的电子邮件地址。  
  
 **收件人**  
 提供收件人的电子邮件地址，用分号分隔。  
  
 **抄送**  
 指定也可以收到邮件副本的各个人员的电子邮件地址，用分号分隔。  
  
 **密件抄送**  
 指定将收到邮件的隐蔽副本 (Bcc) 的各个人员的电子邮件地址，用分号分隔。  
  
 **主题**  
 提供电子邮件的主题。  
  
 **MessageSourceType**  
 选择消息的源类型。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**直接输入**|将源设置为邮件正文。 选择此值将显示动态选项 **MessageSource**。|  
|**文件连接**|将源设置为包含邮件正文的文件。 选择此值将显示动态选项 **MessageSource**。|  
|**变量**|将源设置为包含消息正文的变量。 选择此值将显示动态选项 **MessageSource**。|  
  
 **Priority**  
 设置邮件的优先级。  
  
 **Attachments**  
 为电子邮件附件提供文件名，用竖线 (|) 字符分隔。  
  
> [!NOTE]  
>  根据 Internet 标准，每个 To、Cc 和 Bcc 行最多包含 256 个字符。  
  
### <a name="messagesourcetype-dynamic-options"></a>MessageSourceType 动态选项  
  
#### <a name="messagesourcetype--direct-input"></a>MessageSourceType = 直接输入  
 **MessageSource**  
 键入邮件正文，或单击浏览按钮 (…)，然后在“消息源”对话框中键入邮件内容  。  
  
#### <a name="messagesourcetype--file-connection"></a>MessageSourceType = 文件连接  
 **MessageSource**  
 从列表中选择“文件连接管理器，或单击“\<**New connection...**>”创建新”的连接管理器。  
  
 **相关主题：** [文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)、[文件连接管理器编辑器](../connection-manager/file-connection-manager.md)  
  
#### <a name="messagesourcetype--variable"></a>MessageSourceType = 变量  
 **MessageSource**  
 在列表中选择变量，或单击“\<**New variable...**>”创建新变量。  
  
 **相关主题：** [Integration Services &#40;SSIS&#41; 变量](../../integration-services/integration-services-ssis-variables.md)、[添加变量](../integration-services-ssis-variables.md)  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)   
 [控制流](../../integration-services/control-flow/control-flow.md)  
  
