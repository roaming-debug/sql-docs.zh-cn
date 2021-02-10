---
description: GetChildren 方法 (ADO)
title: " (ADO) 的 GetChildren 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_GetChildren
- _Record::GetChildren
helpviewer_keywords:
- GetChildren method [ADO]
ms.assetid: b3f09bac-4f66-49f6-aa5a-6fbb4fb28338
author: rothja
ms.author: jroth
ms.openlocfilehash: 3bf4e7c68bbab5c5452a2eb0dabc790ab3cc5e15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021229"
---
# <a name="getchildren-method-ado"></a>GetChildren 方法 (ADO)
返回一个 [记录集](./recordset-object-ado.md) ，其行表示集合 [记录](./record-object-ado.md)的子级。  
  
## <a name="syntax"></a>语法  
  
```  
  
Set recordset = record.GetChildren  
```  
  
## <a name="return-value"></a>返回值  
 每行表示当前 **记录** 对象的子记录 **集的记录集** 对象。 例如，表示目录的 **记录** 的子级将是包含在父目录中的文件和子目录。  
  
## <a name="remarks"></a>备注  
 提供程序确定哪些列存在于返回的 **记录集中**。 例如，文档源提供程序始终返回资源 **记录集**。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [记录对象 (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [记录集对象 (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::