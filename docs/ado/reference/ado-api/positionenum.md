---
description: PositionEnum
title: PositionEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- PositionEnum
helpviewer_keywords:
- PositionEnum enumeration
ms.assetid: e69af0a5-3405-4b72-9c6e-6b188ff746fd
author: rothja
ms.author: jroth
ms.openlocfilehash: 6c563c2d548d56b5b87273a4b3e5c05ef2cdff6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041027"
---
# <a name="positionenum"></a>PositionEnum
指定记录指针在 [记录集中](./recordset-object-ado.md)的当前位置。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adPosBOF**|-2|指示当前记录指针处于 BOF (也就是说， [bof](./bof-eof-properties-ado.md) 属性为 **True**) 。|  
|**adPosEOF**|-3|指示当前记录指针位于 EOF (即， [eof](./bof-eof-properties-ado.md) 属性为 **True**) 。|  
|**adPosUnknown**|-1|指示 **记录集** 为空，当前位置未知，或者提供程序不支持 [AbsolutePage](./absolutepage-property-ado.md) 或 [AbsolutePosition](./absoluteposition-property-ado.md) 属性。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums|  
|AdoEnums|  
|AdoEnums。未知|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [AbsolutePage 属性 (ADO)](./absolutepage-property-ado.md)  
    :::column-end:::
    :::column:::
        [AbsolutePosition 属性 (ADO)](./absoluteposition-property-ado.md)  
    :::column-end:::
:::row-end:::