---
description: 执行包任务
title: 执行包任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executepackagetask.f1
- sql13.dts.designer.executepackagetask.package.f1
- sql13.dts.designer.executepackagetask.parameter.F1
- sql13.dts.designer.executepackagetask.general.f1
helpviewer_keywords:
- Execution Package task [Integration Services]
- child packages
- parent packages [Integration Services]
ms.assetid: 042d4ec0-0668-401c-bb3a-a25fe2602eac
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8251f2ff30fafff584b4f56fe93a9b7423ea412b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352078"
---
# <a name="execute-package-task"></a>执行包任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  执行包任务通过允许包将其他包作为工作流的组成部分运行来扩展 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的企业功能。  
  
 可以将执行包任务用于下列用途：  
  
-   细分复杂的包工作流。 此任务允许您将工作流细分为多个更易于读取、测试和维护的包。 例如，如果将数据加载到星型架构中，则可以生成一个单独的包来填充每个维度和事实数据表。  
  
-   重用部分包。 其他包可以重用部分包工作流。 例如，您可以生成可从不同的包中进行调用的数据提取模块。 调用提取模块的每个包都可以执行不同的数据清理、筛选或聚合操作。  
  
-   对工作单元进行分组。 可以将工作单元封装到单独的包中，并作为事务组件联接到父包的工作流中。 例如，父包运行辅助包，并根据辅助包的成功或失败来提交事务或回滚事务。  
  
-   控制包的安全性。 包作者需要访问的仅是多包解决方案的一部分。 通过将某个包细分为多个包，可以提供更高级别的安全性，因为可以只授予作者对相关包的访问权。  
  
 运行其他包的包通常称为父包，由父工作流运行的包称为子包。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含执行工作流操作的任务，如执行可执行文件和批处理文件。 有关详细信息，请参阅 [Execute Process Task](../../integration-services/control-flow/execute-process-task.md)。  
  
## <a name="running-packages"></a>运行包  
 “执行包”任务可运行与父包一起包含在同一个项目中的子包。 您可以通过将 **ReferenceType** 属性设置为 **“项目引用”** ，然后设置 **PackageNameFromProjectReference** 属性，从项目中选择子包。  
  
> [!NOTE]  
>  “ReferenceType”  选项是只读的，如果包含包的项目尚未转换为项目部署模型，则该选项将设置为“外部引用”  。 [部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。  
  
 执行包任务也可以运行存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb 数据库中的包和存储在文件系统中的包。 此任务使用 OLE DB 连接管理器连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，或者使用文件连接管理器访问文件系统。 有关详细信息，请参阅 [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) 和 [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md)。  
  
 执行包任务还可以运行数据库维护计划，该计划允许您管理同一 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 解决方案中的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包和数据库维护计划。 数据库维护计划类似于 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包，但计划只能包含数据库维护任务，并始终存储在 msdb 数据库中。  
  
 如果选择存储在文件系统中的包，则必须提供包的名称和位置。 包可以驻留在文件系统中的任何位置；而不必与父包位于相同的文件夹中。  
  
 子包可以在父包的进程中运行，也可以在其自身的进程中运行。 子包在其自身的进程中运行需要更多内存，但可以提供更大的灵活性。 例如，如果子进程失败，父进程可以继续运行。  
  
 而有时您则希望父包和子包作为一个单元一起失败，或者您可能不希望引发其他进程的额外开销。 例如，如果子进程失败，而包的父进程中的后续处理取决于子进程的成功，则子包应当在父包的进程中运行。  
  
 默认情况下，“执行包”任务的 ExecuteOutOfProcess 属性被设置为 **False**，并且子包与父包运行在同一进程中。 如果将此属性设置为 **True**，则在单独的进程中运行子包。 这可能减慢子包的启动。 此外，如果将属性设置为 **True**，则不能在仅工具安装中调试包。 必须安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]。 有关详细信息，请参阅 [安装 Integration Services](../../integration-services/install-windows/install-integration-services.md)  
  
