---
title: 'Updategram (SQLXML 的批注映射架构) '
description: 了解如何使用 SQLXML 4.0 updategram 中指定的批注 XSD 或 XDR 映射架构来处理对数据库的更新。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotated XSD schemas, updategrams
- data types [SQLXML], mapping schema in updategrams
- updategrams [SQLXML], annotated mapping schemas
- annotated XDR schemas, updategrams
- inverse attribute
- parent-child relationships [SQLXML]
- mapping-schema attribute
- mapping schema [SQLXML], updategrams
- sql:inverse
ms.assetid: 2e266ed9-4cfb-434a-af55-d0839f64bb9a
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8c8643c40f7b62c0a0fdc3d85d32111d05ee955f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97405104"
---
# <a name="specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-40"></a>在 updategram 中指定带批注的映射架构 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用在 updategram 中指定的映射架构（XSD 或 XDR）来处理更新。 在 updategram 中，你可以提供要在将 updategram 中的元素和属性映射到中的表和列时使用的已注释映射架构的名称 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 在 updategram 中指定映射架构时，updategram 中指定的元素和属性名称必须映射为该映射架构的元素和属性。  
  
 若要指定映射架构，请使用元素的 **映射架构** 特性 **\<sync>** 。 以下示例显示两个 updategram：其中一个使用简单映射架构，而另一个使用较复杂的架构。  
  
