---
description: 字段相关错误信息
title: Field-Related 错误信息 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- field-related errors [ADO]
- errors [ADO], field-related
ms.assetid: 5e7b1af4-996b-47c5-9161-c5575ad4fec9
author: rothja
ms.author: jroth
ms.openlocfilehash: cce74cf105aa7b38c2a3d7b157ef0fa17a1e8c1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037467"
---
# <a name="field-related-error-information"></a>字段相关错误信息
如果错误与字段直接相关（例如，如果缺少数据或字段对于字段是错误的类型），则可以通过检查 **字段** 对象的 **Status** 属性来检索有关问题原因的详细信息。 此属性已得到增强，可提供有关此问题的具体信息。 例如，当调用 **UpdateBatch** 失败时，可以通过检查每个受影响记录中的 **字段** 的 "**状态**" 属性来确定问题的原因。 属性将包含 **FieldStatusEnum** 常量中的值之一。 下表包括在发生错误时特别感兴趣的那些值。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adFieldCantConvertValue**|2|指示无法检索或存储字段，而不会丢失数据。|  
|**adFieldDataOverflow**|6|指示从提供程序返回的数据溢出了字段的数据类型。|  
|**adFieldDefault**|13|指示在设置数据时使用了字段的默认值。|  
|**adFieldIgnore**|15|指示在源中设置数据值时跳过此字段。 提供程序未设置值。|  
|**adFieldIntegrityViolation**|10|指示无法修改字段，因为它是计算实体或派生实体。|  
|**adFieldIsNull**|3|指示提供程序返回了 null 值。|  
|**adFieldOutOfSpace**|22|指示提供程序无法获得足够的存储空间来完成移动或复制操作。|
