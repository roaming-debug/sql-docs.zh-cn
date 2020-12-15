---
title: '设置与 sql： relationship 的关系 (SQLXML) '
description: 了解如何使用 SQLXML 4.0 中的 sql： relationship 批注指定 XML 元素之间的关系。
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- IDREFS relationships [SQLXML]
- parent attribute [SQLXML]
- element relationships [SQLXML]
- multiple element relationships
- attribute relationships [SQLXML]
- sql:relationship
- relationship chains [SQLXML]
- IDREF relationships [SQLXML]
- parent-child relationships [SQLXML]
- relationship annotation
- XSD schemas [SQLXML], relationships
- annotated XSD schemas, relationships
- relationships [SQLXML], specifying
- unnamed relationships
- ID relationships [SQLXML]
- hierarchical relationships [SQLXML]
- named relationships [SQLXML]
ms.assetid: 98820afa-74e1-4e62-b336-6111a3dede4c
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6c050a6b34781fc6312e7d410aeccace29e3dc84
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461758"
---
# <a name="specifying-relationships-using-sqlrelationship-sqlxml-40"></a>使用 sql:relationship 指定关系 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  可以对 XML 文档中的元素建立相关性。 元素可以按层次结构方式嵌套，并且可以在元素之间指定 ID、IDREF 或 IDREFS 关系。  
  
 例如，在 XSD 架构中， **\<Customer>** 元素包含 **\<Order>** 子元素。 将架构映射到 AdventureWorks 数据库后， **\<Customer>** 元素映射到 Customer 表， **\<Order>** 元素映射到 SalesOrderHeader 表。 由于是由客户下订单，因此，基础表 Sales.Customer 和 Sales.SalesOrderHeader 是相关的。 Sales.SalesOrderHeader 表中的 CustomerID 是外键，它引用 Sales.Customer 表中的 CustomerID 主键。 可以通过使用 **sql： relationship** 批注在映射架构元素之间建立这些关系。  
  
 在带批注的 XSD 架构中， **sql： relationship** 批注用于根据元素所映射到的基础表中的主键和外键关系，按层次结构嵌套架构元素。 在指定 **sql： relationship** 批注时，必须确定以下各项：  
  
-   父表 (Sales.Customer) 和子表 (Sales.SalesOrderHeader)。  
  
-   组成父表与子表间关系的一列或多列。 例如，CustomerID 列，该列同时出现在父表和子表中。  
  
 此信息用于生成适当的层次结构。  
  
 为了提供表名和必需的联接信息，以下属性在 **sql： relationship** 批注上指定。 这些属性仅对于 **\<sql:relationship>** 元素有效：  
  
 名称  
 指定关系的唯一名称。  
  
 **父级**  
 指定父关系（表）。 这是一个可选属性；如果未指定此属性，将从文档的子层次结构中的信息获得父表名称。 如果架构指定了两个父子层次结构，它们使用相同 **\<sql:relationship>** 但不同的父元素，则不在中指定父属性 **\<sql:relationship>** 。 此信息将从架构的层次结构中获得。  
  
 **父键**  
 指定父项的父键。 如果父键由多列组成，则指定值时应在各值之间使用空格。 在为多列键指定的值与为对应的子键指定的值之间存在位置映射。  
  
 **子级**  
 指定子关系（表）。  
  
 **child-key**  
 在引用父项中父键的子项中指定子键。 如果子键由多个属性（列）组成，则指定子键值时应在各值之间使用空格。 在为多列键指定的值与为对应的父键指定的值之间存在位置映射。  
  
 **反转**  
 此上指定的属性 **\<sql:relationship>** 由 updategram 使用。 有关详细信息，请参阅 [在 sql： relationship 上指定 sql：反向特性](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-the-sql-inverse-attribute-on-sql-relationship-sqlxml-4-0.md)。  
  
 必须 **在包含** 子元素的元素（该元素具有在 **\<sql:relationship>** 元素和子元素之间定义）的元素中指定，并且该元素不提供父元素中指定的表的主键。 即使架构未指定 **\<sql:relationship>** ，也必须指定 **sql： key-字段** 以生成适当的层次结构。 有关详细信息，请参阅 [使用 sql： key-字段标识键列](../../relational-databases/sqlxml-annotated-xsd-schemas-using/identifying-key-columns-using-sql-key-fields-sqlxml-4-0.md)。  
  
 若要在结果中生成正确的嵌套，建议在所有架构中指定 **sql：键字段** 。  
  
