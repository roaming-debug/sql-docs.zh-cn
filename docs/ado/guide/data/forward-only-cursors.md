---
description: 只进游标
title: Forward-Only 游标 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], forward-only
- forward-only cursors [ADO]
ms.assetid: 2b1e062f-3294-4a6f-8241-a17045c4df18
author: rothja
ms.author: jroth
ms.openlocfilehash: 0ac51891368a5551c29190de959f8a9559d03b4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033137"
---
# <a name="forward-only-cursors"></a>只进游标
典型的默认游标类型称为只进 (或不可滚动的) 游标，只能通过结果集向前移动。 只进游标不支持滚动 (在结果集中向前和向后移动) ;它仅支持从结果集的开始处提取行。 使用某些只进游标 (例如，使用 SQL Server 游标库) ，当前用户所做的所有 insert、update 和 delete 语句 (，或由其他) 用户提交，则在提取行时将会看到影响结果集中的行的所有 insert、update 和 delete 语句。 由于游标无法向后滚动，但是，在提取行后对数据库中的行进行的更改通过游标均不可见。  
  
 处理当前行的数据后，只进游标将释放用于保存这些数据的资源。 默认只进游标是动态的，这意味着处理当前行时会检测到所有更改。 这可实现更快速的游标打开，并使结果集能够显示对基础表所做的更新。  
  
 当只进游标不支持向后滚动时，您的应用程序可以通过关闭和重新打开游标返回到结果集的开头。 这是使用少量数据的有效方法。 作为替代方法，应用程序可以读取结果集一次，将数据缓存在本地，然后浏览本地数据缓存。  
  
 如果您的应用程序不需要在结果集中滚动，则只进游标是使用最少的开销快速检索数据的最佳方式。 使用 **AdOpenForwardOnly CursorTypeEnum** 指示你希望在 ADO 中使用只进游标。  
  
## <a name="see-also"></a>另请参阅  
 [静态游标](./static-cursors.md)   
 [键集游标](./keyset-cursors.md)   
 [动态游标](./dynamic-cursors.md)