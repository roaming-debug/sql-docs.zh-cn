---
title: '在 sql 中创建常量元素：是常量 (SQLXML) '
description: 了解如何使用 SQLXML 4.0 中的 sql： is 常量批注在不映射到任何数据库表或列的 XSD 架构中创建常量元素。
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- element does not map [SQLXML]
- sql:is-constant
- XSD schemas [SQLXML], constant elements
- element mapping [SQLXML], constant elements
- is-constant annotation
- constant elements [SQLXML]
- annotated XSD schemas, constant elements
ms.assetid: 940eea1b-54f5-445f-b844-c894d9f3941b
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 952c7e905e40e869d459f1ada3225d5524b3d799
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461818"
---
# <a name="creating-constant-elements-using-sqlis-constant-sqlxml-40"></a>使用 sql:is-constant 创建常量元素 (SQLXML 4.0)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  若要指定常量元素，即 XSD 架构中未映射到任何数据库表或列的元素，可以使用 **sql： is 常量** 批注。 该批注取布尔值（0 = false，1 = true）。 可接受的值为 0、1、true 和 false。 可以在没有任何属性的元素上指定 **sql： is 常量** 批注。 如果使用值 true（或 1）在元素中指定该批注，则该元素不会被映射到数据库，但仍出现在 XML 文档中。  
  
 **Sql： is 常量** 批注可用于：  
  
-   将顶级元素添加到 XML 文档。 XML 要求为文档提供一个顶级元素（根元素）。  
  
-   创建容器元素，例如 **\<Orders>** 包装所有订单的元素。  
  
 可以将 **sql： is 常量** 批注添加到 **\<complexType>** 元素。  
  
## <a name="examples"></a>示例  
 若要创建使用以下示例的工作示例，必须满足某些要求。 有关详细信息，请参阅 [运行 SQLXML 示例的要求](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)。  
  
### <a name="a-specifying-sqlis-constant-to-add-a-container-element"></a>A. 指定 sql:is-constant 以添加容器元素  
 在此带批注的 XSD 架构中， **\<CustomerOrders>** 通过指定值为1的 **sql： is 常量** 属性将定义为常量元素。 因此， **\<CustomerOrders>** 不会映射到任何数据库表或列。 此常量元素由 **\<Order>** 子元素组成。  
  
 尽管不 **\<CustomerOrders>** 会映射到任何数据库表或列，但它仍将作为包含子元素的容器元素出现在生成的 XML 中 **\<Order>** 。  
  
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
  
  <xsd:element name="Customer" sql:relation="Sales.Customer" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="CustomerOrders" sql:is-constant="1" >  
          <xsd:complexType>  
            <xsd:sequence>  
              <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader"  
                           sql:relationship="CustOrders"   
                           maxOccurs="unbounded" >  
                <xsd:complexType>  
                   <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
                   <xsd:attribute name="OrderDate" type="xsd:date" />  
                   <xsd:attribute name="CustomerID" type="xsd:string" />  
                </xsd:complexType>  
              </xsd:element>  
            </xsd:sequence>  
           </xsd:complexType>  
          </xsd:element>  
         </xsd:sequence>  
          <xsd:attribute name="CustomerID" type="xsd:string" />  
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将该文件另存为 isConstant.xml。  
  
2.  复制以下模板，并将它粘贴到文本文件中。 在保存 isConstant.xml 的同一目录中将该文件另存为 isConstantT.xml。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="isConstant.xml">  
            Customer[@CustomerID=1]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (isConstant.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\MyDir\isConstant.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  

     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 部分结果集如下：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">   
<Customer CustomerID="1">   
  <CustomerOrders>   
    <Order SalesOrderID="43860" OrderDate="2001-08-01" CustomerID="1" />   
    <Order SalesOrderID="44501" OrderDate="2001-11-01" CustomerID="1" />   
    <Order SalesOrderID="45283" OrderDate="2002-02-01" CustomerID="1" />   
    <Order SalesOrderID="46042" OrderDate="2002-05-01" CustomerID="1" />   
    ...  
  </CustomerOrders>   
</Customer>   
</ROOT>  
```  
  
  