## <a name="examples"></a>示例  
 若要创建使用以下示例的工作示例，必须满足某些要求。 有关详细信息，请参阅 [运行 SQLXML 示例的要求](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)。  
  
### <a name="a-specifying-the-sqlrelationship-annotation-on-an-element"></a>A. 针对元素指定 sql:relationship 批注  
 以下带批注的 XSD 架构包含 **\<Customer>** 和 **\<Order>** 元素。 **\<Order>** 元素是元素的子元素 **\<Customer>** 。  
  
 在架构中，在子元素上指定 **sql： relationship** 批注 **\<Order>** 。 关系本身是在元素中定义的 **\<xsd:appinfo>** 。  
  
 **\<relationship>** 元素将 SalesOrderHeader 表中的 CustomerID 标识为一个外键，该外键引用 Customer 表中的 customerid 主键。 因此，属于某个客户的订单将显示为该元素的子元素 **\<Customer>** 。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustOrders"  
          parent="Sales.Customer"  
          parent-key="CustomerID"  
          child="Sales.SalesOrderHeader"  
          child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customer" sql:relation="Sales.Customer" type="CustomerType" />  
   <xsd:complexType name="CustomerType" >  
     <xsd:sequence>  
        <xsd:element name="Order"   
                     sql:relation="Sales.SalesOrderHeader"  
                    sql:relationship="CustOrders" >  
           <xsd:complexType>  
              <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
              <xsd:attribute name="CustomerID" type="xsd:string" />  
           </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
        <xsd:attribute name="CustomerID"   type="xsd:string" />   
    </xsd:complexType>  
  
</xsd:schema>  
```  
  
 前一个架构使用命名关系。 还可以指定未命名关系。 结果是相同的。  
  
 这是一个经修订的架构，在其中指定未命名关系：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  
  <xsd:element name="Customer" sql:relation="Sales.Customer"  type="CustomerType" />  
   <xsd:complexType name="CustomerType" >  
     <xsd:sequence>  
        <xsd:element name="Order"   
                     sql:relation="Sales.SalesOrderHeader">  
           <xsd:annotation>  
            <xsd:appinfo>  
              <sql:relationship   
                parent="Sales.Customer"  
                parent-key="CustomerID"  
                child="Sales.SalesOrderHeader"  
                child-key="CustomerID" />  
            </xsd:appinfo>  
           </xsd:annotation>  
           <xsd:complexType>  
              <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
              <xsd:attribute name="CustomerID" type="xsd:string" />  
           </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
        <xsd:attribute name="CustomerID"   type="xsd:string" />   
    </xsd:complexType>  
  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 sql-relationship.xml。  
  
2.  复制下面的模板，然后将其粘贴到文本文件中。 在您保存 sql-relationship.xml 的相同目录中将该文件另存为 sql-relationshipT.xml。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="sql-relationship.xml">  
            /Customer[@CustomerID=1]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (sql-relationship.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\MyDir\sql-relationship.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 下面是结果集：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">   
  <Customer CustomerID="1">   
    <Order OrderID="43860" CustomerID="1" />   
    <Order OrderID="44501" CustomerID="1" />   
    <Order OrderID="45283" CustomerID="1" />   
    <Order OrderID="46042" CustomerID="1" />   
  </Customer>   
</ROOT>  
```  
  
### <a name="b-specifying-a-relationship-chain"></a>B. 指定关系链  
 对于本示例，假定您需要以下 XML 文档，该文档使用从 AdventureWorks 数据库获得的数据：  
  
