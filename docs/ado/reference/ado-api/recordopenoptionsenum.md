---
description: RecordOpenOptionsEnum
title: RecordOpenOptionsEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordOpenOptionsEnum
helpviewer_keywords:
- RecordOpenOptionsEnum enumeration [ADO]
ms.assetid: 9028aba4-90fc-4dfc-88e4-fa8a7b6fedee
author: rothja
ms.author: jroth
ms.openlocfilehash: 087b8bc6540fdfb3e504273ed2e02b71163088fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051818"
---
# <a name="recordopenoptionsenum"></a>RecordOpenOptionsEnum
指定用于打开 [记录](./record-object-ado.md)的选项。 可以使用或组合这些值。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adDelayFetchFields**|0x8000|向提供程序指出，不需要最初检索与 **记录** 关联的字段，但在第一次尝试访问该字段时可进行检索。 缺少此标志时，默认行为是检索所有 **记录** 对象字段。|  
|**adDelayFetchStream**|0x4000|向提供程序指示，不需要最初检索与 **记录** 关联的默认流。 缺少此标志时，默认行为是检索与 **Record** 对象关联的默认流。|  
|**adOpenAsync**|0x1000|指示在异步模式下打开 **记录** 对象。|  
|**adOpenExecuteCommand**|0x10000|指示源字符串包含应执行的命令文本。 此值等效于 Recordset 上的 **adCmdText** 选项。 **打开**。|  
|**adOpenRecordUnspecified**|-1|默认。 指示未指定任何选项。|  
|**adOpenOutput**|0x800000|指示如果源指向包含可执行脚本的节点 (如。ASP page) 中，打开的 **记录** 将包含已执行脚本的结果。 此值仅对非集合记录有效。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 这些常量没有 ADO/WFC 等效项。  
  
## <a name="applies-to"></a>应用于  
 [Open 方法（ADO 记录）](./open-method-ado-record.md)