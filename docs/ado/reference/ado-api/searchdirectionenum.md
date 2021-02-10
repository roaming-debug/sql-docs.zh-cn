---
description: SearchDirectionEnum
title: SearchDirectionEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SearchDirectionEnum
helpviewer_keywords:
- SearchDirectionEnum enumeration [ADO]
ms.assetid: 81272ae3-2165-4f4e-adfe-9ede0368cb17
author: rothja
ms.author: jroth
ms.openlocfilehash: 0d3b03d203873a9d14aeecb7e44c7184dea274f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040647"
---
# <a name="searchdirectionenum"></a>SearchDirectionEnum
指定记录 [集](./recordset-object-ado.md)内的记录搜索的方向。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adSearchBackward**|-1|向后搜索，在 **记录集** 的开头处停止。 如果找不到匹配项，则记录指针将置于 [BOF](./bof-eof-properties-ado.md)。|  
|**adSearchForward**|1|向前搜索，在 **记录集** 末尾停止。 如果未找到匹配项，则记录指针定位于 [EOF](./bof-eof-properties-ado.md)。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.SearchDirection.BACKWARD|  
|AdoEnums.SearchDirection.FORWARD|  
  
## <a name="applies-to"></a>应用于  
 [Find 方法 (ADO)](./find-method-ado.md)