```  
<Order SalesOrderID="43659">  
  <Product Name="Mountain Bike Socks, M"/>   
  <Product Name="Sport-100 Helmet, Blue"/>  
  ...  
</Order>  
...  
```  
  
 对于 SalesOrderHeader 表中的每个订单，XML 文档都有一个 **\<Order>** 元素。 每个 **\<Order>** 元素都有一个 **\<Product>** 子元素列表，每个元素对应于按顺序请求的每个产品。  
  
 若要指定将生成此层次结构的 XSD 架构，您必须指定两个关系：OrderOD 和 ODProduct。 OrderOD 关系在 Sales.SalesOrderHeader 表与 Sales.SalesOrderDetail 表之间指定父子关系。 ODProduct 关系指定 Sales.SalesOrderDetail 表与 Production.Product 表之间的关系。  
  
 在下面的架构中，元素上的 **msdata： relationship** 批注 **\<Product>** 指定了两个值： OrderOD 和 ODProduct。 指定这些值的顺序非常重要。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:msdata="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <msdata:relationship name="OrderOD"  
          parent="Sales.SalesOrderHeader"  
          parent-key="SalesOrderID"  
          child="Sales.SalesOrderDetail"  
          child-key="SalesOrderID" />  
  
    <msdata:relationship name="ODProduct"  
          parent="Sales.SalesOrderDetail"  
          parent-key="ProductID"  
          child="Production.Product"  
          child-key="ProductID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Order" msdata:relation="Sales.SalesOrderHeader"   
               msdata:key-fields="SalesOrderID" type="OrderType" />  
   <xsd:complexType name="OrderType" >  
     <xsd:sequence>  
        <xsd:element name="Product" msdata:relation="Production.Product"   
                     msdata:key-fields="ProductID"  
                     msdata:relationship="OrderOD ODProduct">  
          <xsd:complexType>  
             <xsd:attribute name="Name" type="xsd:string" />  
          </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
        <xsd:attribute name="SalesOrderID"   type="xsd:integer" />   
    </xsd:complexType>  
</xsd:schema>  
```  
  
 您可以指定匿名关系，而不指定命名关系。 在这种情况下， **\<annotation>** **\</annotation>** 描述两个关系的全部内容将显示为的子元素 **\<Product>** 。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:msdata="urn:schemas-microsoft-com:mapping-schema">  
  
  <xsd:element name="Order" msdata:relation="Sales.SalesOrderHeader"   
               msdata:key-fields="SalesOrderID" type="OrderType" />  
  
   <xsd:complexType name="OrderType" >  
     <xsd:sequence>  
        <xsd:element name="Product" msdata:relation="Production.Product"   
                     msdata:key-fields="ProductID" >  
         <xsd:annotation>  
          <xsd:appinfo>  
           <msdata:relationship   
               parent="Sales.SalesOrderHeader"  
               parent-key="SalesOrderID"  
               child="Sales.SalesOrderDetail"  
               child-key="SalesOrderID" />  
  
           <msdata:relationship   
               parent="Sales.SalesOrderDetail"  
               parent-key="ProductID"  
               child="Production.Product"  
               child-key="ProductID" />  
         </xsd:appinfo>  
       </xsd:annotation>  
       <xsd:complexType>  
          <xsd:attribute name="Name" type="xsd:string" />  
       </xsd:complexType>  
     </xsd:element>  
   </xsd:sequence>  
   <xsd:attribute name="SalesOrderID"   type="xsd:integer" />   
  </xsd:complexType>  
 </xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 relationshipChain.xml。  
  
2.  复制下面的模板，然后将其粘贴到文本文件中。 在您保存 relationshipChain.xml 的相同目录中将该文件另存为 relationshipChainT.xml。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="relationshipChain.xml">  
            /Order  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (relationshipChain.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\MyDir\relationshipChain.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 下面是结果集：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">   
  <Order SalesOrderID="43659">  
    <Product Name="Mountain Bike Socks, M" />   
    <Product Name="Sport-100 Helmet, Blue" />   
    <Product Name="AWC Logo Cap" />   
    <Product Name="Long-Sleeve Logo Jersey, M" />   
    <Product Name="Long-Sleeve Logo Jersey, XL" />   
    ...  
  </Order>  
  ...  
</ROOT>  
```  
  
### <a name="c-specifying-the-relationship-annotation-on-an-attribute"></a>C. 针对属性指定关系批注  
 此示例中的架构包含一个 \<Customer> 元素，该元素具有一个 \<CustomerID> 子元素和一个 IDREFS 类型的 OrderIDList 属性。 \<Customer>元素映射到 AdventureWorks 数据库中的 Customer 表。 默认情况下，此映射的作用域应用于所有子元素或属性，除非在子元素或属性上指定了 **sql：** relationship，在这种情况下，必须使用元素定义适当的主键/外键关系 \<relationship> 。 另外，使用 **关系** 批注指定不同表的子元素或属性还必须指定 **关系** 注释。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustOrders"  
          parent="Sales.Customer"  
          parent-key="CustomerID"  
          child="Sales.SalesOrderHeader"  
          child-key="CustomerID" />  
     </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customer" sql:relation="Sales.Customer" type="CustomerType" />  
   <xsd:complexType name="CustomerType" >  
     <xsd:sequence>  
        <xsd:element name="CustomerID"   type="xsd:string" />   
     </xsd:sequence>  
     <xsd:attribute name="OrderIDList"   
                     type="xsd:IDREFS"   
                     sql:relation="Sales.SalesOrderHeader"   
                     sql:field="SalesOrderID"  
                     sql:relationship="CustOrders" >  
        </xsd:attribute>  
    </xsd:complexType>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 relationship-on-attribute.xml。  
  
