---
title: '用 sql： datatype 转换数据类型 (SQLXML) '
description: 了解如何使用 SQLXML 4.0 中的 xsd： type 和 sql： datatype 特性来控制 XSD 数据类型与 SQL Server 数据类型之间的映射。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- mapping data types [SQLXML]
- type attribute
- sql:datatype
- data types [SQLXML], converting
- annotated XSD schemas, mapping data types
- xsd:type
- datatype annotation
- converting data types [SQLXML]
- data types [SQLXML], mapping data types
- XSD schemas [SQLXML], mapping data types
ms.assetid: db192105-e8aa-4392-b812-9d727918c005
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0e457aa6c1d1e16e96682f19898e22813254e9ef
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461798"
---
# <a name="data-type-conversions-and-the-sqldatatype-annotation-sqlxml-40"></a>数据类型转换和 sql： datatype 批注 (SQLXML 4.0) 
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  在 XSD 架构中， **xsd： type** 特性指定元素或属性的 XSD 数据类型。 在 XSD 架构用于从数据库中提取数据时，指定的数据类型用于将数据格式化。  
  
 除了在架构中指定 XSD 类型之外，还可以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 **sql： datatype** 批注指定 Microsoft 数据类型。 **Xsd： type** 和 **sql： DATATYPE** 特性控制 xsd 数据类型和数据类型之间的映射 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="xsdtype-attribute"></a>xsd:type 属性  
 您可以使用 **xsd： type** 特性来指定映射到列的属性或元素的 XML 数据类型。 **Xsd： type** 会影响从服务器返回的文档以及执行的 XPath 查询。 对包含 **xsd： type** 的映射架构执行 xpath 查询时，xpath 在处理查询时使用指定的数据类型。 有关 XPath 如何使用 **xsd： type** 的详细信息，请参阅 [将 Xsd 数据类型映射到 xpath 数据类型 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/mapping-xsd-data-types-to-xpath-data-types-sqlxml-4-0.md)。  
  
 在返回的文档中，所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型都转换为字符串表示形式。 某些数据类型需要其他转换。 下表列出了用于各种 **xsd： type** 值的转换。  
  
|XSD 数据类型|SQL Server 转换|  
|-------------------|---------------------------|  
|Boolean|CONVERT(bit, COLUMN)|  
|Date|LEFT(CONVERT(nvarchar(4000), COLUMN, 126), 10)|  
|Decimal|CONVERT(money, COLUMN)|  
|id/idref/idrefs|id-prefix + CONVERT(nvarchar(4000), COLUMN, 126)|  
|nmtoken/nmtokens|id-prefix + CONVERT(nvarchar(4000), COLUMN, 126)|  
|时间|SUBSTRING(CONVERT(nvarchar(4000), COLUMN, 126), 1+CHARINDEX(N'T', CONVERT(nvarchar(4000), COLUMN, 126)), 24)|  
|所有其他|无其他转换|  
  
> [!NOTE]  
>  返回的某些值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能与使用 **xsd： type** 指定的 XML 数据类型不兼容，这可能是因为转换不可能 (例如，将 "XYZ" 转换为 **decimal** 数据类型) ，或是因为值超出了该数据类型的范围 (例如，-100000 转换为 **UnsignedShort** xsd 类型) 。 不兼容的类型转换可能导致无效的 XML 文档或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误。  
  
## <a name="mapping-from-sql-server-data-types-to-xsd-data-types"></a>从 SQL Server 数据类型映射到 XSD 数据类型  
 下表显示从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型到 XSD 数据类型的显式映射。 如果您知道 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 类型，此表将提供可以在 XSD 架构中指定的相应 XSD 类型。  
  
|SQL Server 数据类型|XSD 数据类型|  
|--------------------------|-------------------|  
|**bigint**|**long**|  
|**binary**|**base64Binary**|  
|**bit**|**boolean**|  
|**char**|**string**|  
|**datetime**|**dateTime**|  
|**decimal**|**decimal**|  
|**float**|**double**|  
|**image**|**base64Binary**|  
|**int**|**int**|  
|**money**|**decimal**|  
|**nchar**|**string**|  
|**ntext**|**string**|  
|**nvarchar**|**string**|  
|**numeric**|**decimal**|  
|**real**|**float**|  
|**smalldatetime**|**dateTime**|  
|**smallint**|**short**|  
|**smallmoney**|**decimal**|  
|**sql_variant**|**string**|  
|**sysname**|**string**|  
|**text**|**string**|  
|**timestamp**|**dateTime**|  
|**tinyint**|**unsignedByte**|  
|**varbinary**|**base64Binary**|  
|**varchar**|**string**|  
|**uniqueidentifier**|**string**|  
  
