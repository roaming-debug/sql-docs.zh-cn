---
description: RecordTypeEnum
title: RecordTypeEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordTypeEnum
helpviewer_keywords:
- RecordTypeEnum enumeration [ADO]
ms.assetid: f557e537-015d-4ba7-8a41-a6f00b366a91
author: rothja
ms.author: jroth
ms.openlocfilehash: 8ef671384a552f240930e24a073f979f6e84da8d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166676"
---
# <a name="recordtypeenum"></a>RecordTypeEnum
指定 [记录](./record-object-ado.md) 对象的类型。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adSimpleRecord**|0|指示 (不包含) 子节点的 *简单* 记录。|  
|**adCollectionRecord**|1|指示 (包含) 子节点的 *集合* 记录。|  
|**adRecordUnknown**|-1|指示此 **记录** 的类型是未知的。|  
|**adStructDoc**|2|指示表示 COM 结构化文档的一种特殊类型的 *集合* 记录。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 这些常量没有 ADO/WFC 等效项。  
  
## <a name="applies-to"></a>应用于  
 [RecordType 属性 (ADO)](./recordtype-property-ado.md)