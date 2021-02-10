---
description: 接收多个记录集
title: 接收多个记录集 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- receiving multiple Recordsets [ADO]
- Recordset object [ADO], receiving multiple Recordsets
ms.assetid: 2a7ad7a6-f00d-4355-b0b5-d0ab957b0566
author: rothja
ms.author: jroth
ms.openlocfilehash: d0b4213b70498ea021b2267748c0f6fe3c6adf5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037117"
---
# <a name="receiving-multiple-recordsets"></a>接收多个记录集
[适用于 SQL Server 的 Microsoft OLE DB 提供程序](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md)支持为包含多个 sql 语句的单个命令返回多个 **记录集** 对象，每个 Sql 语句一个 **记录集**。 返回 **记录集** 的顺序遵循 SQL 语句在命令文本中的放置顺序。  
  
 当命令包含 COMPUTE 子句时，适用于 SQL Server 的 Microsoft OLE DB 提供程序也会向 ADO 返回多个结果集。 例如，包含以下 SQL 语句的命令将在两个 **记录集** 对象中返回结果：一个用于 (*ProductID*、 *ProductName*、 *单价*) 的行集，另一个用于表中所有产品的平均价格。  
  
```  
SELECT ProductID, ProductName, UnitPrice   
  FROM PRODUCTS   
  COMPUTE AVG(UnitPrice)  
```  
  
 可以使用 **NextRecordset** 方法枚举两个对象。  
  
 有关详细信息，请参阅 [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)。
