---
description: StreamWriteEnum
title: StreamWriteEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StreamWriteEnum
helpviewer_keywords:
- StreamWriteEnum enumeration [ADO]
ms.assetid: bdbf3405-a0bd-4f02-85d4-e3fe8da3f3f7
author: rothja
ms.author: jroth
ms.openlocfilehash: 5be021aab95fb9fa2c0571deba176f30836a7f4e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170112"
---
# <a name="streamwriteenum"></a>StreamWriteEnum
指定是否将行分隔符追加到写入 [流](./stream-object-ado.md) 对象的字符串。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adWriteChar**|0|默认。 将 *数据* 参数) 指定 (指定的文本字符串写入 **流** 对象。|  
|**adWriteLine**|1|向 **流** 对象写入一个文本字符串和一个行分隔符。 如果未定义 [LineSeparator](./lineseparator-property-ado.md) 属性，则会返回运行时错误。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 这些常量没有 ADO/WFC 等效项。  
  
## <a name="applies-to"></a>应用于  
 [WriteText 方法](./writetext-method.md)