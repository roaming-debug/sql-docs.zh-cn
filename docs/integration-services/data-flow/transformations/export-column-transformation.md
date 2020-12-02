---
description: 导出列转换
title: 导出列转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.exportcolumntrans.f1
- sql13.dts.designer.fileextractortransformation.columns.f1
- sql13.dts.designer.fileextractortransformation.errorhandling.f1
helpviewer_keywords:
- exporting data
- append options [Integration Services]
- Export Column transformation [Integration Services]
- columns [Integration Services], exporting
- inserting data
- truncate options [Integration Services]
ms.assetid: 678d2dfc-e40c-4fbb-b2cc-42fffc44478a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1cf2f0799896bfaac652e00a7736a0de3d8be97e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123372"
---
# <a name="export-column-transformation"></a>导出列转换

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  导出列转换读取数据流中的数据，并将数据插入到文件中。 例如，如果数据流包含产品信息（如每件产品的图片），则可使用导出列转换将图像保存到文件中。  
  
## <a name="append-and-truncate-options"></a>追加和截断选项  
 下表说明了追加和截断选项的设置如何影响结果。  
  
|Append|Truncate|文件存在|结果|  
|------------|--------------|-----------------|-------------|  
|False|False|否|该转换将创建一个新文件并将数据写入到该文件中。|  
|True|False|否|该转换将创建一个新文件并将数据写入到该文件中。|  
|False|True|否|该转换将创建一个新文件并将数据写入到该文件中。|  
|True|True|否|该转换的设计时验证失败。 将两个属性都设置为 **true** 是无效的。|  
|False|False|是|发生运行时错误。 文件存在，但转换无法写入到文件中。|  
|False|True|是|转换将删除文件，然后重新创建文件并将数据写入到文件中。|  
|True|错误|是|转换将打开文件并将数据写入到文件末尾。|  
|True|True|是|该转换的设计时验证失败。 将两个属性都设置为 **true** 是无效的。|  
  
## <a name="configuration-of-the-export-column-transformation"></a>导出列转换的配置  
 可以按照下列方式配置导出列转换：  
  
-   指定数据列和包含要向其写入数据的文件的路径的列。  
  
-   指定数据插入操作是追加还是截断现有文件。  
  
-   指定是否将字节顺序标记 (BOM) 写入到文件中。  
  
    > [!NOTE]  
    >  仅在不将数据追加到现有文件且数据具有 DT_NTEXT 数据类型时写入 BOM。  
  
 此转换使用成对的输入列：一列包含文件名，另一列包含数据。 数据集中的每一行都可指定一个不同的文件。 转换在处理行时，数据将插入到指定的文件中。 在运行时，如果这些文件不存在，转换将创建这些文件，然后将数据写入到文件中。 要写入的数据必须具有 DT_TEXT、DT_NTEXT 或 DT_IMAGE 数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../../integration-services/data-flow/integration-services-data-types.md)。  
  
 此转换有一个输入、一个输出和一个错误输出。  
  
 可以通过 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](../set-the-properties-of-a-data-flow-component.md)  
  
-   [转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 有关如何设置属性的详细信息，请参阅 [设置数据流组件的属性](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)。  
  
## <a name="export-column-transformation-editor-columns-page"></a>导出列转换编辑器（“列”页）
  可以使用 **“导出列转换编辑器”** 对话框的 **“列”** 页，指定数据流中要提取到文件的列。 可以指定导出列转换是将数据追加到文件还是覆盖现有文件。  
  
### <a name="options"></a>选项  
 **提取列**  
 从包含文本数据或图像数据的输入列的列表中进行选择。 所有行都应包含 **“提取列”** 和 **“文件路径列”** 的定义。  
  
 **“文件路径列”**  
 从包含文件路径和文件名的输入列的列表中进行选择。 所有行都应包含 **“提取列”** 和 **“文件路径列”** 的定义。  
  
 **允许追加**  
 指定转换是否将数据追加到现有文件。 默认值为 **false**。  
  
 **强制截断**  
 指定转换在写入数据之前是否删除现有文件的内容。 默认值为 **false**。  
  
 **写入 BOM**  
 指定是否将字节顺序标记 (BOM) 写入文件。 只有在数据具有 **DT_NTEXT** 或 DT_WSTR 数据类型，并且未将数据追加到现有数据文件时，才会写入 BOM。  
  
## <a name="export-column-transformation-editor-error-output-page"></a>导出列转换编辑器（“错误输出”页）
  可以使用 **“导出列转换编辑器”** 对话框的 **“错误输出”** 页指定错误的处理方式。  
  
### <a name="options"></a>选项  
 **输入/输出**  
 查看输出的名称。 单击名称可以展开视图以包括列。  
  
 **列**  
 查看在“导出列转换编辑器”对话框的“列”页中选择的输出列。  
  
 **错误**  
 指定发生错误时要执行的操作：忽略失败、重定向行或使组件失败。  
  
 **截断**  
 指定发生截断时要执行的操作：忽略失败、重定向行或使组件失败。  
  
 **说明**  
 查看操作的说明。  
  
 **将此值设置到选定的单元格**  
 指定发生错误或截断时应对所有选定单元格执行的操作：忽略失败、重定向行或使组件失败。  
  
 **应用**  
 将错误处理选项应用到选定的单元格。  
  
