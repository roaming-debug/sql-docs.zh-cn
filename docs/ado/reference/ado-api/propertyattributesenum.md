---
description: PropertyAttributesEnum
title: PropertyAttributesEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- PropertyAttributesEnum
helpviewer_keywords:
- PropertyAttributesEnum enumeration [ADO]
ms.assetid: 96a01955-a6b4-4cbf-9c73-52bcd1e9fb25
author: rothja
ms.author: jroth
ms.openlocfilehash: 2bca96a1f3e1e4f39254335d19853493768fe8c9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166793"
---
# <a name="propertyattributesenum"></a>PropertyAttributesEnum
指定 [属性](./property-object-ado.md) 对象的特性。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adPropNotSupported**|0|指示该提供程序不支持该属性。|  
|**adPropRequired**|1|指示在初始化数据源之前，用户必须为此属性指定一个值。|  
|**adPropOptional**|2|指示在初始化数据源之前，用户无需指定此属性的值。|  
|**adPropRead**|512|指示用户可以读取属性。|  
|**adPropWrite**|1024|指示用户可以设置属性。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums. PropertyAttributes. NOTSUPPORTED|  
|AdoEnums. PropertyAttributes. 必需|  
|AdoEnums. PropertyAttributes。可选|  
|AdoEnums. PropertyAttributes. READ|  
|AdoEnums. PropertyAttributes|  
  
## <a name="applies-to"></a>应用于  
 [Attributes 属性 (ADO)](./attributes-property-ado.md)