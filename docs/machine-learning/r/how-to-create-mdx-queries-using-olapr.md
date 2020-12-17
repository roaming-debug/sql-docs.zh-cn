---
title: 使用 olapR 在 R 中创建 MDX 查询
description: 了解如何使用 SQL Server 中的 olapR 包库在 R 语言脚本中编写 MDX 查询或执行现有的 MDX 查询。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/22/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 8e2f37542ae3363e654370f6dcdcbc76cc941335
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470848"
---
# <a name="how-to-create-mdx-queries-in-r-using-olapr"></a>如何使用 olapR 在 R 中创建 MDX 查询
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

[olapR](/machine-learning-server/r-reference/olapr/olapr) 包支持对 SQL Server Analysis Services 中托管的多维数据集执行 MDX 查询。 可以针对现有多维数据集生成查询，浏览维度和其他多维数据集对象，并粘贴进现有 MDX 查询以检索数据。

本文介绍 olapR 包的两大主要用途  ：

+ [使用 olapR 包中提供的构造函数，从 R 生成 MDX 查询](#buildMDX)
+ [使用 olapR 和 OLAP 提供程序执行现有的有效 MDX 查询](#executeMDX)

不支持以下操作：

+ 针对表格模型的 DAX 查询
+ 创建新的 OLAP 对象
+ 写回分区，包括度量值或总和

## <a name="build-an-mdx-query-from-r"></a><a name="buildMDX"></a> 从 R 生成 MDX 查询

1. 定义指定 OLAP 数据源（SSAS 实例）和 MSOLAP 提供程序的连接字符串。

2. 使用函数 `OlapConnection(connectionString)` 创建 MDX 查询的句柄并传递连接字符串。

3. 使用 `Query()` 构造函数实例化查询对象。

4. 使用以下帮助程序函数，提供要包含在 MDX 查询中的维度和度量值的相关详细信息：

     + `cube()` 指定 SSAS 数据库的名称。 如果连接到命名实例，请提供计算机名称和实例名称。 
     + `columns()` 提供要在 ON COLUMNS 参数中使用的度量值的名称  。
     + `rows()` 提供要在 ON ROWS 参数中使用的度量值的名称  。
     + `slicers()` 指定要作为切片器使用的字段或成员。 切片器就像应用于所有 MDX 查询数据的筛选器一样。
     
     + `axis()` 指定要在查询中使用的附加轴的名称。 
     
         OLAP 多维数据集最多可以包含 128 个查询轴。 通常前四个轴称为“列”、“行”、“页”和“章”     。 
         
         如果查询相对简单，可以使用函数 `columns`、 `rows`等生成查询。 但也可以使用带非零索引值的 `axis()` 函数，生成具有许多限定符的 MDX 查询或将额外维度添加为限定符。

5. 将句柄和已完成的 MDX 查询传递到下面其中一个函数，具体取决于结果的外观： 

  + `executeMD` 返回多维数组
  + `execute2D` 返回二维（表格）数据帧

## <a name="execute-a-valid-mdx-query-from-r"></a><a name="executeMDX"></a> 从 R 执行有效的 MDX 查询

1. 定义指定 OLAP 数据源（SSAS 实例）和 MSOLAP 提供程序的连接字符串。

2. 使用函数 `OlapConnection(connectionString)` 创建 MDX 查询的句柄并传递连接字符串。

3. 定义 R 变量来存储 MDX 查询的文本。

4. 将包含 MDX 查询的句柄和变量传递到函数 `executeMD` 或 `execute2D`，具体取决于结果的外观。

    + `executeMD` 返回多维数组
    + `execute2D` 返回二维（表格）数据帧

## <a name="examples"></a>示例

下面的示例基于 AdventureWorks 数据市场和多维数据集项目，因为该项目在多个版本中广泛可用，包括可以轻松还原到 Analysis Services 的备份文件。 如果没有现有的多维数据集，请使用以下任一选项获取示例多维数据集：

+ 按照 Analysis Services 教程第 4 课，创建在这些示例中使用的多维数据集：[创建 OLAP 多维数据集](/analysis-services/multidimensional-tutorial/multidimensional-modeling-adventure-works-tutorial)

+ 将现有多维数据集作为备份下载，然后将其还原为 Analysis Services 的实例。 例如，此站点提供压缩格式的完全处理的多维数据集：[Adventure Works Multidimensional Model SQL 2014](https://msftdbprodsamples.codeplex.com/downloads/get/882334)。 提取文件，然后将其还原到 SSAS 实例。 有关详细信息，请参阅[备份和还原](/analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases)或 [Restore-ASDatabase Cmdlet](/powershell/module/sqlserver/restore-asdatabase)。

### <a name="1-basic-mdx-with-slicer"></a>1.带切片器的基本 MDX

此 MDX 查询将选择度量值  ，用于 Internet 销售计数和销量的计数和数量，并将它们放置到“列”轴。 它将 SalesTerritory 维度的成员添加为切片器  来筛选查询，使计算中仅使用澳大利亚的销售额。

```MDX
SELECT {[Measures].[Internet Sales Count], [Measures].[InternetSales-Sales Amount]} ON COLUMNS, 
{[Product].[Product Line].[Product Line].MEMBERS} ON ROWS 
FROM [Analysis Services Tutorial] 
WHERE [Sales Territory].[Sales Territory Country].[Australia]
```

+ 在列上可将多个度量值指定为逗号分隔字符串的元素。
+ “行”轴使用“产品系列”维度的所有可能的值（所有成员）。 
+ 此查询将返回具有三列的表格，其中包含所有国家/地区的 Internet 销售额的汇总摘要  。
+ WHERE 子句指定了切片器轴  。 在此示例中，该切片器使用 SalesTerritory 维度的成员筛选查询，以便在计算中仅使用澳大利亚的销售额  。

#### <a name="to-build-this-query-using-the-functions-provided-in-olapr"></a>使用 olapR 中提供的函数生成此查询

```R
cnnstr <- "Data Source=localhost; Provider=MSOLAP; initial catalog=Analysis Services Tutorial"
ocs <- OlapConnection(cnnstr)

qry <- Query()
cube(qry) <- "[Analysis Services Tutorial]"
columns(qry) <- c("[Measures].[Internet Sales Count]", "[Measures].[Internet Sales-Sales Amount]")
rows(qry) <- c("[Product].[Product Line].[Product Line].MEMBERS") 
slicers(qry) <- c("[Sales Territory].[Sales Territory Country].[Australia]")

result1 <- executeMD(ocs, qry)

```

对于命名实例，请务必对 R 中可能被视为控制字符的任何字符进行转义。例如，下面的连接字符串引用名为 ContosoHQ 的服务器上的实例 OLAP01：

```R
cnnstr <- "Data Source=ContosoHQ\\OLAP01; Provider=MSOLAP; initial catalog=Analysis Services Tutorial"
```

#### <a name="to-run-this-query-as-a-predefined-mdx-string"></a>将此查询作为预定义的 MDX 字符串运行

```R
cnnstr <- "Data Source=localhost; Provider=MSOLAP; initial catalog=Analysis Services Tutorial"
ocs <- OlapConnection(cnnstr)

mdx <- "SELECT {[Measures].[Internet Sales Count], [Measures].[InternetSales-Sales Amount]} ON COLUMNS, {[Product].[Product Line].[Product Line].MEMBERS} ON ROWS FROM [Analysis Services Tutorial] WHERE [Sales Territory].[Sales Territory Country].[Australia]"

result2 <- execute2D(ocs, mdx)
```

如果使用 SQL Server Management Studio 中的 MDX 生成器定义查询，然后保存 MDX 字符串，它将从 0 开始为轴编号，如下所示： 

```MDX
SELECT {[Measures].[Internet Sales Count], [Measures].[Internet Sales-Sales Amount]} ON AXIS(0), 
   {[Product].[Product Line].[Product Line].MEMBERS} ON AXIS(1) 
   FROM [Analysis Services Tutorial] 
   WHERE [Sales Territory].[Sales Territory Countr,y].[Australia]
```

仍可将此查询作为预定义的 MDX 字符串运行。 但是，若要使用 `axis()` 函数通过 R 生成相同的查询，则务必从 1 开始为轴重新编号。

### <a name="2-explore-cubes-and-their-fields-on-an-ssas-instance"></a>2.浏览 SSAS 实例上的多维数据集及其字段

可以使用 `explore` 函数返回要用于构造查询的多维数据集、 维度或成员的列表。 如果无法使用其他 OLAP 浏览工具，或要通过编程方式操作或构造 MDX 查询，这非常方便。

#### <a name="to-list-the-cubes-available-on-the-specified-connection"></a>列出指定连接上可用的多维数据集

若要查看有权查看的实例上的所有多维数据集或透视，将句柄作为 `explore`的参数提供。

> [!IMPORTANT]
> 最终结果不是多维数据集；TRUE 仅表示元数据操作成功  。 如果参数无效，将引发错误。

```R
cnnstr <- "Data Source=localhost; Provider=MSOLAP; initial catalog=Analysis Services Tutorial"
ocs <- OlapConnection(cnnstr)
explore(ocs)
```

| 结果  |
| ----|
| Analysis Services 教程 |
|_Internet Sales_|
|分销商销售额 |
|销售汇总 |
|[1] TRUE |

#### <a name="to-get-a-list-of-cube-dimensions"></a>获取多维数据集维度列表

若要查看多维数据集或透视中的所有维度，请指定多维数据集或透视名称。

```R
cnnstr <- "Data Source=localhost; Provider=MSOLAP; initial catalog=Analysis Services Tutorial"
ocs \<- OlapConnection(cnnstr)
explore(ocs, "Sales")
```

| 结果  |
| ----|
| _客户_|
|_Date_|
|_区域_|


#### <a name="to-return-all-members-of-the-specified-dimension-and-hierarchy"></a>返回指定维度和层次结构的所有成员

定义源并创建句柄后，指定要返回的多维数据集、维度和层次结构。 返回结果中前缀为 -> 的项表示前一个成员的子级  。

```R
cnnstr <- "Data Source=localhost; Provider=MSOLAP; initial catalog=Analysis Services Tutorial"
ocs <- OlapConnection(cnnstr)
explore(ocs, "Analysis Services Tutorial", "Product", "Product Categories", "Category")
```

| 结果  |
| ----|
| _Accessories_|
|_Bikes_|
|_Clothing_|
|_组件_|
|-> 程序集组件|
|-> 程序集组件|


## <a name="see-also"></a>另请参阅

[在 R 中使用来自 OLAP 多维数据集的数据](../../machine-learning/r/using-data-from-olap-cubes-in-r.md)