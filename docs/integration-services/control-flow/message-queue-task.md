---
description: Message Queue Task
title: 消息队列任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.messagequeuetask.f1
- sql13.dts.designer.msgqueuetask.general.f1
- sql13.dts.designer.msgqueuetask.send.f1
- sql13.dts.designer.msgqueuetask.receive.f1
- sql13.dts.designer.selectvariables.f1
helpviewer_keywords:
- Message Queue task [Integration Services]
- receiving messages
- messages [Integration Services]
- sending messages
ms.assetid: ae1d8fad-6649-4e93-b589-14a32d07da33
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e39187e32afbcf72bf1eb950f18d5f21cef38211
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348304"
---
# <a name="message-queue-task"></a>Message Queue Task

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  通过消息队列任务，可以使用消息队列（也称为 MSMQ）在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包之间发送和接收消息，或将消息发送到由自定义应用程序处理的应用程序队列。 这些消息可以采用简单文本格式、文件格式或变量及其值的格式。  
  
 用消息队列任务可以协调整个企业的运作。 如果目标不可用或繁忙，可以将消息排队供以后发送；例如，该任务可以将要发给销售代表的离线便携式计算机的消息排队，当他们连接到网络时就会收到消息。 可以将消息队列任务用于下列目的：  
  
-   将任务推迟到其他包签入时执行。 例如，消息队列任务在各个零售站点夜间维护完成之后向公司计算机发送消息。 在公司计算机上运行的包含有消息队列任务，每个任务等待来自特定零售站点的消息。 当来自站点的消息到达时，任务便从该站点上载数据。 所有站点都签入完毕之后，包才计算汇总和。  
  
-   将数据文件发送到负责处理这些文件的计算机中。 例如，可以用数据文件消息把餐馆收银机的输出发送到公司的工资系统，在此处提取出有关各服务员小费的数据。  
  
-   在整个企业分发文件。 例如，包可以使用消息队列任务将包文件发送给另一台计算机。 然后，在目标计算机上运行的包可以使用消息队列任务在本地检索并保存该包。  
  
 发送或接收消息时，“消息队列”任务使用以下四种消息类型之一：数据文件、字符串、变量的字符串消息或变量。 仅在接收消息时才能使用针对变量消息类型的字符串消息。  
  
 任务使用 MSMQ 连接管理器连接到消息队列。 有关详细信息，请参阅 [MSMQ 连接管理器](../../integration-services/connection-manager/msmq-connection-manager.md)。 有关消息队列的详细信息，请参阅 [MSDN Library](../../sql-server/index.yml)。  
  
 消息队列任务要求安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务。 如果在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导的 **“要安装的组件”** 页或 **“功能选择”** 页上选择安装某些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件，将同时安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件的部分子集。 这些组件对特定任务是有用的，但 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 功能将受到限制。 例如， [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 选项会安装设计包所需的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件，但不安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务，因此消息队列任务不起作用。 为了确保完整安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]，必须在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] “要安装的组件” **页上选择** 。 有关安装和运行“消息队列”任务的详细信息，请参阅 [安装 Integration Services](../../integration-services/install-windows/install-integration-services.md)。  
  
> [!NOTE]  
>  如果将计算机的操作系统配置为 FIPS 模式并且任务使用加密，则“消息队列”任务将无法遵守联邦信息处理标准 (FIPS) 140-2。 如果“消息队列”任务不使用加密，则该任务可以成功运行。  
  
## <a name="message-types"></a>消息类型  
 可以按下列方式配置消息队列任务提供的消息类型：  
  
-   **Data file** 消息指定文件包含消息。 在接收消息时，可以将任务配置为保存文件、覆盖现有文件以及指定该任务可从中接收消息的包。  
  
-   **String** 消息将消息指定为字符串。 在接收消息时，可以将任务配置为比较接收到的字符串和用户定义字符串，并根据对比结果执行操作。 字符串比较可以是完全匹配、区分大小写或不区分大小写，或者使用子字符串。  
  
