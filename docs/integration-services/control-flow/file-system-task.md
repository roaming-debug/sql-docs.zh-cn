---
description: 文件系统任务
title: 文件系统任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.filesystemtask.f1
- sql13.dts.designer.filesystemtask.general.f1
helpviewer_keywords:
- File System task [Integration Services]
ms.assetid: 7dd79a6a-e066-4028-a385-1d40f31056f8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 51646edb5b2937d04d4915e2a7d4a66327343a6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345712"
---
# <a name="file-system-task"></a>文件系统任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  文件系统任务对文件系统中的文件和目录执行操作。 例如，通过使用文件系统任务，包可以创建、移动或删除目录和文件。 您还可以使用文件系统任务设置文件和目录的属性。 例如，文件系统任务可以让文件隐藏或只读。  
  
 所有文件系统任务操作都使用源，源可以是文件或目录。 例如，任务复制的文件或删除的目录都是源。 源可以通过使用指向目录或文件的文件连接管理器来指定，也可以通过提供包含源路径的变量的名称来指定。 有关详细信息，请参阅[文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)和 [Integration Services (SSIS) 变量](../../integration-services/integration-services-ssis-variables.md)。  
  
 复制和移动文件及目录的操作与重命名文件的操作都使用目标和源。 目标可以使用文件连接管理器或变量指定。 文件系统任务操作可以配置为允许覆盖目标文件和目录。 创建新目录的操作可以配置为使用具有指定名称的现有目录，而不是在目录已经存在时失败。  
  
## <a name="predefined-file-system-operations"></a>预定义的文件系统操作  
 文件系统任务包含一组预定义的操作。 下表介绍了这些运算。  
  
|Operation|说明|  
|---------------|-----------------|  
|复制目录|将文件夹从一个位置复制到另一个位置。|  
|复制文件|将文件从一个位置复制到另一个位置。|  
|创建目录|在指定位置创建文件夹。|  
|删除目录|删除指定位置的文件夹。|  
|删除目录内容|删除文件夹中的所有文件和文件夹。|  
|删除文件|删除指定位置的文件。|  
|移动目录|将文件夹从一个位置移动到另一个位置。|  
|移动文件|将文件从一个位置移动到另一个位置。|  
|重命名文件|重命名指定位置的文件。|  
|设置属性|设置文件和文件夹的属性。 属性包括“存档”、“隐藏”、“正常”、“只读”和“系统”。 “正常”指没有属性，它不能与其他属性结合使用。 所有其他属性都可以组合使用。|  
  
 文件系统任务对单个文件或目录进行操作。 因此，该任务不支持使用通配符对多个文件执行相同的操作。 若要使此文件系统任务对多个文件或目录重复执行某个操作，请将此文件系统任务放置于一个 Foreach 循环容器中，如下面的步骤所述：  
  
-   **配置 Foreach 循环容器** 在 Foreach 循环编辑器的 **“集合”** 页上，将枚举器设置为 **“Foreach 文件枚举器”** ，然后输入通配符表达式作为 **“文件”** 的枚举器配置。 在 Foreach 循环编辑器的 **“变量映射”** 页上，将要用来传递文件名称的变量按照一次一个的方式映射至文件系统任务。  
  
-   **添加和配置文件系统任务** 将文件系统任务添加到 Foreach 循环容器。 在文件系统任务编辑器的 **“常规”** 页上，将 **“SourceVariable”** 或 **“DestinationVariable”** 属性设置为您在 Foreach 循环容器中定义的变量。  
  
## <a name="custom-log-entries-available-on-the-file-system-task"></a>文件系统任务可用的自定义日志项  
 下表介绍了文件系统任务的自定义日志项。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
|日志项|说明|  
|---------------|-----------------|  
|**FileSystemOperation**|报告任务所执行的操作。 在文件系统操作开始时写入日志项，日志项包括有关源和目标的信息。|  
  
## <a name="configuring-the-file-system-task"></a>配置文件系统任务  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请参阅以下主题：  
  
-   [文件系统任务编辑器（“常规”页）]()  
  
-   [“表达式”页](../../integration-services/expressions/expressions-page.md)  
  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请参阅以下主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
 有关如何以编程方式设置这些属性的详细信息，请参阅以下主题：  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包括一个可下载和上载数据文件以及管理服务器上目录的任务。 有关详细信息，请参阅 [FTP Task](../../integration-services/control-flow/ftp-task.md)。  
  
## <a name="file-system-task-editor-general-page"></a>文件系统任务编辑器（“常规”页）
  可以使用 **“文件系统任务编辑器”** 对话框的 **“常规”** 页，配置任务执行的文件系统操作。  
  
 必须通过设置 SourceConnection 和 DestinationConnection 属性来指定源和目标连接管理器。 您可以提供指向任务将其用作源或目标的文件的文件连接管理器的名称，如果文件路径存储在变量中，则可以提供变量的名称。 若要使用变量来存储文件路径，必须先将源连接的 IsSourcePathVariable 选项和目标连接的 IsDestinationPatheVariable 选项设置为 **True**。 然后，您可以选择使用现有的系统或用户定义变量，也可以创建新变量。 在 **“添加变量”** 对话框中，可以配置和指定变量的作用域。 该作用域必须是文件系统任务或父容器。 有关详细信息，请参阅 [Integration Services (SSIS) 变量](../../integration-services/integration-services-ssis-variables.md)和[在包中使用变量](../integration-services-ssis-variables.md)。  
  
