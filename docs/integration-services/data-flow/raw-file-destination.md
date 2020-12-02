---
description: Raw File Destination
title: 原始文件目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.rawfiledest.f1
- sql13.dts.designer.rawfiledestinationconnectionmanager.f1
- sql13.dts.designer.rawfiledestinationcolumns.f1
helpviewer_keywords:
- append options [Integration Services]
- destinations [Integration Services], Raw File
- raw data [Integration Services]
- writing raw data
- Raw File destination
ms.assetid: d311b458-aefc-4b4d-b1a1-4c0ebbb34214
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 27b28672540d25fe84573c37004161992d3d3827
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194181"
---
# <a name="raw-file-destination"></a>Raw File Destination

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  原始文件目标将原始数据写入文件。 因为数据的格式是目标的本机格式，所以数据无需转换，并且几乎不需要分析。 这意味着原始文件目标可以比其他目标（如平面文件和 OLE DB 目标）更快地写入数据。  
  
 除了将原始数据写入文件外，还可以使用原始文件目标生成仅包含列的空原始文件（仅元数据文件），而无需运行包。 使用原始文件源检索先前由目标写入的原始数据。 还可以将原始文件源指向仅元数据文件。  
  
 原始文件格式包含排序信息。 原始文件目标将保存所有排序信息，包括字符串列的比较标志。 原始文件源读取和采用排序信息。 您可以使用高级编辑器选择将原始文件源配置为忽略文件中的排序标志。 有关比较标志的详细信息，请参阅 [Comparing String Data](../../integration-services/data-flow/comparing-string-data.md)。  
  
 可以用下列方式配置原始文件目标：  
  
-   指定访问模式，此访问模式可以是原始文件目标要写入的文件的名称，也可以是包含此文件名称的变量。  
  
-   指示原始文件目标是将数据追加到具有相同名称的现有文件中，还是创建新文件。  
  
 原始文件目标经常用于在包执行之间写入经部分处理的数据的中间结果。 存储原始数据意味着数据可以由原始文件源快速读取，然后在将其加载到其最终目标之前进一步转换。 例如，包可能运行多次，每次都将原始数据写入文件。 随后，其他包可以使用原始文件源从每个文件读取，使用 Union All 转换将数据合并为一个数据集，然后应用其他转换在将数据加载到其最终目标（如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表）之前编写数据摘要。  
  
> [!NOTE]  
>  原始文件目标支持空数据，但不支持二进制大型对象 (BLOB) 数据。  
  
> [!NOTE]  
>  原始文件目标不使用连接管理器。  
  
 此源具有一个常规输入。 它不支持错误输出。  
  
## <a name="append-and-new-file-options"></a>追加和新建文件选项  
 WriteOption 属性包含向现有文件中追加数据或创建新文件的选项。  
  
 下表介绍 WriteOption 属性的可用选项。  
  
|选项|说明|  
|------------|-----------------|  
|追加|将数据追加到现有文件中。 追加数据的元数据必须与文件格式匹配。|  
|始终创建|始终创建新文件。|  
|创建一次|创建新文件。 如果该文件已经存在，该组件将失败。|  
|截断和追加|截断现有文件，然后将数据写入此文件。 追加数据的元数据必须与文件格式匹配。|  
  
 以下是有关追加数据的重要事项：  
  
-   将数据追加到现有原始文件不重新对数据排序。  
  
     您需要确保已排序的键保持正确的顺序。  
  
-   将数据追加到现有原始文件不更改文件元数据（排序信息）。  
  
 例如，包读取针对 ProductKey (PK) 排序的数据。 包数据流将数据追加到现有原始文件。 首次运行包时，收到三行 (PK 1000, 1100, 1200)。 原始文件现在包含以下数据：  
  
-   1000, productA  
  
-   1100, productB  
  
-   1200, productC  
  
 第二次运行包时，收到新的两行 (PK 1001, 1300)。 原始文件现在包含以下数据：  
  
-   1000, productA  
  
-   1100, productB  
  
-   1200, productC  
  
-   1001, productD  
  
-   1300, productE  
  
 将新数据追加到原始文件末尾，已排序的键 (PK) 顺序打乱。 此外，追加操作没有更改文件元数据（排序信息）。 如果使用原始文件源读取文件，该组件指示文件仍针对 PK 排序，即使文件中的数据顺序不再正确。  
  
 为了在追加数据时使已排序的键保持正确的顺序，可以按以下方式设计包数据流：  
  
1.  使用源 A 检索新行。  
  
2.  使用源 B 从 RawFile1 检索现有行。  
  
3.  使用 Union All 转换合并源 A 和源 B 中的输入。  
  
4.  针对 PK 排序。  
  
5.  使用原始文件目标写入 RawFile2。  
  
     RawFile1 被锁定，因为在数据流中正在读取它。  
  
6.  使用 RawFile2 替换 RawFile1。  
  
### <a name="using-the-raw-file-destination-in-a-loop"></a>在循环中使用原始文件目标  
 如果使用原始文件目标的数据流位于循环中，您可能要创建一次文件，然后在循环重复时向该文件中追加数据。 若要向该文件中追加数据，要追加的数据必须与现有的文件格式匹配。  
  
 若要在循环的第一次迭代中创建文件，然后在循环的后续迭代中追加行，您需要在设计时执行下列操作：  
  
