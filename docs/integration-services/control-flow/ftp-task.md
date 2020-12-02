---
description: FTP 任务
title: FTP 任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.ftptask.f1
- sql13.dts.designer.ftptask.general.f1
- sql13.dts.designer.ftptask.filetransfer.f1
helpviewer_keywords:
- FTP task [Integration Services]
ms.assetid: 41c3f2c4-ee04-460a-9822-bb9ae4036c2e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 013fffc66c8e40ca2124b949f555e0638df7967e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127407"
---
# <a name="ftp-task"></a>FTP 任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  FTP 任务可以下载和上载数据文件，并管理服务器上的目录。 例如，在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包工作流中，包可以从远程服务器或 Internet 位置下载数据文件。 可以将 FTP 任务用于下列用途：  
  
-   在移动数据之前或之后，将目录和数据文件从一个目录复制到另一个目录，并对数据应用转换。  
  
-   登录到源 FTP 位置并将文件或包复制到目标目录。  
  
-   从 FTP 位置下载文件并在将数据加载到数据库之前对列数据应用转换。  
  
 在运行时，FTP 任务通过使用 FTP 连接管理器连接到服务器。 FTP 连接管理器与 FTP 任务分开配置，然后在 FTP 任务中引用连接管理器。 FTP 连接管理器包括服务器设置、用于访问 FTP 服务器的凭据，以及连接到服务器的超时值和重试次数之类的选项。 有关详细信息，请参阅 [FTP 连接管理器](../../integration-services/connection-manager/ftp-connection-manager.md)。  
  
> [!IMPORTANT]  
>  FTP 连接管理器仅支持匿名身份验证和基本身份验证， 而不支持 Windows 身份验证。  
  
 访问本地文件或本地目录时，FTP 任务使用文件连接管理器或存储在变量中的路径信息。 与此相反，访问远程文件或远程目录时，FTP 任务使用远程服务器上的直接指定路径（在 FTP 连接管理器中指定）或存储在变量中的路径信息。 有关详细信息，请参阅[文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)和 [Integration Services (SSIS) 变量](../../integration-services/integration-services-ssis-variables.md)。  
  
 这意味着 FTP 任务可以接收多个文件和删除多个远程文件；但如果使用连接管理器，则该任务只能发送一个文件和删除一个本地文件，因为文件连接管理器只能访问一个文件。 若要访问多个本地文件，FTP 任务必须使用变量来提供路径信息。 例如，包含“C:\Test\&#42;.txt”的变量所提供的路径可以支持删除或发送 Test 目录中所有以 .txt 为扩展名的文件。  
  
 若要发送多个文件和访问多个本地文件及目录，还可以通过在 Foreach 循环中包含 FTP 任务来多次执行 FTP 任务。 Foreach 循环可以使用 For Each 文件枚举器对目录中的文件进行枚举。 有关详细信息，请参阅 [Foreach 循环容器](../../integration-services/control-flow/foreach-loop-container.md)。  
  
 FTP 任务支持在路径中使用通配符 *?* 和 *\** 。 这使得任务可以访问多个文件。 但是，只能在路径中指定文件名的部分使用通配符。 例如，C:\MyDirectory\\*.txt 是有效路径，而 C:\\\*\MyText.txt 则不是。  
  
 FTP 操作可以配置为在操作失败时停止文件系统任务，或以 ASCII 模式传输文件。 发送和接收文件副本的操作可以配置为覆盖目标文件和目录。  
  
## <a name="predefined-ftp-operations"></a>预定义的 FTP 操作  
 FTP 任务包含一组预定义的操作。 下表介绍了这些运算。  
  
|Operation|说明|  
|---------------|-----------------|  
|发送文件|将文件从本地计算机发送到 FTP 服务器。|  
|接收文件|将文件从 FTP 服务器保存到本地计算机。|  
|创建本地目录|在本地计算机上创建文件夹。|  
|创建远程目录|在 FTP 服务器上创建文件夹。|  
|删除本地目录|删除本地计算机上的文件夹。|  
|删除远程目录|删除 FTP 服务器上的文件夹。|  
|删除本地文件|删除本地计算机上的文件。|  
|删除远程文件|删除 FTP 服务器上的文件。|  
  
## <a name="custom-log-entries-available-on-the-ftp-task"></a>FTP 任务可用的自定义日志项  
 下表列出了 FTP 任务的自定义日志项。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
|日志项|说明|  
|---------------|-----------------|  
|**FTPConnectingToServer**|指示任务已启动与 FTP 服务器的连接。|  
|**FTPOperation**|报告任务所执行的 FTP 操作的开始及其类型。|  
  
## <a name="related-tasks"></a>Related Tasks  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的信息，请参阅 [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)。  
  
 有关如何以编程方式设置这些属性的详细信息，请参阅 <xref:Microsoft.SqlServer.Dts.Tasks.FtpTask.FtpTask>。  
  
