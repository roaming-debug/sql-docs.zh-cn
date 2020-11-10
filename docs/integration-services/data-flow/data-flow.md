---
description: 数据流
title: 数据流 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- output data [Integration Services]
- data flow [Integration Services], elements
- input data [Integration Services]
- external metadata [Integration Services]
- data flow [Integration Services]
- errors [Integration Services], data flow outputs
ms.assetid: 7a50de3c-4ca0-4922-8028-fdddeb47e5b0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9e81fc5acefbe8014e06ccd173c32004b0e02dd0
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384652"
---
# <a name="data-flow"></a>数据流

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供三种不同类型的数据流组件：源、转换和目标。 源从数据存储区（如关系数据库中的表和视图、文件以及 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据库）中提取数据。 转换修改、汇总和清除数据。 目标将数据加载到数据存储区，或创建内存中的数据集。  
  
> [!NOTE]  
>  在您使用自定义提供程序时，需要使用元数据列值更新 ProviderDescriptors.xml 文件。  
  
 此外， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 还提供将一个组件的输出连接到另一个组件的输入的路径。 路径定义组件的序列，并允许您向数据流添加批注或查看列的源。  
  
 您可以通过把源和目标的输出连接到转换和目标的输入来连接数据流组件。 在构造数据流的过程中，通常在将第二个以及后续组件添加到该数据流时连接这些组件。 连接组件后，输入列即可用在对该组件的配置中。 如果没有可用的输入列，则在组件连接到数据流后才能完成对该组件的配置。 有关详细信息，请参阅 [Integration Services 路径](../../integration-services/data-flow/integration-services-paths.md) 和 [使用路径连接组件](./connect-components-in-a-data-flow.md)。  
  
 以下关系图显示的数据流具有一个源、带有一个输入和一个输出的转换以及一个目标。 除了输入列、输出列和外部列之外，该关系图还包含输入、输出和错误输出。  
  
 ![数据流组件及其输入和输出](../../integration-services/data-flow/media/mw-dts-dataflow.gif "数据流组件及其输入和输出")  
  
## <a name="data-flow-implementation"></a>数据流实现  
 将数据流任务添加到包的控制流是在包中实现数据流的第一步。 包可以包含多个数据流任务，每个数据流任务有自己的数据流。 例如，如果包需要按指定顺序运行数据流，或需要在数据流之间执行其他任务，则必须对每个数据流使用单独的数据流任务。  
  
 控制流包含数据流任务后，就可以开始生成包所使用的数据流。 有关详细信息，请参阅 [数据流任务](../../integration-services/control-flow/data-flow-task.md)。  
  
 创建数据流包括下列步骤：  
  
-   添加一个或多个源以便从字段和数据库提取数据，并且添加连接管理器以便连接到这些源。  
  
-   添加满足包的业务要求的转换。 数据流并非必须包含转换。  
  
     某些转换要求连接管理器。 例如，查找转换使用连接管理器来连接到包含查找数据的数据库。  
  
-   通过将源和转换的输出连接到转换和目标的输入来连接数据流组件。  
  
-   添加一个或多个目标以便将数据加载到字段和数据库之类的数据存储区，并且添加连接管理器以便连接到这些源。  
  
-   配置针对组件的错误输出以便处理问题。  
  
     在运行时，行级错误可能在数据流组件转换数据、执行查找或计算表达式时发生。 例如，值为字符串的数据列不能转换为整数，或尝试除以零的表达式。 两种操作都导致错误，而包含错误的行可以用错误流分别进行处理。 有关在包数据流中如何使用错误流的详细信息，请参阅 [数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)。  
  
-   包括批注以使数据流自文档化。 有关详细信息，请参阅 [在包中使用批注](../../integration-services/use-annotations-in-packages.md)。  
  
> [!NOTE]  
>  创建新包时，还可以使用向导来帮助您正确配置连接管理器、源和目标。 有关详细信息，请参阅 [Create Packages in SQL Server Data Tools](../../integration-services/create-packages-in-sql-server-data-tools.md)。  
  
 在 **“数据流”** 选项卡处于活动状态时，工具箱将包含可添加到数据流的源、转换和目标。  
  