## <a name="sqldatatype-annotation"></a>sql:datatype 批注  
 **Sql： datatype** 批注用于指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型; 在下列情况下，必须指定此批注：  
  
-   将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从 XSD **日期时间**、**日期** 或 **时间** 类型大容量加载到 dateTime 列。 在这种情况下，必须 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 **sql： datatype = "dateTime"** 标识列数据类型。 此规则也适用于 updategram。  
  
-   要大容量加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **uniqueidentifier** 类型的列中，并且 XSD 值是包含大括号 ( {and} ) 的 GUID。 指定 **sql： datatype = "uniqueidentifier"** 时，会先从值中删除大括号，然后再将其插入列中。 如果未指定 **sql： datatype** ，则用大括号发送该值，并且 insert 或 update 会失败。  
  
-   XML 数据类型 **base64Binary** 映射到各种 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型 (**二进制**、 **图像** 或 **varbinary**) 。 若要将 XML 数据类型 **base64Binary** 映射到特定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型，请使用 **sql： datatype** 批注。 此批注指定属性要映射到的列的显式 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。 当正在数据库中存储数据时，这很有用。 通过指定 **sql： datatype** 批注，可以标识显式 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。  
  
 通常建议在架构中指定 **sql： datatype** 。  
  
## <a name="examples"></a>示例  
 若要创建使用以下示例的工作示例，必须满足某些要求。 有关详细信息，请参阅 [运行 SQLXML 示例的要求](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)。  
  
### <a name="a-specifying-xsdtype"></a>A. 指定 xsd:type  
 此示例演示如何使用架构中的 **xsd： type** 特性指定的 xsd **日期** 类型会影响生成的 XML 文档。 该架构提供 AdventureWorks 数据库中 Sales.SalesOrderHeader 表的 XML 视图。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader">  
     <xsd:complexType>  
       <xsd:attribute name="SalesOrderID" type="xsd:string" />   
       <xsd:attribute name="CustomerID"   type="xsd:string" />   
       <xsd:attribute name="OrderDate"    type="xsd:date" />   
       <xsd:attribute name="DueDate"  />   
       <xsd:attribute name="ShipDate"  type="xsd:time" />   
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 在该 XSD 架构中，有三个从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回日期值的属性。 当该架构：  
  
-   指定 "**订购** 日期" 属性上的 **xsd： type = date** ，将显示 "订货日期" 属性返回的值的日期部分 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。   
  
-   指定 **ShipDate** 特性上的 **xsd： type = time** ，将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 显示为 **ShipDate** 属性返回的值的时间部分。  
  
-   不在 **DueDate** 特性上指定 **xsd： type** ，则显示返回的相同值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将该文件另存为 xsdType.xml。  
  
2.  复制以下模板，并将它粘贴到文本文件中。 在保存 xsdType.xml 的相同目录中将文件另存为 xsdTypeT.xml。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="xsdType.xml">  
        /Order  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (xsdType.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\SqlXmlTest\xsdType.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  

     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 下面是部分结果集：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Order SalesOrderID="43659"   
         CustomerID="676"   
         OrderDate="2001-07-01"   
         DueDate="2001-07-13T00:00:00"   
         ShipDate="00:00:00" />   
  <Order SalesOrderID="43660"   
         CustomerID="117"   
         OrderDate="2001-07-01"   
         DueDate="2001-07-13T00:00:00"   
         ShipDate="00:00:00" />   
 ...  
</ROOT>  
```  
  
 这是等效的 XDR 架构：  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:dt="urn:schemas-microsoft-com:datatypes"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  
<ElementType name="Order" sql:relation="Sales.SalesOrderHeader">  
    <AttributeType name="SalesOrderID" />  
    <AttributeType name="CustomerID"  />  
    <AttributeType name="OrderDate" dt:type="date" />  
    <AttributeType name="DueDate" />  
    <AttributeType name="ShipDate" dt:type="time" />  
  
    <attribute type="SalesOrderID" sql:field="OrderID" />  
    <attribute type="CustomerID" sql:field="CustomerID" />  
    <attribute type="OrderDate" sql:field="OrderDate" />  
    <attribute type="DueDate" sql:field="DueDate" />  
    <attribute type="ShipDate" sql:field="ShipDate" />  
</ElementType>  
</Schema>  
```  
  
### <a name="b-specifying-sql-data-type-using-sqldatatype"></a>B. 使用 sql:datatype 指定 SQL 数据类型  
 有关工作示例，请参阅 XML 大容量加载示例中的示例 G [&#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/xml-bulk-load-examples-sqlxml-4-0.md)。 在此示例中，大容量加载包含“{”和“}”的 GUID 值。 此示例中的架构指定 **sql： datatype** ，以将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型标识为 **uniqueidentifier**。 此示例阐释了在架构中必须指定 **sql： datatype** 的时间。  
  
  
