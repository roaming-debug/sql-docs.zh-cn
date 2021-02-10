---
description: RecordStatusEnum
title: RecordStatusEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordStatusEnum
helpviewer_keywords:
- RecordStatusEnum enumeration [ADO]
ms.assetid: 506fdd70-4452-4e83-95d5-c94311988dfa
author: rothja
ms.author: jroth
ms.openlocfilehash: 4facc7f958c6fe8eab09c6889a54c63deb664ac0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051708"
---
# <a name="recordstatusenum"></a>RecordStatusEnum
指定有关批更新和其他批量操作的记录的 [状态](./status-property-ado-recordset.md) 。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adRecCanceled**|0x100|指示未保存记录，因为该操作已取消。|  
|**adRecCantRelease**|0x400|指示未保存新记录，因为现有记录已锁定。|  
|**adRecConcurrencyViolation**|0x800|指示未保存记录，因为正在使用乐观并发。|  
|**adRecDBDeleted**|0x40000|指示已从数据源中删除记录。|  
|**adRecDeleted**|0x4|指示记录已被删除。|  
|**adRecIntegrityViolation**|0x1000|指示未保存记录，因为用户违反了完整性约束。|  
|**adRecInvalid**|0x10|指示未保存记录，因为其书签无效。|  
|**adRecMaxChangesExceeded**|0x2000|指示未保存记录，因为挂起的更改太多。|  
|**adRecModified**|0x2|指示已修改记录。|  
|**adRecMultipleChanges**|0x40|指示未保存记录，因为它会影响多个记录。|  
|**adRecNew**|0x1|指示记录是新记录。|  
|**adRecObjectOpen**|0x4000|指示由于与打开的存储对象冲突而未保存记录。|  
|**adRecOK**|0|指示已成功更新记录。|  
|**adRecOutOfMemory**|0x8000|指示由于计算机内存不足而未保存记录。|  
|**adRecPendingChanges**|0x80|指示未保存记录，因为它引用了挂起的插入。|  
|**adRecPermissionDenied**|0x10000|指示未保存记录，因为用户没有足够的权限。|  
|**adRecSchemaViolation**|0x20000|指示未保存记录，因为它违反了基础数据库的结构。|  
|**adRecUnmodified**|0x8|指示未修改记录。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 AdoEnums.RecordStatus.  
  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.RecordStatus.CANCELED|  
|AdoEnums.RecordStatus.CANTRELEASE|  
|AdoEnums.RecordStatus.CONCURRENCYVIOLATION|  
|AdoEnums.RecordStatus.DBDELETED|  
|AdoEnums.RecordStatus.DELETED|  
|AdoEnums.RecordStatus.INTEGRITYVIOLATION|  
|AdoEnums.RecordStatus.INVALID|  
|AdoEnums.RecordStatus.MAXCHANGESEXCEEDED|  
|AdoEnums.RecordStatus.MODIFIED|  
|AdoEnums.RecordStatus.MULTIPLECHANGES|  
|AdoEnums.RecordStatus.NEW|  
|AdoEnums.RecordStatus.OBJECTOPEN|  
|AdoEnums.RecordStatus.OK|  
|AdoEnums.RecordStatus.OUTOFMEMORY|  
|AdoEnums.RecordStatus.PENDINGCHANGES|  
|AdoEnums.RecordStatus.PERMISSIONDENIED|  
|AdoEnums.RecordStatus.SCHEMAVIOLATION|  
|AdoEnums.RecordStatus.UNMODIFIED|  
  
## <a name="applies-to"></a>应用于  
 [Status 属性（ADO 记录集）](./status-property-ado-recordset.md)