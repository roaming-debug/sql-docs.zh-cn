---
description: 确定支持的功能
title: 确定受支持的内容 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- editing data [ADO], Supports method
- Supports method [ADO]
ms.assetid: 65090cba-6d46-4775-8d61-f6838e7752a6
author: rothja
ms.author: jroth
ms.openlocfilehash: 6a20514318de8222d016f2d345bd6758a075ab5e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037527"
---
# <a name="determining-what-is-supported"></a>确定支持的功能
**支持** 方法用于确定指定的 **记录集** 对象是否支持特定类型的功能。 它采用以下语法：  
  
```  
  
boolean = recordset.Supports(CursorOptions )  
```  
  
## <a name="remarks"></a>备注  
 **支持** 方法返回一个布尔值，该值指示提供程序是否支持由 CursorOptions 参数标识的所有功能。 您可以使用 **支持** 方法来确定 **Recordset** 对象支持的功能类型。 如果 **Recordset** 对象支持其对应常量在 *CursorOptions* 中的功能，则 **支持** 方法返回 **True**。 否则，返回 **False**。  
  
 使用 **支持** 方法，您可以检查 **Recordset** 对象是否能够添加新记录、使用书签、使用 **Find** 方法、使用滚动、使用 **索引** 属性以及执行批处理更新。 有关常量及其含义的完整列表，请参阅 [CursorOptionEnum](../../reference/ado-api/cursoroptionenum.md)。  
  
 尽管 **支持** 方法对于给定的功能可能会返回 **True** ，但并不保证提供程序在所有情况下都可以使用该功能。 如果满足某些条件，则 **支持** 方法只返回提供程序是否可以支持指定的功能。 例如， **支持** 方法可能指示 **Recordset** 对象支持更新，即使游标基于多个表联接（某些列不可更新）。