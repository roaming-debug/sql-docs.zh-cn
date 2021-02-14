---
description: 如何参与编辑 SQL Server 文档
title: 如何参与编辑 SQL Server 文档 | Microsoft Docs
ms.date: 08/13/2018
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: a0f3f665421c04208af1123e659aadcfe513d407
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080298"
---
# <a name="how-to-contribute-to-sql-server-documentation"></a>如何参与编辑 SQL Server 文档

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

任何人都可以参与编辑 SQL Server 文档。 这包括更正拼写错误、提出更好的解释以及改进技术的准确性。 本文介绍如何开始参与内容编辑以及此过程的工作原理。

可通过以下两个主要工作流参与编辑：

|工作流|说明|
|---|---|
| [在浏览器中编辑](#githubui) | 适用于对任何文章进行少量快速编辑。 |
| [使用工具在本地编辑](#tools) | 适用于更复杂的编辑、涉及多篇文章的编辑，以及针对 docs.microsoft.com 的频繁编辑。 |

SQL 内容团队会对所有公开发表的内容进行验证，以确保技术准确性和一致性。 

## <a name="edit-in-your-browser"></a><a id="githubui"></a>在浏览器中编辑

可以在浏览器中对 SQL Server 内容进行简单编辑，然后将其提交到 Microsoft。 有关详细信息, 请参阅 [Microsoft Docs 参与者指南概述](/contribute/#quick-edits-to-existing-documents)。 

以下步骤概述了这一过程： 

1. 在要进行反馈的页面上，选择右上角的“编辑”链接。
1. 在下一页中，选择右上角的铅笔图标。
1. 在下一页上的“编辑文件”文本窗口中，对要更改的文本直接进行编辑。
    如果需要有关对新文本或更改的文本设置格式的帮助，请参阅 [Markdown 速查表](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)。
1. 进行编辑之后，请在“提交更改”下：
    1. 在第一个文本框中，输入所进行的更改的简要说明。
    1. 在“添加一个可选的扩展说明”框中，提供对更改的简短解释。
1. 选择“建议文件更改”。
1. 在“比较更改”页面上，选择“创建拉取请求”。 
1. 在“打开拉取请求”页面上，选择“创建拉取请求”。 

下面的 GIF 演示了在浏览器中提交更改的完整过程：

![编辑 SQL 文档](media/sql-server-docs-navigation-guide/edit-sql-docs.gif)

## <a name="edit-locally-with-tools"></a><a id="tools"></a>使用工具在本地编辑

另一个编辑选项是创建 sql-docs 或 azure-docs 存储库的分支并将其克隆到本地计算机 。 然后可以使用 Markdown 编辑器和 git 客户端提交所做的更改。 此工作流适用于更复杂或涉及多个文件的编辑。 另外，它也适用于针对 docs.microsoft.com 的频繁参与者。

若要使用此方法参与编辑，请参阅以下文章：

- [创建 GitHub 帐户](/contribute/get-started-setup-github)
- [安装内容创作工具](/contribute/get-started-setup-tools)
- [设置本地 Git 存储库](/contribute/get-started-setup-local)
- [使用工具参与编辑](/contribute/how-to-write-workflows-major)

如果提交包含对文档的重大更改的拉取请求，则将在 GitHub 中收到一条注释，要求提交联机“贡献许可协议 (CLA)”。 必须先完成联机窗体，然后系统才会接受你的拉取请求。

## <a name="recognition"></a>识别

如果接受了你所做的更改，则系统会在文章顶部将你识别为参与者。

![内容编辑识别](./media/sql-server-docs-contribute/contribution-recognition.png)

## <a name="sql-docs-overview"></a>sql-docs 概述

本部分提供一些有关在 sql-docs 存储库中工作的其他指导。

> [!IMPORTANT]
> 本部分中的信息特定于 sql-docs。若要编辑 Azure 文档中的 SQL 文章，请参阅 [GitHub 上 azure-docs 存储库的自述文件](https://github.com/MicrosoftDocs/azure-docs/blob/master/README.md)。

[sql-docs](https://github.com/MicrosoftDocs/sql-docs) 存储库使用多个标准文件夹来组织内容。

| Folder | 说明 |
|---|---|
| [docs](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs) | 包含所有已发布的 SQL Server 内容。 子文件夹有逻辑地组织内容的不同区域。 |
| [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) | 包括包含文件。 这些文件是可以包含在一个或多个其他主题中的内容块。 |
| **./media** | 每个文件夹可以有一个用于文章图像的“媒体”子文件夹。 “媒体”文件夹又包含名称与图像出现的主题名称相同的子文件夹。 图像应为字母全部小写且没有空格的 .png 文件。 |
| **TOC.MD** | 目录文件。 每个子文件夹都可选择使用 TOC.MD 文件。 |

#### <a name="applies-to-includes"></a>“适用范围”包含

每篇 SQL Server 文章的标题后都包含“适用范围”包含文件。 这表示文章适用于哪些 SQL Server 区域或版本。

请查看以下从 appliesto-ss-asdb-asdw-pdw-md.md 包含文件中拉取的 Markdown 示例。

```Markdown
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
```

这将在文章顶部添加以下文本：

![“适用范围”文本](./media/sql-server-docs-contribute/applies-to.png)

要为文章查找正确的“适用范围”include 文件，请参考以下提示：

- 有关常用 include 的列表，请参阅 [SQL Server 版本和 applies-to include文件](applies-to-includes.md)。
- 查看包含相同功能或相关任务的其他文章。 如果编辑此文章，则可针对适用范围包含链接复制 Markdown（可取消编辑而不提交编辑）。
- 针对包含文本“适用范围”的文件搜索 [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) 目录。 可使用 github 中的“查找”按钮快速筛选。 单击文件，查看其呈现方式。
- 注意命名约定。 如果名称中包含 x，它们通常是占位符，表示缺少对服务的支持。 例如，appliesto-xx-xxxx-asdw-xxx-md.md 表示仅支持 Azure Synapse Analytics，因为仅拼写出了 asdw，而其他字段具有 x 内容 。
- 某些包含指定了版本号，如 tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md。 仅当知道通过特定版本的 SQL Server 引入了此功能时才使用这些包含。

## <a name="contributor-resources"></a>参与者资源

- [docs.microsoft.com 参与者指南](/contribute/)
- [Microsoft 风格指南](/teamblog/style-guide)
- [Markdown 基本信息](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/)

> [!TIP]
> 如果有产品反馈而不是文档反馈，[请在此处提供有关 SQL Server 产品的反馈](https://feedback.azure.com/forums/908035-sql-server)。

## <a name="next-steps"></a>后续步骤

浏览 GitHub 上的 [sql-docs 存储库](https://github.com/MicrosoftDocs/sql-docs)。

查找文章，提交更改，并帮助 SQL Server 社区。 

谢谢！