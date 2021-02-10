---
description: 什么是锁定？
title: 什么是锁定？ | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], locking
- locks [ADO], about locking
ms.assetid: f8989555-28c6-4c17-9bf8-7f44a8a5c407
author: rothja
ms.author: jroth
ms.openlocfilehash: 245a0cd91b9d6c7675df146d9d5808b42bb8c618
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036727"
---
# <a name="what-is-a-lock"></a>什么是锁定？
锁定是 DBMS 限制对多用户环境中的行的访问的过程。 如果行或列被独占锁定，则在解除锁定之前，不允许其他用户访问锁定的数据。 这可确保两个用户不能同时更新行中的同一列。  
  
 从资源角度来看，锁的开销可能很大，只应在需要时才使用来保持数据完整性。 在数百个或数千个用户可能尝试访问一条记录的数据库（例如连接到 Internet 的数据库）上，可能会快速导致应用程序的性能降低。  
  
 您可以通过选择相应的锁定选项来控制数据源和 ADO 游标库管理并发的方式。  
  
 在打开 **记录集** 之前设置 **LockType** 属性，以指定提供程序在打开它时应使用哪种类型的锁定。 读取属性，返回在打开的 **记录集** 对象上使用的锁定类型。  
  
 提供程序可能不支持所有锁定类型。 如果提供程序不支持请求的 **LockType** 设置，它将替换另一种类型的锁定。 若要确定 **Recordset** 对象中可用的实际锁定功能，请使用 [支持](../../../ado/reference/ado-api/supports-method.md) 方法和 **adUpdate** 和 **adUpdateBatch**。  
  
 如果 [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md)属性设置为 adUseClient，则不支持 **adLockPessimistic** 设置 **。** 如果设置了不受支持的值，则不会产生错误;将改用最近支持的 **LockType** 。  
  
 当 **记录集** 关闭时， **LockType** 属性是可读/写的，并且在打开时为只读。  
  
 本部分包含以下主题。  
  
-   [锁定类型](../../../ado/guide/data/types-of-locks.md)
