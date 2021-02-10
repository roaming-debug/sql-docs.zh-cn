---
description: XSLT 转换
title: XSLT 转换 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XSLT transformations in ADO
ms.assetid: 1a46196e-839f-4734-a59e-2c64609ffb9e
author: rothja
ms.author: jroth
ms.openlocfilehash: 75a29504d0d19300f8137959be150670f71e8b5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032200"
---
# <a name="xslt-transformations"></a>XSLT 转换
XSLT 可应用于生成的 XML，以将其转换为另一种格式。 了解 ADO 中的 XML 格式可帮助开发 XSLT 模板，从而将其转换为更易于用户使用的形式。  
  
 例如，您知道记录集的每一行都保存为 rs： data 元素中的 z:row 元素。 同样，记录集的每个字段都将保存为此元素的属性/值对。  
  
## <a name="remarks"></a>备注  
 以下 XSLT 脚本可应用于上一部分所示的 XML，以将其转换为要在浏览器中显示的 HTML 表：  
  
```  
<?xml version="1.0" encoding="ISO-8859-1"?>  
<html xmlns:xsl="http://www.w3.org/TR/WD-xsl">  
<body STYLE="font-family:Arial, helvetica, sans-serif; font-size:12pt; background-color:white">  
<table border="1" style="table-layout:fixed" width="600">  
  <col width="200"></col>  
  <tr bgcolor="teal">  
    <th><font color="white">CustomerId</font></th>  
    <th><font color="white">CompanyName</font></th>  
    <th><font color="white">ContactName</font></th>  
  </tr>  
<xsl:for-each select="xml/rs:data/z:row">  
  <tr bgcolor="navy">  
    <td><font color="white"><xsl:value-of select="@CustomerID"/></font></td>  
    <td><font color="white"><xsl:value-of select="@CompanyName"/></font></td>  
    <td><font color="white"><xsl:value-of select="@ContactName"/></font></td>   
  </tr>  
</xsl:for-each>  
</table>  
</body>  
</html>  
```  
  
 XSLT 将 ADO Save 方法生成的 XML 流转换为 HTML 表，其中显示记录集的每个字段以及表标题。 还会为表标题和行分配不同的字体和颜色。  
  
## <a name="see-also"></a>另请参阅  
 [以 XML 格式保留记录](./persisting-records-in-xml-format.md)