---
description: Shape APPEND 子句
title: Shape APPEND 子句 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- data shaping [ADO], APPEND clause
- append clause [ADO]
ms.assetid: f90fcf55-6b24-401d-94e1-d65bd24bd342
author: rothja
ms.author: jroth
ms.openlocfilehash: 7c6d62d410e6709c93ad6e3d04db627f14f091a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036987"
---
# <a name="shape-append-clause"></a>Shape APPEND 子句
Shape command APPEND 子句向 **记录集** 追加一列或多列。 通常，这些列是章节列，它们引用子 **记录集**。  
  
## <a name="syntax"></a>语法  
  
```  
SHAPE [parent-command [[AS] parent-alias]] APPEND column-list  
```  
  
## <a name="description"></a>说明  
 此子句的组成部分如下所示：  
  
 *parent-command*  
 零个或以下 (可以完全省略 *父命令*) ：  
  
-   括在大括号中的提供程序命令 ( " {} " ) 返回 **记录集** 对象。 将向基础数据提供程序发出命令，其语法取决于提供程序的要求。 这通常是 SQL 语言，尽管 ADO 不需要任何特定的查询语言。  
  
-   嵌入在括号内的另一个 shape 命令。  
  
-   TABLE 关键字，后跟数据提供程序中表的名称。  
  
 *父-别名*  
 引用父 **记录集** 的可选别名。  
  
 *列列表*  
 以下一项或多项操作：  
  
-   聚合列。  
  
-   计算列。  
  
-   使用 NEW 子句创建的新列。  
  
-   章节列。 章节列定义括在括号中， ( " ( # A2" ) 中。 请参阅以下语法。  
  
```  
SHAPE [parent-command [[AS] parent-alias]]  
   APPEND (child-recordset [ [[AS] child-alias]   
      RELATE parent-column TO child-column | PARAMETER param-number, ... ])  
   [[AS] chapter-alias]   
   [, ... ]  
```  
  
## <a name="remarks"></a>备注  
 *子记录集*  
 -   括在大括号中的提供程序命令 ( " {} " ) 返回 **记录集** 对象。 将向基础数据提供程序发出命令，其语法取决于提供程序的要求。 这通常是 SQL 语言，尽管 ADO 不需要任何特定的查询语言。  
  
-   嵌入在括号内的另一个 shape 命令。  
  
-   现有整形 **记录集** 的名称。  
  
-   TABLE 关键字，后跟数据提供程序中表的名称。  
  
 *子别名*  
 引用子 **记录集** 的别名。  
  
 *父列*  
 *父-命令* 返回的 **记录集中** 的列。  
  
 *子列*  
 *子命令* 返回的 **记录集中** 的列。  
  
 *param-数字*  
 请参阅 [参数化命令的操作](../../../ado/guide/data/operation-of-parameterized-commands.md)。  
  
 *章节-别名*  
 引用追加到父级的章节列的别名。  
  
> [!NOTE]
>  *"父列* 到 *子列"* 子句实际上是一个列表，其中定义的每个关系都用逗号分隔  
  
> [!NOTE]
>  追加关键字后面的子句实际上是一个列表，其中每个子句都用逗号分隔，并定义另一个要追加到父级的列。  
  
当你作为 SHAPE 命令的一部分从用户输入构造提供程序命令时，形状会将用户提供的提供程序命令视为不透明的字符串，并将其切实传递给提供程序。 例如，在下面的 SHAPE 命令中，  
  
```  
SHAPE {select * from t1} APPEND ({select * from t2} RELATE k1 TO k2)  
```  
  
 形状将执行两个命令： `select * from t1` 和 (`select * from t2 RELATE k1 TO k2)` 。 如果用户提供了一个由多个提供程序命令（用分号分隔）组成的复合命令，则形状无法区分差别。 因此，在下面的 SHAPE 命令中，  
  
```  
SHAPE {select * from t1; drop table t1} APPEND ({select * from t2} RELATE k1 TO k2)  
```  
  
 `select * from t1; drop table t1` `select * from t2 RELATE k1 TO k2),` 在此示例中，形状执行并 (未实现， `drop table t1` 这种情况下是一种危险的、提供程序命令。 应用程序必须始终验证用户输入，以防发生此类潜在的黑客攻击。  
  
 本部分包含以下主题。  
  
-   [操作非参数化命令](../../../ado/guide/data/operation-of-non-parameterized-commands.md)  
  
-   [参数化命令的操作](../../../ado/guide/data/operation-of-parameterized-commands.md)  
  
-   [混合命令](../../../ado/guide/data/hybrid-commands.md)  
  
-   [中间 Shape COMPUTE 子句](../../../ado/guide/data/intervening-shape-compute-clauses.md)  
  
## <a name="see-also"></a>另请参阅  
 [数据定形示例](../../../ado/guide/data/data-shaping-example.md)   
 [正式形状语法](../../../ado/guide/data/formal-shape-grammar.md)   
 [常用 Shape 命令](../../../ado/guide/data/shape-commands-in-general.md)
