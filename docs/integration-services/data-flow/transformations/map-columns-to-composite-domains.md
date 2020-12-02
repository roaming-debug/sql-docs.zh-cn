---
description: 将列映射到复合域
title: 将列映射到复合域 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: d9422412-8a3d-45ae-af7f-072c902a09ba
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cc1bddc579ccce64b4068fd2a5235481fc7b7629
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193197"
---
# <a name="map-columns-to-composite-domains"></a>将列映射到复合域

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  一个复合域包含两个或多个单一域。 您可以将多个列映射到域，也可以将具有分隔值的单个列映射到域。  
  
 当您具有多个列时，必须将一个列映射到复合域中的每个单一域，以便应用复合域规则来执行数据清理。 在数据质量客户端中选择复合域中包含的单一域。 有关详细信息，请参阅 [创建复合域](../../../data-quality-services/create-a-composite-domain.md)。  
  
 在您具有含分隔值的单个列时，必须将该单个列映射到复合域。 这些值的出现顺序必须与单一域在复合域中的出现顺序相同。 数据源中的分隔符必须匹配用于解析复合域值的分隔符。 在数据质量客户端中为复合域选择分隔符，以及设置其他属性。 有关详细信息，请参阅 [创建复合域](../../../data-quality-services/create-a-composite-domain.md)。  
  
### <a name="to-map-multiple-columns-to-a-composite-domain"></a>将多个列映射到一个复合域  
  
1.  右键单击 DQS 清理转换，然后单击“编辑”。  
  
2.  在 **“连接管理器”** 选项卡上，确认复合域显示在可用域列表中。  
  
3.  在 **“映射”** 选项卡上，在 **“可用输入列”** 区域中选择列。  
  
4.  对于 **“输入列”** 字段中列出的每个列，在 **“域”** 字段中选择一个单一域。 仅选择处于复合域中的单一域。  
  
5.  根据需要，修改在 **“源别名”**、 **“输出别名”** 和 **“状态别名”** 字段中出现的名称。  
  
6.  根据需要，在 **“高级”** 选项卡上设置属性。有关属性的详细信息，请参阅 [DQS Cleansing Transformation Editor Dialog Box](./dqs-cleansing-transformation.md)。  
  
### <a name="to-map-a-column-with-delimited-values-to-a-composite-domain"></a>将具有分隔值的列映射到复合域  
  
1.  右键单击 DQS 清理转换，然后单击“编辑”。  
  
2.  在 **“连接管理器”** 选项卡上，确认复合域显示在可用域列表中。  
  
3.  在 **“映射”** 选项卡上，在 **“可用输入列”** 区域中选择列。  
  
4.  对于 **“输入列”** 字段中列出的列，在 **“域”** 字段中选择复合域。  
  
5.  根据需要，修改在 **“源别名”**、 **“输出别名”** 和 **“状态别名”** 字段中出现的名称。  
  
6.  根据需要，在 **“高级”** 选项卡上设置属性。有关属性的详细信息，请参阅 [DQS Cleansing Transformation Editor Dialog Box](./dqs-cleansing-transformation.md)。  
  
## <a name="see-also"></a>另请参阅  
 [DQS 清理转换](../../../integration-services/data-flow/transformations/dqs-cleansing-transformation.md)  
  
