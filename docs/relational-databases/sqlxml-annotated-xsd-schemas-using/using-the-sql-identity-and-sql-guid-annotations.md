---
title: 使用 sql:identity 和 sql:guid 批注
description: 了解如何使用 XSD 架构中的 sql： identity 和 sql： guid 批注定义 XML updategram 的行为。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sql:guid
- annotated XSD schemas, IDENTITY-type columns
- annotated XSD schemas, GUID values
- DiffGrams [SQLXML], annotations
- identity annotation
- XSD schemas [SQLXML], GUID values
- GUIDs [SQLXML]
- guid annotation
- sql:identity
- IDENTITY-type column
- XSD schemas [SQLXML], IDENTITY-type columns
- updategrams [SQLXML], GUID values
ms.assetid: 7661dfd0-6573-4692-a8f1-3597adcd33c4
author: MightyPen
ms.author: genemi
ms.reviewer: ''
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7c66bac3f83aff00a458d3fed14296d2b03071f7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479318"
---
# <a name="using-the-sqlidentity-and-sqlguid-annotations"></a>使用 sql:identity 和 sql:guid 批注
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  您可以在映射到中的数据库列的任何节点上的 XSD 架构中指定 **sql： identity** 和 **sql： guid** 批注 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 Updategram 格式支持 **updg： at 标识** 和 **updg： guid** 特性，而 DiffGram 格式则不支持。 **Updg： at 标识** 特性定义更新 identity 类型列时的行为。 **Updg： guid** 属性允许你从获取 guid 值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，并在 updategram 中使用它。 有关详细信息和工作示例，请参阅 [使用 XML Updategram 插入数据 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md)。  
  
 **Sql： identity** 和 **sql： guid** 批注将此功能扩展到 diffgram。  
  
 执行 DiffGram 时，应首先将其转换为 updategram，然后再执行该 updategram。 通过在 XSD 架构中指定 **sql： identity** 和 **sql： guid** 批注，实际上是在定义 updategram 的行为。 因此，所有批注都在 updategram 的上下文中进行描述。 这些批注可用于 DiffGram 和 updategram；但是，updategram 已提供了一种更强大的处理标识和 GUID 值的方式。  
  
 **Sql： identity** 和 **sql： guid** 批注可在复杂内容元素上定义。  
  
## <a name="sqlidentity-annotation"></a>sql:identity 批注  
 您可以在映射到 IDENTITY 类型数据库列的任何节点上的 XSD 架构中指定 **sql： identity** 批注。 为此批注指定的值定义了如何通过使用 updategram 中提供的值来修改列或通过忽略值（在这种情况下，为) 使用生成的值）来 (更新 IDENTITY 类型的列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 可以为 **sql： identity** 批注分配两个值：  
  
 ignore  
 指示 updategram 忽略在其中为该列提供的任何值，并依赖于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成标识值。  
  
 useValue  
 指示 updategram 使用在其中提供的值来更新 IDENTITY 类型的列。 updategram 不会检查列是否为标识值。  
  
 如果 updategram 指定了 IDENTITY 类型列的值，则必须在该架构中指定 **sql： IDENTITY = "useValue"** 。  
  
## <a name="sqlguid-annotation"></a>sql:guid 批注  
 updategram 可以使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成一个 GUID 值，并将此值用于该 updategram 中。 在 Diffgram 的上下文中，可以使用 **sql： guid** 批注指定是使用由 SQL Server 生成的 guid 值，还是使用在 updategram 中为该列提供的值。  
  
 可以为 **sql： guid** 批注分配两个值：  
  
 generate  
 指定应在更新操作中使用由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成的该列的 GUID。  
  
 useValue  
 指定在 updategram 中指定的值可以用于该列。 这是默认值。  
  
  
