---
title: '在位置路径中指定节点测试 (SQLXML) '
description: 了解如何在 SQLXML 4.0 XPath 查询的位置路径中指定节点测试。
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XPath queries [SQLXML], location paths
- principal node types [SQLXML]
- node tests [SQLXML]
- location path for XPath query
ms.assetid: f46c30bf-1e24-4435-9ac2-f8ba43a8ff94
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b8fdee8bdc7f3fbc3281ecab7682efdc8378bbdf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97414658"
---
# <a name="specifying-a-node-test-in-the-location-path-sqlxml-40"></a>在位置路径中指定节点测试 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  节点测试指定根据位置步骤选择的节点类型。 每个轴 (**child**、 **parent**、 **attribute** 或 **self**) 具有一个主体节点类型。 对于 **属性** 轴，主体节点类型为 **\<attribute>** 。 对于 **parent**、 **child** 和 **self** 轴，主体节点类型为 **\<element>** 。  
  
> [!NOTE]  
>  不支持通配符节点测试 *（例如 `child::*`）。  
  
## <a name="node-test-example-1"></a>节点测试：示例1  
 位置路径 `child::Customer` 选择 **\<Customer>** 上下文节点的子元素。  
  
 在本示例中，`child` 是轴，`Customer` 是节点测试。 **子** 轴的主体节点类型为 **\<element>** 。 因此，如果节点是节点，则节点测试为 TRUE **\<Customer>** **\<element>** 。 如果上下文节点没有任何 **\<Customer>** 子级，则返回一组空节点。  
  
## <a name="node-test-example-2"></a>节点测试：示例 2  
 位置路径 `attribute::CustomerID` 选择上下文节点的 **CustomerID** 属性。  
  
 在此示例中， `attribute` 是轴， `CustomerID` 是节点测试。 **特性** 轴的主要节点类型为 **\<attribute>** 。 因此，如果 **CustomerID** 是节点，则节点测试为 TRUE **\<attribute>** 。 如果上下文节点没有 **CustomerID**，则返回一组空节点。  
  
> [!NOTE]  
>  在此 XPath 实现中，如果定位步骤引用 **\<element>** **\<attribute>** 架构中未声明的或类型，则会生成错误。 这与 MSXML 中的 XPath 实现不同，在 MSXML 中返回空节点集。  
  
## <a name="abbreviated-syntax-for-the-axes"></a>轴的缩写语法  
 支持以下位置路径的缩写语法：  
  
-   `attribute::` 可缩写为 `@`。  
  
     位置路径 `Customer[@CustomerID="ALFKI"]` 与 `child::Customer[attribute::CustomerID="ALFKI"]` 相同。  
  
-   可以从位置步骤中省略 `child::`。  
  
     因此， **子级** 为默认轴。 位置路径 `Customer/Order` 与 `child::Customer/child::Order` 相同。  
  
-   `self::node()` 可缩写为一个句点 (.)，`parent::node()` 可缩写为两个句点 (..)。  
  
  
