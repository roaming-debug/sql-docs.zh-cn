---
description: 数据整理示例
title: 数据定形示例 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], about data shaping
ms.assetid: 1bfdcad4-52e1-45bc-ad21-783657ef0a44
author: rothja
ms.author: jroth
ms.openlocfilehash: 0199dfc2165d5ae54b4b59a45ec740a9c399f114
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033277"
---
# <a name="data-shaping-example"></a>数据整理示例
以下数据定形命令演示了如何在 Northwind 数据库的 **Customers** 和 **Orders** 表中生成分层 **记录集**。  
  
```  
SHAPE {SELECT CustomerID, ContactName FROM Customers}   
APPEND ({SELECT OrderID, OrderDate, CustomerID FROM Orders} AS chapOrders   
RELATE customerID TO customerID)   
```  
  
 如果使用此命令打开 **记录集** 对象 (如 [Visual Basic 数据定形) 示例](./visual-basic-example-of-data-shaping.md)所示），则它将为从 **Customers** 表返回的每个记录创建一个 (**chapOrders**) 章节。 本章包含从 **Orders** 表返回的 **记录集** 的一个子集。 **ChapOrders** 章节包含给定客户所下订单的所有请求信息。 在此示例中，本章包含三列： **订单 id**、 **订货日期** 和 **CustomerID**。  
  
 结果形状 **记录集** 的前两个条目如下所示：  
  
|CustomerID|ContactName|OrderID|OrderDate|CustomerID|  
|----------------|-----------------|-------------|---------------|----------------|  
|ALFKI|Maria Ander|10643<br /><br /> 10692<br /><br /> 10702<br /><br /> 10835<br /><br /> 10952<br /><br /> 11011|1997-08-25<br /><br /> 1997-10-03<br /><br /> 1997-10-13<br /><br /> 1998-01-15<br /><br /> 1998-03-16<br /><br /> 1998-04-09|ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI|  
|ANATR|Ana Trujillo|10308<br /><br /> 10625<br /><br /> 10759<br /><br /> 10926|1996-09-18<br /><br /> 1997-08-08<br /><br /> 1997-11-28<br /><br /> 1998-03-04|ANATR<br /><br /> ANATR<br /><br /> ANATR<br /><br /> ANATR|  
  
 在 SHAPE 命令中，APPEND 用于创建与父 **记录集** 相关的子 **记录集**， (在与) 关系子句之前讨论的 SHAPE 关键字之后，从提供程序特定的命令返回。 父节点和子节点通常至少具有一个公共列：父级的行中的列的值与子的所有行中的列的值相同。  
  
 还有另一种使用形状命令的方法：即，从子 **记录集** 生成父 **记录集**。 子 **记录集中** 的记录通常通过使用 by 子句进行分组，并向子记录集中的每个结果组的父记录 **集** 添加一行。 如果省略了 BY 子句，则子 **记录集** 将形成单个组并且父 **记录集** 将只包含一行。 这对于计算整个子 **记录集** 的 "总计" 聚合很有用。  
  
 使用 SHAPE command 构造，还可以以编程方式创建形状 **记录集**。 然后，你可以通过编程方式或通过适当的视觉对象访问该 **记录集** 的组件。 像任何其他 ADO 命令文本一样发出一个 shape 命令。 有关详细信息，请参阅 [常规的形状命令](./shape-commands-in-general.md)。  
  
 无论父 **记录集** 的构成方式如何，它都将包含一个用于将其与子 **记录集** 相关联的章节列。 如果需要，父 **记录集** 还可以具有包含聚合的列 (SUM、MIN、MAX 等) 子行。 父 **记录集和子记录集** 的列都可以包含 **记录集中** 行的表达式，还可以包含新的和初始为空的列。  
  
 本部分继续介绍以下主题。  
  
-   [Visual Basic 数据整理示例](./visual-basic-example-of-data-shaping.md)