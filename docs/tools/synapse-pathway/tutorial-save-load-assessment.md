---
title: 通过 Azure Synapse Pathway 预览版保存和加载评估
description: 通过 Azure Synapse Pathway 预览版保存和加载数据仓库评估
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: c2893272c3bc2cebf21b85378565af8ba0383bc8
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873027"
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
 
1. 运行转换后，你应会看到概述了代码转换的报表 ![Azure Synapse Pathway 评估。](./media/save-load-assessment/report-overview.png)
3. 选择“保存评估”按钮，指定文件的名称，然后选择“保存” 。
![Azure Synapse Pathway 评估。](./media/save-load-assessment/save-assessment.png)

4. 随即一个 .asmprj 文件在指定的目标处创建

## <a name="loading-an-assessment-from-a-file"></a>从文件加载评估

1. 若要打开相同的评估，请选择“加载评估”，并提供 .asmprj 文件名 ![Azure Synapse Pathway 评估。](./media/save-load-assessment/browse-location.png)

1. 源、输入和输出文件夹将基于所选的评估进行填充。
![Azure Synapse Pathway 评估。](./media/save-load-assessment/load-assessment.png)
1. 选择“转换”，再次重新运行代码转换

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [报表生成](report-generation.md)
