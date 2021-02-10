---
description: FieldStatusEnum
title: FieldStatusEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldStatusEnum
helpviewer_keywords:
- FieldStatusEnum enumeration [ADO]
ms.assetid: e06da1e2-303f-41b2-a3b0-61e233da152c
author: rothja
ms.author: jroth
ms.openlocfilehash: eab778d35ac9d5f1b5cefa4fdd7001a4772dc56d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035067"
---
# <a name="fieldstatusenum"></a>FieldStatusEnum
指定[字段对象](./field-object.md)的[状态](./status-property-ado-field.md)。  
  
 **AdFieldPending \*** 值指示导致状态设置的操作，并且可以与其他状态值组合。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adFieldAlreadyExists**|26|指示指定字段已存在。|  
|**adFieldBadStatus**|12|指示从 ADO 发送到 OLE DB 提供程序的状态值无效。 可能的原因包括 OLE DB 1.0 或1.1 提供程序，或者 [值](./value-property-ado.md) 和 [状态](./status-property-ado-field.md)的组合不正确。|  
|**adFieldCannotComplete**|20|指示由 [源](./source-property-ado-record.md) 指定的 URL 的服务器无法完成操作。|  
|**adFieldCannotDeleteSource**|23|指示在移动操作过程中，树或子树已移动到新位置，但无法删除该源。|  
|**adFieldCantConvertValue**|2|指示无法检索或存储字段，而不会丢失数据。|  
|**adFieldCantCreate**|7|指示未能添加字段，因为提供程序超出了 (的限制，例如允许) 的字段数。|  
|**adFieldDataOverflow**|6|指示从提供程序返回的数据溢出了字段的数据类型。|  
|**adFieldDefault**|13|指示在设置数据时使用了字段的默认值。|  
|**adFieldDoesNotExist**|16|指示指定的字段不存在。|  
|**adFieldIgnore**|15|指示在源中设置数据值时跳过此字段。 提供程序未设置值。|  
|**adFieldIntegrityViolation**|10|指示无法修改字段，因为它是计算实体或派生实体。|  
|**adFieldInvalidURL**|17|指示数据源 URL 包含无效字符。|  
|**adFieldIsNull**|3|指示提供程序返回 VT_NULL 类型的变量值，并且该字段不为空。|  
|**adFieldOK**|0|默认。 指示已成功添加或删除字段。|  
|**adFieldOutOfSpace**|22|指示提供程序无法获得足够的存储空间来完成移动或复制操作。|  
|**adFieldPendingChange**|0x40000|指示字段已被删除，然后重新添加，可能是使用不同的数据类型，或者之前状态为 **adFieldOK** 的字段的值已更改。 在调用[Update](./update-method.md)方法之后，字段的最终形式将修改[字段](./fields-collection-ado.md)集合。|  
|**adFieldPendingDelete**|0x20000|指示 **删除** 操作导致设置状态。 在调用 **Update** 方法之后，字段已标记为要从 **字段** 集合中删除。|  
|**adFieldPendingInsert**|0x10000|指示 **追加** 操作导致设置状态。 在调用 **Update** 方法之后，该 **字段** 已标记为要添加到 **字段** 集合。|  
|**adFieldPendingUnknown**|0x80000|指示提供程序无法确定哪个操作导致设置字段状态。|  
|**adFieldPendingUnknownDelete**|0x100000|指示提供程序无法确定哪个操作导致设置字段状态，并且在调用 **Update** 方法之后将从 **字段** 集合中删除该字段。|  
|**adFieldPermissionDenied**|9|指示无法修改字段，因为该字段定义为只读。|  
|**adFieldReadOnly**|24|指示数据源中的字段定义为只读。|  
|**adFieldResourceExists**|19|指示提供程序无法执行该操作，因为目标 URL 中已存在一个对象，但该对象不能覆盖该对象。|  
|**adFieldResourceLocked**|18|指示提供程序无法执行该操作，因为数据源被一个或多个其他应用程序或进程锁定。|  
|**adFieldResourceOutOfScope**|25|指示源或目标 URL 超出了当前记录的范围。|  
|**adFieldSchemaViolation**|11|指示值违反了字段的数据源架构约束。|  
|**adFieldSignMismatch**|5|指示提供程序返回的数据值已签名，但 ADO 字段值的数据类型未签名。|  
|**adFieldTruncated**|4|指示在从数据源读取时，长度可变的数据被截断。|  
|**adFieldUnavailable**|8|指示在从数据源读取时，提供程序无法确定该值。 例如，行刚创建，该列的默认值不可用，并且尚未指定新值。|  
|**adFieldVolumeNotFound**|21|指示提供程序找不到 URL 所指示的存储卷。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 这些常量没有 ADO/WFC 等效项。  
  
## <a name="applies-to"></a>应用于  
 [Status 属性（ADO 字段）](./status-property-ado-field.md)