## <a name="extending-transactions"></a>扩展事务  
 父包使用的事务可扩展到子包；因此，两个包都执行的工作可以提交或回滚。 例如，取决于子包执行的数据库插入，父包执行的数据库插入可以提交或回滚，反之亦然。 有关详细信息，请参阅 [Inherited Transactions](../integration-services-transactions.md)。  
  
## <a name="propagating-logging-details"></a>传播日志记录详细信息  
 执行包任务运行的子包可能配置为使用日志记录，也可能没有配置为使用日志记录，但子包将始终将日志详细信息转发给父包。 如果执行包任务配置为使用日志记录，则任务将记录来自子包的日志详细信息。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
## <a name="passing-values-to-child-packages"></a>将值传递给子包  
 子包经常使用调用它的其他包（通常是其父包）传递给它的值。 使用来自父包的值在如下情况下非常有用：  
  
-   较大工作流的各部分分配给了不同的包。 例如，某个包每夜下载数据、汇总数据、将汇总数据值赋给变量，然后将这些值传递给其他包以进行其他数据处理。  
  
-   父包动态协调子包中的任务。 例如，父包确定当月的天数，将该数值赋给某一变量，然后子包即会将任务执行相应的次数。  
  
-   子包需要访问父包动态派生的数据。 例如，父包从表中提取数据并将行集加载到变量中，而子包执行对数据的其他操作。  
  
 您可以使用以下方法将值传递到子包：  
  
