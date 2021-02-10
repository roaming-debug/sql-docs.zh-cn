---
description: 用于 OLE DB 的 Microsoft 游标服务
title: 用于 OLE DB 的 Microsoft 游标服务 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursor service for ole db [ADO]
- cursors [ADO], cursor service for OLE DB
ms.assetid: 1ac3bd9b-2d45-4cc8-88ec-bd8a218cfb49
author: rothja
ms.author: jroth
ms.openlocfilehash: c2a785ba338164392f1364166365df31b2bd6dfd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036947"
---
# <a name="the-microsoft-cursor-service-for-ole-db"></a>用于 OLE DB 的 Microsoft 游标服务
选择客户端游标时，或将 **CursorLocation** 属性设置为 **adUseClient** 时，将调用 OLE DB 的 Microsoft 游标服务。 你可能还会看到对 "客户端游标引擎" 的引用，这在 ADO 上下文中本质上是相同的。 此服务对数据提供程序的游标支持函数进行了补充。 因此，你可以从所有数据访问接口感知相对统一的功能。  
  
 OLE DB 的游标服务使动态属性可用，并增强特定方法的行为。 例如，通过 " **优化** 动态属性"，可以创建临时索引以简化特定操作，例如 **Find** 方法。  
  
 在所有情况下，游标服务都支持批处理更新。 当数据访问接口只能提供不太好的游标（如静态游标）时，它还会模拟更有功能的游标类型（如动态游标）。  
  
## <a name="see-also"></a>另请参阅  
 [用于 OLE DB (ADO 服务组件的 Microsoft 游标服务) ](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)
