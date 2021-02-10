---
description: 记录集定位
title: 记录集定位 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- record positioning [ADO]
- Recordset object [ADO]
- repositioning record [ADO]
- AbsolutePosition property [ADO]
ms.assetid: c8f6fbcb-6675-4133-b37e-430de43949c1
author: rothja
ms.author: jroth
ms.openlocfilehash: 41bc6310f5fe9e75a57032bc13bc64cba62306f1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032549"
---
# <a name="recordset-positioning"></a>记录集定位
使用 **AbsolutePosition** 属性可以根据其在 **Recordset** 对象中的序号位置移动到记录，或确定当前记录的序号位置。 提供程序必须支持适当的功能，此属性才可用。  
  
 **AbsolutePosition** 从1开始，在当前记录是 **记录集** 的第一条记录时等于1。 如前所述，您可以从 **RecordCount** 属性中获取记录 **集** 对象中的记录总数。  
  
 设置 **AbsolutePosition** 属性时，即使该属性是对当前缓存中的记录的设置，ADO 也将使用您指定的记录开头的一组新记录重新加载缓存。 **CacheSize** 属性确定此组的大小。  
  
> [!NOTE]
>  不应将 **AbsolutePosition** 属性用作代理项记录号。 删除前面的记录后，给定记录的位置会发生更改。 如果重新查询或重新打开 **Recordset** 对象，则不保证给定记录将具有相同的 **AbsolutePosition** 。 建议使用书签保留并返回到给定位置，这是在所有类型的 **Recordset** 对象中定位的唯一方法。