-   **包配置**  
  
     [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供了一种配置类型（父包变量配置），用于将值从父包传递到子包。 配置在子包中生成，并使用父包中的变量。 将配置映射到子包中的某个变量，或映射到子包中某个对象的属性。 该变量还可以用于脚本任务或脚本组件所使用的脚本中。  
  
-   **参数**  
  
     您可以配置执行包任务以将父包变量或参数（或项目参数）映射到子包参数。 项目必须使用项目部署模型，并且子包必须包含在父包所在的同一项目中。 有关详细信息，请参阅 [Execute Package Task Editor]()。  
  
    > [!NOTE]  
    >  如果子包参数不敏感且映射到敏感的父参数，则子包将无法运行。  
    >   
    >  支持以下映射条件：  
    >   
    >  将敏感的子包参数映射到敏感的父参数  
    >   
    >  将敏感的子包参数映射到不敏感的父参数  
    >   
    >  将不敏感的子包参数映射到不敏感的父参数  
  
 父包变量可以在执行包任务作用域内或父容器（例如包）中定义。 如果有多个同名的变量可用，则使用在执行包任务作用域内定义的变量，或使用作用域内与该任务最近的变量。  
  
 有关详细信息，请参阅 [在子包中使用变量和参数的值](../../integration-services/packages/legacy-package-deployment-ssis.md#child)。  
  
### <a name="accessing-parent-package-variables"></a>访问父包变量  
 子包可以通过使用脚本任务访问父包变量。 在“脚本任务编辑器”的“脚本”页上输入父包变量的名称时，不要在变量名称中包括“用户:”    。 否则，子包在运行父包时找不到该变量。  
  
## <a name="configuring-the-execute-package-task"></a>配置执行包任务  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请单击以下主题：  
  
-   [“表达式”页](../../integration-services/expressions/expressions-page.md)  
  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请单击下列主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="configuring-the-execute-package-task-programmatically"></a>以编程方式配置执行包任务  
 有关以编程方式设置这些属性的详细信息，请单击以下主题：  
  
-   [N:Microsoft.SqlServer.Dts.Tasks.ExecutePackageTask](https://technet.microsoft.com/library/microsoft.sqlserver.dts.tasks.executepackagetask\(v=sql.110\).aspx)  
  
## <a name="execute-package-task-editor"></a>执行包任务编辑器
  可以使用执行包任务编辑器来配置执行包任务。 执行包任务通过允许包将其他包作为工作流的组成部分运行来扩展 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的企业功能。  
  
 **您希望做什么？**  
  
-   [打开执行包任务编辑器](#open)  
  
-   [设置“常规”页上的选项](#general)  
  
-   [设置“包”页上的选项](#package)  
  
-   [设置“参数绑定”页上的选项](#parameter)  
  
###  <a name="open-the-execute-package-task-editor"></a><a name="open"></a> 打开执行包任务编辑器  
  
1.  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中，打开包含执行包任务的 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 项目。  
  
2.  在 SSIS 设计器中右键单击该任务，然后单击“编辑”  。  
  
###  <a name="set-the-options-on-the-general-page"></a><a name="general"></a> 设置“常规”页上的选项  
 **名称**  
 为执行包任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入对执行包任务的说明。  
  
###  <a name="set-the-options-on-the-package-page"></a><a name="package"></a> 设置“包”页上的选项  
 **ReferenceType**  
 为项目中的子包选择“项目引用”  。 为位于包外部的子包选择 **“外部引用”** 。  
  
> [!NOTE]  
>  “ReferenceType”  选项是只读的，如果包含包的项目尚未转换为项目部署模型，则该选项将设置为“外部引用”  。 [部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。  
  
 **密码**  
 如果子包受密码保护，请提供子包的密码，或单击省略号按钮 (…)，为子包创建新的密码。  
  
 **ExecuteOutOfProcess**  
 指定子包是在父包的进程中运行还是在单独的进程中运行。 默认情况下，“执行包”任务的 ExecuteOutOfProcess 属性被设置为 **False**，并且子包与父包运行在同一进程中。 如果将此属性设置为 **true**，则在单独的进程中运行子包。 这可能减慢子包的启动。 此外，如果将该属性设置为 **true**，则不能在仅工具安装中调试包；必须安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 产品。 有关详细信息，请参阅 [安装 Integration Services](../../integration-services/install-windows/install-integration-services.md)。  
  
#### <a name="referencetype-dynamic-options"></a>ReferenceType 动态选项  
  
##### <a name="referencetype--external-reference"></a>ReferenceType = 外部引用  
 **位置**  
 选择子包的位置。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**SQL Server**|将位置设置为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。|  
|**文件系统**|将位置设置为文件系统。|  
  
 **Connection**  
 选择子包的存储位置的类型。  
  
 **PackageNameReadOnly**  
 显示包的名称。  
  
##### <a name="referencetype--project-reference"></a>ReferenceType = 项目引用  
 **PackageNameFromProjectReference**  
 选择项目中包含的要成为子包的包。  
  
#### <a name="location-dynamic-options"></a>位置动态选项  
  
##### <a name="location--sql-server"></a>位置 = SQL Server  
 **Connection**  
 从列表中选择一个 OLE DB 连接管理器，或单击 \<**New connection...**> 创建新的连接管理器。  
  
 **相关主题：** [OLE DB 连接管理器](../../integration-services/connection-manager/ole-db-connection-manager.md)  
  
 **PackageName**  
 键入子包的名称，或单击省略号 (…) 再定位到包。  
  
##### <a name="location--file-system"></a>位置 = 文件系统  
 **Connection**  
 从列表中选择文件连接管理器，或单击 \<**New connection...**> 创建新的连接管理器。  
  
 **相关主题：** [文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)  
  
 **PackageNameReadOnly**  
 显示包的名称。  
  
###  <a name="set-the-options-on-the-parameter-bindings-page"></a><a name="parameter"></a> 设置“参数绑定”页上的选项  
 您可以将父包或项目中的值传递到子包。 项目必须使用项目部署模型，并且子包必须包含在父包所在的同一项目中。  
  
 有关将项目转换为项目部署模型的信息，请参阅[部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。  
  
 **子包参数**  
 输入或选择子包参数的名称。  
  
 **绑定参数或变量**  
 选择包含要传递到子包的值的参数或变量。  
  
 **添加**  
 单击此选项可将参数或变量映射到子包参数。  
  
 **删除**  
 单击此选项可删除参数或变量与子包参数之间的映射。  
  
