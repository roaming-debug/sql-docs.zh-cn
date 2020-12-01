---
description: 第 1-2 课：添加和配置平面文件连接管理器
title: 步骤 2：添加和配置平面文件连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 9a77dd32-d8c2-4961-ad37-2a971f9d6043
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d83b2c6a19b7a73be4293dcb66edc6cbc22909d9
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88449729"
---
# <a name="lesson-1-2-add-and-configure-a-flat-file-connection-manager"></a>第 1-2 课：添加和配置平面文件连接管理器

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在本任务中，将在刚创建的包中添加一个平面文件连接管理器。 通过平面文件连接管理器，包可从平面文件中提取数据。 使用平面文件连接管理器，可以指定包从平面文件中提取数据时要应用的文件的名称与位置、区域设置与代码页以及文件格式，其中包括列分隔符。 另外，还可以为各个列手动指定数据类型；也可以使用“提供列类型建议”对话框，自动将提取出来的数据列映射到 **数据类型。**[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]  
  
必须为要使用的每种文件格式创建新的平面文件连接管理器。 由于本教程从多个具有相同数据格式的平面文件中提取数据，因此只需为示例包添加和配置平面文件连接管理器。  
  
在本课程中，将在平面文件连接管理器中配置以下属性：  
  
-   **列名称：** 由于平面文件没有列名称，因此平面文件连接管理器会创建默认列名称。 这些默认名称不能用于标识每个列代表的内容。 将默认名称更改为与要加载平面文件数据的事实表匹配的名称。  
  
-   **数据映射：** 为平面文件连接管理器指定的数据类型映射，由引用该连接管理器的所有平面文件数据源组件使用。 可以使用平面文件连接管理器，或者使用“提供列类型建议”对话框来手动映射数据类型。  在本任务中，查看“提供列类型建议”对话框中建议的映射，然后在“平面文件连接管理器编辑器”对话框中手动创建必要的映射。  
  
> [!NOTE]
> 平面文件连接管理器提供了有关数据文件的区域设置信息。 如果未将计算机配置为使用区域设置选项“英语(美国)”，则必须在“平面文件连接管理器编辑器”对话框中设置其他属性。  
  
## <a name="add-a-flat-file-connection-manager-to-the-ssis-package"></a>向 SSIS 包添加平面文件连接管理器  
  
1.  在“解决方案资源管理器”窗格中，右键单击“连接管理器”，然后选择“新建连接管理器”。
1. 在“添加 SSIS 连接管理器”对话框中，选择“FLATFILE”，然后选择“添加”。
  
2.  在“平面文件连接管理器编辑器”对话框的“连接管理器名称”字段中，输入“示例平面文件源数据”。  
  
3.  选择“浏览”。  
  
4.  在“打开”对话框中，找到计算机上的“SampleCurrencyData.txt”文件。  
  
5.  清除第一个数据行复选框中的列名称。  
  
### <a name="set-locale-sensitive-properties"></a>设置“地域敏感”属性  
  
1.  在“平面文件连接管理器编辑器”对话框中，选择“常规”。  
  
2.  将“区域设置”设置为“英语(美国)”，并将“代码页”设置为“1252”。  
  
### <a name="rename-columns-in-the-flat-file-connection-manager"></a>重命名平面文件连接管理器中的列  
  
1.  在“平面文件连接管理器编辑器”对话框中，选择“高级”。  
  
2.  在属性窗格中，进行如下更改：  
  
    -   将 **Column 0** 名称属性改为 **AverageRate**。  
  
    -   将 **Column 1** 名称属性改为 **CurrencyID**。  
  
    -   将 **Column 2** 名称属性改为 **CurrencyDate**。  
  
    -   将 **Column 3** 名称属性改为 **EndOfDayRate**。  
  
### <a name="remap-column-data-types"></a>重新映射列数据类型  
  
默认情况下，所有四个列最初都设置为字符串数据类型 [DT_STR]，其 **OutputColumnWidth** 为 50。  

1.  在“平面文件连接管理器编辑器”对话框中，选择“建议类型”。  
  
    [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 根据前 200 行数据自动建议合适的数据类型。 您还可以将这些建议选项改为增加或减少取样数据，以便指定整数数据或布尔数据的默认数据类型，或添加作为填充量添加到字符串列中的空格。  
  
    现在，不对“提供列类型建议”对话框中的选项进行任何更改，选择“确定”使 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 提供列数据类型的建议。 此操作将转到“平面文件连接管理器编辑器”对话框的“高级”窗格，在此可以查看 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 建议使用的列数据类型。 或者，如果单击“取消”，则不对列元数据提供任何建议，并使用默认字符串 (DT_STR) 数据类型。  
  
    在本教程中， [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 为 SampleCurrencyData.txt 文件中的数据建议了下表第二列中显示的数据类型。 第四个列提供目标中列所需的数据类型，在后续步骤中定义了这些类型。  
  
    |平面文件列|建议的类型|目标列|目标类型|  
    |--------------------|------------------|----------------------|--------------------|  
    |AverageRate|float [DT_R4]|FactCurrencyRate.AverageRate|FLOAT|  
    |CurrencyID|string [DT_STR]|DimCurrency.CurrencyAlternateKey|nchar(3)|  
    |CurrencyDate|date [DT_DATE]|DimDate.FullDateAlternateKey|date|  
    |EndOfDayRate|float [DT_R4]|FactCurrencyRate.EndOfDayRate|FLOAT|  
  
    为 **CurrencyID** 列建议的数据类型与目标表中字段的数据类型不兼容。 由于 `DimCurrency.CurrencyAlternateKey` 的数据类型为 nchar (3)，因此必须将 CurrencyID 从字符串 [DT_STR] 更改为 Unicode 字符串 [DT_WSTR]。 另外，字段 `DimDate.FullDateAlternateKey` 定义为日期数据类型，因此需要将 CurrencyDate 的类型从日期 [DT_Date] 更改为数据库日期 [DT_DBDATE]。  
  
2.  在列表中选择“CurrencyID”列，在属性窗格中将列 CurrencyID 的数据类型从字符串 [DT_STR] 更改为 Unicode 字符串 [DT_WSTR]。  
  
3.  在属性窗格中，将列 **CurrencyDate** 的数据类型从日期 [DT_DATE] 改为数据库日期 [DT_DBDATE]。  
  
4.  选择“确定”  。  
  
## <a name="go-to-next-task"></a>转到下一个任务
[步骤 3：添加和配置 OLE DB 连接管理器](../integration-services/lesson-1-3-adding-and-configuring-an-ole-db-connection-manager.md)  
  
## <a name="see-also"></a>另请参阅  
[平面文件连接管理器](../integration-services/connection-manager/flat-file-connection-manager.md)  
[Integration Services 数据类型](../integration-services/data-flow/integration-services-data-types.md)  
  
  
  
