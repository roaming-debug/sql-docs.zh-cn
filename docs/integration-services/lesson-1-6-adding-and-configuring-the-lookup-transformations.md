---
description: 第 1-6 课：添加和配置查找转换
title: 步骤 6：添加和配置查找转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/19/2019
ms.prod: sql
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5c59f723-9707-4407-80ae-f05f483cf65f
author: chugugrace
ms.author: chugu
ms.reviewer: ''
ms.openlocfilehash: dbfe5a2fd1472ce67edddeb2eff8a223352dccce
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125605"
---
# <a name="lesson-1-6-add-and-configure-the-lookup-transformations"></a>第 1-6 课：添加和配置查找转换

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在配置用于从源文件中提取数据的平面文件源后，定义获取 CurrencyKey 和 DateKey 值所需的查找转换。 查找转换通过将指定输入列中的数据联接到引用数据集中的列来执行查找。 引用数据集可以是现有的表或视图，也可以是新表或 SQL 语句的结果。 在本教程中，查找转换使用 OLE DB 连接管理器连接到包含引用数据集的源数据的数据库。  
  
> [!NOTE]  
> 还可以配置查找转换，以连接到包含引用数据集的缓存。 有关详细信息，请参阅[查找转换](../integration-services/data-flow/transformations/lookup-transformation.md)。  
  
在此任务中，将向包中添加以下两个查找转换组件并对其进行配置：  
  
-   一个转换是根据平面文件中匹配的“CurrencyID”列值对“DimCurrency”维度表的“CurrencyKey”列中的值执行查找。  
  
-   另一个转换是根据平面文件中匹配的“CurrencyDate”列值对“DimDate”维度表的“DateKey”列中的值执行查找。  
  
无论在哪种情况下，查找转换都使用前面创建的 OLE DB 连接管理器。  
  
## <a name="add-and-configure-the-lookup-currency-key-transformation"></a>添加和配置 Lookup Currency Key 转换  
  
1.  在“SSIS 工具箱”中，展开“公共”，然后将“查找”拖到“数据流”选项卡的设计图面上。将“查找”直接放在“Extract Sample Currency Data”源的下面 。  
  
2.  单击“Extract Sample Currency Data”平面文件源，然后将蓝色箭头拖到新添加的“查找”转换中，以连接这两个组件。  
  
3.  在“数据流”设计图面上，选择“查找”转换中的“查找”，然后将该名称更改为“Lookup Currency Key”。  
  
4.  双击“Lookup Currency Key”转换，以显示查找转换编辑器。  
  
5.  在“常规”页上，进行以下选择：  
  
    1.  选择“完全缓存”。  
  
    2.  在 **“连接类型”** 区域，选择 **“OLE DB 连接管理器”**。  
  
6.  在“连接”页上，进行以下选择：  
  
    1.  在“OLE DB 连接管理器”对话框中，确保显示 **localhost.AdventureWorksDW2012**。  
  
    2.  选择“使用 SQL 查询的结果”，然后键入或粘贴以下 SQL 语句：  
  
        ```sql
        SELECT * FROM [Sales].[Currency]
        WHERE [CurrencyCode]
        IN ('ARS', 'AUD', 'BRL', 'CAD', 'CNY',
            'DEM', 'EUR', 'FRF', 'GBP', 'JPY',
            'MXN', 'SAR', 'USD', 'VEB')
        ```  
    3.  选择“预览”验证查询结果。
  
7.  在“列”页上，进行以下选择：  
  
    1.  在“可用输入列”面板中，将“CurrencyID”拖到“可用查找列”面板的“CurrencyAlternateKey”上。  
  
    2.  在“可用查找列”列表中，选中“CurrencyKey”左侧的复选框。  
  
8.  选择“确定”，返回“数据流”设计图面。  
  
9. 右键单击“Lookup Currency Key”转换，然后选择“属性”。  
  
10. 在“属性”窗口中，确认“LocaleID”属性为“英语(美国)”，且“DefaultCodePage”属性为“1252”。  
  
## <a name="add-and-configure-the-lookup-date-key-transformation"></a>添加和配置 Lookup Date Key 转换  
  
1.  在“SSIS 工具箱”中，将“查找”拖到“数据流”设计图面上。 将“查找”直接放置在“Lookup Currency Key”转换的下面。  
  
2.  选择 Lookup Currency Key 转换，并将蓝色箭头拖动到新的“查找”转换中，以连接这两个组件 。  
  
3.  在“选择输入输出”对话框中，选择“输出”列表框中的“查找匹配输出”，然后选择“确定”。  
  
4.  在“数据流”设计图面上，在新添加的“查找”转换中选择名称“查找”，然后将名称更改为“Lookup Date Key”。  
  
5.  双击 **Lookup Date Key** 转换。  
  
6.  在“常规”页上，选择“部分缓存”。  
  
7.  在“连接”页上，进行以下选择：  
  
    1.  在“OLEDB 连接管理器”对话框中，确保已显示 localhost.AdventureWorksDW2012。  
  
    2.  在“使用表或视图”框中，输入或选择 [dbo].[DimDate]。  
  
8.  在“列”页上，进行以下选择：  
  
    1.  在“可用输入列”面板中，将“CurrencyDate”拖到“可用查找列”面板的“FullDateAlternateKey”上。  如果出现一条消息，指示数据类型不匹配，则将 CurrencyDate 的数据类型更改为 [DT_DBDATE]。
  
    2.  在“可用查找列”列表中，选中“DateKey”左侧的复选框。  
  
9. 在“高级”页上，查看缓存选项。  
  
10. 选择“确定”，返回“数据流”设计图面。  
  
11. 双击“Lookup Date Key”转换，然后选择“属性”。
  
12. 在“属性”窗口中，确认“LocaleID”属性为“英语(美国)”，且“DefaultCodePage”属性为“1252”。  
  
## <a name="go-to-next-task"></a>转到下一个任务
[步骤 7：添加和配置 OLE DB 目标](../integration-services/lesson-1-7-adding-and-configuring-the-ole-db-destination.md)  
  
## <a name="see-also"></a>另请参阅  
[查找转换](../integration-services/data-flow/transformations/lookup-transformation.md)  
