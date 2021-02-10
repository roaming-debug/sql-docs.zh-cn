---
description: 静态游标
title: 静态游标 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], static
- static cursors [ADO]
ms.assetid: cce93ace-c4ed-4c6c-940c-28a50ff2fd12
author: rothja
ms.author: jroth
ms.openlocfilehash: a6b6ef12f2da2416e5f2c3c6e2e6c0c94e2c5327
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036977"
---
# <a name="static-cursors"></a>静态游标
静态游标始终显示第一次打开游标时的结果集。 静态游标是只读的或读/写的，并提供向前和向后滚动，具体取决于实现。 静态游标通常不会检测在打开游标后对结果集的成员资格、顺序或值所做的更改。 尽管不需要，但静态游标可检测其自己的更新、删除和插入。  
  
 静态游标从不检测其他更新、删除和插入操作。 例如，假定静态游标提取行，然后另一个应用程序将更新该行。 如果应用程序通过静态游标重新提取行，尽管更改由其他应用程序执行，但看到的值将保持不变。 支持所有类型的滚动，但提供程序可能支持或不支持书签。  
  
 如果您的应用程序不需要检测数据更改并且需要滚动，则静态游标是最佳选择。 使用 **AdOpenStatic CursorTypeEnum** 指示要在 ADO 中使用静态游标。  
  
## <a name="see-also"></a>另请参阅  
 [只进游标](../../../ado/guide/data/forward-only-cursors.md)   
 [键集游标](../../../ado/guide/data/keyset-cursors.md)   
 [动态游标](../../../ado/guide/data/dynamic-cursors.md)
