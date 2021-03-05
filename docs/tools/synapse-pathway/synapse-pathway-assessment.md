---
title: Azure Synapse Pathway 预览版评估。
description: 使用 Azure Synapse Pathway 执行数据仓库代码转换
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: b76fecf9a8a7eafc84a1b9eebd746287dddf3af9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873060"
---
# <a name="tutorial-to-perform-your-first-code-translation-with-azure-synapse-pathway-preview"></a>使用 Azure Synapse Pathway 预览版执行首次代码转换的教程
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Azure Synapse Pathway 预览版引入了对将 Netezza、Snowflake 和 Microsoft SQL Server 中的架构、表、视图、函数等转换为 T-SQL 兼容代码的支持，这些代码可自动执行向 Azure Synapse Analytics 的迁移  。

有关详细信息，请参阅 [Azure Synapse Pathway 预览版概述](azure-synapse-pathway-overview)。

在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
> * 运行从现有数据仓库中的 SQL 脚本到 Azure Synpase SQL 的 T-SQL 脚本的首次转换 
> * 选择一个可用的源
> * 查看有关未转换对象的错误和警告

## <a name="prerequisites"></a>先决条件

若要完成本教程，请确保已安装 [Azure Synapse Pathway](synapse-pathway-download.md)。 如果需要简介，请参阅 [Azure Synapse Pathway 概述](azure-synapse-pathway-overview.md)。

## <a name="run-the-translation"></a>运行转换

1. 启动 Azure Synapse Pathway MSI。 

1. 选择一个可用的源，即将添加的源呈灰色显示。
1. 在输入目录文件夹中，选择“浏览”，然后将工具指向需要转换的 DDL 和 DML 脚本的文件夹位置 。

    > [!Note]
    > 只有扩展名为 .sql 的文件才能作为输入源提供。 如果用户提供 .txt 文件形式的 DDL 和 DML 脚本，则工具将不执行任何转换。

1. 将 Netezza 代码转换到 Azure Synapse Analytics 时，请在“转换类型”下拉菜单中选择“IBM Netezza”。
  ![Azure Synapse 评估输入。](./media/perform-assessment/assessment-input.png)

1. 若要选择输出目录，请选择“浏览”，指定要生成输出的位置。
 ![Azure Synapse 输出目录。](./media/perform-assessment/output-directory.png)

1. 选择“转换”，开始转换

## <a name="view-results"></a>查看结果

1. 评估的持续时间取决于添加的数据库数和每个数据库的架构大小。 每个数据库的结果一旦生成，便会立即显示。
 ![Azure Synapse 评估报表。](./media/perform-assessment/assessment-report.png)

1. 若选择“查看结果”，你将转到上一步中指定的输出目录，并且将看到基于输入目录结构的转换后的脚本文件。

1. 它包含可轻松提交到 GitHub 存储库的项目结构。
  
1. 包含错误和警告列表的结果文件将被上传到相同的输出目录中。

## <a name="next-steps"></a>后续步骤

[了解如何保存和加载评估](tutorial-save-load-assessment.md)
