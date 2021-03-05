---
title: Azure Synapse Pathway 预览版概述
description: Azure Synapse Pathway 是一种将数据仓库迁移到 Azure Synapse Analytics 的工具。
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: Azure Synapse Pathway
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: d7289d2bfe099dad7bbc91ccd5060797f7aad997
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873070"
---
# <a name="azure-synapse-pathway-preview"></a>Azure Synapse Pathway 预览版
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

当客户考虑实现其数据仓库系统的现代化时，他们面临的一个关键障碍是转换其 SQL 代码。 现有的代码是针对当前系统编写和优化的，但需要针对要迁移到的新系统进行优化。

世界各地的组织都希望实现其分析平台的现代化，以同时享受总拥有成本 (TCO) 和创新权益。 然而，客户已针对他们的数据仓库投入了数千个工作小时、数百万美元并编写了数万行代码。
 
若要转换这一重要的 SQL 代码，客户必须手动重写现有的 SQL 代码，或投入大量预算进行外部实践以重写或转换他们的代码。

> [!IMPORTANT]
> Azure Synapse Pathway 当前提供公共预览版。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
 
> 有关详细信息，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。 

Azure Synapse Pathway 通过自动执行现有数据仓库的代码转换，帮助你升级到新式数据仓库平台。 它是一个免费、直观且易于使用的工具，可自动执行代码转换，从而更快地迁移到 Azure Synapse Analytics。

 ![Azure Synapse Pathway 概述。](./media/pathway-overview/synapse-pathway-overview.png) 

Synapse Pathway 将数据定义语言 (DDL) 和数据操作语言 (DML) 语句转换为与 Azure Synapse SQL 兼容的 T-SQL 兼容语言。

## <a name="behind-the-scenes"></a>幕后

若要详细了解 Azure Synapse Pathway，请参阅[幕后](synapse-pathway-behind-the-scenes.md)，了解有关 Azure Synapse Pathway 如何工作的分步过程。

## <a name="get-azure-synapse-pathway"></a>获取 Azure Synapse Pathway

若要安装 Synapse Pathway，请查看 [Azure Synapse Pathway 下载](synapse-pathway-download.md)，了解先决条件和下载最新版本的链接。

## <a name="supported-sources"></a>受支持的源

Azure Synapse Pathway 支持以下源的数据库、架构和表的代码转换：
- **IBM Netezza** 
- **Microsoft SQL Server**
- **Snowflake**

## <a name="frequently-asked-questions"></a>常见问题

有关 Azure Synapse Pathway 的其他信息，请查看[常见问题解答页面](pathway-faq.md)

## <a name="next-steps"></a>后续步骤

- [通过 Azure Synapse Pathway 运行首个转换](synapse-pathway-assessment.md)
- 公告博客 - [宣布推出 Azure Synapse Pathway：加速数据仓库迁移 - Microsoft 技术社区](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/announcing-azure-synapse-pathway-turbocharge-your-data-warehouse/ba-p/2176630)