> [!NOTE]  
>  本文档假定您熟悉 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的模板和映射架构支持。 有关详细信息，请参阅 [&#40;SQLXML 4.0&#41;中带批注的 XSD 架构简介 ](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md)。 对于使用 XDR 的旧版应用程序，请参阅 [SQLXML 4.0&#41;中 &#40;弃用的带批注 XDR 架构 ](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md)。  
  
## <a name="dealing-with-data-types"></a>处理数据类型  
 如果架构使用 sql： datatype 指定了 image、 **binary** 或 **varbinary** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型 () 并且未指定 xml 数据类型，则 updategram 将假定 xml 数据类型为 **二进制基数 64**。 如果数据为 **bin** ，则必须显式指定类型 (**dt： type = bin** 或 **Type = "xsd： hexBinary"**) 。  
  
 如果架构指定 **dateTime**、 **date** 或 **time** XSD 数据类型，则还必须 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用 **sql： datatype = "dateTime"** 指定相应的数据类型。  
  
 处理 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **money** 类型的参数时，必须在映射架构中的相应节点上显式指定 **sql： datatype = "money"** 。  
  
## <a name="examples"></a>示例  
 若要创建使用以下示例的工作示例，必须满足 [运行 SQLXML 示例的要求](../../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)中指定的要求。  
  
### <a name="a-creating-an-updategram-with-a-simple-mapping-schema"></a>A. 创建采用简单映射架构的 Updategram  
 以下 XSD 架构 ( # A0) 是将 **\<Customer>** 元素映射到 Customer 表的映射架构：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Customer" sql:relation="Sales.Customer" >  
   <xsd:complexType>  
        <xsd:attribute name="CustID"    
                       sql:field="CustomerID"   
                       type="xsd:string" />  
        <xsd:attribute name="RegionID"    
                       sql:field="TerritoryID"    
                       type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 以下 updategram 在 Sales.Customer 表中插入一条记录，并使用前面的映射架构将此数据正确映射到表。 请注意，updategram 使用架构中定义的相同元素名称 **\<Customer>** 。 这是强制性的，因为 updategram 会指定特定的架构。  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 SampleUpdateSchema.xml。  
  
2.  复制下面的 updategram 模板，然后将其粘贴到文本文件中。 在您保存 SampleUpdateSchema.xml 的相同目录中将文件另存为 SampleUpdategram.xml。  
  
    ```  
    <ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
      <updg:sync mapping-schema="SampleUpdateSchema.xml">  
        <updg:before>  
          <Customer CustID="1" RegionID="1"  />  
        </updg:before>  
        <updg:after>  
          <Customer CustID="1" RegionID="2" />  
        </updg:after>  
      </updg:sync>  
    </ROOT>  
    ```  
  
     为映射架构 (SampleUpdateSchema.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\SqlXmlTest\SampleUpdateSchema.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 这是等效的 XDR 架构：  
  
```  
<?xml version="1.0" ?>  
   <Schema xmlns="urn:schemas-microsoft-com:xml-data"   
         xmlns:dt="urn:schemas-microsoft-com:datatypes"   
         xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
     <ElementType name="Customer" sql:relation="Sales.Customer" >  
       <AttributeType name="CustID" />  
       <AttributeType name="RegionID" />  
  
       <attribute type="CustID" sql:field="CustomerID" />  
       <attribute type="RegionID" sql:field="TerritoryID" />  
     </ElementType>  
   </Schema>   
```  
  
### <a name="b-inserting-a-record-by-using-the-parent-child-relationship-specified-in-the-mapping-schema"></a>B. 使用在映射架构中指定的父子关系插入记录  
 架构元素可以相互关联。 **\<sql:relationship>** 元素指定架构元素之间的父子关系。 此信息用于更新具有主键/外键关系的相应表。  
  
 以下映射架构 ( # A0) 包含两个元素 **\<Order>** **\<OD>** ：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrderOD"  
          parent="Sales.SalesOrderHeader"  
          parent-key="SalesOrderID"  
          child="Sales.SalesOrderDetail"  
          child-key="SalesOrderID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="OD"   
                     sql:relation="Sales.SalesOrderDetail"  
                     sql:relationship="OrderOD" >  
           <xsd:complexType>  
              <xsd:attribute name="SalesOrderID"   type="xsd:integer" />  
              <xsd:attribute name="ProductID" type="xsd:integer" />  
             <xsd:attribute name="UnitPrice"  type="xsd:decimal" />  
             <xsd:attribute name="OrderQty"   type="xsd:integer" />  
             <xsd:attribute name="UnitPriceDiscount"   type="xsd:decimal" />  
  
           </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
        <xsd:attribute name="CustomerID"   type="xsd:string" />   
        <xsd:attribute name="SalesOrderID"  type="xsd:integer" />  
        <xsd:attribute name="OrderDate"  type="xsd:date" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 以下 updategram 使用此 XSD 架构为订单43860添加新的订单详细记录 (**\<OD>** 块) 中的元素 **\<after>** 。 **映射架构** 特性用于指定 updategram 中的映射架构。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync mapping-schema="SampleUpdateSchema.xml" >  
    <updg:before>  
       <Order SalesOrderID="43860" />  
    </updg:before>  
    <updg:after>  
      <Order SalesOrderID="43860" >  
           <OD ProductID="753" UnitPrice="$10.00"  
               Quantity="5" Discount="0.0" />  
      </Order>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 SampleUpdateSchema.xml。  
  
2.  复制上面的 Updategram 模板，并将它粘贴到文本文件中。 在您保存 SampleUpdateSchema.xml 的相同目录中将文件另存为 SampleUpdategram.xml。  
  
     为映射架构 (SampleUpdateSchema.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\SqlXmlTest\SampleUpdateSchema.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 这是等效的 XDR 架构：  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:dt="urn:schemas-microsoft-com:datatypes"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  
<ElementType name="OD" sql:relation="Sales.SalesOrderDetail" >  
    <AttributeType name="SalesOrderID" />  
    <AttributeType name="ProductID" />  
    <AttributeType name="UnitPrice"  dt:type="fixed.14.4" />  
    <AttributeType name="OrderQty" />  
    <AttributeType name="UnitPriceDiscount" />  
  
    <attribute type="SalesOrderID" />  
    <attribute type="ProductID" />  
    <attribute type="UnitPrice" />  
    <attribute type="OrderQty" />  
    <attribute type="UnitPriceDiscount" />  
</ElementType>  
  
<ElementType name="Order" sql:relation="Sales.SalesOrderHeader" >  
    <AttributeType name="CustomerID" />  
    <AttributeType name="SalesOrderID" />  
    <AttributeType name="OrderDate" />  
  
    <attribute type="CustomerID" />  
    <attribute type="SalesOrderID" />  
    <attribute type="OrderDate" />  
    <element type="OD" >  
             <sql:relationship   
                   key-relation="Sales.SalesOrderHeader"  
                   key="SalesOrderID"  
                   foreign-key="SalesOrderID"  
                   foreign-relation="Sales.SalesOrderDetail" />  
    </element>  
</ElementType>  
</Schema>  
```  
  
### <a name="c-inserting-a-record-by-using-the-parent-child-relationship-and-inverse-annotation-specified-in-the-xsd-schema"></a>C. 使用在 XSD 架构中指定的父子关系和反转批注插入记录  
 此示例演示了 updategram 逻辑如何使用 XSD 中指定的父子关系来处理更新，以及如何使用 **反转** 批注。 有关 **反** 批注的详细信息，请参阅 [在 sql： relationship 上指定 Sql：反向特性 &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-the-sql-inverse-attribute-on-sql-relationship-sqlxml-4-0.md)。  
  
 此示例假设以下表位于 **tempdb** 数据库中：  
  
-   `Cust (CustomerID, CompanyName)`，其中 `CustomerID` 为主键。  
  
-   `Ord (OrderID, CustomerID)`，其中 `CustomerID` 是引用 `CustomerID` 表中的 `Cust` 主键的外键。  
  
 updategram 使用以下 XSD 架构将记录插入到 Cust 和 Ord 表中：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
       <sql:relationship name="OrdCust" inverse="true"  
                  parent="Ord"  
                  parent-key="CustomerID"  
                  child-key="CustomerID"  
                  child="Cust"/>  
  </xsd:appinfo>  
</xsd:annotation>  
  
<xsd:element name="Order" sql:relation="Ord">  
  <xsd:complexType>  
    <xsd:sequence>  
      <xsd:element ref="Customer" sql:relationship="OrdCust"/>  
    </xsd:sequence>  
    <xsd:attribute name="OrderID"   type="xsd:int"/>  
    <xsd:attribute name="CustomerID" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:element>  
  
<xsd:element name="Customer" sql:relation="Cust">  
  <xsd:complexType>  
     <xsd:attribute name="CustomerID"  type="xsd:string"/>  
    <xsd:attribute name="CompanyName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:element>  
  
</xsd:schema>  
```  
  
 此示例中的 XSD 架构包含 **\<Customer>** 和 **\<Order>** 元素，它指定两个元素之间的父子关系。 它将标识 **\<Order>** 为父元素和 **\<Customer>** 子元素。  
  
 updategram 处理逻辑使用父子关系相关信息来确定将记录插入到表中的顺序。 在此示例中，updategram 逻辑将首先尝试将记录插入到 Ord 表中 (因为 **\<Order>** 是父级) ，然后尝试将记录插入到 "加入" 表中 (因为 **\<Customer>** 是子) 。 但是，鉴于数据库表架构中包含的主键/外键信息，此插入操作将导致在数据库中的外键冲突，继而导致插入失败。  
  
 若要指示 updategram 逻辑在更新操作过程中反转父子关系，请在元素上指定 **反转** 批注 **\<relationship>** 。 这会导致首先向 Cust 表添加记录，然后再向 Ord 表添加记录，并且该操作将成功。  
  
 以下 updategram 使用指定的 XSD 架构向 Ord 表插入订单 (OrderID=2)，并向 Cust 表插入客户 (CustomerID='AAAAA')：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync mapping-schema="SampleUpdateSchema.xml" >  
    <updg:before/>  
    <updg:after>  
      <Order OrderID="2" CustomerID="AAAAA" >  
        <Customer CustomerID="AAAAA" CompanyName="AAAAA Company" />  
      </Order>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  在 **tempdb** 数据库中创建以下表：  
  
    ```  
    USE tempdb  
    CREATE TABLE Cust(CustomerID varchar(5) primary key,   
                      CompanyName varchar(20))  
    GO  
    CREATE TABLE Ord (OrderID int primary key,   
                      CustomerID varchar(5) references Cust(CustomerID))  
    GO  
    ```  
  
2.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 SampleUpdateSchema.xml。  
  
3.  复制上面的 Updategram 模板，并将它粘贴到文本文件中。 在您保存 SampleUpdateSchema.xml 的相同目录中将文件另存为 SampleUpdategram.xml。  
  
     为映射架构 (SampleUpdateSchema.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\SqlXmlTest\SampleUpdateSchema.xml"  
    ```  
  
4.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;SQLXML 4.0&#41;的 Updategram 安全注意事项 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