## <a name="expressions"></a>表达式  
 很多数据流组件（源、转换和目标）都支持使用属性表达式来表示它们的某些属性。 属性表达式是在加载包时用于替换属性值的表达式。 在运行时，包使用更新后的属性值。 表达式是使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 表达式语法生成的，并且可以包括 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 函数、运算符、标识符和变量。 有关详细信息，请参阅 [Integration Services (SSIS) 表达式](../../integration-services/expressions/integration-services-ssis-expressions.md)、[Integration Services (SSIS) 表达式](../../integration-services/expressions/integration-services-ssis-expressions.md)和[在包中使用属性表达式](../../integration-services/expressions/use-property-expressions-in-packages.md)。  
  
 如果在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中构造包，则支持属性表达式的任何数据流组件的属性将显示在它们所属的数据流任务上。 若要添加、更改或删除数据流组件的属性表达式，请单击数据流任务，然后使用任务的“属性”窗口或编辑器来添加、更改或删除属性表达式。 数据流任务自身的属性表达式也在“属性”窗口中进行管理。  
  
 如果数据流包含任何使用表达式的组件，则表达式也显示在“属性”窗口中。 若要查看表达式，请选择该组件所属的数据流任务。 可以按类别或者字母顺序查看属性。 如果在“属性”窗口中使用“按分类顺序”视图，任何没有用于特定属性的表达式将显示在 **“杂项”** 类别中。 如果使用字母顺序视图，则按数据流组件的名称顺序列出表达式。  
  
## <a name="sources"></a>源  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]中，源是使数据流中的其他组件可以访问来自不同外部数据源的数据的数据流组件。 可以从平面文件、XML 文件、Microsoft Excel 工作簿和包含原始数据的文件中提取数据。 还可以通过访问数据库中的表和视图以及通过运行查询来提取数据。  
  
 数据流可以包含单个或多个源。  
  
 数据流的源通常具有一个常规输出。 常规输出包含输出列，这些输出列是由源添加到数据流中的。  
  
 常规输出引用外部列。 外部列是源中的列。 例如 **AdventureWorks** 数据库的 **Product** 表中的 **MadeFlag** 列是可以添加常规输出的外部列。 外部列的元数据包含诸如源列的名称、数据类型和长度等信息。  
  
 源的错误输出包含与常规输出相同的列，此外，还包括提供有关错误信息的两个额外列。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型不限制源可拥有的常规输出和错误输出的数目。 除脚本组件外， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含的大多数源都具有一个常规输出，许多源具有一个错误输出。 对于自定义源，可以通过编码方式实现多个常规输出和错误输出。  
  
 所有输出列都可用作数据流中下一个数据流组件的输入列。  
  
 您也可以编写自定义源。 有关详细信息，请参阅 [开发自定义数据流组件](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) 和 [开发特定类型的数据流组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-specific-types-of-data-flow-components.md)。  
  
 以下源拥有可以使用属性表达式来更新的属性：  
  
-   [ADO NET 源](../../integration-services/data-flow/ado-net-source.md)  
  
-   [XML 源](../../integration-services/data-flow/xml-source.md)  
  
### <a name="sources-available-for-download"></a>可供下载的源  
 下表列出了可从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 网站下载的其他源。  
  
