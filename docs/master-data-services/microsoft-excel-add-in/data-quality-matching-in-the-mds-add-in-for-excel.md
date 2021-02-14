---
description: 用于 Excel 的 MDS 外接程序中的数据质量匹配
title: 数据质量匹配
ms.custom: microsoft-excel-add-in, seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: be78d051-0d56-46d3-bb89-327e218dadd6
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 9654001f67eae4894d65bfbb8b6bda5bc6faaec5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272388"
---
# <a name="data-quality-matching-in-the-mds-add-in-for-excel"></a>用于 Excel 的 MDS 外接程序中的数据质量匹配

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  随着时间的推移，您将会想要向 MDS 存储库中添加更多的数据。 在添加数据前，将新数据与已在 MDS 中进行管理的数据进行比较可能会很有用，因为这样可以确保不会添加重复数据或不正确的数据。  
  
 MDS [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Data Quality Services (DQS) 功能来匹配相似的数据。 当您在该外接程序中使用此匹配功能时，相似的记录将组合在一起，并且显示表示结果精确性的得分。 有关 DQS 提供的匹配功能的详细信息，请参阅 [Data Matching](../../data-quality-services/data-matching.md)。  
  
## <a name="workflow-for-data-quality-matching"></a>用于数据质量匹配的工作流  
 在将 DQS 用于 MDS [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]时，使用以下工作流：  
  
1.  检索 MDS 管理的数据的列表并将其与未在 MDS 中管理的列表合并。 有关详细信息，请参阅 [合并数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/combine-data-mds-add-in-for-excel.md)。  
  
2.  使用 DQS 知识可以比较合并的列表中的数据。 有关详细信息，请参阅 [匹配相似数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/match-similar-data-mds-add-in-for-excel.md)。  
  
3.  若要查看与 DQS 发现的相似性有关的详细信息，请显示详细信息列。  
  
4.  浏览结果并且确定哪些数据应该添加到 MDS 存储库中以及哪些数据是重复的。  
  
5.  将新的和/或已更新的数据发布到 MDS 存储库中。  
  
## <a name="knowledge-bases"></a>知识库  
 在外接程序中提供的匹配结果基于 DQS 知识库。  
  
-   默认知识库（DQS 数据）是在安装 DQS 时创建的。 如果您选择使用默认知识库（没有将匹配策略添加到数据质量客户端的默认知识库中），则必须将工作表中的列映射到知识库中的域，然后将权重值分配给您选择的域。  
  
-   您可以使用数据质量客户端创建具有匹配策略的新的知识库，或者将一个匹配策略添加到默认知识库。 在此情况下，权重值由您已创建的匹配策略确定，并且您仅需将列映射到域。 有关详细信息，请参阅 [Create a Matching Policy](../../data-quality-services/create-a-matching-policy.md)。  
  
 有关知识库的详细信息，请参阅 [DQS Knowledge Bases and Domains](../../data-quality-services/dqs-knowledge-bases-and-domains.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|将外部数据与 MDS 管理的数据进行合并以便准备进行比较。|[合并数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/combine-data-mds-add-in-for-excel.md)|  
|使用 DQS 知识来查找您的数据中的相似之处。|[匹配相似数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/match-similar-data-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [概述：从 Excel 导入数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
-   [数据匹配](../../data-quality-services/data-matching.md)  
  
  
