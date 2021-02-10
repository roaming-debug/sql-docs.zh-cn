---
description: 游标位置的重要性
title: 游标位置的重要性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- server-side cursors [ADO]
- cursors [ADO], client-side
- client-side cursors [ADO]
- cursors [ADO], server-side
ms.assetid: 70ef5b1c-0459-41a1-b796-031f61a29a8a
author: rothja
ms.author: jroth
ms.openlocfilehash: a3caa56a5c3560367cc2f816c6aa2b1c12cbba2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036927"
---
# <a name="the-significance-of-cursor-location"></a>游标位置的重要性
每个游标都使用临时资源来保存其数据。 这些资源可以是内存、磁盘分页文件、临时磁盘文件，甚至可以是数据库中的临时存储。 当这些资源位于客户端计算机上时，游标称为 *客户端* 游标。 当这些资源位于服务器上时，游标称为 *服务器端* 游标。  
  
## <a name="client-side-cursors"></a>Client-Side 游标  
 在 ADO 中，使用 **AdUseClient CursorLocationEnum** 调用客户端游标。 使用非键集客户端游标，服务器会将整个结果集发送到客户端计算机。 客户端计算机提供游标和结果集所需的临时资源并对其进行管理。 客户端应用程序可以浏览整个结果集来确定所需的行。  
  
 如果静态和由键集驱动的客户端游标包含过多的行，则可能会在工作站上施加大量负载。 尽管所有游标库都能够生成包含数千行的游标，但旨在提取此类大行集的应用程序可能会表现得很糟糕。 当然，还有一些例外。 对于某些应用程序，较大的客户端游标可能完全合适，并且性能可能不是问题。  
  
 客户端游标的一个明显优势是快速响应。 将结果集下载到客户端计算机后，浏览行的速度非常快。 通过客户端游标，你的应用程序的可伸缩性更强，因为游标的资源要求放置在每个单独的客户端上，而不是放置在服务器上。  
  
## <a name="server-side-cursors"></a>Server-Side 游标  
 在 ADO 中，使用 **AdUseServer CursorLocationEnum** 调用服务器端游标。 使用服务器端游标，服务器将使用服务器计算机提供的资源管理结果集。 服务器端游标仅返回通过网络请求的数据。 与客户端游标相比，这种类型的游标有时可以提供更好的性能，尤其是在网络流量过多的情况下。  
  
 但是，请务必指出，服务器端游标至少为每个活动的客户端都暂时消耗宝贵的服务器资源。 你必须相应地进行规划，以确保你的服务器硬件能够管理活动客户端请求的所有服务器端游标。 此外，服务器端游标可能会很慢，因为它仅提供单行访问权限，因此没有可用的批处理游标。  
  
 在插入、更新或删除记录时，服务器端游标非常有用。 使用服务器端游标时，可以在同一连接上具有多个活动语句。
