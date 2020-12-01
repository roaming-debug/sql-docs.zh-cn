---
description: 推断维度成员（渐变维度向导）
title: 推断维度成员（渐变维度向导）| Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.loaddimwizard.inferrdim.f1
ms.assetid: 809e395f-2e10-48ff-8860-56403f130628
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4066795817035e6ac149de7fd86b2f4fb816f828
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425769"
---
# <a name="inferred-dimension-members-slowly-changing-dimension-wizard"></a>推断维度成员（渐变维度向导）

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用 **“推断维度成员”** 对话框指定使用推断成员的选项。 当事实数据表引用尚未加载的维度成员时，存在推断成员。 当加载推断成员的数据后，可以更新现有记录，而不必创建一个新记录。  
  
 若要了解有关此向导的详细信息，请参阅 [Slowly Changing Dimension Transformation](../../../integration-services/data-flow/transformations/slowly-changing-dimension-transformation.md)。  
  
## <a name="options"></a>选项  
 **启用推断成员支持**  
 如果选择启用推断成员，必须选择以下两个选项之一。  
  
 **带更改类型的列均为空**  
 指定是否对新推断成员记录中所有带更改类型的列输入空值。  
  
 **使用布尔值列指示当前记录是否为推断成员**  
 指定是否使用一个现有的布尔值列来指示当前记录是不是推断成员。  
  
 **推断成员指示器**  
 如果如上所述选择了使用布尔值列来指示推断成员，请从该列表中选择此列。  
  
## <a name="see-also"></a>另请参阅  
 [使用渐变维度向导配置输出](../../../integration-services/data-flow/transformations/configure-outputs-using-the-slowly-changing-dimension-wizard.md)  
  
  