-   **String message to variable** 将源消息指定为发送到目标变量的字符串。 可以将任务配置为使用完全匹配、不区分大小写或子字符串的比较方式来比较接收到的字符串和用户定义字符串。 这种消息类型只能在任务接收消息时使用。  
  
-   **Variable** 指定消息包含一个或多个变量。 可以将任务配置为指定消息中所包含变量的名称。 在接收消息时，可以将任务配置为指定任务从中接收消息的包和作为消息目标的变量。  
  
## <a name="sending-messages"></a>发送消息  
 配置消息队列任务以发送消息时，可以使用消息队列技术当前支持的加密算法（RC2 和 RC4）之一来加密消息。 与较新的算法相比，这两种加密算法都被视为加密性很弱的算法，但消息队列技术尚不支持较新的算法。 因此，在使用消息队列任务发送消息时，应考虑您的加密需要。  
  
## <a name="receiving-messages"></a>接收消息  
 在接收消息时，可以按下列方式配置消息队列任务：  
  
-   跳过消息，或从队列中删除消息。  
  
-   指定超时。  
  
-   如果出现超时便失败。  
  
-   如果消息存储在 **Data file** 中，则覆盖现有文件。  
  
-   如果消息使用 **Data file message** 类型，则将消息文件另存为其他文件名。  
  
## <a name="custom-logging-messages-available-on-the-message-queue-task"></a>消息队列任务可用的自定义日志记录消息  
 下表列出了消息队列任务的自定义日志项。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
|日志项|说明|  
|---------------|-----------------|  
|**MSMQAfterOpen**|指示任务已完成打开消息队列的操作。|  
|**MSMQBeforeOpen**|指示任务开始打开消息队列。|  
|**MSMQBeginReceive**|指示任务开始接收消息。|  
|**MSMQBeginSend**|指示任务开始发送消息。|  
|**MSMQEndReceive**|指示任务完成接收消息。|  
|**MSMQEndSend**|指示任务已完成了发送消息的操作。|  
|**MSMQTaskInfo**|提供有关任务的说明性信息。|  
|**MSMQTaskTimeOut**|指示任务已超时。|  
  
## <a name="configuration-of-the-message-queue-task"></a>消息队列任务的配置  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的信息，请单击以下主题：  
  
-   [“表达式”页](../../integration-services/expressions/expressions-page.md)  
  
 有关以编程方式设置这些属性的信息，请参阅开发人员指南中针对 **Microsoft.SqlServer.Dts.Tasks.MessageQueueTask.MessageQueueTask** 类的文档。  
  
## <a name="related-tasks"></a>Related Tasks  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请参阅 [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)。  
  
## <a name="message-queue-task-editor-general-page"></a>消息队列任务编辑器（“常规”页）
  可以使用 **“消息队列任务编辑器”** 对话框的 **“常规”** 页，对消息队列任务进行命名和说明，指定消息格式，以及指示任务是发送还是接收消息。  
  
### <a name="options"></a>选项  
 **名称**  
 为消息队列任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入对消息队列任务的说明。  
  
 **Use2000Format**  
 指示是否使用消息队列（也称为 MSMQ）的 2000 格式。 默认值为 **False**。  
  
 **MSMQConnection**  
 选择现有 MSMQ 连接管理器，或单击“\<**New connection...**>”以创建新的连接管理器。  
  
 **相关主题**：[MSMQ 连接管理器](../../integration-services/connection-manager/msmq-connection-manager.md)、[MSMQ 连接管理器编辑器](../connection-manager/msmq-connection-manager.md)  
  
 **消息**  
 指定消息队列任务是发送消息还是接收消息。 如果选择了 **“发送消息”** ，则该对话框的左窗格将列出“发送”页；如果选择了 **“接收消息”** ，则将列出“接收”页。 默认情况下，此值设置为 **“发送消息”** 。  
  
## <a name="message-queue-task-editor-send-page"></a>消息队列任务编辑器（“发送”页）
  使用“消息队列任务编辑器”对话框的“发送”页面，能配置消息队列任务以便从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包发送消息   。  
  
