---
title: 通过 Azure Synapse Pathway 预览版保存和加载评估
description: 通过 Azure Synapse Pathway 预览版保存和加载数据仓库评估
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: tools-other
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: fbaa518e2f2a5485f85fc7812291beb88896ac6e
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186203"
---
# <a name="save-and-load-assessments-with-azure-synapse-pathway-preview"></a>通过 Azure Synapse Pathway 预览版保存和加载评估
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

以下分步说明演示了如何使用 Azure Synapse Pathway 将数据仓库评估保存到文件并从文件中上传该评估。

在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
> * 将评估保存到文件
> * 从文件加载评估

## <a name="prerequisites"></a>先决条件

若要完成本教程，请确保已安装 [Azure Synapse Pathway](synapse-pathway-download.md)。若要详细了解该工具，请参阅 [Azure Synapse Pathway 概述](azure-synapse-pathway-overview.md)。

## <a name="saving-an-assessment-to-a-file"></a>将评估保存到文件

1. 运行转换后，你应会看到概述了代码转换的报告 ![Azure Synapse Pathway 评估报告概述。](./media/tutorial-save-load-assessment/report-overview.png)
1. 选择“保存评估”按钮，指定文件的名称，然后选择“保存” 。
![Azure Synapse Pathway 评估。](./media/tutorial-save-load-assessment/save-assessment.png)

1. 一个 .asmprj 文件随即在指定的目标处创建。

## <a name="loading-an-assessment-from-a-file"></a>从文件加载评估

1. 若要打开相同的评估，请选择“加载评估”，并提供 .asmprj 文件名 ![Azure Synapse Pathway 浏览到评估位置。](./media/tutorial-save-load-assessment/browse-location.png)

1. 源、输入和输出文件夹将基于所选的评估进行填充。
![显示转换类型、输入目录和输出目录的 Azure Synapse Pathway 评估配置。](./media/tutorial-save-load-assessment/load-assessment.png)
1. 选择“转换”，再次重新运行代码转换

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [报表生成](report-generation.md)
