---
title: '使用 XML Updategram 插入数据 (SQLXML) '
description: 了解如何使用 XML updategram 在 SQLXML 4.0 中插入数据。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- xsi:nil attribute
- unique values
- <after> block
- id attribute
- data insertions [SQLXML]
- nil attribute
- <before> block
- updg:guid attribute
- multiple record insertions
- returnid attribute
- updategrams [SQLXML], inserting data
- updg:at-identity attribute
- invalid characters [SQLXML]
- updg:returnid attribute
- updg:id attribute
- namespaces [SQLXML], updategrams
- IDENTITY-type column
- guid attribute
- record insertion [SQLXML]
- null values [SQLXML]
- at-identity attribute
- xml data type [SQL Server], SQLXML
ms.assetid: 4dc48762-bc12-43fb-b356-ea1b9c1e287e
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ac801a52e89e60bb05d1431de77078fa750f6d34
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473108"
---
# <a name="inserting-data-using-xml-updategrams-sqlxml-40"></a>使用 XML updategram 插入数据 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  当记录实例出现在 **\<after>** 块中但不在对应的块中时，updategram 指示插入操作 **\<before>** 。 在这种情况下，updategram 将块中的记录插入 **\<after>** 到数据库中。  
  
 以下是 updategram 的插入操作格式：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema="SampleSchema.xml"]  >  
   [<updg:before>  
   </updg:before>]  
    <updg:after [updg:returnid="x y ...] >  
       <ElementName [updg:id="value"]   
                   [updg:at-identity="x"]   
                   [updg:guid="y"]  
                   attribute="value"   
                   attribute="value"  
                   ...  
       />  
      [<ElementName .../>... ]  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
## <a name="before-block"></a>\<before> 模块  
 **\<before>** 对于插入操作，可以省略块。 如果未指定可选的 **映射架构** 特性，则 **\<ElementName>** 在 updategram 中指定的将映射到数据库表，并且子元素或属性映射到表中的列。  
  
## <a name="after-block"></a>\<after> 模块  
 可以在块中指定一个或多个记录 **\<after>** 。  
  
 如果 **\<after>** 块没有为特定列提供值，则 updategram 将使用批注架构 (中指定的默认值（如果) 指定了架构）。 如果架构未指定列的默认值，则 updategram 不会为此列指定任何显式值，而是会将 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认值指定 (如果) 指定此列。 如果没有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认值并且此列接受 NULL 值，则 updategram 将此列的值设置为 NULL。 如果此列既没有默认值也不接受 NULL 值，则命令将失败并且 updategram 将返回一个错误。 当将记录添加到具有 IDENTITY 类型列的表中时，可选 **updg： returnid** 属性用于返回系统生成的标识值。  
  
## <a name="updgid-attribute"></a>updg:id 属性  
 如果 updategram 只插入记录，则 updategram 不需要 **updg： id** 属性。 有关 **updg： id** 的详细信息，请参阅 [使用 XML updategram 更新数据 &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/updating-data-using-xml-updategrams-sqlxml-4-0.md)。  
  
## <a name="updgat-identity-attribute"></a>updg:at-identity 属性  
 当 updategram 在具有 IDENTITY 类型列的表中插入一条记录时，updategram 可以通过使用可选的 **updg： IDENTITY** 特性来捕获系统赋值的值。 然后，updategram 可以在后续操作中使用此值。 执行 updategram 时，可以通过指定 **updg： returnid** 特性来返回生成的标识值。  
  
## <a name="updgguid-attribute"></a>updg:guid 属性  
 **Updg： guid** 特性是一个可选的属性，该属性可生成全局唯一标识符。 此值保留在指定它的整个块的作用域中 **\<sync>** 。 可以在块中的任何位置使用此值 **\<sync>** 。 属性调用 **NEWGUID ( # B1** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 函数以生成唯一标识符。  
  
## <a name="examples"></a>示例  
 若要创建使用以下示例的工作示例，必须满足 [运行 SQLXML 示例的要求](../../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)中指定的要求。  
  
 在使用 updategram 示例前，请注意以下事项：  
  
-   大多数示例使用默认映射（即，未在 updategram 中指定任何映射架构）。 有关使用映射架构的 updategram 的更多示例，请参阅 [在 Updategram &#40;SQLXML 4.0&#41;中指定带批注的映射架构 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md)。  
  
-   大多数示例使用 [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] 示例数据库。 已对该数据库中的表应用所有更新。  
  
### <a name="a-inserting-a-record-by-using-an-updategram"></a>A. 使用 updategram 插入记录  
 这个以属性为中心的 updategram 在 [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] 数据库的 HumanResources.Employee 表中插入一条记录。  
  
 在此示例中，updategram 没有指定映射架构。 因此，updategram 使用默认映射，在默认映射中，元素名称映射到表名，而属性或子元素则映射到该表中的列。  
  
 HumanResources.Department 表的 [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] 架构对所有列施加了“not null”限制。 所以，updategram 必须包括为所有列指定的值。 DepartmentID 是一个 IDENTITY 类型的列。 因此，没有为其指定任何值。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
       <HumanResources.Department   
            Name="New Product Research"   
            GroupName="Research and Development"   
            ModifiedDate="2010-08-31"/>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的 updategram，并将它粘贴到文本文件中。 将文件另存为 MyUpdategram.xml。  
  
2.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 在以元素为中心的映射中，updategram 如下所示：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
       <HumanResources.Department>  
            <Name> New Product Research </Name>  
            <GroupName> Research and Development </GroupName>  
            <ModifiedDate>2010-08-31</ModifiedDate>  
       </HumanResources.Department>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 在混合模式（以元素为中心和以属性为中心）的 updategram 中，元素可以同时具有属性和子元素，如以下 updategram 所示：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
       <HumanResources.Department   
            Name=" New Product Research "   
            <GroupName>Research and Development</GroupName>  
            <ModifiedDate>2010-08-31</ModifiedDate>  
       </HumanResources.Department>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
### <a name="b-inserting-multiple-records-by-using-an-updategram"></a>B. 使用 updategram 插入多个记录  
 此 updategram 向 HumanResources.Shift 表添加两个新的轮班记录。 Updategram 不指定可选的 **\<before>** 块。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync>  
    <updg:after >  
       <HumanResources.Shift Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
       <HumanResources.Shift Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的 updategram，并将它粘贴到文本文件中。 将文件另存为 Updategram-AddShifts.xml。  
  
2.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 此示例的另一个版本是 updategram，它使用两个单独的 **\<after>** 块而不是一个块来插入两个雇员。 这种做法是有效的，并且可以按照如下形式进行编码：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync>  
    <updg:after >  
       <HumanResources.Shift Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
    </updg:after>  
    <updg:before>  
    </updg:before>  
    <updg:after >  
       <HumanResources.Shift Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
### <a name="c-working-with-valid-sql-server-characters-that-are-not-valid-in-xml"></a>C. 使用在 XML 中无效的有效 SQL Server 字符  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中，表名可以包括空格，例如 Northwind 数据库中的 Order Details 表。 但是，这在作为有效标识符的 XML 字符中无效， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 但不能使用 "__xHHHH" 作为编码值对有效的 xml 标识符进行编码 \_ \_ ，其中 HHHH 代表最高有效位第一次的字符的四位十六进制 UCS-2 代码。  
  
> [!NOTE]  
>  此示例使用 Northwind 数据库。 可以通过使用可从 [Microsoft 网站](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs)下载的 SQL 脚本来安装 Northwind 数据库。  
  
 此外，元素名必须括在方括号 ([ ]) 内。 因为字符 [and] 在 XML 中无效，所以必须分别将它们编码为 _x005B \_ 和 _x005D \_ 。 （如果使用映射架构，可以提供不包含无效字符（如空格）的元素名。 映射架构会执行必要的映射；因此，无需对这些字符进行编码。）  
  
 此 updategram 向 Northwind 数据库中的 Order Details 表添加一条记录：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
      <_x005B_Order_x0020_Details_x005D_ OrderID="1"  
            ProductID="11"  
            UnitPrice="$1.0"  
            Quantity="1"  
            Discount="0.0" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 "订单详细信息" 表中的 "单价" 列的类型为 **money** 。 若要将适当的类型转换 (从 **字符串** 类型转换为 **money** 类型) ，则必须将美元符号字符 ($) 添加为值的一部分。 如果 updategram 未指定映射架构，则将计算 **字符串** 值的第一个字符。 如果第一个字符为美元符号 ($)，则会应用适当的转换。  
  
 如果针对映射架构指定了 updategram，其中列正确地标记为 **dt： type = "fixed. 14.4"** 或 **sql： datatype = "money"**，则不需要美元符号 ($) ，该转换由映射进行处理。 建议采用这种方式以确保能够进行适当的类型转换。  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的 updategram，并将它粘贴到文本文件中。 将文件另存为 UpdategramSpacesInTableName.xml。  
  
2.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
### <a name="d-using-the-at-identity-attribute-to-retrieve-the-value-that-has-been-inserted-in-the-identity-type-column"></a>D. 使用 at-identity 属性检索已在 IDENTITY 类型的列中插入的值  
 以下 updategram 插入两条记录：在 Sales.SalesOrderHeader 表中插入一条记录而在 Sales.SalesOrderDetail 表中插入另一条记录。  
  
 首先，updategram 向 Sales.SalesOrderHeader 表中添加一条记录。 在该表中，SalesOrderID 列为 IDENTITY 类型的列。 因此，当您将此记录添加到表中时，updategram 将使用 " **标识** " 属性来捕获指定的 SalesOrderID 值为 "x" () 的占位符值。 然后，updategam 将此 **at 标识** 变量指定为元素中 SalesOrderID 属性的值 \<Sales.SalesOrderDetail> 。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
 <updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
   <Sales.SalesOrderHeader updg:at-identity="x"   
             RevisionNumber="1"  
             OrderDate="2001-07-01 00:00:00.000"  
             DueDate="2001-07-13 00:00:00.000"  
             OnlineOrderFlag="0"  
             CustomerID="676"  
             ContactID="378"  
             BillToAddressID="985"  
             ShipToAddressID="985"  
             ShipMethodID="5"  
             SubTotal="24643.9362"  
             TaxAmt="1971.5149"  
             Freight="616.0984"  
             rowguid="00001111-2222-3333-4444-556677889900"  
             ModifiedDate="2001-07-08 00:00:00.000" />  
      <Sales.SalesOrderDetail SalesOrderID="x"  
                LineNumber="1"  
                OrderQty="1"  
                ProductID="776"  
                SpecialOfferID="1"  
                UnitPrice="2429.9928"  
                UnitPriceDiscount="0.00"  
                rowguid="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"  
                ModifiedDate="2001-07-01 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 如果要返回 **updg： identity** 属性生成的标识值，则可以使用 **updg： returnid** 特性。 以下是经过修改的返回此标识值的 updategram。 （此 updategram 添加两条订单记录和两条订单详细信息记录，目的在于让示例变得稍微复杂一点。）  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
 <updg:sync>  
  <updg:before>  
  </updg:before>  
  <updg:after updg:returnid="x y" >  
       <HumanResources.Shift updg:at-identity="x" Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
       <HumanResources.Shift updg:at-identity="y" Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
  </updg:after>  
 </updg:sync>  
</ROOT>  
```  
  
 在执行 updategram 时，它会返回类似于如下的结果，包括生成的标识值（用于表标识的 ShiftID 列的生成值）：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">   
  <returnid>   
    <x>4</x>   
    <y>5</y>   
  </returnid>   
</ROOT>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的 updategram，并将它粘贴到文本文件中。 将文件另存为 Updategram-returnId.xml。  
  
2.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
### <a name="e-using-the-updgguid-attribute-to-generate-a-unique-value"></a>E. 使用 updg:guid 属性生成唯一值  
 在本示例中，updategram 在 Cust 和 CustOrder 表中插入一条新记录。 此外，updategram 通过使用 **updg： guid** 特性为 CustomerID 属性生成唯一值。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after updg:returnid="x" >  
      <Cust updg:guid="x" >  
         <CustID>x</CustID>  
         <LastName>Fuller</LastName>  
      </Cust>  
      <CustOrder>  
         <CustID>x</CustID>  
         <OrderID>1</OrderID>  
      </CustOrder>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Updategram 指定 **returnid** 属性。 生成的 GUID 作为结果返回：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <returnid>  
    <x>7111BD1A-7F0B-4CEE-B411-260DADFEFA2A</x>   
  </returnid>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  复制上面的 updategram，并将它粘贴到文本文件中。 将文件另存为 Updategram-GenerateGuid.xml。  
  
2.  创建以下表：  
  
    ```  
    USE tempdb  
    CREATE TABLE Cust (CustID uniqueidentifier, LastName varchar(20))  
    CREATE TABLE CustOrder (CustID uniqueidentifier, OrderID int)  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
### <a name="f-specifying-a-schema-in-an-updategram"></a>F. 在 updategram 中指定架构  
 在此示例中，updategram 在以下表中插入一条记录：  
  
```  
CustOrder(OrderID, EmployeeID, OrderType)  
```  
  
 在此 updategram 中指定了一个 XSD 架构（即，updategram 元素和属性不存在任何默认映射）。 架构提供了元素和属性与数据库表和列之间的必要映射。  
  
 以下架构 ( # A0) 描述 **\<CustOrder>** 包含 **订单 Id** 和 **雇员 id** 属性的元素。 为了使该架构更有趣，将为 " **雇员 id** " 属性分配一个默认值。 updategram 仅在执行插入操作以及仅在没有指定该属性时才使用属性的默认值。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="CustOrder" >  
   <xsd:complexType>  
        <xsd:attribute name="OrderID"     type="xsd:integer" />   
        <xsd:attribute name="EmployeeID"  type="xsd:integer" />  
        <xsd:attribute name="OrderType  " type="xsd:integer" default="1"/>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 此 updategram 在 CustOrder 表中插入一条记录。 updategram 仅指定了 OrderID 和 EmployeeID 属性值。 它未指定 OrderType 属性值。 因此，updategram 使用在前面的架构中指定的 EmployeeID 属性的默认值。  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql"  
             xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync mapping-schema='CustOrderSchema.xml'>  
<updg:after>  
       <CustOrder OrderID="98000" EmployeeID="1" />  
</updg:after>  
</updg:sync>  
</ROOT>  
```  
  
 有关指定映射架构的 updategram 的更多示例，请参阅 [在 Updategram &#40;SQLXML 4.0&#41;中指定带批注的映射架构 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md)。  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  在 **tempdb** 数据库中创建此表：  
  
    ```  
    USE tempdb  
    CREATE TABLE CustOrder(  
                     OrderID int,   
                     EmployeeID int,   
                     OrderType int)  
    ```  
  
2.  复制上面的架构，并将它粘贴到文本文件中。 将文件另存为 CustOrderSchema.xml。  
  
3.  复制上面的 updategram，并将它粘贴到文本文件中。 在与上一步骤中所使用文件夹相同的文件夹中将文件另存为 CustOrderUpdategram.xml。  
  
4.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 这是等效的 XDR 架构：  
  
```  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
 <ElementType name="CustOrder" >  
    <AttributeType name="OrderID" />  
    <AttributeType name="EmployeeID" />  
    <AttributeType name="OrderType" default="1" />  
    <attribute type="OrderID"  />  
    <attribute type="EmployeeID" />  
    <attribute type="OrderType" />  
  </ElementType>  
</Schema>  
```  
  
### <a name="g-using-the-xsinil-attribute-to-insert-null-values-in-a-column"></a>G. 使用 xsi:nil 属性在列中插入 null 值  
 如果要在表的相应列中插入 null 值，可以在 updategram 中的元素上指定 **xsi： nil** 特性。 在对应的 XSD 架构中，还必须指定 XSD **nillable** 属性。  
  
 例如，请看此 XSD 架构：  
  
```  
<?xml version="1.0"?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:element name="Student" sql:relation="Students">  
  <xsd:complexType>  
    <xsd:all>  
      <xsd:element name="fname" sql:field="first_name"   
                                type="xsd:string"   
                                 nillable="true"/>  
    </xsd:all>  
    <xsd:attribute name="SID"   
                        sql:field="StudentID"  
                        type="xsd:ID"/>      
    <xsd:attribute name="lname"       
                        sql:field="last_name"  
                        type="xsd:string"/>  
    <xsd:attribute name="minitial"    
                        sql:field="middle_initial"   
                        type="xsd:string"/>  
    <xsd:attribute name="years"       
                         sql:field="no_of_years"  
                         type="xsd:integer"/>  
  </xsd:complexType>  
 </xsd:element>  
</xsd:schema>  
```  
  
 XSD 架构为元素指定 **nillable = "true"** **\<fname>** 。 以下 updategram 使用此架构：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql"  
      xmlns:updg="urn:schemas-microsoft-com:xml-updategram"  
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
<updg:sync mapping-schema='StudentSchema.xml'>  
  <updg:before/>  
  <updg:after>  
    <Student SID="S00004" lname="Elmaci" minitial="" years="2">  
      <fname xsi:nil="true">  
    </fname>  
    </Student>  
  </updg:after>  
</updg:sync>  
  
</ROOT>  
```  
  
 Updategram 为块中的元素指定 **xsi： nil** **\<fname>** **\<after>** 。 因此，在执行此 updategram 时，会为表中的 first_name 列插入 NULL 值。  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  在 **tempdb** 数据库中创建以下表：  
  
    ```  
    USE tempdb  
    CREATE TABLE Students (  
       StudentID char(6)NOT NULL ,  
       first_name varchar(50),  
       last_name varchar(50),  
       middle_initial char(1),  
       no_of_years int NULL)  
    GO  
    ```  
  
2.  复制上面的架构，并将它粘贴到文本文件中。 将文件另存为 StudentSchema.xml。  
  
3.  复制上面的 updategram，并将它粘贴到文本文件中。 在与上一步骤中保存 StudentSchema.xml 所使用文件夹相同的文件夹中将文件另存为 StudentUpdategram.xml。  
  
4.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
### <a name="h-specifying-namespaces-in-an-updategram"></a>H. 在 updategram 中指定命名空间  
 在 updategram 中，元素所属的命名空间可以在 updategram 中的同一元素中进行声明。 在这种情况下，对应的架构也必须声明相同的命名空间，并且元素必须属于该目标命名空间。  
  
 例如，在以下 updategram ( # A0) 中， **\<Order>** 元素属于元素中声明的命名空间。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync mapping-schema='XSD-ElementHavingNameSpace.xml'>  
    <updg:after>  
       <x:Order  xmlns:x="https://server/xyz/schemas/"  
             updg:at-identity="SalesOrderID"   
             RevisionNumber="1"  
             OrderDate="2001-07-01 00:00:00.000"  
             DueDate="2001-07-13 00:00:00.000"  
             OnlineOrderFlag="0"  
             CustomerID="676"  
             ContactID="378"  
             BillToAddressID="985"  
             ShipToAddressID="985"  
             ShipMethodID="5"  
             SubTotal="24643.9362"  
             TaxAmt="1971.5149"  
             Freight="616.0984"  
             rowguid="00009999-8888-7777-6666-554433221100"  
             ModifiedDate="2001-07-08 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 在这种情况下，架构也必须声明该命名空间，如此架构中所示：  
  
 以下架构 (XSD-ElementHavingNamespace.xml) 演示如何必须声明对应的元素和属性。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
     xmlns:dt="urn:schemas-microsoft-com:datatypes"   
     xmlns:sql="urn:schemas-microsoft-com:mapping-schema"    
     xmlns:x="https://server/xyz/schemas/"   
     targetNamespace="https://server/xyz/schemas/" >  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader" type="x:Order_type"/>  
  <xsd:complexType name="Order_type">  
    <xsd:attribute name="SalesOrderID"  type="xsd:ID"/>  
    <xsd:attribute name="RevisionNumber" type="xsd:unsignedByte"/>  
    <xsd:attribute name="OrderDate" type="xsd:dateTime"/>  
    <xsd:attribute name="DueDate" type="xsd:dateTime"/>  
    <xsd:attribute name="ShipDate" type="xsd:dateTime"/>  
    <xsd:attribute name="Status" type="xsd:unsignedByte"/>  
    <xsd:attribute name="OnlineOrderFlag" type="xsd:boolean"/>  
    <xsd:attribute name="SalesOrderNumber" type="xsd:string"/>  
    <xsd:attribute name="PurchaseOrderNumber" type="xsd:string"/>  
    <xsd:attribute name="AccountNumber" type="xsd:string"/>  
    <xsd:attribute name="CustomerID" type="xsd:int"/>  
    <xsd:attribute name="ContactID" type="xsd:int"/>  
    <xsd:attribute name="SalesPersonID" type="xsd:int"/>  
    <xsd:attribute name="TerritoryID" type="xsd:int"/>  
    <xsd:attribute name="BillToAddressID" type="xsd:int"/>  
    <xsd:attribute name="ShipToAddressID" type="xsd:int"/>  
    <xsd:attribute name="ShipMethodID" type="xsd:int"/>  
    <xsd:attribute name="CreditCardID" type="xsd:int"/>  
    <xsd:attribute name="CreditCardApprovalCode" type="xsd:string"/>  
    <xsd:attribute name="CurrencyRateID" type="xsd:int"/>  
    <xsd:attribute name="SubTotal" type="xsd:decimal"/>  
    <xsd:attribute name="TaxAmt" type="xsd:decimal"/>  
    <xsd:attribute name="Freight" type="xsd:decimal"/>  
    <xsd:attribute name="TotalDue" type="xsd:decimal"/>  
    <xsd:attribute name="Comment" type="xsd:string"/>  
    <xsd:attribute name="rowguid" type="xsd:string"/>  
    <xsd:attribute name="ModifiedDate" type="xsd:dateTime"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  复制上面的架构，并将它粘贴到文本文件中。 将文件另存为 XSD-ElementHavingNamespace.xml。  
  
2.  复制上面的 updategram，并将它粘贴到文本文件中。 在用于保存 XSD-ElementHavingnamespace.xml 的同一文件夹中将文件另存为 Updategram-ElementHavingNamespace.xml。  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
### <a name="i-inserting-data-into-an-xml-data-type-column"></a>I. 将数据插入到 XML 数据类型列  
 **Xml** 数据类型是在中引入的 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 。 您可以使用 updategram 来插入和更新存储在 **xml** 数据类型列中的数据，并提供以下规定：  
  
-   **Xml** 列不能用于标识现有行。 因此，它不能包含在 updategram 的 **updg： before** 部分。  
  
-   将保留在插入到 **xml** 列中的 xml 片段范围内的命名空间，并将其命名空间声明添加到插入片段的顶层元素。  
  
 例如，在下面的 updategram 中 ( # A0) ，该 **\<Desc>** 元素将更新示例数据库中生产>productModel 表中的 ProductDescription 列 [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] 。 此 updategram 的结果是，ProductDescription 列的 XML 内容是更新元素的 XML 内容 **\<Desc>** 。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
    <updg:sync mapping-schema="SampleSchema.xml" >  
       <updg:before>  
<ProductModel ProductModelID="19">  
   <Name>Mountain-100</Name>  
</ProductModel>  
    </updg:before>  
    <updg:after>  
 <ProductModel>  
    <Name>Mountain-100</Name>  
    <Desc><?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>  
        <p1:ProductDescription xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
              xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
              xmlns:wf="https://www.adventure-works.com/schemas/OtherFeatures"   
              xmlns:html="http://www.w3.org/1999/xhtml"   
              xmlns="">  
  <p1:Summary>  
     <html:p>Insert Example</html:p>  
  </p1:Summary>  
  <p1:Manufacturer>  
    <p1:Name>AdventureWorks</p1:Name>  
    <p1:Copyright>2002</p1:Copyright>  
    <p1:ProductURL>HTTP://www.Adventure-works.com</p1:ProductURL>  
  </p1:Manufacturer>  
  <p1:Features>These are the product highlights.   
    <wm:Warranty>  
       <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
       <wm:Description>parts and labor</wm:Description>  
    </wm:Warranty>  
    <wm:Maintenance>  
       <wm:NoOfYears>10 years</wm:NoOfYears>  
       <wm:Description>maintenance contract available through your dealer or any AdventureWorks retail store.</wm:Description>  
    </wm:Maintenance>  
    <wf:wheel>High performance wheels.</wf:wheel>  
    <wf:saddle>  
      <html:i>Anatomic design</html:i> and made from durable leather for a full-day of riding in comfort.</wf:saddle>  
    <wf:pedal>  
       <html:b>Top-of-the-line</html:b> clipless pedals with adjustable tension.</wf:pedal>  
    <wf:BikeFrame>Each frame is hand-crafted in our Bothell facility to the optimum diameter and wall-thickness required of a premium mountain frame. The heat-treated welded aluminum frame has a larger diameter tube that absorbs the bumps.</wf:BikeFrame>  
    <wf:crankset> Triple crankset; alumunim crank arm; flawless shifting. </wf:crankset>  
   </p1:Features>  
   <p1:Picture>  
      <p1:Angle>front</p1:Angle>  
      <p1:Size>small</p1:Size>  
      <p1:ProductPhotoID>118</p1:ProductPhotoID>  
   </p1:Picture>  
   <p1:Specifications> These are the product specifications.  
     <Material>Almuminum Alloy</Material>  
     <Color>Available in most colors</Color>  
     <ProductLine>Mountain bike</ProductLine>  
     <Style>Unisex</Style>  
     <RiderExperience>Advanced to Professional riders</RiderExperience>  
   </p1:Specifications>  
  </p1:ProductDescription>  
 </Desc>  
      </ProductModel>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 updategram 引用以下带批注的 XSD 架构 (SampleSchema.xml)。  
  
```  
<?xml version="1.0" encoding="utf-8" ?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
           xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
           xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">   
  <xsd:element name="ProductModel"  sql:relation="Production.ProductModel" >  
     <xsd:complexType>  
       <xsd:sequence>  
          <xsd:element name="Name" type="xsd:string"></xsd:element>  
          <xsd:element name="Desc" sql:field="CatalogDescription" sql:datatype="xml">  
           <xsd:complexType>  
            <xsd:sequence>  
              <xsd:element name="ProductDescription">  
                 <xsd:complexType>  
                   <xsd:sequence>  
                     <xsd:element name="Summary" type="xsd:anyType">  
                     </xsd:element>  
                   </xsd:sequence>  
                 </xsd:complexType>  
              </xsd:element>  
            </xsd:sequence>  
           </xsd:complexType>  
          </xsd:element>   
       </xsd:sequence>  
       <xsd:attribute name="ProductModelID" sql:field="ProductModelID"/>  
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  复制上面的架构，并将它粘贴到文本文件中。 将文件另存为 XSD-SampleSchema.xml。  
  
    > [!NOTE]  
    >  由于 updategram 支持默认映射，因此无法确定 **xml** 数据类型的开头和结尾。 这实际上意味着，当使用 **xml** 数据类型列插入或更新表时，需要使用映射架构。 如果没有提供架构，SQLXML 将返回错误，指出表中缺少某一列。  
  
2.  复制上面的 updategram，并将它粘贴到文本文件中。 在用于保存 SampleSchema.xml 的同一文件夹中将文件另存为 SampleUpdategram.xml。  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;SQLXML 4.0&#41;的 Updategram 安全注意事项 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
