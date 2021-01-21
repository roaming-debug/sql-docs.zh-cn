---
title: 向报表添加级联参数（报表生成器）| Microsoft Docs
description: 了解如何在报表生成器中使用报表中的级联参数来管理大量报表数据。
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 3a22eec3-57a7-478e-b6fc-102a9dbe0591
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 77a96c10d94bb27d52f8527e6822ff036e4a214e
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596765"
---
# <a name="add-cascading-parameters-to-a-report-report-builder-and-ssrs"></a>向报表添加级联参数（报表生成器和 SSRS）
  级联参数提供了一种管理大量报表数据的方法。 您可以定义一组相关参数，使一个参数的值列表取决于其他参数选取的值。 例如，第一个参数是独立的，并且可能提供产品类别列表。 当用户选中某个类别后，第二个参数则取决于第一个参数的值。 第二个参数的值根据所选类别中的子类别列表进行更新。 用户查看报表时，类别和子类别参数的值用于筛选报表数据。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
 若要创建级联参数，请首先定义数据集查询，并为所需的每个级联参数包括一个查询参数。 您还必须为每个级联参数创建单独的数据集以提供可用值。 有关详细信息，请参阅[为报表参数添加、更改或删除可用值（报表生成器和 SSRS）](../../reporting-services/report-design/add-change-or-delete-available-values-for-a-report-parameter.md)。  
  
 顺序对于级联参数来说很重要，因为对较晚出现在列表中的参数的数据集查询将包含对较早出现在列表中的每个参数的引用。 在运行时，参数在“报表数据”窗格中的顺序确定参数查询在报表中的显示顺序，并由此确定用户选择每个连续参数值的顺序。  
  
 有关创建具有多个值的级联参数以及包括“全选”功能的信息，请参阅 [如何创建“全选”多值级联参数](/archive/blogs/psssql/)。  
  
## <a name="to-create-the-main-dataset-with-a-query-that-includes-multiple-related-parameters"></a>使用包含多个相关参数的查询创建主数据集  
  
1.  在“报表数据”窗格中，右键单击数据源，然后单击 **“添加数据集”** 。  
  
2.  在 **“名称”** 中，键入数据集的名称。  
  
3.  在 **“数据源”** 中，选择数据源的名称，或单击 **“新建”** 创建一个数据源。  
  
4.  在 **“查询类型”** 中，选择所选数据源的查询类型。 在本主题中，假定查询类型为 **“文本”** 。  
  
