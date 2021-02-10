---
description: 架构部分
title: Schema 部分 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Schema section [ADO]
ms.assetid: 4ac6e524-2c92-48e8-b871-0a4b5c8fda18
author: rothja
ms.author: jroth
ms.openlocfilehash: 346957b2b060bf990cf77bfe7784f67b13a37c37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037007"
---
# <a name="schema-section"></a>架构部分
架构部分是必需的。 如前面的示例所示，ADO 写出了有关每个列的详细元数据，以尽可能多地保留数据值的语义。 但是，若要在 XML 中加载，ADO 只需要列的名称及其所属的行集。 下面是最小架构的示例：  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"  
    xmlns:rs="urn:schemas-microsoft-com:rowset"  
    xmlns:z="#RowsetSchema">  
  <s:Schema id="RowsetSchema">  
    <s:ElementType name="row" content="eltOnly">  
      <s:AttributeType name="ShipperID"/>  
      <s:AttributeType name="CompanyName"/>  
      <s:AttributeType name="Phone"/>  
      <s:Extends type="rs:rowbase"/>  
    </s:ElementType>  
  </s:Schema>  
  <rs:data>  
...  
  </rs:data>  
</xml>  
```  
  
 在上面的示例中，ADO 将数据视为可变长度字符串，因为架构中未包含任何类型信息。  
  
## <a name="creating-aliases-for-column-names"></a>为列名称创建别名  
 Rs： name 属性允许您为列名创建别名，以便可以在行集公开的列信息中显示友好名称，并且可以在 data 节中使用较短的名称。 例如，可以修改上一个架构，以将 ShipperID 映射到 s1，将其名称更改为 s2，并按如下所示将手机改为 s3：  
  
```  
<s:Schema id="RowsetSchema">   
<s:ElementType name="row" content="eltOnly" rs:updatable="true">   
<s:AttributeType name="s1" rs:name="ShipperID" rs:number="1" ...>   
...  
</s:AttributeType>   
<s:AttributeType name="s2" rs:name="CompanyName" rs:number="2" ...>   
...  
</s:AttributeType>   
<s:AttributeType name="s3" rs:name="Phone" rs:number="3" ...>   
...  
</s:AttributeType>   
...  
</s:ElementType>   
</s:Schema>  
```  
  
 然后，在 "数据" 部分中，行将使用 name 属性， (不是 rs： name) 以引用该列：  
  
```  
"<row s1="1" s2="Speedy Express" s3="(503) 555-9831"/>  
```  
  
 如果列名不是 XML 中的有效属性或标记名称，则需要为列名创建别名。 例如，"LastName" 必须具有别名，因为带有嵌入空格的名称是无效的属性。 XML 分析器将无法正确处理下面的行，因此，您必须为不具有嵌入空格的其他名称创建别名。  
  
```  
<row last name="Jones"/>  
```  
  
 对于 name 属性使用的任何值，都必须在 XML 文档的 "架构" 和 "数据" 部分中引用该列的每个位置一致地使用。 下面的示例演示了 s1 的一致性使用：  
  
```  
<s:Schema id="RowsetSchema">  
  <s:ElementType name="row" content="eltOnly">  
    <s:attribute type="s1"/>  
    <s:attribute type="CompanyName"/>  
    <s:attribute type="s3"/>  
    <s:extends type="rs:rowbase"/>  
  </s:ElementType>  
  <s:AttributeType name="s1" rs:name="ShipperID" rs:number="1"   
    rs:maydefer="true" rs:writeunknown="true">  
    <s:datatype dt:type="i4" dt:maxLength="4" rs:precision="10"   
      rs:fixedlength="true" rs:maybenull="true"/>  
  </s:AttributeType>  
</s:Schema>  
<rs:data>  
  <z:row s1="1" CompanyName="Speedy Express" s3="(503) 555-9831"/>  