### <a name="options"></a>选项  
 **UseEncryption**  
 指示是否对消息进行加密。 默认值为 **False**。  
  
 **EncryptionAlgorithm**  
 如果选择使用加密，请指定要使用的加密算法的名称。 消息队列任务可以使用 RC2 和 RC4 算法。 默认值为 **RC2**。  
  
> [!NOTE]  
>  RC4 算法仅用于支持向后兼容性。 仅当数据库兼容级别为 90 或 100 时，才能使用 RC4 或 RC4_128 对新材料进行加密。 （建议不要使用。）而是使用一种较新的算法，如 AES 算法之一。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的当前版本中，可以通过任何兼容级别对使用 RC4 或 RC4_128 加密的材料进行解密。  
  
> [!IMPORTANT]  
>  这些算法是消息队列（也称为 MSMQ）技术支持的加密算法。 与较新的算法相比，这两种加密算法都被视为加密性很弱的算法，但消息队列尚不支持新的算法。 因此，在使用消息队列任务发送消息时，应考虑您的加密需要。  
  
 **MessageType**  
 选择消息类型。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**数据文件消息**|消息存储在文件中。 选择该值将显示动态选项 **DataFileMessage**。|  
|**变量消息**|消息存储在变量中。 选择该值将显示动态选项 **VariableMessage**。|  
|**字符串消息**|消息存储在消息队列任务中。 选择该值将显示动态选项 **StringMessage**。|  
  
### <a name="messagetype-dynamic-options"></a>MessageType 动态选项  
  
#### <a name="messagetype--data-file-message"></a>MessageType = 数据文件消息  
 **DataFileMessage**  
 键入数据文件的路径，或单击省略号 (…) 后再定位到相应的文件。  
  
#### <a name="messagetype--variable-message"></a>MessageType = 变量消息  
 **VariableMessage**  
 键入变量名称，或单击省略号 (…) 后再选择相应的变量。 各变量之间用逗号分隔。  
  
 **相关主题：** 选择变量  
  
#### <a name="messagetype--string-message"></a>MessageType = 字符串消息  
 **StringMessage**  
 键入字符串消息，或单击省略号 (…) 后在“输入字符串消息”对话框中键入消息。  
  
## <a name="message-queue-task-editor-receive-page"></a>消息队列任务编辑器（“接收”页）
  可以使用“消息队列任务编辑器”对话框的“接收”页，配置消息队列任务以接收 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 消息队列 (MSMQ) 消息   。  
  
### <a name="options"></a>选项  
 **RemoveFromMessageQueue**  
 指示在接收后是否从队列中删除消息。 默认情况下，此值设置为 **False**。  
  
 **ErrorIfMessageTimeOut**  
 指示当消息超时时任务是否失败，并显示错误消息。 默认值为 **False**。  
  
 **TimeoutAfter**  
 如果选择任务失败时显示错误消息，则请指定显示超时消息之前等待的秒数。  
  
 **MessageType**  
 选择消息类型。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**数据文件消息**|消息存储在文件中。 选择该值将显示动态选项 **DataFileMessage**。|  
|**变量消息**|消息存储在变量中。 选择该值将显示动态选项 **VariableMessage**。|  
|**字符串消息**|消息存储在消息队列任务中。 选择该值将显示动态选项 **StringMessage**。|  
|**变量的字符串消息**|消息<br /><br /> 选择该值将显示动态选项 **StringMessage**。|  
  
### <a name="messagetype-dynamic-options"></a>MessageType 动态选项  
  
#### <a name="messagetype--data-file-message"></a>MessageType = 数据文件消息  
 **SaveFileAs**  
 键入要使用的文件的路径，或单击省略号按钮 (…) 后再定位到该文件。  
  
 **Overwrite**  
 指示在保存数据文件消息的内容时是否覆盖现有文件中的数据。 默认值为 **False**。  
  
 **筛选器**  
 指定是否对消息应用筛选器。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**无筛选器**|该任务不筛选消息。 选择该值将显示动态选项 **IdentifierReadOnly**。|  
|**来源包**|该消息仅接收来自指定包的消息。 选择该值将显示动态选项 **Identifier**。|  
  
