---
description: FilterGroupEnum
title: FilterGroupEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FilterGroupEnum
helpviewer_keywords:
- FilterGroupEnum enumeration [ADO]
ms.assetid: b22e725e-84bd-4286-a070-290c278c3783
author: rothja
ms.author: jroth
ms.openlocfilehash: 27b8b217287f3c623fc626de1cb46dcc2eed5394
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024593"
---
# <a name="filtergroupenum"></a>FilterGroupEnum
指定要从 [记录集中](./recordset-object-ado.md)筛选的一组记录。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adFilterAffectedRecords**|2|用于仅查看受上次 [删除](./delete-method-ado-recordset.md)、重新 [同步](./resync-method.md)、 [UpdateBatch](./updatebatch-method.md)或 [CancelBatch](./cancelbatch-method-ado.md) 调用影响的记录的筛选器。|  
|**adFilterConflictingRecords**|5|用于查看上次批处理更新失败的记录的筛选器。|  
|**adFilterFetchedRecords**|3|用于查看当前缓存中的记录的筛选器，即，最后一次调用的结果是从数据库中检索记录。|  
|**adFilterNone**|0|删除当前筛选器并还原所有记录以供查看。|  
|**adFilterPendingRecords**|1|用于仅查看已更改但尚未发送到服务器的记录的筛选器。 仅适用于批处理更新模式。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.FilterGroup.AFFECTEDRECORDS|  
|AdoEnums.FilterGroup.CONFLICTINGRECORDS|  
|AdoEnums.FilterGroup.FETCHEDRECORDS|  
|AdoEnums. FilterGroup|  
|AdoEnums.FilterGroup.PENDINGRECORDS|  
  
## <a name="applies-to"></a>应用于  
 [Filter 属性](./filter-property.md)