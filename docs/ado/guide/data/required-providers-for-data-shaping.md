---
description: 数据整理所需的提供程序
title: 数据整理所需的提供程序 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], data shaping
- data shaping [ADO], providers required
ms.assetid: d49d48d2-ac2d-4c11-895c-5a149b444620
author: rothja
ms.author: jroth
ms.openlocfilehash: eb3d91f23666c900f59a6ffc8e6b94af9bcdb4d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032529"
---
# <a name="required-providers-for-data-shaping"></a>数据整理所需的提供程序
数据定形通常需要两个提供程序。 OLE DB 的服务提供商、 [数据定形服务](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)提供数据定形功能，数据访问接口（例如 SQL Server 的 OLE DB 提供程序）提供数据行来填充形状的 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md)。  
  
 可以将服务提供程序 (MSDataShape) 的名称指定为 [连接](../../../ado/reference/ado-api/connection-object-ado.md) 对象 [提供程序](../../../ado/reference/ado-api/provider-property-ado.md) 属性的值或连接字符串关键字 "provider = MSDataShape;"。  
  
 可以将数据提供程序的名称指定为 **数据提供程序** 动态属性的值，该属性将添加到 OLE DB 的数据定形服务或连接字符串关键字 "**data provider =** _Provider_" 的 **连接** 对象 [属性](../../../ado/reference/ado-api/properties-collection-ado.md)集合中。  
  
 如果未填充 **记录集**，则不需要任何数据提供程序 (例如，如在创建列时使用新关键字) 创建的集。 在这种情况下，请指定 "**Data Provider =** none;"。  
  
## <a name="example"></a>示例  
  
```  
Dim cnn As New ADODB.Connection  
cnn.Provider = "MSDataShape"  
cnn.Open "Data Provider=SQLOLEDB;Integrated Security=SSPI;Database=Northwind"  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据定形示例](../../../ado/guide/data/data-shaping-example.md)   
 [正式形状语法](../../../ado/guide/data/formal-shape-grammar.md)   
 [常用 Shape 命令](../../../ado/guide/data/shape-commands-in-general.md)
