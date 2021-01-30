---
description: FieldEnum
title: FieldEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldEnum
helpviewer_keywords:
- FieldEnum enumeration [ADO]
ms.assetid: be4eda13-d4e4-4d6b-bb0d-3310b0a96fc2
author: rothja
ms.author: jroth
ms.openlocfilehash: 7a6975e606544ddc05a838fa0238d95132c13bf1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171078"
---
# <a name="fieldenum"></a>FieldEnum
指定 [记录](./record-object-ado.md) 对象的 [fields](./fields-collection-ado.md) 集合中引用的特殊字段。  
  
## <a name="remarks"></a>备注  
 这些常量提供了访问与 **记录** 关联的特殊字段的 "快捷方式"。 从 "**字段**" 集合中检索 [字段](./field-object.md)对象，然后获取其内容和 **字段** 对象的 [Value](./value-property-ado.md)属性。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adDefaultStream**|-1|引用包含与 **记录** 关联的默认 [流](./stream-object-ado.md)对象的字段。|  
|**adRecordURL**|-2|引用包含当前 **记录** 的绝对 URL 字符串的字段。|