|源|说明|  
|------------|-----------------|  
|Oracle 源|Oracle 源是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Oracle by Attunity 的源组件。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Oracle by Attunity 还包括连接管理器和目标。 有关详细信息，请访问下载页 [Microsoft Connectors for Oracle and Teradata by Attunity](https://go.microsoft.com/fwlink/?LinkId=789384)。|  
|SAP BI 源|SAP BI 源是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for SAP BI 的源组件。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for SAP BI 还包括连接管理器和目标。 有关详细信息，请访问下载页 [Microsoft SQL Server 功能包](https://www.microsoft.com/download/details.aspx?id=55992)。|  
|Teradata 源|Teradata 源是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Teradata 的源组件。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Teradata 还包括连接管理器和目标。 有关详细信息，请访问下载页 [Microsoft Connectors for Oracle and Teradata by Attunity](https://go.microsoft.com/fwlink/?LinkId=789384)。|  
  
 有关如何利用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Oracle by Attunity 提高性能的演示，请参阅 [Microsoft Connector for Oracle by Attunity 的性能（SQL Server 视频）](/previous-versions/dn912438(v=msdn.10))。  
  
## <a name="transformations"></a>转换  
 转换的功能非常广泛。 转换可以执行如更新、汇总、清除、合并和分发数据等任务。 可以修改列中的值、查找表中的值、清理数据以及聚合列值。  
  
 转换的输入和输出定义传入和传出数据的列。 根据对数据执行的操作，一些转换具有一个输入和多个输出，而其他转换具有多个输入和一个输出。 转换还可以包含错误输出，它们提供关于发生的错误以及失败的数据的信息，例如，无法转换为整数数据类型的字符串数据。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型不限制转换可以包含的输入、常规输出和错误输出的数目。 您可以创建自定义转换，这些转换可实现多个输入、常规输出和错误输出的任意组合。  
  
 转换的输入被定义为一个或多个输入列。 某些 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 转换还可以引用外部列作为输入。 例如 OLE DB 命令转换的输入包含外部列。 输出列是转换添加到数据流的列。 常规输出和错误输出都包含输出列。 这些输出列转而充当数据流中下一个组件（其他转换或目标）的输入列。  
  
 以下转换有可以用属性表达式来更新的属性：  
  
-   [有条件拆分转换](../../integration-services/data-flow/transformations/conditional-split-transformation.md)  
  
-   [派生列转换](../../integration-services/data-flow/transformations/derived-column-transformation.md)  
  
-   [模糊分组转换](../../integration-services/data-flow/transformations/fuzzy-grouping-transformation.md)  
  
-   [模糊查找转换](../../integration-services/data-flow/transformations/fuzzy-lookup-transformation.md)  
  
-   [OLE DB 命令转换](../../integration-services/data-flow/transformations/ole-db-command-transformation.md)  
  
-   [百分比抽样转换](../../integration-services/data-flow/transformations/percentage-sampling-transformation.md)  
  
-   [透视转换](../../integration-services/data-flow/transformations/pivot-transformation.md)  
  
-   [行抽样转换](../../integration-services/data-flow/transformations/row-sampling-transformation.md)  
  
-   [排序转换](../../integration-services/data-flow/transformations/sort-transformation.md)  
  
-   [逆透视转换](../../integration-services/data-flow/transformations/unpivot-transformation.md)  
  
 有关详细信息，请参阅 [Integration Services Transformations](../../integration-services/data-flow/transformations/integration-services-transformations.md)。  
  
## <a name="destinations"></a>Destinations  
 目标是将数据流中的数据写入特定数据存储区或创建内存中的数据集的数据流组件。 可以将数据加载到平面文件、处理分析对象以及为其他进程提供数据。 还可以通过访问数据库中的表和视图以及运行查询来提取数据。  
  
 数据流可以包含多个将数据加载到不同数据存储区的目标。  
  
 一个 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目标必须具有至少一个输入。 输入包含来自其他数据流组件的输入列。 输入列映射到目标中的列。  
  
 许多目标还具有一个错误输出。 目标的错误输出包含输出列，该输出列通常包含向目标数据存储区写入数据时发生的错误信息。 错误的发生有许多不同的原因。 例如，列可能包含空值，而目标列不能设置为空值。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型不限制目标可包含的常规输入和错误输出的数目，因此，您可以创建具有多个输入和多个错误输出的自定义目标。  
  
 您也可以编写自定义目标。 有关详细信息，请参阅 [开发自定义数据流组件](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) 和 [开发特定类型的数据流组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-specific-types-of-data-flow-components.md)。  
  
 以下目标拥有可以使用属性表达式来更新的属性：  
  
-   [平面文件目标](../../integration-services/data-flow/flat-file-destination.md)  
  
-   [SQL Server Compact Edition 目标](../../integration-services/data-flow/sql-server-compact-edition-destination.md)  
  
### <a name="destinations-available-for-download"></a>可供下载的目标  
 下表列出了可从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 网站下载的其他目标。  
  
|源|说明|  
|------------|-----------------|  
|Oracle 目标|Oracle 目标是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Oracle by Attunity 的目标组件。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Oracle by Attunity 还包括连接管理器和源。 有关详细信息，请访问下载页 [Microsoft Connectors for Oracle and Teradata by Attunity](https://go.microsoft.com/fwlink/?LinkId=789384)。|  
|SAP BI 目标|SAP BI 目标是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for SAP BI 的目标组件。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for SAP BI 还包括连接管理器和源。 有关详细信息，请访问下载页 [Microsoft SQL Server 功能包](https://www.microsoft.com/en-us/download/details.aspx?id=55992)。|  
|Teradata 目标|Teradata 目标是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Teradata by Attunity 的目标组件。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Teradata by Attunity 还包括连接管理器和源。 有关详细信息，请访问下载页 [Microsoft Connectors for Oracle and Teradata by Attunity](https://go.microsoft.com/fwlink/?LinkId=789384)。|  
  
 有关如何利用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Oracle by Attunity 提高性能的演示，请参阅 [Microsoft Connector for Oracle by Attunity 的性能（SQL Server 视频）](/previous-versions/dn912438(v=msdn.10))。  
  
## <a name="connection-managers"></a>连接管理器  
 许多数据流组件都连接到数据源，因此，必须将组件所需的连接管理器添加到包，然后才能正确配置组件。 可以在构造数据流时或开始构造数据流之前添加连接管理器。 有关详细信息，请参阅 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)和[创建连接管理器](../connection-manager/integration-services-ssis-connections.md)。  
  
## <a name="external-metadata"></a>外部元数据  
 使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器在包中创建数据流时，源和目标的元数据被复制到源和目标的外部列，充当架构的快照。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 验证包时， [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器将此快照与源或目标的架构比较，并根据更改发布错误和警告。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目提供脱机模式。 当您离线工作时，不会建立与包所使用的源或目标的连接，也不会更新外部列的元数据。  
  
## <a name="inputs-and-outputs"></a>输入和输出  
 源有输出，目标有输入，而转换既有输入，又有输出。 此外，可将许多数据流组件配置为使用错误输出。  
  
### <a name="inputs"></a>输入  
 目标和转换具有输入。 输入包含一个或多个输入列，如果数据流组件已配置为使用外部列，则输入列可引用外部列。 输入可配置为监视和控制数据流，例如，可指定在出现错误时组件是应失败，忽略错误，还是将错误行重定向至错误输出。 还可为输入指派说明，或更新输入名称。 在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，使用 **“高级编辑器”** 对话框对输入进行配置。 有关 **“高级编辑器”** 的详细信息，请参阅 [Integration Services User Interface](../../integration-services/integration-services-user-interface.md)。  
  
### <a name="outputs"></a>输出  
 源和转换始终具有输出。 输出包含一个或多个输出列，如果数据流组件已配置为使用外部列，则输出列可引用外部列。 可对输出进行配置以提供对数据的下游处理有用的信息。 例如，可指示输出是否已排序。 还可为输出提供说明或更新输出名称。 在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，使用 **“高级编辑器”** 对话框对输出进行配置。  
  
### <a name="error-outputs"></a>错误输出  
 源、目标和转换都可包含错误输出。 可使用 **“配置错误输出”** 对话框指定数据流组件响应每个输入中错误或列中错误的方式。 如果错误或数据截断在运行时发生，且将数据流组件配置为重定向行，则有错误的数据行将被发送到错误输出。 可以将错误输出连接到转换，这些转换应用其他转换或将数据定向到其他目标。 默认情况下，错误输出包含输出列和两个错误列： **ErrorCode** 和 **ErrorColumn** 。 输出列包含失败行的数据， **ErrorCode** 提供错误代码，而 **ErrorColumn** 标识失败的列。  
  
 有关详细信息，请参阅 [数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)。  
  
### <a name="columns"></a>列  
 输入、输出和错误输出是列的集合。 每一列都可配置并且根据列类型（输入、输出或外部），[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 会为列提供不同的属性。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供 3 种不同的列属性设置方法：编程方式、使用组件特定对话框，或使用“高级编辑器”对话框。  
  
## <a name="paths"></a>路径  
 路径连接数据流组件。 在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，可以查看和修改路径属性，查看路径开始点的输出元数据，以及将数据查看器附加到路径。  
  
 有关详细信息，请参阅 [Integration Services Paths](../../integration-services/data-flow/integration-services-paths.md) 和 [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md)。  
  
## <a name="configuration-of-data-flow-components"></a>配置数据流组件  
 数据流组件可在下列级别配置：组件级；输入级、输出级和错误输出级；列级。  
  
-   在组件级，设置对所有组件都通用的属性，并设置组件的自定义属性。  
  
-   在输入级、输出级和错误输出级，设置输入、输出和错误输出的公用属性。 如果组件支持多个输出，可添加输出。  
  
-   在列级，除了组件为列提供的任何自定义属性之外，还可以设置所有列共有的属性。 如果组件支持添加输出列，则可向输出添加列。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。 在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，您可以使用为每个元素类型提供的自定义对话框，或者使用“属性”窗口或 **“高级编辑器”** 对话框来设置元素属性。  
  
 有关如何使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器设置属性的详细信息，请参阅 [设置数据流组件的属性](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
 [在数据流中添加或删除组件](../../integration-services/data-flow/add-or-delete-a-component-in-a-data-flow.md)  
  
 [连接数据流中的组件](../../integration-services/data-flow/connect-components-in-a-data-flow.md)  
  
## <a name="related-content"></a>相关内容  
 technet.microsoft.com 上的视频 [Microsoft Connector for Oracle by Attunity 的性能（SQL Server 视频）](/previous-versions/dn912438(v=msdn.10))。