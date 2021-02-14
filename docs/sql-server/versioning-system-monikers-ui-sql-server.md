---
description: SQL 文档的版本控制系统
title: SQL 文档的版本控制系统
ms.date: 08/12/2020
ms.prod: sql
ms.technology: release-landing
ms.topic: conceptual
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016||>=sql-server-linux-2017||>=sql-server-2016
ms.openlocfilehash: 56ef939790de90d02add427f14f8817df885383f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079735"
---
# <a name="versioning-system-for-sql-documentation"></a>SQL 文档的版本控制系统

[!INCLUDE[includes_appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

本文介绍了用于 SQL 文档的版本控制系统。 版本控制系统了解产品及其版本。 通过该系统，你可选择感兴趣的产品和版本。 该系统随后会显示相应的文档。

## <a name="applies-to-products"></a>“适用范围”产品

大多数 SQL Server 文章的标题下都列出了“适用范围”。 在同一行上，有一个方便的 SQL 产品，其中包含该文章是否与产品相关的指示。 例如，产品 SQL Server 可以表示为相关，而 Azure SQL 数据库可以表示为与文章无关。

“适用范围”行并不了解产品版本。 我们会尽力避免“适用范围”行与版本控制系统配置的产品方面之间的差异。

## <a name="history-of-separate-file-sets"></a>单独文件集的历史记录

对于 SQL Server 2014 及更低版本，每个版本都有自己的文档文件的完整单独副本。 例如，SQL Server 2014 的文档最初是作为 SQL Server 2012 文档的副本开始的。 然后在产品开发周期中编辑 2014 副本。

这种旧方法意味着如果在 2014 文档中发现了一个漏洞，那么该漏洞也可能存在于 2012 和 2008 文档。 这使得修复缺陷和常规维护变得更加困难。

## <a name="multiple-versions-in-the-same-files"></a>同一文件中有多个版本

出于此原因和其他原因，SQL Server 2016 的文档文件也适用于 2017、2019，可能还适用于 \<vNext\>。 这种合并是切实可行的，因为我们现在已将版本控制名字对象分配给了 SQL Server 文档文件。 无论给定文档文件的粒度如何，都可指定或显式嵌入版本控制名字对象。

## <a name="versioning-control-in-the-ui"></a>UI 中的版本控制控件

当使用 :::no-loc text="Docs"::: 网站查看任何 SQL 文档文章时，当前选择的版本控制名字对象在目录 (TOC) 上方可见。 该控件是一个下拉列表。

![media_versioning-control-10-sql-server-2017.png](media/versioning-control-10-sql-server-2017.png)

如果要查看不同版本的 SQL Server 文档，请单击位于当前版本名字对象末尾的扩展箭头。 然后单击以选择所需的任何产品和版本组合。 单击其他版本时，显示的文档会突然更改以显示新选择版本的差异。 可能有也可能没有任何变化，这两种情况都很常见。

![media_versioning-control-20-expanded.png](media/versioning-control-20-expanded.png)

### <a name="https-parameter-no-loc-textview"></a>HTTPS 参数 :::no-loc text="view=":::

其 Web 地址以 `https://docs.microsoft.com/sql/` 开头的每篇文章都有一个名为 `?view=` 的参数附加到其地址。 此参数值是版本控制名字对象代码。

`https` 地址中的名字对象代码始终与版本控制中显示的名字对象名称匹配。

## <a name="products-not-editions"></a>这是产品，不是版本

### <a name="editions"></a>版本

在 20 世纪 90 年代和 2000 年代，Microsoft SQL Server 只有一个产品。 每个版本的 SQL Server 都有不同的版本，例如 SQL Server 2008 的开发人员版和企业版  。 这些版本代表的功能集略有不同，但核心产品是相同的。 新 SQL Server 版本可能仍有多种版本。

### <a name="products"></a>Products

随着最近云计算和 Microsoft Azure 的兴起，Microsoft 发布了其 Azure SQL 数据库产品。 虽然传统的 SQL Server 本地产品和 Azure SQL 数据库产品共享了很多代码，但这些产品是两个真正独立的产品。

对于 SQL，版本控制名字对象可以区分产品，但不能区分版本。

#### <a name="azure-cloud-sql-products"></a>Azure 云 SQL 产品

对于以 `https://docs.microsoft.com/sql/` 开头的文章，几乎所有的 Web 地址都适用于名为 SQL Server 产品的至少一个版本。 这些文章的大部分内容也适用于 Azure 云中托管的一个或多个 SQL 服务产品。 一种这样的 SQL 云产品被命名为 Azure SQL 数据库。

通常，Azure SQL 数据库产品只有一个版本。 几乎所有适用于 Azure SQL 数据库但不适用于 SQL Server 的文章都有以 `https://docs.microsoft.com/azure/sql-database/` 开头的 Web 地址。

## <a name="scenarios-of-version-filtering"></a>版本筛选方案

版本控制系统会筛选掉不适用于当前活动名字对象的所有文档内容。 每次选择不同的版本控制名字对象时，隐藏的内容集都会发生变化。 筛选将隐藏以下级别的内容：

- 文章中的节或句子。
- 目录 (TOC) 中的文章条目。

接下来是介绍选择不同名字对象的效果的方案。

### <a name="scenario-1-within-the-current-article"></a>应用场景 1：在当前文章中

以下方案重点介绍了当前文章中的各个部分：

1. 当前版本控制名字对象是 SQL Server 2017。
2. 你正在阅读描述首次添加到 SQL Server 2017 版本的功能的部分。
3. 将名字对象更改为 SQL Server 2016。
4. 你会注意到你正在阅读的部分已消失。
5. 再次将名字对象更改为 SQL Server 2019。
6. 你会注意到正在阅读的 2017 部分已重新显示。

在上述方案中，有关 2017 新功能的部分可能标有“名字对象范围”，其中包含以下名字对象代码：

- `>=sql-server-2017`

当选择了名字对象 SQL Server 2019 时，版本控制系统意识到 2019 大于或等于 2017，于是就会显示该部分。

### <a name="scenario-2-click-a-link-to-a-hidden-article"></a>应用场景 2：单击指向隐藏文章的链接

以下罕见的方案说明了单击当前从目录 (TOC) 中隐藏的文章链接时会发生的情况。 简而言之，该链接有效：

1. 当前版本控制名字对象是 SQL Server 2017。
2. 在当前文章 :::no-loc text="A"::: 中，单击指向仅适用于 SQL Server 2016 的文章 :::no-loc text="B"::: 的链接。
    - 在单击之前，目录中隐藏了文章 :::no-loc text="B"::: 的条目。
3. 单击后，将显示 :::no-loc text="B"::: 文章。
    - 文章 :::no-loc text="B"::: 的显示强制版本控制切换到 SQL Server 2016 名字对象。
    - 因为必须放弃原始名字 SQL Server 2017。 这种放弃导致信息性消息显示在网页顶部附近。 [消息](#anchor-message-unavailable-for-moniker)说明了必须切换当前名字对象以适应新文章 :::no-loc text="B":::。

### <a name="scenario-3-navigate-to-an-https-address"></a>应用场景 3：导航到 https 地址

以下文章为 SQL Server 2017 添加了新内容。 本文介绍了在 2017 版中添加到 SQL Server 的功能。 大多数或所有这些新功能也是 2019 版的一部分。 下面是文章的属性。

| Attribute | 值 |
| :-------- | :---- |
| 标题 | SQL Server 2017 的新增功能 |
| 名字对象范围 | `=">= sql-server-2017"` |
| `https` 地址 | `https://docs.microsoft.com/sql/sql-server/what-s-new-in-sql-server-2017` |
| &nbsp; | &nbsp; |

给定 `https` 基址，下表说明了当用户使用各种值附加 `?view=` 参数后发生的情况。

| `?view=` 的值 | `https` 地址导航的行为 |
| :---------------- | :------------------------------ |
| （无参数。） | 版本控制系统将尝试其默认的名字对象值。 通常我们将其设置为 SQL Server 的最新非预览版本。<br/><br/>SQL Server 2017 或 2019 的默认值将满足属性 `>= sql-server-2017`。<br/><br/>系统会将参数附加到 `https` 地址，可能为 `?view=sql-server-2017`。<br/>然后将版本控制下拉列表控件设置为匹配名字对象名称。 |
| `sql-server-2016` | 版本控制系统会意识到文章的名字对象范围不包括 2016 版本。<br/><br/>然后系统将选择满足该范围的一个标记。<br/><br/>然后，与 2016 版本的情况一样，将附加参数 `?view=`，并且控件名称将与参数值匹配。 |
| `sql-server-2017` | 版本控制系统理解参数值包含在文章的名字对象范围内。<br/><br/>版本控制将设置为与参数值匹配。 |
| `sql-server-2019` | 与值 `sql-server-2017` 的情况相同，但参数和控件设置为 2019。 |
| &nbsp; | &nbsp; |

### <a name="all-sql---hide-nothing-special-moniker"></a><a name="anchor-allsql-hidenothing"></a> 所有 SQL - 不隐藏任何内容的特殊名字对象

有一个特殊名字对象产品，即“所有 SQL”，其唯一版本为“不隐藏任何内容” 。 这个名字对象的目的是对某些更改进行内部测试。 如果客户使用此名字对象，则比告知更容易产生误导。

某些文章包含有关多个版本的 SQL Server 信息。 每个常规名字对象都隐藏有版本控制的部分，否则这些部分可能会显示与名字对象的版本不准确、混淆或矛盾的信息。 特殊的“所有 SQL”名字对象将显示所有版本部分，并且可能不会明显显示出不准确的信息。

## <a name="message-the-requested-page-is-not-available-for-moniker"></a><a name="anchor-message-unavailable-for-moniker"></a> 消息：请求的页面不适用于 \<moniker\>

以下方案导致在 :::no-loc text="Docs"::: 网页顶部附近显示信息性消息：

1. 当前版本控制名字对象是 SQL Server 2017。
2. 你当前阅读的文章就是与 SQL Server 2017 相关。
    - 该文章与产品 Azure SQL 数据库无关。
3. 尝试将名字对象更改为“Azure SQL 数据库 - 最新版”。
4. 你将看到尝试操作被拒，并显示一条消息。

此种情况结束时，你会在文档网页顶部附近看到以下信息性消息：

> 请求的页面不适用于 Azure SQL 数据库 - 最新版。 你已重定向到可用于此页的最新产品版本。

最新版本可能会排除尚未完全发布且处于预览状态的版本 。

![media_versioning-control-30-viewfallbackfrom.png](media/versioning-control-30-viewfallbackfrom.png)

## <a name="previous-versions-of-sql-server"></a>SQL Server 的早期版本

版本控制系统完全实现 SQL Server 2016 版本。

- 2012及早期版本：&nbsp; 版本控制系统不用于 SQL Server 2012 或更早版本。
    - “SQL Server - 旧版本”的特殊名字对象意味着隐藏几乎所有文章。 极少数例外是旧版本客户可能一次需要几篇文章。
    - [SQL Server 的早期版本，2012-2005](./previous-versions-sql-server.md)

- 2014：&nbsp; 版本控制系统为 SQL Server 2014 实现了一半。 可以在版本控制控件中选择 SQL Server 2014，此方法有效。 然而在内部，2014 的文件专门用于 2014 年，就像 2008 年的文件专用于 2008 年一样。
    - [SQL Server 2014 脱机文档](./sql-server-offline-documentation.md)

- 2016 及更高版本：&nbsp; SQL Server 2016 及更高版本已完全实现版本控制系统。
    - [欢迎使用 2016 及更高版本的 SQL Server 文档](./index.yml?preserve-view=true&view=sql-server-2016)
    - [SQL Server 2016 脱机文档](sql-server-offline-documentation.md)

## <a name="see-also"></a>另请参阅

[SQL Server 的早期版本，2014-2005](./previous-versions-sql-server.md)  
[SQL Server 文档导航指南](sql-docs-navigation-guide.md)  
[如何参与编辑 SQL Server 文档](sql-server-docs-contribute.md)