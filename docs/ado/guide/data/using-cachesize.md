---
description: 使用 CacheSize
title: 使用 CacheSize |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- locks [ADO], CacheSize property
- CacheSize property [ADO]
ms.assetid: ca1c3422-b6a4-4ba6-af55-54f975b698b1
author: rothja
ms.author: jroth
ms.openlocfilehash: 31c9b1dd1ccc3ef61b120bf3d075b81fc34c0eb6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032279"
---
# <a name="using-cachesize"></a>使用 CacheSize
使用 **CacheSize** 属性可控制从提供程序在本地内存中一次检索的记录数。 例如，如果 **CacheSize** 为10，则在第一次打开 **Recordset** 对象之后，提供程序会在本地内存中检索前10条记录。 当您在 **Recordset** 对象中移动时，提供程序将从本地内存缓冲区返回数据。 一旦移过缓存中的最后一条记录，提供程序就会将数据源中的后10个记录检索到缓存中。  
  
> [!NOTE]
>  **CacheSize** 基于) 的 **记录集** 对象的 **Properties** 集合中 (的 **最大打开行** 特定于提供程序的属性。 不能将 **CacheSize** 设置为大于 **最大打开行数** 的值。 若要修改提供程序可以打开的行数，请设置 " **最大打开行** 数"。  
  
 **CacheSize** 的值可以在 **Recordset** 对象的生命周期内进行调整，但更改此值只会影响在后续从数据源中检索后缓存中的记录数。 仅更改属性值不会更改缓存的当前内容。  
  
 如果要检索的记录数少于 **CacheSize** 指定的数目，则提供程序将返回其余记录并且不会发生错误。  
  
 不允许使用零的 **CacheSize** 设置，并且将返回错误。  
  
 从缓存检索的记录不反映其他用户对源数据所做的并发更改。 若要强制执行所有缓存数据的更新，请使用 [Resync](../../../ado/reference/ado-api/resync-method.md) 方法。  
  
 如果 **CacheSize** 设置为大于1的值，则在检索记录后发生删除时，导航方法 ([Move](../../../ado/reference/ado-api/move-method-ado.md)、 [MoveFirst、MoveLast、MoveNext 和 MovePrevious](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)) 可能会导致导航到已删除的记录。 初始提取后，在您尝试从已删除的行访问数据值之前，后续删除将不会反映在您的数据缓存中。 但是，将 **CacheSize** 设置为1可消除此问题，因为无法提取删除的行。
