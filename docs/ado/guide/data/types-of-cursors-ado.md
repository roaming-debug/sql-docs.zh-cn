---
description: 游标类型 (ADO)
title: ADO)  (的游标类型 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], types
ms.assetid: 7cc01544-e814-403b-bbfe-a2750bf921bd
author: rothja
ms.author: jroth
ms.openlocfilehash: c264cda3a42dd49848d4ad047f24d54102c60a3c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037817"
---
# <a name="types-of-cursors-ado"></a>游标类型 (ADO)
通常，应用程序应使用提供所需数据访问的最简单的游标。 除基础知识之外的每个附加游标特征 (只进、只读、静态、滚动、无缓冲) 在客户端内存、网络负载或性能方面都有价格。 在许多情况下，默认游标选项生成的游标比应用程序实际需要的更复杂。  
  
 游标类型的选择取决于应用程序使用结果集的方式，还取决于多个设计注意事项，包括结果集的大小、可能使用的数据百分比、数据更改的敏感度和应用程序性能要求。  
  
 光标的最基本选择取决于你是需要更改数据还是仅查看数据：  
  
-   如果只需滚动一组结果而不是更改数据，请使用 [只进](../../../ado/guide/data/forward-only-cursors.md) 或 [静态](../../../ado/guide/data/static-cursors.md) 游标。  
  
-   如果有一个较大的结果集，只需选择几行，请使用 [键集](../../../ado/guide/data/keyset-cursors.md) 游标。  
  
-   如果要将结果集与所有并发用户的最近添加、更改和删除同步，请使用 [动态](../../../ado/guide/data/dynamic-cursors.md) 游标。  
  
 尽管每个游标类型看起来都是不同的，但请记住，这些游标类型的不同之处不只是重叠特性和选项的结果。  
  
 本部分包含以下主题。  
  
-   [只进游标](../../../ado/guide/data/forward-only-cursors.md)  
  
-   [静态游标](../../../ado/guide/data/static-cursors.md)  
  
-   [键集游标](../../../ado/guide/data/keyset-cursors.md)  
  
-   [动态游标](../../../ado/guide/data/dynamic-cursors.md)  
  
## <a name="see-also"></a>另请参阅  
 [只进游标](../../../ado/guide/data/forward-only-cursors.md)   
 [静态游标](../../../ado/guide/data/static-cursors.md)   
 [键集游标](../../../ado/guide/data/keyset-cursors.md)   
 [动态游标](../../../ado/guide/data/dynamic-cursors.md)
