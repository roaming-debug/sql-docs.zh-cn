---
title: 数据迁移助手中保存和加载评估
description: 了解如何使用数据迁移助手来保存和加载评估。
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 9f427c7d706e3665cc5cbca092bfbc99a333a1ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030625"
---
# <a name="save-and-load-assessments-with-data-migration-assistant"></a>数据迁移助手中保存和加载评估

下面的分步说明将帮助你使用数据迁移助手的5.0 或更高版本将数据库评估保存到文件，然后从文件中加载评估。

> [!NOTE]
> 除了加载使用最新的 DMA 版本保存的评估外，用户还可以利用此功能将从以前版本的数据迁移助手中导出为 json 文件的评估加载到5.0 及更高版本中。

## <a name="saving-an-assessment-to-a-file"></a>将评估保存到文件

1. 使用数据迁移助手运行评估后，请选择 " **保存评估**"。

   ![打开 "保存评估" 对话框](../dma/media/dma-save-load-assessments/dma-open-save-dialog.png)

   标准 **保存 ...** 对话框。

   > [!NOTE]
   > 有关如何在数据迁移助手中运行评估的详细信息，请参阅文章 [使用数据迁移助手执行 SQL Server 迁移评估](../dma/dma-assesssqlonprem.md)。

2. 指定文件的名称，然后选择 " **保存**"。

   ![命名和保存评估文件](../dma/media/dma-save-load-assessments/dma-name-save-assessment.png)

## <a name="loading-an-assessment-saved-to-a-file"></a>加载保存到文件的评估

1. 若要加载以前保存到文件的评估，请从数据迁移助手开始，然后在 " **所有评估** " 选项卡上，选择 " **负载评估**"。

   ![打开 "负载评估" 对话框](../dma/media/dma-save-load-assessments/dma-open-load-dialog.png)

2. 导航到要加载的已保存评估文件，选择该文件，然后选择 " **打开**"。

   ![打开评估文件](../dma/media/dma-save-load-assessments/dma-open-assessment.png)

   " **所有评估** " 选项卡上会显示已加载评估的条目。

   ![显示评估条目](../dma/media/dma-save-load-assessments/dma-display-assessment-entry.png)

3. 选择 "评估" 条目，然后选择 " **打开评估**"。

   ![打开评估详细信息](../dma/media/dma-save-load-assessments/dma-open-assessment-detail.png)

   现在数据迁移助手显示之前运行的评估的详细信息。

   ![显示评估详细信息](../dma/media/dma-save-load-assessments/dma-display-assessment-detail.png)
