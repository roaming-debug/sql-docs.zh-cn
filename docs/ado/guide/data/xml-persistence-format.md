---
description: XML 暂留格式
title: XML 持久性格式 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML persistence [ADO], persistence format
ms.assetid: 6e146738-ac4d-47bb-b6cd-d87b2260aead
author: rothja
ms.author: jroth
ms.openlocfilehash: 3a1a1b832f9e34cbf99e5fd01c424bf54c50c3bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036717"
---
# <a name="xml-persistence-format"></a>XML 暂留格式
ADO 对 XML 流使用 UTF-8 编码。  
  
 ADO XML 格式分为两部分：架构部分和数据部分。 下面是来自 Northwind 数据库的 "货主" 表的示例 XML 文件。 示例中讨论了 XML 的各个部分。  
  
## <a name="remarks"></a>备注  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"   
xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882"   
xmlns:rs="urn:schemas-microsoft-com:rowset"   
xmlns:z="#RowsetSchema">   
  <s:Schema id="RowsetSchema">   
    <s:ElementType name="row" content="eltOnly" rs:updatable="true">   
      <s:AttributeType name="ShipperID" rs:number="1"   
        rs:basetable="shippers" rs:basecolumn="ShipperID"  
        rs:keycolumn="true">   
        <s:datatype dt:type="int" dt:maxLength="4" rs:precision="10"   
          rs:fixedlength="true" rs:maybenull="false"/>   
      </s:AttributeType>   
      <s:AttributeType name="CompanyName" rs:number="2"   
        rs:nullable="true" rs:write="true" rs:basetable="shippers"   
        rs:basecolumn="CompanyName">   
        <s:datatype dt:type="string" dt:maxLength="40" />   
      </s:AttributeType>   
      <s:AttributeType name="Phone" rs:number="3" rs:nullable="true"   
        rs:write="true" rs:basetable="shippers"   
        rs:basecolumn="Phone">   
        <s:datatype dt:type="string" dt:maxLength="24"/>   
      </s:AttributeType>   
      <s:extends type="rs:rowbase"/>   
    </s:ElementType>   
  </s:Schema>   
  
  <rs:data>   
    <z:row ShipperID="1" CompanyName="Speedy Express"   
      Phone="(503) 555-9831"/>   
    <z:row ShipperID="2" CompanyName="United Package"   
      Phone="(503) 555-3199"/>   
    <z:row ShipperID="3" CompanyName="Federal Shipping"   
      Phone="(503) 555-9931"/>   
  </rs:data>   
</xml>  
```  
  
 该架构显示命名空间、架构部分和数据部分的声明。 Schema 部分包含行、ShipperID、公司名称和电话的定义。  
  
 架构定义符合 [W3C XML-Data 规范](http://www.w3.org/TR/1998/NOTE-XML-data/) ，并且可以通过完全验证， (但在 Internet Explorer 5) 中不会进行验证。 XML-Data 是目前唯一受支持的记录集持久性架构格式。  
  
 Data 节包含三行，其中包含有关货主的信息。 对于空行集，data 节可以为空，但 \<rs:data> 标记必须存在。 不带数据的情况下，您只需编写一个标记简写即可 \<rs:data/> 。 以 "rs" 为前缀的任何标记均表示它位于 urn：架构-microsoft-com：行集定义的命名空间中。  
  
## <a name="see-also"></a>另请参阅  
 [以 XML 格式保留记录](./persisting-records-in-xml-format.md)