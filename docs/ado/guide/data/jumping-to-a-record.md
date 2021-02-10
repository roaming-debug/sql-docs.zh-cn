---
description: 跳转到记录
title: 跳转到记录 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- record jumping [ADO]
- jumping to record [ADO]
ms.assetid: 6caf6299-2eea-4d34-9b0e-b75aab07b740
author: rothja
ms.author: jroth
ms.openlocfilehash: 1a8a34ac8cafffe9f22595a76c5389369290a2b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032719"
---
# <a name="jumping-to-a-record"></a>跳转到记录
使用 [Move](../../reference/ado-api/move-method-ado.md) 方法，您可以使用以下语法在 **记录集中** 向前或向后移动指定数量的记录：  
  
```  
oRs.Move NumRecords, Start  
```  
  
## <a name="remarks"></a>备注  
 **Move** 方法在所有 **记录集** 对象上都受支持。  
  
 如果 *NumRecords* 参数大于零，则当前记录位置将向前移动 (向 **记录集** 的末尾) 。 如果 *NumRecords* 小于零，则当前记录位置 (向后) **记录集** 的开头向后移动。  
  
 如果 **移动** 调用会将当前记录位置移动到第一条记录之前的某个点，则 ADO 会将当前记录设置为 **记录集中** 第一条记录之前的位置 (**BOF** 为 **True**) 。 当 **BOF** 属性为 **True** 时，尝试向后移动将生成错误。  
  
 如果 **移动** 调用会将当前记录位置移到最后一条记录之后的某个点，则 ADO 会将当前记录设置为 **记录集中** 最后一条记录之后的位置 (**EOF** 为 **True**) 。 当 **EOF** 属性已 **为 True** 时，尝试向前移动将生成错误。  
  
 从空 **记录集** 对象调用 **Move** 方法会生成错误。  
  
 如果在 *开始* 参数中传递书签，则移动将相对于带有此书签的记录，假设 **Recordset** 对象支持书签。 使用 [书签](../../reference/ado-api/bookmark-property-ado.md) 属性获取书签。 如果未指定，则移动相对于当前记录。  
  
 如果使用 **CacheSize** 属性在本地缓存来自提供程序的记录，并且传递的是在当前缓存记录组外部移动当前记录位置的 *NumRecords* 参数，则会强制 ADO 从目标记录开始检索一组新记录。 **CacheSize** 属性确定新检索到的组的大小，目标记录是检索的第一条记录。