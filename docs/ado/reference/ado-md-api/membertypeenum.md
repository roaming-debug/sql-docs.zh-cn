---
description: MemberTypeEnum
title: MemberTypeEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- MemberTypeEnum
helpviewer_keywords:
- MemberTypeEnum enumeration [ADO MD]
ms.assetid: 5d8132c0-7ca2-4f86-8336-1b34213869ad
author: rothja
ms.author: jroth
ms.openlocfilehash: 8486d7c9a34188c45a6ae633ab91d99715d30a1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169761"
---
# <a name="membertypeenum"></a>MemberTypeEnum
指定[成员](./member-object-ado-md.md)对象的[Type](./type-property-ado-md.md)属性的设置。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adMemberAll**|4|指示 **成员** 对象表示级别的所有成员。|  
|**adMemberFormula**|3|指示使用公式表达式计算 **成员** 对象。|  
|**adMemberMeasure**|2|指示 **成员** 对象属于度量值维度，并表示定量属性。|  
|**adMemberRegular**|1|默认。 指示 **成员** 对象表示业务实体的实例。|  
|**adMemberUnknown**|0|无法确定成员的类型。|