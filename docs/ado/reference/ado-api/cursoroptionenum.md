---
description: CursorOptionEnum
title: CursorOptionEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CursorOptionEnum
helpviewer_keywords:
- CursorOptionEnum enumeration [ADO]
ms.assetid: 4e10cda7-ce81-4466-94c2-844d38191cf1
author: rothja
ms.author: jroth
ms.openlocfilehash: 53916dc6392bc2889b694233c80dae809e254240
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025971"
---
# <a name="cursoroptionenum"></a>CursorOptionEnum
指定 [支持](./supports-method.md) 方法应对哪些功能进行测试。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adAddNew**|0x1000400|支持 [AddNew](./addnew-method-ado.md) 方法添加新记录。|  
|**adApproxPosition**|0x4000|支持 [AbsolutePosition](./absoluteposition-property-ado.md) 和 [AbsolutePage](./absolutepage-property-ado.md) 属性。|  
|**adBookmark**|0x2000|支持 [Bookmark](./bookmark-property-ado.md) 属性以获取对特定记录的访问权限。|  
|**adDelete**|0x1000800|支持 [delete](./delete-method-ado-recordset.md) 方法以删除记录。|  
|**adFind**|0x80000|支持 [Find](./find-method-ado.md) 方法查找 [记录集中](./recordset-object-ado.md)的行。|  
|**adHoldRecords**|0x100|检索更多记录或更改下一个位置，而不提交所有挂起的更改。|  
|**a**|0x100000|支持 [index](./index-property.md) 属性为索引命名。|  
|**adMovePrevious**|0x200|支持 [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) 和 [MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) 方法，并 [移动](./move-method-ado.md) 或 [GetRows](./getrows-method-ado.md) 方法以移动当前记录位置，而无需书签。|  
|**adNotify**|0x40000|指示基础数据访问接口支持通知 (确定是否) 支持 **记录集** 事件。|  
|**adResync**|0x20000|支持 "重新 [同步](./resync-method.md) " 方法，以通过在基础数据库中可见的数据来更新光标。|  
|**adSeek**|0x200000|支持 [Seek](./seek-method.md) 方法查找 **记录集中** 的行。|  
|**adUpdate**|0x1008000|支持 [更新](./update-method.md) 方法来修改现有数据。|  
|**adUpdateBatch**|0x10000|支持 ([UpdateBatch](./updatebatch-method.md) 和 [CancelBatch](./cancelbatch-method-ado.md) 方法进行批量更新，) 将更改组传输到提供程序。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums. CursorOption|  
|AdoEnums.CursorOption.APPROXPOSITION|  
|AdoEnums. CursorOption. 书签|  
|AdoEnums. CursorOption. DELETE|  
|AdoEnums. CursorOption. 查找|  
|AdoEnums.CursorOption.HOLDRECORDS|  
|AdoEnums. CursorOption|  
|AdoEnums. CursorOption. MOVEPREVIOUS|  
|AdoEnums. CursorOption. 通知|  
|AdoEnums. CursorOption|  
|AdoEnums. CursorOption|  
|AdoEnums. CursorOption. UPDATE|  
|AdoEnums. CursorOption|  
  
## <a name="applies-to"></a>应用于  
 [Supports 方法](./supports-method.md)