> [!NOTE]  
>  若要覆盖为 **SourceConnection** 和 **DestinationConnection** 属性选择的变量，请为 **“源”** 和 **“目标”** 属性输入表达式。 在 **“文件系统任务编辑器”** 的 **“表达式”** 页上输入表达式。 例如，若要设置任务作为目标的文件路径，您可能要在某些情况下使用变量 A 并在另一些情况下使用变量 B。  
  
> [!NOTE]  
>  文件系统任务对单个文件或目录进行操作。 因此，该任务不支持使用通配符对多个文件或目录执行同一操作。 若要使此文件系统任务对多个文件或目录重复执行某个操作，请将此文件系统任务放置于一个 Foreach 循环容器中。 有关详细信息，请参阅 [File System Task](../../integration-services/control-flow/file-system-task.md)。  
  
 您可以使用表达式来使用不同的变量  
  
### <a name="options"></a>选项  
 **IsDestinationPathVariable**  
 指示目标路径是否存储在变量中。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**True**|目标路径存储在变量中。 选择此值将显示动态选项 **DestinationVariable**。|  
|**False**|目标路径在文件连接管理器中指定。 选择此值将显示动态选项 **DestinationConnection**。|  
  
 **OverwriteDestination**  
 指定操作是否可以覆盖目标目录中的文件。  
  
 **名称**  
 为文件系统任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入文件系统任务的说明。  
  
 **操作**  
 选择要执行的文件系统操作。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**复制目录**|复制目录。 选择此值将显示源和目标的动态选项。|  
|**复制文件**|复制文件。 选择此值将显示源和目标的动态选项。|  
|**创建目录**|创建目录。 选择此值将显示源和目标目录的动态选项。|  
|**删除目录**|删除目录。 选择此值将显示源的动态选项。|  
|**删除目录内容**|删除目录的内容。 选择此值将显示源的动态选项。|  
|**删除文件**|删除文件。 选择此值将显示源的动态选项。|  
|**移动目录**|移动目录。 选择此值将显示源和目标的动态选项。|  
|**移动文件**|移动文件。 选择此值将显示源和目标的动态选项。 移动文件时，在作为目标提供的目录路径中不要包含文件名。|  
|**重命名文件**|重命名文件。 选择此值将显示源和目标的动态选项。 重命名文件时，请在为目标提供的目录路径中包含新文件名。|  
|**设置属性**|设置文件或目录的属性。 选择此值将显示源和操作的动态选项。|  
  
 **IsSourcePathVariable**  
 指示目标路径是否存储在变量中。 此属性具有下表所列的选项。  
  
|值||  
|-----------|-|  
|**True**|目标路径存储在变量中。 选择此值将显示动态选项 **SourceVariable**。|  
|**False**|目标路径在文件连接管理器中指定。 选择此值将显示动态选项 **DestinationVariable**。|  
  
### <a name="isdestinationpathvariable-dynamic-options"></a>IsDestinationPathVariable 动态选项  
  
#### <a name="isdestinationpathvariable--true"></a>IsDestinationPathVariable = True  
 **DestinationVariable**  
 从列表中选择变量名称，或单击“\<**New variable...**>”创建新变量。  
  
 **相关主题：** [Integration Services &#40;SSIS&#41; 变量](../../integration-services/integration-services-ssis-variables.md)、[添加变量](../integration-services-ssis-variables.md)  
  
#### <a name="isdestinationpathvariable--false"></a>IsDestinationPathVariable = False  
 **DestinationConnection**  
 从列表中选择文件连接管理器，或单击“\<**New connection...**>”创建新的连接管理器。  
  
 **相关主题：** [文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)、[文件连接管理器编辑器](../connection-manager/file-connection-manager.md)  
  
### <a name="issourcepathvariable-dynamic-options"></a>IsSourcePathVariable 动态选项  
  
#### <a name="issourcepathvariable--true"></a>IsSourcePathVariable = True  
 **SourceVariable**  
 从列表中选择变量名称，或单击“\<**New variable...**>”创建新变量。  
  
 **相关主题：** [Integration Services &#40;SSIS&#41; 变量](../../integration-services/integration-services-ssis-variables.md)、[添加变量](../integration-services-ssis-variables.md)  
  
#### <a name="issourcepathvariable--false"></a>IsSourcePathVariable = False  
 **SourceConnection**  
 从列表中选择文件连接管理器，或单击“\<**New connection...**>”创建新的连接管理器。  
  
 **相关主题：** [文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)  
  
### <a name="operation-dynamic-options"></a>Operation 动态选项  
  
#### <a name="operation--set-attributes"></a>Operation = 设置属性  
 **Hidden**  
 指示文件或目录是否可见。  
  
 **ReadOnly**  
 指示文件是否是只读的。  
  
 **存档**  
 指示文件或目录可以用于存档。  
  
 **系统**  
 指示文件是否是操作系统文件。  
  
#### <a name="operation--create-directory"></a>Operation = 创建目录  
 **UseDirectoryIfExists**  
 指示“创建目录”操作是否使用具有指定名称的现有目录，而不创建新目录。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)   
 [控制流](../../integration-services/control-flow/control-flow.md)  
  
