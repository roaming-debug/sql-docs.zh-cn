---
title: " (SQLXML) 自定义 XSD 映射到表/列"
description: 了解如何在用于 XSD 架构的元素和属性与关系数据库的表和列之间的 SQLXML XPath 查询中创建自定义映射。
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- explicit schema mapping [SQLXML]
- XPath queries [SQLXML], annotated XSD schemas in queries
- sql:field
- row mapping [SQLXML]
- attribute mapping [SQLXML], explicit mapping
- field annotation
- XSD schemas [SQLXML], mapping attributes and elements
- names [SQLXML]
- relation annotation
- table/view mapping [SQLXML], explicit mapping
- sql:relation
- mapping schema [SQLXML], explicit mapping
- annotated XSD schemas, mapping attributes and elements
- column mapping [SQLXML]
- element mapping [SQLXML], explicit mapping
- table mapping [SQLXML], explicit mapping
- element/attribute mapping [SQLXML]
ms.assetid: 7a5ebeb6-7322-4141-a307-ebcf95976146
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 988a15524f4d0fdbdd3174ba1017dfd85562e3a6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97415798"
---
# <a name="custom-xsd-mappings-to-tablescolumns-sqlxml"></a> (SQLXML) 自定义 XSD 映射到表/列
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  当使用 XSD 架构提供关系数据库的 XML 视图时，必须将该架构的元素和属性映射至数据库的表和列。 数据库表/视图中的行将映射至 XML 文档中的元素。 数据库中的列值映射到属性或元素。  
  
 当针对带批注的 XSD 架构指定 XPath 查询时，将从该架构中的元素和属性的数据映射到的表和列中检索这些数据。 若要从数据库中获取单个值，XSD 架构中指定的映射必须同时具备关系和字段规范。 如果元素/属性的名称与它所映射到的表/视图或列名称不同，则使用 **sql： relation** 和 **sql： FIELD** 批注指定 XML 文档中的元素或属性与表 (视图中的表) 或数据库中的列的映射。  
  
## <a name="sql-relation"></a>sql-relation  
 添加了 **sql： relation** 批注，以便将 XSD 架构中的 XML 节点映射到数据库表。  (视图) 的表的名称被指定为 **sql： relation** 批注的值。  
  
 在元素上指定 **sql： relation** 时，此批注的范围适用于该元素的复杂类型定义中所述的所有属性和子元素，因此在编写批注时提供了一个快捷方式。  
  
 当中有效的标识符在 XML 中无效时， **sql：关系** 批注也很有用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 例如，“Order Details”是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的有效表名，但该名称在 XML 中无效。 在这种情况下，可以使用 **sql： relation** 批注指定映射，例如：  
  
```  
<xsd:element name="OD" sql:relation="[Order Details]">  
```  
  
## <a name="sql-field"></a>sql-field  
 **Sql 字段** 批注将元素或属性映射到数据库列。 添加了 **sql：字段** 批注，以便将架构中的 XML 节点映射到数据库列。 不能在空内容元素上指定 **sql：字段** 。  
  
## <a name="examples"></a>示例  
 若要创建使用以下示例的工作示例，必须满足某些要求。 有关详细信息，请参阅 [运行 SQLXML 示例的要求](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)。  
  
### <a name="a-specifying-the-sqlrelation-and-sqlfield-annotations"></a>A. 指定 sql:relation 和 sql:field 批注  
 在此示例中，XSD 架构包含 **\<Contact>** 带有和子元素的复杂类型的元素 **\<FName>** **\<LName>** 和 **ContactID** 属性。  
  
 **Sql： relation** 批注将元素映射 **\<Contact>** 到 AdventureWorks 数据库中的 Person 表。 **Sql： field** 批注将元素映射 **\<FName>** 到 FirstName 列，并将 **\<LName>** 元素映射到 LastName 列。  
  
 没有为 **ContactID** 属性指定批注。 这导致将属性默认映射为具有相同名称的列。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Contact" sql:relation="Person.Contact" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="FName"  
                     sql:field="FirstName"   
                     type="xsd:string" />   
        <xsd:element name="LName"    
                     sql:field="LastName"    
                     type="xsd:string" />  
     </xsd:sequence>  
        <xsd:attribute name="ContactID"   
                       type="xsd:integer" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 MySchema-annotated.xml。  
  
2.  复制下面的模板，然后将其粘贴到文本文件中。 在您保存 MySchema-annotated.xml 的相同目录中将该文件另存为 MySchema-annotatedT.xml。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="MySchema-annotated.xml">  
        /Contact  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (MySchema-annotated.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\SqlXmlTest\MySchema-annotated.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 下面是部分结果集：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">   
 <Contact ContactID="1">   
    <FName>Gustavo</FName>   
    <LName>Achong</LName>   
 </Contact>   
  .....  
</ROOT>  
```  
  
  
