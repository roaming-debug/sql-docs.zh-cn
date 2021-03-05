---
title: Azure Synapse Pathway 预览版常见问题解答
description: Azure Synapse Pathway 常见问题解答
author: anshul82-ms
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: Azure Synapse Pathway
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: 345346d2161800810ba3d07667b831107f70b215
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873026"
---
# <a name="azure-synapse-pathway-preview"></a>Azure Synapse Pathway 预览版
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

本指南解答有关 Azure Synapse Pathway 预览版的最常见问题。

## <a name="general"></a>常规

### <a name="q-what-is-azure-synapse-pathway"></a>问： Azure Synapse Pathway 是什么？

A. Azure Synapse Pathway 是代码转换工具，通过自动执行现有数据仓库的代码转化，使其与基于 T-SQL 的 Azure Synapse SQL 兼容，帮助你升级到新式数据仓库平台。

### <a name="q-how-can-i-download-azure-synapse-pathway"></a>Q. 如何下载 Azure Synapse Pathway？

A. 可以从 [Microsoft 下载中心](https://aka.ms/synapse-pathway-download)下载 Synapse Pathway。

### <a name="q-how-much-does-azure-synapse-pathway-cost"></a>Q. Azure Synapse Pathway 需花费多少费用？

A. 使用 Synapse Pathway 下载和运行代码转换不会产生任何相关费用。

### <a name="q-what-sourcetarget-pairs-does-azure-synapse-pathway-currently-support"></a>Q. Azure Synapse Pathway 当前支持哪些源/目标对？

A. 在此预览版本的 Synapse Pathway 中，以下数据仓库作为源提供：
- Microsoft SQL Server
- Snowflake
- Netezza

### <a name="q-what-is-included-as-part-of-the-code-conversion"></a>Q. 代码转换包括哪些内容？

A. Synapse Pathway 支持对表、架构、视图和存储过程进行代码转换。

### <a name="q-can-it-also-scan-my-environment-and-provide-an-assessment-report-of-all-the-objects-that-need-to-be-convertedtranslated"></a>Q. 它是否还可以扫描环境，并提供需要转换的所有对象的评估报告？

A. 在此预览版本的 Synapse Pathway 中，必须提供指向需要转换的 DDL/DML 脚本的链接。 Synapse Pathway 不会扫描当前环境以确定需要转换的对象。

### <a name="q-what-information-does-azure-synapse-pathway-capture-about-my-current-data-warehouse-instance"></a>Q. Azure Synapse Pathway 捕获有关当前数据仓库实例的哪些信息？

A. 由于你可在本地环境中运行 Synapse Pathway，因此它不会捕获除你的姓名和组织（从 Microsoft 下载中心下载 MSI 时，需要这些信息）之外的任何数据。

### <a name="q-where-can-i-raise-issues-encountered-in-azure-synapse-pathway"></a>Q. 可以在何处提出在 Azure Synapse Pathway 中遇到的问题？

A. Azure Synapse Pathway 当前提供预览版。   通过 Microsoft 支持渠道提供对 Synapse Pathway 的支持。 可在 Azure 门户或标准（通常是本地支持）门户中提交票证。

> [!NOTE] 与任何其他 Azure 服务一样，所有预览版服务都有资格获得支持，而无需实施 SLA。

<!-- ### Troubleshooting and optimization

#### Q. Why do I see slow performance while running the code conversion?

#### Q. Translation of errors or unexpected results? -->

## <a name="next-steps"></a>后续步骤

[下载 Azure Synapse Pathway](synapse-pathway-download.md)