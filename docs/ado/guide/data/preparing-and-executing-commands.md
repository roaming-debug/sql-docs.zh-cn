---
description: 准备和执行命令
title: 准备和执行命令 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Command object [ADO], preparing and executing commands
ms.assetid: 7448d9ee-7f4b-47e3-be54-2df8c9bbac32
author: rothja
ms.author: jroth
ms.openlocfilehash: fd88799bce8f3b8a0842c9f2d2752eac062dc4b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037177"
---
# <a name="preparing-and-executing-commands"></a>准备和执行命令
命令是向提供程序发出的指令，用于对基础数据源执行一些操作。 例如，SQL 语句是 Microsoft SQL 数据访问接口的命令。 在 ADO 中，命令通常由 **命令** 对象表示，不过也可以通过 **连接** 或 **Recordset** 对象发出简单的命令。  
  
 您可以使用 **Command** 对象从提供程序请求任何支持的操作类型，前提是该提供程序可以正确解释命令字符串。 数据访问接口的常见操作是查询数据库并返回记录 **集** 对象中的记录，这可以被视为容器来容纳结果和查看结果的工具。 与许多 ADO 对象一样，某些 **命令** 对象集合、方法或属性在被引用时可能会生成错误，具体情况视提供程序的功能而定。  
  
 除了使用 **命令** 对象之外，还可以对 **连接** 对象使用 **Execute** 方法，或对 **Recordset** 对象使用 **Open** 方法来发出命令并使其执行。 但是，如果需要在代码中重复使用命令，或者需要通过命令传递详细的参数信息，则应使用 **命令** 对象。 本部分稍后将更详细地介绍这些方案。  
  
> [!NOTE]
>  某些 **命令** 可以将结果集作为二进制流或单个 **记录** 而不是 **记录集** 返回，如果提供程序支持此方法。 此外，某些 **命令** 不打算返回任何 (的结果集，例如，SQL Update 查询) 。 此部分将介绍最典型的方案，但是：执行将结果作为 **记录集** 对象返回的 **命令**。 有关将结果返回到 **Record** 或 **Stream** 的详细信息，请参阅 [记录和流](../../../ado/guide/data/records-and-streams.md)。  
  
 本部分包含以下主题。  
  
-   [命令对象概述](../../../ado/guide/data/command-object-overview.md)  
  
-   [创建和执行简单的命令](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [命令对象参数](../../../ado/guide/data/command-object-parameters.md)  
  
-   [使用命令调用存储过程](../../../ado/guide/data/calling-a-stored-procedure-with-a-command.md)  
  
-   [在连接对象上调用存储过程作为方法](../../../ado/guide/data/calling-a-stored-procedure-as-a-method-on-a-connection-object.md)  
  
-   [命名命令](../../../ado/guide/data/named-commands.md)  
  
-   [将参数传递给命名命令](../../../ado/guide/data/passing-parameters-to-a-named-command.md)
