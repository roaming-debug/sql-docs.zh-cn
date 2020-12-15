---
title: 'sql：关系和键排序规则 (SQLXML) '
description: 了解如何在 SQLXML 中使用 sql： relationship 元素和键排序规则。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sql:relationship
- key ordering rules [SQLXML]
- relationship annotation
ms.assetid: 914cb152-09f5-4b08-b35d-71940e4e9986
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1846a3db3194a81ce1d1522ac175e3988d3d3d23
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462928"
---
# <a name="annotation-interpretation---sqlrelationship-and-key-ordering-rule"></a>批注解释 - sql:relationship 和键排序规则
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  由于 XML 大容量加载在其节点进入作用域时会生成记录，并在其节点退出作用域时会将这些记录发送到 Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，因此，有关记录的数据必须存在于节点的作用域中。  
  
 请考虑下面的 XSD 架构，其中，和元素之间的一对多关系 **\<Customer>** **\<Order>** (一个客户可以将多个订单) 使用 **\<sql:relationship>** 元素指定：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"<>   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustCustOrder"  
          parent="Cust"  
          parent-key="CustomerID"  
          child="CustOrder"  
          child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customers" sql:relation="Cust" >  
   <xsd:complexType>  
     <xsd:sequence>  
       <xsd:element name="CustomerID"  type="xsd:integer" />  
       <xsd:element name="CompanyName" type="xsd:string" />  
       <xsd:element name="City"        type="xsd:string" />  
       <xsd:element name="Order"   
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder" >  
         <xsd:complexType>  
          <xsd:attribute name="OrderID" type="xsd:integer" />  
         </xsd:complexType>  
       </xsd:element>  
     </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 当 **\<Customer>** 元素节点进入作用域时，XML 大容量加载将生成一个客户记录。 此记录一直保持到 XML 大容量加载读取 **\</Customer>** 。 在处理 **\<Order>** 元素节点时，XML 大容量加载使用 **\<sql:relationship>** 从父元素中获取 CustOrder 表的 CustomerID 外键列的值 **\<Customer>** ，因为该 **\<Order>** 元素未指定 **customerid** 属性。 这意味着在定义 **\<Customer>** 元素时，必须在指定之前在架构中指定 **CustomerID** 属性 **\<sql:relationship>** 。 否则，当 **\<Order>** 元素进入作用域时，Xml 大容量加载为 CustOrder 表生成一条记录，并且当 XML 大容量加载到达 **\</Order>** 结束标记时，它会将该记录发送到， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 而不会出现 CustomerID 外键列值。  
  
 将在该示例中提供的架构另存为 SampleSchema.xml。  
  
### <a name="to-test-a-working-sample"></a>测试工作示例  
  
1.  创建以下表：  
  
    ```  
    CREATE TABLE Cust (  
                  CustomerID     int          PRIMARY KEY,  
               CompanyName    varchar(20)  NOT NULL,  
                  City           varchar(20)  DEFAULT 'Seattle')  
    GO  
    CREATE TABLE CustOrder (  
                  OrderID        varchar(10) PRIMARY KEY,  
               CustomerID     int         FOREIGN KEY REFERENCES                                          Cust(CustomerID))  
    GO  
    ```  
  
2.  将下面的示例数据另存为 SampleXMLData.xml：  
  
    ```  
    <ROOT>    
      <Customers>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City>NY</City>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
        <CustomerID>1111</CustomerID>  
      </Customers>  
      <Customers>  
        <CompanyName>Toms Spezialitten</CompanyName>  
         <City>LA</City>    
        <Order OrderID="3" />  
        <CustomerID>1112</CustomerID>  
      </Customers>  
      <Customers>  
        <CompanyName>Victuailles en stock</CompanyName>  
        <Order OrderID="4" />  
        <CustomerID>1113</CustomerID>  
      </Customers>  
    </ROOT>  
    ```  
  
3.  若要执行 XML 大容量加载，请将下面的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic Scripting Edition (VBScript) 示例另存为 MySample.vbs，并执行它：  

    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Transaction=True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
     结果是 XML 大容量加载在 CustOrder 表的 CustomerID 外键列中插入一个 NULL 值。 如果修改 XML 示例数据，使 **\<CustomerID>** 子元素出现在 **\<Order>** 子元素之前，则会获得预期结果： XML 大容量加载将指定的外键值插入到列中。  
  
 这是等效的 XDR 架构：  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >   
   <ElementType name="CustomerID"  />  
   <ElementType name="CompanyName" />  
   <ElementType name="City"        />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust" >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
                 <sql:relationship  
                        key-relation    ="Cust"  
                        key             ="CustomerID"  
                        foreign-key     ="CustomerID"  
                        foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
</Schema>  
```  
  
  