1.  将 WriteOption 属性设置为 **CreateOnce** 或 **CreateAlways** 并运行循环的一次迭代过程。 此时将创建文件。 这可确保追加数据的元数据与文件匹配。  
  
2.  将 WriteOption 属性重置为“追加”并将 ValidateExternalMetadata 属性设置为 **False**。  
  
 如果使用 **TruncateAppend** 选项而不是 **“追加”** 选项，此操作将截断以前的任何迭代过程中所添加的行，然后追加新行。 使用 **TruncateAppend** 选项也要求数据与文件格式相匹配。  
  
## <a name="configuration-of-the-raw-file-destination"></a>原始文件目标的配置  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [原始文件自定义属性](../../integration-services/data-flow/raw-file-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 有关如何设置数据流组件属性的信息，请参阅 [设置数据流组件属性](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)。  
  
## <a name="related-content"></a>相关内容  
 sqlservercentral.com 上的博客文章： [原始文件令人生畏](https://www.sqlservercentral.com/blogs/31-days-of-ssis-%e2%80%93-raw-files-are-awesome-131)。  
  
## <a name="raw-file-destination-editor-connection-manager-page"></a>原始文件目标编辑器（“连接管理器”页）
  使用原始文件目标编辑器配置原始文件目标以将原始数据写入文件。  
  
 **您希望做什么？**  
  
-   [打开原始文件目标编辑器](#open)  
  
-   [设置“连接管理器”选项卡上的选项](#connection)  
  
-   [设置“列”选项卡上的选项](#mapping)  
  
###  <a name="open-the-raw-file-destination-editor"></a><a name="open"></a> 打开原始文件目标编辑器  
  
1.  将原始文件目标添加到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中的 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]包。  
  
2.  右键单击该组件，然后单击 **“编辑”**。  
  
###  <a name="set-options-on-the-connection-manager-tab"></a><a name="connection"></a> 设置“连接管理器”选项卡上的选项  
 **访问模式**  
 选择指定文件名的方式。 选择 **“文件名”** 可以直接输入文件名和路径，选择 **“变量中的文件名”** 可以指定包含文件名的变量。  
  
 **“文件名”** 或 **“变量名称”**  
 输入原始文件的名称和路径，或者选择包含文件名的变量。  
  
 **写入选项**  
 选择用来创建和写入文件的方法。  
  
 **生成初始原始文件**  
 单击此按钮可以生成仅包含列的空原始文件（仅元数据文件），而无需运行包。 该文件包含在 **“原始文件目标编辑器”** 的 **“列”** 页上选择的列。 可以将原始文件源指向此仅元数据文件。  
  
 单击 **“生成初始原始文件”** 时，将显示一个消息框。 单击 **“确定”** 可继续创建文件。 单击 **“取消”** 可选择 **“列”** 页上的其他列列表。  
  
###  <a name="set-options-on-the-columns-tab"></a><a name="mapping"></a> 设置“列”选项卡上的选项  
 **可用输入列**  
 选择要写入原始文件的一个或多个输入列。  
  
 **输入列**  
 当您在 **“可用输入列”** 下选择某一输入列时，该输入列将自动添加到此表中，或者，您可以直接在此表中选择该输入列。  
  
 **输出别名**  
 指定要用于输出列的备用名称。  
  
## <a name="raw-file-destination-editor-columns-page"></a>原始文件目标编辑器（“列”页）
  使用原始文件目标编辑器配置原始文件目标以将原始数据写入文件。  
  
 **您希望做什么？**  
  
-   [打开原始文件目标编辑器](#open)  
  
-   [设置“连接管理器”选项卡上的选项](#connection)  
  
-   [设置“列”选项卡上的选项](#mapping)  
  
###  <a name="open-the-raw-file-destination-editor"></a><a name="open"></a> 打开原始文件目标编辑器  
  
1.  将原始文件目标添加到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中的 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]包。  
  
2.  右键单击该组件，然后单击 **“编辑”**。  
  
###  <a name="set-options-on-the-connection-manager-tab"></a><a name="connection"></a> 设置“连接管理器”选项卡上的选项  
 **访问模式**  
 选择指定文件名的方式。 选择 **“文件名”** 可以直接输入文件名和路径，选择 **“变量中的文件名”** 可以指定包含文件名的变量。  
  
 **“文件名”** 或 **“变量名称”**  
 输入原始文件的名称和路径，或者选择包含文件名的变量。  
  
 **写入选项**  
 选择用来创建和写入文件的方法。  
  
 **生成初始原始文件**  
 单击此按钮可以生成仅包含列的空原始文件（仅元数据文件），而无需运行包。 可以将原始文件源指向仅元数据文件。  
  
 单击此按钮时，将显示列的列表。 您可以单击“取消”以修改列或单击“确定”以继续创建该文件。  
  
###  <a name="set-options-on-the-columns-tab"></a><a name="mapping"></a> 设置“列”选项卡上的选项  
 **可用输入列**  
 选择要写入原始文件的一个或多个输入列。  
  
 **输入列**  
 当您在 **“可用输入列”** 下选择某一输入列时，该输入列将自动添加到此表中，或者，您可以直接在此表中选择该输入列。  
  
 **输出别名**  
 指定要用于输出列的备用名称。  
  
## <a name="see-also"></a>另请参阅  
 [原始文件源](../../integration-services/data-flow/raw-file-source.md)   
 [数据流](../../integration-services/data-flow/data-flow.md)  
  
