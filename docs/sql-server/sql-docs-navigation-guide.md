---
title: SQL Server 文档导航提示
description: SQL Server 技术文档导航提示和技巧 - 介绍了中心页、目录和标题等内容，以及如何使用痕迹导航和版本筛选器。
ms.date: 08/12/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: 4c8852c9b2ce3f7bce027632b741511a008cb646
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595878"
---
# <a name="sql-server-docs-navigation-guide"></a>SQL Server 文档导航指南

本主题提供一些用于导航 SQL Server 技术文档空间的提示和技巧。  

## <a name="hub-page"></a>中心页

SQL Server 中心页可以在 [https://aka.ms/sqldocs](../index.yml?WT.mc_id=akams) 中找到，是用于查找相关 SQL Server 内容的入口点。

始终可以通过从 SQL Server 技术文档集中每个页面顶部的标题中选择“SQL 文档”  来导航回到此页面： 

![标题中的 SQL 文档](media/sql-server-docs-navigation-guide/sql-docs-in-header.png)

## <a name="offline-documentation"></a>脱机文档

如果要在脱机系统上查看 SQL Server 文档，有两个选项可实现此操作。 可以在 SQL Server 技术文档中的任何位置创建 PDF，也可以使用 [SQL Server 脱机帮助查看器](./sql-server-offline-documentation.md)下载脱机内容。 

如果要创建 PDF，请选择每个目录底部的“下载 PDF”链接  。


![下载 PDF](media/sql-server-docs-navigation-guide/download-pdf.png)

## <a name="toc-symbols"></a>TOC 符号 

目录 (TOC) 中以 `>` 结尾的条目表明，你会转到具有不同目录的技术文档。 

![toc 中的单个胡萝卜](media/sql-server-docs-navigation-guide/single-carrots-in-sql-docs-toc.png)

TOC 中包含 `>>` 的条目表明，你会离开 docs.microsoft.com。 

![TOC 导航标记](media/sql-server-docs-navigation-guide/double-carrots-in-sql-docs-toc.png)

如果转到这些页面之一，可以返回到 SQL Server 主技术页和目录，具体方法为选择每个目录顶部的“欢迎使用 SQL Server >”条目。 

![导航回到 SQL toc](media/sql-server-docs-navigation-guide/navigate-back-to-sql-toc.png)

## <a name="toc-search"></a>TOC 搜索 
在 docs.microsoft.com 上，可以使用顶部的筛选器搜索框来搜索目录中的内容： 

![使用筛选器框](media/sql-server-docs-navigation-guide/sql-docs-toc-filter.gif)

## <a name="version-filter"></a>版本筛选器
SQL Server 技术文档为几种受支持的 SQL Server 版本和风格提供内容。 功能可能因 SQL Server 的版本和风格而异，因此有时内容本身可能会有所不同。 

使用[版本筛选器](versioning-system-monikers-ui-sql-server.md)，可确保所看到的内容符合 SQL Server 的相应版本和风格： 

![SQL 文档版本筛选器](media/sql-server-docs-navigation-guide/sql-docs-version-filter.gif)

依次选择“所有 SQL”  \>“不隐藏任何内容”  ，可确保所有内容都可见，并且版本筛选器没有隐藏任何内容。 “不隐藏任何内容”  选项可能会在同一篇文章中显示与几个不同 SQL Server 版本相关的内容，这可能会令内容相互矛盾、不明确或产生混淆。 因此，[不建议将“不隐藏任何内容”  选项用于常规用途](versioning-system-monikers-ui-sql-server.md#anchor-allsql-hidenothing)。 

## <a name="breadcrumbs"></a>痕迹导航

痕迹导航可以在标题下方和目录上方找到，用于指示当前文章在目录中所处的位置。  这不仅有助于将上下文设置为所读取的内容类型，而且还使你可以在目录树中导航回来：

![SQL 文档痕迹导航](media/sql-server-docs-navigation-guide/sql-docs-bread-crumbs.gif)

## <a name="article-section-navigation"></a>文章节导航

使用右侧导航窗格可以快速导航到文章中的各节，还可以确定在文章中的位置。  

![右侧导航](media/sql-server-docs-navigation-guide/sql-docs-right-hand-navigation.gif)


## <a name="submit-docs-feedback"></a>提交文档反馈

如果在文章中发现错误内容，则可以向下滚动到页面底部并选择“内容反馈”  ，将反馈提交给 SQL 内容团队。

![Git Issue 内容反馈](media/sql-server-get-help/git-issues.png)

还可以在 [https://aka.ms/sqldocsfeedback](https://aka.ms/sqldocsfeedback) 上提交一般文档反馈和建议。 

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![编辑 SQL 文档](media/sql-server-docs-navigation-guide/edit-sql-docs.gif)

## <a name="next-steps"></a>后续步骤

- 开始使用 [SQL Server 技术文档](index.yml)。
- 有关针对 SQL Server 提交反馈或获取帮助的详细信息，请参阅[获取帮助](sql-server-get-help.md)页面。 
- 若要快速访问所有快速入门和教程，请参阅 [SQL 教育资源](../sql-server/educational-sql-resources.yml)。