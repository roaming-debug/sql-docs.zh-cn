---
description: 使用书签
title: 使用书签 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- bookmarks [ADO]
- Recordset object [ADO]
ms.assetid: cca244e6-84f8-4394-bca9-f7a819b8f4df
author: rothja
ms.author: jroth
ms.openlocfilehash: f07ad3e4f5dc69da31a74af96842b1c8aab97eda
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036767"
---
# <a name="using-bookmarks"></a>使用书签
当在 **记录集中** 四处移动后直接返回到特定记录，而不必滚动每个记录和比较值时，这通常很有用。 例如，如果您尝试使用 **Find** 方法搜索一条记录，但该搜索未返回任何记录，则您将自动置于该记录 **集** 的任一端。 如果提供程序支持，则可以使用书签来标记你的位置，然后再使用 **Find** 方法，以便可以返回到你的位置。 书签是唯一标识 **Recordset** 对象中的记录的 **变量** 类型值。  
  
 您还可以使用包含 **记录集筛选器** 方法的书签的变体数组来筛选选定的一组记录。 有关此技术的详细信息，请参阅本部分后面的 [使用记录集](../../../ado/guide/data/working-with-recordsets.md)筛选主题中的结果。  
  
 您可以使用 " **书签** " 属性来获取记录的书签，或将 **记录集** 对象中的当前记录设置为有效书签标识的记录。 下面的代码使用 **bookmark** 属性设置书签，然后在移到其他记录后返回到带有书签的记录。 若要确定您的 **记录集** 是否支持书签，请使用 **支持** 方法。  
  
```  
'BeginBookmarkEg  
Dim varBookmark As Variant  
Dim blnCanBkmrk As Boolean  
  
objRs.Open strSQL, strConnStr, adOpenStatic, adLockOptimistic, adCmdText  
  
If objRs.RecordCount > 4 Then  
    objRs.Move 4                       ' move to the fifth record  
    blnCanBkmrk = objRs.Supports(adBookmark)  
    If blnCanBkmrk = True Then  
        varBookmark = objRs.Bookmark   ' record the bookmark  
        objRs.MoveLast                 ' move to a different record  
        objRs.Bookmark = varBookmark   ' return to the bookmarked (sixth) record  
    End If  
End If  
'EndBookmarkEg  
```  
  
 稍后更详细地介绍了 [支持](../../../ado/reference/ado-api/supports-method.md) 方法。  
  
 除了克隆的 **记录集**，书签对于创建它们的 **记录集** 是唯一的，即使使用相同的命令也是如此。 这意味着不能使用从一个 **记录集** 获得的 **书签** 移动到使用同一个命令打开的第二个 **记录集中** 的同一记录。