2.  复制以下模板并将其粘贴到文件中。 在您保存 relationship-on-attribute.xml 的相同目录中将该文件另存为 relationship-on-attributeT.xml。 模板中的查询选择 CustomerID 为 1 的客户。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="relationship-on-attribute.xml">  
        /Customer[CustomerID=1]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (relationship-on-attribute.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\MyDir\relationship-on-attribute.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 下面是结果集：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">   
  <Customer OrderIDList="43860 44501 45283 46042">  
    <CustomerID>1</CustomerID>   
  </Customer>  
</ROOT>  
```  
  
### <a name="d-specifying-sqlrelationship-on-multiple-elements"></a>D. 针对多个元素指定 sql:relationship  
 在此示例中，带批注的 XSD 架构包含 **\<Customer>** 、 **\<Order>** 和 **\<OrderDetail>** 元素。  
  
 **\<Order>** 元素是元素的子元素 **\<Customer>** 。 **\<sql:relationship>** 在 **\<Order>** 子元素上指定，因此，属于某个客户的订单将显示为的子元素 **\<Customer>** 。  
  
 **\<Order>** 元素包含 **\<OrderDetail>** 子元素。 **\<sql:relationship>** 在 **\<OrderDetail>** 子元素上指定，因此，属于某个订单的订单详细信息将显示为该元素的子元素 **\<Order>** 。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustOrders"  
        parent="Sales.Customer"  
        parent-key="CustomerID"  
        child="Sales.SalesOrderHeader"  
        child-key="CustomerID" />  
  
    <sql:relationship name="OrderOrderDetail"  
        parent="Sales.SalesOrderHeader"  
        parent-key="SalesOrderID"  
        child="Sales.SalesOrderDetail"  
        child-key="SalesOrderID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customer" sql:relation="Sales.Customer" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader"    
              sql:relationship="CustOrders" maxOccurs="unbounded" >  
          <xsd:complexType>  
              <xsd:sequence>  
                <xsd:element name="OrderDetail"   
                             sql:relation="Sales.SalesOrderDetail"   
                             sql:relationship="OrderOrderDetail"   
                             maxOccurs="unbounded" >  
                  <xsd:complexType>  
                    <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
                    <xsd:attribute name="ProductID" type="xsd:string" />  
                    <xsd:attribute name="OrderQty" type="xsd:integer" />  
                  </xsd:complexType>  
                </xsd:element>  
              </xsd:sequence>  
              <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
              <xsd:attribute name="OrderDate" type="xsd:date" />  
              <xsd:attribute name="CustomerID" type="xsd:string" />  
          </xsd:complexType>  
        </xsd:element>  
      </xsd:sequence>  
      <xsd:attribute name="CustomerID" type="xsd:string" />  
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 relationship-multiple-elements.xml。  
  
2.  复制以下模板，并将它粘贴到文本文件中。 在您保存 relationship-multiple-elements.xml 的相同目录中将该文件另存为 relationship-multiple-elementsT.xml。 模板中的查询将返回 CustomerID 为 1 且 SalesOrderID 为 43860 的客户的订单信息。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="relationship-multiple-elements.xml">  
        /Customer[@CustomerID=1]/Order[@SalesOrderID=43860]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (relationship-multiple-elements.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\MyDir\relationship-multiple-elements.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 下面是结果集：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Order SalesOrderID="43860" OrderDate="2001-08-01" CustomerID="1">  
     <OrderDetail SalesOrderID="43860" ProductID="761" OrderQty="2" />   
     <OrderDetail SalesOrderID="43860" ProductID="770" OrderQty="1" />   
     <OrderDetail SalesOrderID="43860" ProductID="758" OrderQty="2" />   
     <OrderDetail SalesOrderID="43860" ProductID="765" OrderQty="2" />   
     <OrderDetail SalesOrderID="43860" ProductID="732" OrderQty="1" />   
     <OrderDetail SalesOrderID="43860" ProductID="762" OrderQty="1" />   
     <OrderDetail SalesOrderID="43860" ProductID="738" OrderQty="1" />   
     <OrderDetail SalesOrderID="43860" ProductID="768" OrderQty="1" />   
     <OrderDetail SalesOrderID="43860" ProductID="753" OrderQty="2" />   
     <OrderDetail SalesOrderID="43860" ProductID="729" OrderQty="1" />   
     <OrderDetail SalesOrderID="43860" ProductID="763" OrderQty="1" />   
     <OrderDetail SalesOrderID="43860" ProductID="756" OrderQty="1" />   
  </Order>  
</ROOT>  
```  
  
### <a name="e-specifying-the-sqlrelationship-without-the-parent-attribute"></a>E. 指定 \<sql:relationship> 无父属性  
 此示例阐释了如何指定， **\<sql:relationship>** 而没有 **父** 属性。 例如，假设您具有以下员工表：  
  
```  
Emp1(SalesPersonID, FirstName, LastName, ReportsTo)  
Emp2(SalesPersonID, FirstName, LastName, ReportsTo)  
```  
  
 下面的 XML 视图具有 **\<Emp1>** **\<Emp2>** 映射到 Emp1 和 Emp2 表的和元素：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="EmpOrders"  
          parent-key="SalesPersonID"  
          child="Sales.SalesOrderHeader"  
          child-key="SalesPersonID" />  
     </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Emp1" sql:relation="Sales.Emp1" type="EmpType" />  
  <xsd:element name="Emp2" sql:relation="Sales.Emp2" type="EmpType" />  
   <xsd:complexType name="EmpType" >  
     <xsd:sequence>  
        <xsd:element name="Order"   
                     sql:relation="Sales.SalesOrderHeader"   
                     sql:relationship="EmpOrders" >  
          <xsd:complexType>  
             <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
             <xsd:attribute name="CustomerID" type="xsd:string" />  
          </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
        <xsd:attribute name="SalesPersonID"   type="xsd:integer" />   
        <xsd:attribute name="LastName"   type="xsd:string" />   
    </xsd:complexType>  
  
</xsd:schema>  
```  
  
 在架构中， **\<Emp1>** 元素和 **\<Emp2>** 元素都是 **EmpType** 类型。 类型 **EmpType** 描述了 **\<Order>** 子元素和相应的 **\<sql:relationship>** 。 在这种情况下，不存在可以 **\<sql:relationship>** 使用 **parent** 属性在中标识的单个父。 在这种情况下，你不能在中指定 **父** 属性，而 **\<sql:relationship>** 是从架构的层次结构中获取 **父** 属性信息。  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  在 AdventureWorks 数据库中创建这些表：  
  
    ```  
    USE AdventureWorks  
    CREATE TABLE Sales.Emp1 (  
           SalesPersonID int primary key,   
           FirstName  varchar(20),   
           LastName   varchar(20),   
           ReportsTo int)  
    Go  
    CREATE TABLE Sales.Emp2 (  
           SalesPersonID int primary key,   
           FirstName  varchar(20),   
           LastName   varchar(20),   
           ReportsTo int)  
    Go  
    ```  
  
2.  在这些表中添加此示例数据：  
  
    ```  
    INSERT INTO Sales.Emp1 values (279, 'Nancy', 'Devolio',NULL)  
    INSERT INTO Sales.Emp1 values (282, 'Andrew', 'Fuller',1)  
    INSERT INTO Sales.Emp1 values (276, 'Janet', 'Leverling',1)  
    INSERT INTO Sales.Emp2 values (277, 'Margaret', 'Peacock',3)  
    INSERT INTO Sales.Emp2 values (283, 'Steven', 'Devolio',4)  
    INSERT INTO Sales.Emp2 values (275, 'Nancy', 'Buchanan',5)  
    INSERT INTO Sales.Emp2 values (281, 'Michael', 'Suyama',6)  
    ```  
  
3.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 relationship-noparent.xml。  
  
4.  复制以下模板，并将它粘贴到文本文件中。 在您保存 relationship-noparent.xml 的相同目录中将该文件另存为 relationship-noparentT.xml。 模板中的查询选择 (的所有 \<Emp1> 元素，父项为 Emp1) 。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="relationship-noparent.xml">  
            /Emp1  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (relationship-noparent.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\MyDir\relationship-noparent.xml"  
    ```  
  
5.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 以下为部分结果集：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
<Emp1 SalesPersonID="276" LastName="Leverling">  
  <Order SalesOrderID="43663" CustomerID="510" />   
  <Order SalesOrderID="43666" CustomerID="511" />   
  <Order SalesOrderID="43859" CustomerID="259" />  
  ...  
</Emp1>  
```  
  
  
