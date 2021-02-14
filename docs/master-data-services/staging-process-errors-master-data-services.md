---
title: 临时过程错误
description: 本文介绍在 Master Data Services 中的临时过程中添加到所有已处理记录的错误代码。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- staging process [Master Data Services], error messages
ms.assetid: 0d9be0dd-638f-4dd4-92b2-253fda655455
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 84a5a66cbb8bacb3e1df896fedfa3a051e2f4dbb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336265"
---
# <a name="staging-process-errors-master-data-services"></a>临时过程错误 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  当临时过程完成后，临时表中的所有已处理记录在 ErrorCode 列中都对应一个值。 下表中列出了这些值。  
  
|代码|错误|发生条件/详细信息|适用于表|  
|----------|-----------|--------------------------|----------------------|  
|210001|临时表中多次出现同一成员代码。|您的临时批次多次包含同一成员代码。 既不创建也不更新成员。|叶<br /><br /> 合并<br /><br /> 关系|  
|210003|属性值引用不存在或非活动的成员。|暂存基于域的属性时，您必须使用代码而不是名称。 适用于 **ImportType0**、 **1** 和 **2**。|叶<br /><br /> 合并|  
|210006|该成员代码处于非活动状态。|**ImportType**  = **1** 您指定了一个不存在的成员代码。|叶<br /><br /> 合并<br /><br /> 关系|  
|210032|该层次结构名称缺失或无效。|找不到显式层次结构或 **HierarchyName** 值为空白。|合并<br /><br /> 关系|  
|210035|因为不存在代码生成业务规则，所以 **MemberCode** 是必需的。|创建或更新成员时，始终需要 **MemberCode** ，除非您使用的是自动代码生成。 有关详细信息，请参阅 [&#41;&#40;Master Data Services 自动创建代码 ](../master-data-services/automatic-code-creation-master-data-services.md)。|叶<br /><br /> 合并|  
|210036|因为存在代码生成业务规则，所以 **MemberCode** 不是必需的。|创建或更新成员时，如果您使用的是自动代码生成，则不需要 **MemberCode** 。 不过，您可以选择指定一个代码。 有关详细信息，请参阅 [&#41;&#40;Master Data Services 自动创建代码 ](../master-data-services/automatic-code-creation-master-data-services.md)。|叶<br /><br /> 合并|  
|210041|“ROOT”不是有效的成员代码。|**MemberCode** 值包含单词 "ROOT"。|叶<br /><br /> 合并<br /><br /> 关系|  
|210042|“MDMUNUSED”不是有效的成员代码。|**MemberCode** 值包含单词 "MDMUNUSED"。|叶<br /><br /> 合并<br /><br /> 关系|  
|210052|由于将 MemberCode 用作基于域的属性值，所以无法停用该代码。|当 **ImportType**  =  **3** 或 **4** 时，如果将成员用作其他成员的属性值，则过渡会失败。 使用 **ImportType5** 或 **6** 将值设置为 NULL，或者在运行暂存过程之前更改值。|叶<br /><br /> 合并|  
|300002|该成员代码无效。|关系：不存在任何父或子成员代码。<br /><br /> 叶或合并： **ImportType**  =  **3** 或 **4** 并且成员代码不存在。|叶<br /><br /> 合并<br /><br /> 关系|  
|300004|该成员代码已存在。|**ImportType**  = **1** ，你使用了实体中已存在的成员代码。|叶<br /><br /> 合并|  
|210011|**RelationshipType** 为 **1** 时， **ParentCode** 不能为叶成员。|确保 **ParentCode** 值是合并成员代码。|关系|  
|210015|对于层次结构和批次，该成员代码在临时表中多次出现。|对于显式层次结构，您在同一批次中多次指定了同一成员的位置。|关系|  
|210016|未能创建该关系，因为它将引起循环引用。|当您尝试将子级分配为父级时出现此错误。|关系|  
|210046|该成员不能为“根”节点的同级。|当 **RelationshipType**  =  **2** (同级) 并且 **ParentCode** 或 **ChildCode** 为 **Root** 时，会发生这种情况。 成员不能与根节点处于相同级别；它们只能是子节点。|关系|  
|210047|该成员不能为“未使用”节点的同级。|当 **RelationshipType**  =  **2** (同级) 并且 **未使用** **ParentCode** 或 **ChildCode** 时，就会发生这种情况。 成员只能“未使用”节点的子级。|关系|  
|210048|**ParentCode** 和 **ChildCode** 不能相同。|**ParentCode** 值与 **ChildCode** 值相同。 这些值必须不同。|关系|  
  
## <a name="see-also"></a>另请参阅  
 [查看在暂存 &#40;Master Data Services 中发生的错误&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)   
 [概述：导入表中数据 (Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md)  
  
  
