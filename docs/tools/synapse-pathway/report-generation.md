---
title: Azure Synapse Pathway 预览版报表生成
description: Azure Synapse Pathway 提供有关已转换脚本的综合报表。
author: anshul82-ms
ms.author: anrampal
ms.topic: tutorial
ms.prod: sql
ms.technology: tools-other
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: 26701c33f713a1b82e3bab604a03e7dca054a36b
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186319"
---
# <a name="report-generation-for-azure-synapse-pathway-preview"></a>Azure Synapse Pathway 预览版的报表生成
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Azure Synapse Pathway 提供已成功转换的脚本数的综合报表。 报表还会显示未转换的语句的错误数和警告数。

## <a name="generate-report"></a>生成报表

选择“转换”时，你可看到如下所示的报表：

![Azure Synapse Pathway 报表。](./media/report-generaration/report-overview.png)

## <a name="report-summary"></a>报表摘要

“转换成功”将显示已成功转换的脚本百分比。

![Azure Synapse Pathway。](./media/report-generaration/conversion-success.png)

“语句分布”部分详细说明了已转换的 DDL 和 DML 语句的数量，包括已创建的架构、表和数据库的数量。

![Azure Synapse 报表 1。](./media/report-generaration/statement-distribution.png)

迁移节省量根据已转换对象的数量和复杂性（简单、高等或中等）计算得出。 Synapse Pathway 假设每次对象转换需要 30 分钟的手动操作。

![Azure Synapse 报表 2。](./media/report-generaration/migration-savings.png)

## <a name="next-steps"></a>后续步骤

[下载 Azure Synapse Pathway](synapse-pathway-download.md)