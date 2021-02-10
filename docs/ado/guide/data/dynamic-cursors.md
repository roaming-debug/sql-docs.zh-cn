---
description: 动态游标
title: 动态游标 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], dynamic
- dynamic cursors [ADO]
ms.assetid: 00460f30-8cf7-494e-82df-41012f40ae51
author: rothja
ms.author: jroth
ms.openlocfilehash: d78686da853ad62ba4a3c70b36831798542efa36
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037517"
---
# <a name="dynamic-cursors"></a>动态游标
动态游标检测对结果集中的行所做的所有更改，而不考虑更改是发生在游标内部还是游标外的其他用户。 所有用户发出的所有 insert、update 和 delete 语句均通过游标可见。 动态游标可以检测在打开游标后对结果集中的行、顺序和值所做的任何更改。 除非将游标事务隔离级别设置为 "未提交" ) ，否则在游标外进行的更新将不可见，除非将它们提交 (。  
  
 例如，假设动态游标提取两个行和一个应用程序，然后更新其中一个行并删除另一个行。 然后，如果动态游标提取这两行，它将找不到已删除的行，但会显示已更新行的新值。  
  
 如果你的应用程序必须检测其他用户进行的所有并发更新，则动态游标是一个不错的选择。 使用 **AdOpenDynamic CursorTypeEnum** 指示要在 ADO 中使用动态游标。  
  
## <a name="see-also"></a>另请参阅  
 [只进游标](./forward-only-cursors.md)   
 [静态游标](./static-cursors.md)   
 [键集游标](./keyset-cursors.md)