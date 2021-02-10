---
description: 常用 Shape 命令
title: 整体形状命令 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- data shaping [ADO], shape commands
ms.assetid: 1fac7831-a187-4b15-9b43-aad380c5556c
author: rothja
ms.author: jroth
ms.openlocfilehash: 021d47eb011a4c0628b6f356772e82c591747103
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032459"
---
# <a name="shape-commands-in-general"></a>常用 Shape 命令
数据定形定义了整形 **记录集** 的列、列所表示的实体之间的关系，以及使用数据填充 **记录集** 的方式。  
  
 形状的 **记录集** 可以包含以下类型的列。  
  
|列类型|说明|  
|-----------------|-----------------|  
|数据|查询命令返回给数据访问接口、表或之前形状 **记录集** 的 **记录集** 的字段。|  
|段|对另一 **记录集** 的引用，称为 *章节*。 通过章节列可以定义 *父子关系，* 其中的 *父* 项是包含章列的 **记录集** ，而 *子级* 是本章表示的 **记录集** 。|  
|aggregate|通过对子 **记录集** 的所有行的所有行或列执行 *聚合函数* 来派生列的值。  (参见以下主题中的聚合函数： [聚合函数、CALC 函数和 NEW 关键字](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)。 ) |  
|计算表达式|列的值是通过对 **记录集** 的同一行中的列计算 Visual Basic for Applications 表达式来派生的。 表达式是 CALC 函数的参数。  (参见以下主题中的计算表达式： [聚合函数、CALC 函数以及 NEW 关键字](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md) 和 [Visual Basic for Applications 函数](../../../ado/guide/data/visual-basic-for-applications-functions.md)。 ) |  
|new|空的制造字段，可以在以后使用数据进行填充。 列是用 NEW 关键字定义的。  (参见以下主题中的 NEW 关键字： [聚合函数、CALC 函数和 NEW 关键字](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)。 ) |  
  
 Shape 命令可以包含一个子句，该子句指定对将返回 **Recordset** 对象的基础数据提供程序的查询命令。 查询的语法取决于基础数据提供程序的要求。 这通常是 SQL，尽管 ADO 不需要使用任何特定的查询语言。  
  
 可以通过 **Recordset** 对象或通过设置 [命令](../../../ado/reference/ado-api/command-object-ado.md)对象的 [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md)属性，然后调用 [Execute](../../../ado/reference/ado-api/execute-method-ado-command.md)方法来发出形状命令。  
  
 您可以使用 SQL 联接子句来关联两个表;但是，分层 **记录集** 可以更高效地表示信息。 联接创建的 **记录集** 的每一行都重复从一个表中冗余的信息。 分层 **记录集** 的每个子 **记录** 集对象只有一个父 **记录集**。  
  
 形状命令可以嵌套。 也就是说， *父-command* 或 *子命令* 本身可以是另一个 shape 命令。  
  
 即使用户指定了 **adUseServer** 的光标位置，形状提供程序也始终返回客户端游标。  
  
 您可以通过编程方式或通过适当的视觉对象访问形状 **记录集** 的 **记录** 集组件。  
  
 Microsoft 提供了一种工具，该工具生成形状命令 (查看 Visual Basic 6 文档中的 [数据环境设计器](/previous-versions/visualstudio/aa445793(v=vs.60))) ，另一个显示分层游标 (参阅 Visual Basic 6 文档) 中的 "使用 Microsoft 分层 Flexgrid 控件"。  
  
 有关导航分层 **记录集** 的信息，请参阅 [访问分层记录集中的行](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md)。  
  
 有关语法正确的形状命令的详细信息，请参阅 [正式的形状语法](../../../ado/guide/data/formal-shape-grammar.md)。  
  
 本部分包含以下主题。  
  
-   [聚合函数、CALC 函数和 NEW 关键字](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)  
  
-   [向基础数据提供程序发出命令](../../../ado/guide/data/issuing-commands-to-the-underlying-data-provider.md)