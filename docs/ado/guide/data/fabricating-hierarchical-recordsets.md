---
description: 构造分层记录集
title: 制造分层记录集 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset fabrication [ADO]
- hierarchical Recordsets [ADO]
- fabricating hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: a584e642-a4a3-418e-bc20-3aff81a5625a
author: rothja
ms.author: jroth
ms.openlocfilehash: 836c1e0091166485f68c04226c632e9999038469
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033104"
---
# <a name="fabricating-hierarchical-recordsets"></a>构造分层记录集
下面的示例演示如何使用数据定形语法来定义父、子和孙 **记录集** 的列，以创建不包含基础数据源的分层记录集。  
  
 若要创建某个分层 **记录集**，您必须 [为 OLE DB (ADO 服务提供程序)  (MSDataShape) 指定 Microsoft 数据定形服务](../appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)，并且您可以在该 [连接](../../reference/ado-api/connection-object-ado.md)对象的 [Open](../../reference/ado-api/open-method-ado-connection.md)方法的连接字符串参数中指定 "NONE" 的数据提供程序值。 有关详细信息，请参阅 [数据定形所需的提供程序](./required-providers-for-data-shaping.md)。  
  
```  
Dim cn As New ADODB.Connection  
Dim rsCustomers As New ADODB.Recordset  
  
cn.Open "Provider=MSDataShape;Data Provider=NONE;"  
  
strShape = _  
"SHAPE APPEND NEW adInteger AS CustID," & _  
            " NEW adChar(25) AS FirstName," & _  
            " NEW adChar(25) AS LastName," & _  
            " NEW adChar(12) AS SSN," & _  
            " NEW adChar(50) AS Address," & _  
         " ((SHAPE APPEND NEW adChar(80) AS VIN_NO," & _  
                        " NEW adInteger AS CustID," & _  
                        " NEW adChar(20) AS BodyColor, " & _  
                     " ((SHAPE APPEND NEW adChar(80) AS VIN_NO," & _  
                                    " NEW adChar(20) AS Make, " & _  
                                    " NEW adChar(20) AS Model," & _  
                                    " NEW adChar(4) AS Year) " & _  
                        " AS VINS RELATE VIN_NO TO VIN_NO))" & _  
            " AS Vehicles RELATE CustID TO CustID) "  
  
rsCustomers.Open strShape, cn, adOpenStatic, adLockOptimistic, -1  
```  
  
 一旦记录了该 **记录集** ，就可以对其进行填充、操作或保存到文件。  
  
## <a name="see-also"></a>另请参阅  
 [访问分层记录集中的行](./accessing-rows-in-a-hierarchical-recordset.md)   
 [正式形状语法](./formal-shape-grammar.md)   
 [数据整理所需的提供程序](./required-providers-for-data-shaping.md)   
 [Shape APPEND 子句](./shape-append-clause.md)   
 [常用 Shape 命令](./shape-commands-in-general.md)