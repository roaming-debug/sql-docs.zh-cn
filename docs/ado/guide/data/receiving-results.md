---
description: 接收结果
title: 正在接收结果 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- receiving results [ADO]
- Recordset object [ADO], receiving results
ms.assetid: 791aa26e-7aae-477e-9f05-5cd46e1de095
author: rothja
ms.author: jroth
ms.openlocfilehash: 1cdcf6acb2eb9de033f86b9f34739a55dc0dc021
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032579"
---
# <a name="receiving-results"></a>接收结果
在 ADO 中，大多数命令都会导致某些信息返回给调用方。 对于返回行集的命令，会在 **记录集** 对象（可能是最常用的 ADO 对象）中接收结果。  
  
 可以通过多种方法从数据源接收 **记录集** 对象中的数据，包括调用以下内容：  
  
-   [Connection.Exe漂亮方法](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Command.Exe漂亮方法](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Recordset。 Open 方法](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [NamedCommand](../../../ado/guide/data/named-commands.md)  
  
-   [StoredProcedure](../../../ado/guide/data/calling-a-stored-procedure-as-a-method-on-a-connection-object.md)  
  
 使用 **连接** 对象和 **命令** 对象隐式或显式的方式接收 **记录集** 对象中的数据。 在典型的客户端/服务器应用程序系统中，获取数据的整个过程要求每个已填充的 **记录集** 的网络往返。  
  
 若要接收多个结果集，则需要在网络上进行多次往返，其中每个数据集封装在 **记录集** 对象中。 对于慢速网络或拥塞网络，减少往返次数可能有助于改善应用程序的性能。 因此，某些提供程序支持在一次往返中接收多个 **记录集**。 以下主题对此进行了讨论：  
  
-   [接收多个记录集](../../../ado/guide/data/receiving-multiple-recordsets.md)
