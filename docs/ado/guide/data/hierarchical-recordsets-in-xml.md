---
description: XML 中的分层记录集
title: XML 中的分层记录集 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO], in XML
ms.assetid: 5d4b11c4-c94f-4910-b99b-5b9abc50d791
author: rothja
ms.author: jroth
ms.openlocfilehash: 9919489df58853a3fa8eea3711583cdf57a0849f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033707"
---
# <a name="hierarchical-recordsets-in-xml"></a>XML 中的分层记录集
ADO 允许将分层记录集对象持久保存到 XML 中。 使用分层记录集对象，父记录集中某个字段的值就是另一个记录集。 此类字段表示为 XML 流中的子元素，而不是特性。  
  
## <a name="remarks"></a>备注  
 下面的示例演示了这种情况：  
  
```  
Rs.Open "SHAPE {select stor_id, stor_name, state from stores} APPEND ({select stor_id, ord_num, ord_date, qty from sales} AS rsSales RELATE stor_id TO stor_id)", "Provider=MSDataShape;DSN=pubs;Integrated Security=SSPI;"  
```  
  
 下面是持久化记录集的 XML 格式：  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"     xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882"     xmlns:rs="urn:schemas-microsoft-com:rowset"   
    xmlns:z="#RowsetSchema">   
  <s:Schema id="RowsetSchema">   
    <s:ElementType name="row" content="eltOnly" rs:updatable="true">   
      <s:AttributeType name="stor_id" rs:number="1"   
        rs:writeunknown="true">   
        <s:datatype dt:type="string" dt:maxLength="4"   
          rs:fixedlength="true" rs:maybenull="false"/>   
      </s:AttributeType>   
      <s:AttributeType name="stor_name" rs:number="2" rs:nullable="true"   
        rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="40"/>   
      </s:AttributeType>   
      <s:AttributeType name="state" rs:number="3" rs:nullable="true"   
        rs:writeunknown="true">   
        <s:datatype dt:type="string" dt:maxLength="2"   
          rs:fixedlength="true"/>   
      </s:AttributeType>   
      <s:ElementType name="rsSales" content="eltOnly"   
        rs:updatable="true" rs:relation="010000000100000000000000">   
        <s:AttributeType name="stor_id" rs:number="1"   
          rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="4"   
            rs:fixedlength="true" rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="ord_num" rs:number="2"   
          rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="20"   
            rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="ord_date" rs:number="3"   
          rs:writeunknown="true">   
            <s:datatype dt:type="dateTime" dt:maxLength="16"   
              rs:scale="3" rs:precision="23" rs:fixedlength="true"   
              rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="qty" rs:number="4" rs:writeunknown="true">   
          <s:datatype dt:type="i2" dt:maxLength="2" rs:precision="5"   
            rs:fixedlength="true" rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:extends type="rs:rowbase"/>   
      </s:ElementType>   
      <s:extends type="rs:rowbase"/>   
    </s:ElementType>   
  </s:Schema>   
  <rs:data>   
    <z:row stor_id="6380" stor_name="Eric the Read Books" state="WA">   
      <rsSales stor_id="6380" ord_num="6871"   
        ord_date="1994-09-14T00:00:00" qty="5"/>   
      <rsSales stor_id="6380" ord_num="722a"   
        ord_date="1994-09-13T00:00:00" qty="3"/>   
    </z:row>   
    <z:row stor_id="7066" stor_name="Barnum's" state="CA">   
      <rsSales stor_id="7066" ord_num="A2976"   
        ord_date="1993-05-24T00:00:00" qty="50"/>   
      <rsSales stor_id="7066" ord_num="QA7442.3"   
        ord_date="1994-09-13T00:00:00" qty="75"/>   
    </z:row>   
    <z:row stor_id="7067" stor_name="News & Brews" state="CA">   
      <rsSales stor_id="7067" ord_num="D4482"   
        ord_date="1994-09-14T00:00:00" qty="10"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="40"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="20"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="20"/>   
    </z:row>   
  </rs:data>   
</xml>   
```  
  
 当以这种方式持久保存父记录集中的列时，其确切顺序并不明显。 父代中的任何字段可能包含子记录集。 持久性提供程序首先将所有标量列作为属性保留，然后将所有子记录集 "columns" 作为父行的子元素出现。 可以通过查看记录集的架构定义来获取父记录集中字段的序号位置。 每个字段都有一个 OLE DB 属性 rs： number，其中定义了包含该字段的序号的记录集架构命名空间。  
  
 子记录集中所有字段的名称将与包含此子级的父记录集中的字段的名称相连接。 这是为了确保当父记录集和子记录集都包含从两个不同的表中获取但名为特别的字段时，不存在名称冲突。  
  
 将分层记录集保存到 XML 中时，应注意 ADO 中的以下限制：  
  
-   包含待定更新的分层记录集无法保存到 XML 中。  
  
-   使用参数化形状命令创建的分层记录集不能以 XML 格式或 ADTG 格式保存 () 。  
  
-   ADO 目前将父记录集和子记录集之间的关系保存为二进制大型对象 (BLOB) 。 尚未在行集架构命名空间中定义用于描述此关系的 XML 标记。  
  
-   保存分层记录集时，所有子记录集都将连同它一起保存。 如果当前记录集是另一个记录集的子级，则不保存其父级。 将保存构成当前记录集的子树的所有子记录集。  
  
 当分层记录集从其 XML 持久格式重新打开时，必须注意以下限制：  
  
-   如果子记录包含没有对应的父记录的记录，则这些行不会以分层记录集的 XML 表示形式写出。 因此，当记录集从其持久性位置重新打开时，这些行将丢失。  
  
-   如果子记录引用了多个父记录，则在重新打开记录集时，子记录集可能包含重复记录。 但是，仅当用户直接使用基础子行集时，才会显示这些重复项。 如果某个章节用于导航子记录集 (这是通过 ADO) 导航的唯一方法，则不会显示重复项。  
  
## <a name="see-also"></a>另请参阅  
 [以 XML 格式保留记录](./persisting-records-in-xml-format.md)