5.  在 **“查询”** 中，键入检索该报表的数据所使用的查询。 查询必须包含以下部分：  
  
    1.  数据源字段列表。 例如，在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中，SELECT 语句指定给定表或视图中数据库列名的列表。  
  
    2.  每个级联参数的一个查询参数。 查询参数通过指定查询中要包含或排除的特定值来限制从数据源中检索的数据。 通常，查询参数出现在查询的限制子句中。 例如，在 [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT 语句中，查询参数出现在 WHERE 子句中。  
  
6.  单击 **“运行”** ( **“!”** )。 在包括查询参数并运行查询之后，将自动创建对应于查询参数的报表参数。  
  
    > [!NOTE]  
    >  首次运行查询时的查询参数顺序确定它们在报表中的创建顺序。 若要更改顺序，请参阅[更改报表参数的顺序（报表生成器和 SSRS）](../../reporting-services/report-design/change-the-order-of-a-report-parameter-report-builder-and-ssrs.md)  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 接下来，您将创建一个提供独立参数的值的数据集。  
  
## <a name="to-create-a-dataset-to-provide-values-for-an-independent-parameter"></a>创建提供独立参数的值的数据集  
  
1.  在“报表数据”窗格中，右键单击数据源，然后单击 **“添加数据集”** 。  
  
2.  在 **“名称”** 中，键入数据集的名称。  
  
3.  在 **“数据源”** 中，确保该名称是在第 1 步中选择的数据源的名称。  
  
4.  在 **“查询类型”** 中，选择所选数据源的查询类型。 在本主题中，假定查询类型为 **“文本”** 。  
  
5.  在 **“查询”** 中，键入检索该参数的值所使用的查询。 对独立参数的查询通常不包含查询参数。 例如，若要对提供所有类别值的参数创建查询，可以使用如下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    SELECT DISTINCT <column name> FROM <table>  
    ```  
  
     SELECT DISTINCT 命令从结果集删除重复值，以便从指定表的指定列中获取每个唯一值。  
  
     单击 **“运行”** ( **“!”** )。 结果集显示以上第一个参数的可用值。  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 接下来，您将设置第一个参数的属性，以便在运行时使用该数据集填充其可用值。  
  
## <a name="to-set-available-values-for-a-report-parameter"></a>设置报表参数的可用值  
  
1.  在“报表数据”窗格的“参数”文件夹中，右键单击第一个参数，然后单击 **“参数属性”** 。  
  
2.  在 **“名称”** 中，确保参数名称准确无误。  
  
3.  单击 **“可用值”** 。  
  
4.  单击 **“从查询中获取值”** 。 随即将显示以下三个字段。  
  
5.  在 **“数据集”** 的下拉列表中，单击在前面过程中创建的数据集的名称。  
  
6.  在 **“值”** 字段中，单击提供参数值的字段的名称。  
  
7.  在 **“标签”** 字段中，单击提供参数标签的字段的名称。  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 接下来，您将创建一个提供依赖参数的值的数据集。  
  
## <a name="to-create-a-dataset-to-provide-values-for-a-dependent-parameter"></a>创建提供依赖参数的值的数据集  
  
1.  在“报表数据”窗格中，右键单击数据源，然后单击 **“添加数据集”** 。  
  
2.  在 **“名称”** 中，键入数据集的名称。  
  
3.  在 **“数据源”** 中，确保该名称是在第 1 步中选择的数据源的名称。  
  
4.  在 **“查询类型”** 中，选择所选数据源的查询类型。 在本主题中，假定查询类型为 **“文本”** 。  
  
5.  在 **“查询”** 中，键入检索该参数的值所使用的查询。 对依赖参数的查询通常包含该参数所依赖的每个参数的查询参数。 例如，若要对提供某一类别（独立参数）的所有子类别（依赖参数）值的参数创建查询，可以使用如下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```  
    SELECT DISTINCT Subcategory FROM <table>   
    WHERE (Category = @Category)  
    ```  
  
     在 WHERE 子句中，Category 是 \<table> > 的字段名称，@Category 是查询参数。 该语句生成在 @Category 中指定的类别的子类别列表。 在运行时，将使用用户为同名报表参数选择的值来填充该值。  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 接下来，您将设置第二个参数的属性，以便在运行时使用该数据集填充其可用值。  
  
## <a name="to-set-available-values-for-the-second-parameter"></a>设置第二个参数的可用值  
  
1.  在“报表数据”窗格的“参数”文件夹中，右键单击第一个参数，然后单击 **“参数属性”** 。  
  
2.  在 **“名称”** 中，确保参数名称准确无误。  
  
3.  单击 **“可用值”** 。  
  
4.  单击 **“从查询中获取值”** 。  
  
5.  在 **“数据集”** 的下拉列表中，单击在前面过程中创建的数据集的名称。  
  
6.  在 **“值”** 字段中，单击提供参数值的字段的名称。  
  
7.  在 **“标签”** 字段中，单击提供参数标签的字段的名称。  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="to-test-the-cascading-parameters"></a>测试级联参数  
  
1.  单击 **“运行”** 。  
  
2.  从第一个独立参数的下拉列表中，选择一个值。  
  
     报表处理器运行下一个参数的数据集查询，并向该参数传递为第一个参数选择的值。 使用基于第一个参数值的可用值来填充第二个参数的下拉列表。  
  
3.  从第二个依赖参数的下拉列表中，选择一个值。  
  
     在选择最后一个参数之后，不会自动运行报表，这样，您便可以更改您的选择。  
  
4.  单击 **“查看报表”** 。 报表根据您选择的参数来更新显示内容。  
  
## <a name="see-also"></a>另请参阅  
 [添加、更改或删除报表参数（报表生成器和 SSRS）](../../reporting-services/report-design/add-change-or-delete-a-report-parameter-report-builder-and-ssrs.md)   
 [报表参数（报表生成器和报表设计器）](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [教程：向报表添加参数（报表生成器）](../../reporting-services/tutorial-add-a-parameter-to-your-report-report-builder.md)   
 [报表生成器教程](../../reporting-services/report-builder-tutorials.md)   
 [添加数据集筛选器、数据区域筛选器和组筛选器（报表生成器和 SSRS）](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
  
