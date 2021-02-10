---
description: 更新和保留数据
title: 更新和保存数据 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- updating data [ADO]
- data updates [ADO]
- ADO, updating data
ms.assetid: 8dc27274-4f96-43d1-913c-4ff7d01b9a27
author: rothja
ms.author: jroth
ms.openlocfilehash: 90ea623d429a17db86040675b1b0112cda4237e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032319"
---
# <a name="updating-and-persisting-data"></a>更新和保留数据
前面的章节讨论了如何使用 ADO 获取数据源中的数据、如何在数据中移动，甚至如何编辑数据。 当然，如果您的应用程序的目标是允许用户对数据进行更改，则您将需要了解如何保存这些更改。 您可以使用 **Save** 方法将 **记录集** 更改保存到文件，也可以使用 **Update** 或 **UpdateBatch** 方法将更改发送回数据源以进行存储。  
  
 在前面的章节中，你更改了 **记录集** 的多个行中的数据。 ADO 支持两个与数据行的添加、删除和修改相关的基本概念。  
  
 第一种概念是不会立即对 **记录集** 进行更改;相反，它们是在内部 *复制缓冲区* 中进行的。 如果你确定不希望进行这些更改，则会丢弃复制缓冲区中的修改。 如果决定保留这些更改，则复制缓冲区中的更改将应用于 **记录集**。  
  
 第二个概念是，一旦您在某一行上声明了工作，就会立即将更改传播到数据源 (即， *即时* 模式) 或对一组行进行的所有更改都将被收集，直到您声明集完成 (为 *批处理* 模式) 的工作为止。 **LockType** 属性确定对基础数据源进行更改的时间。 **adLockOptimistic** 或 **adLockPessimistic** 指定即时模式，而 **adLockBatchOptimistic** 指定批处理模式。 **CursorLocation** 属性会影响可用的 **LockType** 设置。 例如，如果 **CursorLocation** 属性设置为 **adUseClient**，则不支持 **adLockPessimistic** 设置。  
  
 在即时模式下， **更新** 方法的每个调用都会将更改传播到数据源。 在批处理模式下，当前行位置的 **更新** 或移动的每次调用都会将更改保存到复制缓冲区，但只有 **UpdateBatch** 方法才能将更改传播到数据源。  
  
 本部分包含以下主题。  
  
-   [更新数据](../../../ado/guide/data/updating-data.md)  
  
-   [保留数据](../../../ado/guide/data/persisting-data.md)