</rs:data>  
```  
  
 同样，由于 `CompanyName` 在前面的示例中没有为定义的别名，因此 `CompanyName` 必须在整个文档中一致地使用。  
  
## <a name="data-types"></a>数据类型  
 您可以向具有 dt： type 属性的列应用数据类型。 有关允许的 XML 类型的权威性指南，请参阅 [W3C XML-Data 规范](http://www.w3.org/TR/1998/NOTE-XML-data/)的 "数据类型" 部分。 可以通过两种方式指定数据类型：直接在列定义本身上指定 dt： type 特性，或使用 s:datatype 构造作为列定义的嵌套元素。 例如，  
  
```  
<s:AttributeType name="Phone" >  
  <s:datatype dt:type="string"/>  
</s:AttributeType>  
```  
  
 等效于  
  
```  
<s:AttributeType name="Phone" dt:type="string"/>  
```  
  
 如果完全从行定义中省略 dt： type 属性，则默认情况下，该列的类型将为可变长度字符串。  
  
 如果你的类型信息比类型名称多 (例如，dt： maxLength) ，则使用 s:datatype 子元素可更方便地使用它。 但这只是一种约定，不是必需的。  
  
 下面的示例演示如何在架构中包含类型信息。  
  
```  
<!-- 1. String with no max length -->  
<s:AttributeType name="title_id"/>  
<!-or -->  
<s:AttributeType name="title_id" dt:type="string"/>  
  
<!-- 2. Fixed length string with max length of 6 -->  
<s:AttributeType name="title_id">  
    <s:datatype dt:type="string" dt:maxLength="6" rs:fixedlength="true" />  
</s:AttributeType>  
  
<!-- 3. Variable length string with max length of 6 -->  
<s:AttributeType name="title_id">  
    <s:datatype dt:type="string" dt:maxLength="6" />  
</s:AttributeType>  
  
<!-- 4. Integer -->  
<s:AttributeType name="title_id" dt:type="int"/>  
```  
  
 在第二个示例中，使用了 rs： fixedlength 属性。 将 rs： fixedlength 属性设置为 true 的列意味着数据必须具有架构中定义的长度。 在这种情况下，title_id 的有效值为 "123456"，因为它是 "123"。 但是，"123" 无效，因为其长度为3，而不是6。 有关 fixedlength 属性的更完整说明，请参阅 OLE DB 程序员指南。  
  
## <a name="handling-nulls"></a>处理 Null 值  
 Null 值由 rs： maybenull 属性处理。 如果此特性设置为 true，则列的内容可以包含 null 值。 此外，如果在数据行中找不到该列，则从行集读取数据的用户将从 IRowset：： ( # A1 获取 null 状态。 请考虑运货商表中的以下列定义。  
  
```  
<s:AttributeType name="ShipperID">  
  <s:datatype dt:type="int" dt:maxLength="4"/>  
</s:AttributeType>  
<s:AttributeType name="CompanyName">  
  <s:datatype dt:type="string" dt:maxLength="40" rs:maybenull="true"/>  
</s:AttributeType>  
```  
  
 定义允许为 `CompanyName` null，但 `ShipperID` 不能包含 null 值。 如果 data 节包含以下行，则持久性提供程序将列的数据状态设置 `CompanyName` 为 OLE DB 状态常量 DBSTATUS_S_ISNULL：  
  
```  
<z:row ShipperID="1"/>  
```  
  
 如果行完全为空（如下所示），则持久性提供程序将返回 "DBSTATUS_E_UNAVAILABLE 的 OLE DB 状态， `ShipperID` 并 DBSTATUS_S_ISNULL" 公司名称 "。  
  
```  
<z:row/>   
```  
  
 请注意，长度为零的字符串不同于 null。  
  
```  
<z:row ShipperID="1" CompanyName=""/>  
```  
  
 对于前面的行，持久性提供程序将为这两个列返回 OLE DB 状态 DBSTATUS_S_OK。 `CompanyName`在此示例中，只需 "" (零长度字符串) 。  
  
 有关可用于 OLE DB 的 XML 文档架构中的 OLE DB 构造的详细信息，请参阅 "urn： schema-microsoft com：行集" 的定义和 OLE DB 程序员指南。  
  
## <a name="see-also"></a>另请参阅  
 [以 XML 格式保留记录](../../../ado/guide/data/persisting-records-in-xml-format.md)