#### <a name="filter-dynamic-options"></a>Filter 动态选项  
  
##### <a name="filter--no-filter"></a>Filter = 无筛选器  
 **IdentifierReadOnly**  
 此选项是只读的。 如果以前设置了 Filter 属性，此选项可能为空或包含包的 GUID。  
  
##### <a name="filter--from-package"></a>Filter = 来源包  
 **标识符**  
 如果选择应用筛选器，请键入可以从中接收消息的包的唯一标识符，或者单击省略号按钮 (…)，再指定包。  
  
 **相关主题：** [选择包](../../integration-services/control-flow/select-a-package.md)  
  
#### <a name="messagetype--variable-message"></a>MessageType = 变量消息  
 **筛选器**  
 指定是否将筛选器应用到消息。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**无筛选器**|该任务不筛选消息。 选择该值将显示动态选项 **IdentifierReadOnly**。|  
|**来源包**|该消息仅接收来自指定包的消息。 选择该值将显示动态选项 **Identifier**。|  
  
 **变量**  
 键入变量名称，或单击“\<**New variable...**>”，然后配置新的变量。  
  
 **相关主题：** [添加变量](../integration-services-ssis-variables.md)  
  
#### <a name="filter-dynamic-options"></a>Filter 动态选项  
  
##### <a name="filter--no-filter"></a>Filter = 无筛选器  
 **IdentifierReadOnly**  
 此选项为空白。  
  
##### <a name="filter--from-package"></a>Filter = 来源包  
 **标识符**  
 如果选择应用筛选器，请键入可以从中接收消息的包的唯一标识符，或者单击省略号按钮 (…)，再指定包。  
  
 **相关主题：** [选择包](../../integration-services/control-flow/select-a-package.md)  
  
#### <a name="messagetype--string-message"></a>MessageType = 字符串消息  
 **比较**  
 指定是否将筛选器应用到消息。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|无|不对消息进行比较。|  
|**完全匹配**|消息必须与 **CompareString** 选项中的字符串完全匹配。|  
|**忽略大小写**|消息必须与 **CompareString** 选项中的字符串匹配，但在比较时不区分大小写。|  
|**包含**|消息必须包含 **CompareString** 选项中的字符串。|  
  
 **CompareString**  
 除非将 **Compare** 选项设置为“无”  ，否则请提供与消息进行比较的字符串。  
  
#### <a name="messagetype--string-message-to-variable"></a>MessageType = 变量的字符串消息  
 **比较**  
 指定是否将筛选器应用到消息。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|无|不对消息进行比较。|  
|**完全匹配**|消息必须与 **CompareString** 选项中的字符串完全匹配。|  
|**忽略大小写**|消息必须与 **CompareString** 选项中的字符串匹配，但在比较时不区分大小写。|  
|**包含**|消息必须包含 **CompareString** 选项中的字符串。|  
  
 **CompareString**  
 除非将 **Compare** 选项设置为“无”  ，否则请提供与消息进行比较的字符串。  
  
 **变量**  
 键入要保存接收到的消息的变量名，或单击“\<**New variable...**>”，然后配置新的变量。  
  
 **相关主题：** [添加变量](../integration-services-ssis-variables.md)  
  
## <a name="select-variables"></a>选择变量
  可以使用 **“选择变量”** 对话框，指定在消息队列任务的发送消息操作中使用的变量。 “可用变量”列表包括消息队列任务或其父容器作用域内的系统变量和用户定义变量  。 该任务使用 **“所选变量”** 列表中的变量。  
  
### <a name="options"></a>选项  
 **可用变量**  
 选择一个或多个变量。  
  
 **“所选变量”**  
 选择一个或多个变量。  
  
 **右箭头**  
 将所选变量移动到“所选变量”列表  。  
  
 **左箭头**  
 将所选变量移回“可用变量”  列表。  
  
 **新建变量**  
 创建新变量。  
  
 **相关主题：** [添加变量](../integration-services-ssis-variables.md)  
## <a name="see-also"></a>另请参阅  
 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)   
 [控制流](../../integration-services/control-flow/control-flow.md)  
  