## <a name="ftp-task-editor-general-page"></a>FTP 任务编辑器（“常规”页）
  使用 **“FTP 任务编辑器”** 对话框的 **“常规”** 页可以指定连接到与任务通信的 FTP 服务器的 FTP 连接管理器。 您还可以命名和描述 FTP 任务。  
  
### <a name="options"></a>选项  
 **FtpConnection**  
 选择现有 FTP 连接管理器，或单击“\<**New connection...**>”以创建连接管理器。  
  
> [!IMPORTANT]  
>  FTP 连接管理器仅支持匿名身份验证和基本身份验证， 而不支持 Windows 身份验证。  
  
 **相关主题**：[FTP 连接管理器](../../integration-services/connection-manager/ftp-connection-manager.md)、[FTP 连接管理器编辑器](../connection-manager/ftp-connection-manager.md)  
  
 **StopOnFailure**  
 指示在 FTP 操作失败时是否终止 FTP 任务。  
  
 **名称**  
 为 FTP 任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入对 FTP 任务的说明。  
  
## <a name="ftp-task-editor-file-transfer-page"></a>FTP 任务编辑器（“文件传输”页）
  可以使用 **“FTP 任务编辑器”** 对话框的 **“文件传输”** 页配置任务执行的 FTP 操作。  
  
### <a name="options"></a>选项  
 **IsRemotePathVariable**  
 指示远程路径是否存储在变量中。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**True**|目标路径存储在变量中。 选择该值将显示动态选项 **RemoteVariable**。|  
|**False**|目标路径在文件连接管理器中指定。 选择该值将显示动态选项 **RemotePath**。|  
  
 **OverwriteFileAtDestination**  
 指定是否可以覆盖目标位置中的文件。  
  
 **IsLocalPathVariable**  
 指示本地路径是否存储在变量中。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**True**|目标路径存储在变量中。 选择该值将显示动态选项 **LocalVariable**。|  
|**False**|目标路径在文件连接管理器中指定。 选择该值将显示动态选项 **LocalPath**。|  
  
 **操作**  
 选择要执行的 FTP 操作。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**发送文件**|发送文件。 选择此值将显示动态选项 **LocalVariable**、 **LocalPathRemoteVariable** 和 **RemotePath**。|  
|**接收文件**|接收文件。 选择此值将显示动态选项 **LocalVariable**、 **LocalPathRemoteVariable** 和 **RemotePath**。|  
|**创建本地目录**|创建本地目录。 选择此值将显示动态选项 **LocalVariable** 和 **LocalPath**。|  
|**创建远程目录**|创建远程目录。 选择此值将显示动态选项 **RemoteVariable** 和 **RemotelPath**。|  
|**删除本地目录**|删除本地目录。 选择此值将显示动态选项 **LocalVariable** 和 **LocalPath**。|  
|**删除远程目录**|删除远程目录。 选择此值将显示动态选项 **RemoteVariable** 和 **RemotePath**。|  
|**删除本地文件**|删除本地文件。 选择此值将显示动态选项 **LocalVariable** 和 **LocalPath**。|  
|**删除远程文件**|删除远程文件。 选择此值将显示动态选项 **RemoteVariable** 和 **RemotePath**。|  
  
 **IsTransferASCII**  
 指示是否应以 ASCII 模式传输从远程 FTP 服务器传输来的文件和传输到远程 FTP 服务器上的文件。  
  
### <a name="isremotepathvariable-dynamic-options"></a>IsRemotePathVariable 动态选项  
  
#### <a name="isremotepathvariable--true"></a>IsRemotePathVariable = True  
 **RemoteVariable**  
 选择用户定义的现有变量，或单击“\<**New variable...**>”以创建用户定义变量。  
  
 **相关主题：** [Integration Services &#40;SSIS&#41; 变量](../../integration-services/integration-services-ssis-variables.md)、添加变量  
  
#### <a name="isremotepathvariable--false"></a>IsRemotePathVariable = False  
 **RemotePath**  
 选择现有 FTP 连接管理器，或单击“\<**New connection...**>”以创建连接管理器。  
  
 **相关主题：** [FTP 连接管理器](../../integration-services/connection-manager/ftp-connection-manager.md)、[FTP 连接管理器编辑器](../connection-manager/ftp-connection-manager.md)  
  
### <a name="islocalpathvariable-dynamic-options"></a>IsLocalPathVariable 动态选项  
  
#### <a name="islocalpathvariable--true"></a>IsLocalPathVariable = True  
 **LocalVariable**  
 选择用户定义的现有变量，或单击“\<**New variable...**>”以创建变量。  
  
 **相关主题：** [Integration Services &#40;SSIS&#41; 变量](../../integration-services/integration-services-ssis-variables.md)、添加变量  
  
#### <a name="islocalpathvariable--false"></a>IsLocalPathVariable = False  
 **LocalPath**  
 选择现有“文件连接管理器”，或单击“\<**New connection...**>创建一个连接管理器。  
  
 **相关主题**：[平面文件连接管理器](../../integration-services/connection-manager/flat-file-connection-manager.md)  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)   
 [控制流](../../integration-services/control-flow/control-flow.md)  
  
