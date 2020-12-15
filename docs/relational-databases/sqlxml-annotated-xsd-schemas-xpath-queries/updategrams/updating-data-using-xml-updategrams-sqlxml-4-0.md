---
title: '使用 XML Updategram 更新数据 (SQLXML) '
description: 了解如何使用 SQLXML 4.0 中的 XML updategram 更新现有数据。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- IDREF type attribute [SQLXML]
- before attribute
- <sync> block
- <after> block
- id attribute
- <before> block
- updg:after attribute
- mapping-schema attribute
- IDREFS type attribute [SQLXML]
- updg:id attribute
- multiple record updates
- after attribute
- updategrams [SQLXML], updating data
- updg:before attribute
- record updates [SQLXML]
ms.assetid: 90ef8a33-5ae3-4984-8259-608d2f1d727f
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b5539c07b9faab5c426ad9b101d94c683ed665c5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484809"
---
# <a name="updating-data-using-xml-updategrams-sqlxml-40"></a>使用 XML Updategram 更新数据 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  更新现有数据时，必须同时指定 **\<before>** 和 **\<after>** 块。 和块中指定的 **\<before>** 元素 **\<after>** 描述所需的更改。 Updategram 使用在块中指定的元素 (s) **\<before>** 来标识数据库中的现有记录 () 。 块中 (s) 对应的元素 **\<after>** 指示在执行更新操作后记录的外观。 通过此信息，updategram 创建与块匹配的 SQL 语句 **\<after>** 。 然后，Updategram 使用该语句更新数据库。  
  
 以下是 Updategram 的更新操作格式：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync [mapping-schema="SampleSchema.xml"]  >  
   <updg:before>  
      <ElementName [updg:id="value"] .../>  
      [<ElementName [updg:id="value"] .../> ... ]  
   </updg:before>  
   <updg:after>  
      <ElementName [updg:id="value"] ... />  
      [<ElementName [updg:id="value"] .../> ...]  
   </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
 **\<updg:before>**  
 块中的元素用于 **\<before>** 标识数据库表中的现有记录。  
  
 **\<updg:after>**  
 块中的元素 **\<after>** 说明在应用更新后块中指定的记录的 **\<before>** 外观。  
  
 **映射架构** 属性标识 updategram 要使用的映射架构。 如果 updategram 指定映射架构，则和块中指定的元素和属性名称 **\<before>** **\<after>** 必须与架构中的名称匹配。 该映射架构将这些元素或属性名称映射到数据库表和列名称。  
  
 如果 Updategram 不指定架构，则 Updategam 将使用默认映射。 在默认映射中， **\<ElementName>** updategram 中指定的将映射到数据库表，并且子元素或属性将映射到数据库列。  
  
 块中的元素 **\<before>** 必须与数据库中的一个表行仅匹配。 如果元素与多个表行匹配或与任何表行都不匹配，则 updategram 将返回一个错误，并取消整个 **\<sync>** 块。  
  
 Updategram 可包含多个 **\<sync>** 块。 每个 **\<sync>** 块都被视为一个事务。 每个 **\<sync>** 块可以有多个 **\<before>** 和 **\<after>** 块。 例如，如果您要更新两个现有记录，则可以指定两个 **\<before>** 和 **\<after>** 对，每个记录更新一次。  
  
## <a name="using-the-updgid-attribute"></a>使用 updg:id 属性  
 在和块中指定多个元素时 **\<before>** **\<after>** ，使用 **updg： id** 特性标记和块中的行 **\<before>** **\<after>** 。 处理逻辑使用此信息来确定块中的哪条记录 **\<before>** 与块中的记录 **\<after>** 。  
  
 **Updg： id** 属性不是必需的 (但如果存在以下情况，建议) ：  
  
-   指定映射架构中的元素在其上定义了 **sql： key 字段** 特性。  
  
-   为 Updategram 中的键字段提供了一个或多个特定值。  
  
 如果这两种情况下，updategram 将使用 **sql： key-字段** 中指定的键列来配对和块中的元素 **\<before>** **\<after>** 。  
  
 如果映射架构没有通过使用 **sql： key-fields**) 来标识 (的键列，或者如果 updategram 正在更新键列值，则必须指定 **updg： id**。  
  
 和块中标识的记录不必 **\<before>** **\<after>** 按相同顺序排列。 **Updg： id** 属性强制在和块中指定的元素之间进行关联 **\<before>** **\<after>** 。  
  
 如果在块中指定一个元素， **\<before>** 并且只在块中指定一个相应的元素 **\<after>** ，则不需要使用 **updg： id** 。 不过，建议您指定 **updg： id** ，以避免多义性。  
  
## <a name="examples"></a>示例  
 在使用 Updategram 示例之前，请注意以下事项：  
  
-   大多数示例使用默认映射（即，未在 updategram 中指定任何映射架构）。 有关使用映射架构的 updategram 的更多示例，请参阅 [在 Updategram &#40;SQLXML 4.0&#41;中指定带批注的映射架构 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md)。  
  
-   大多数示例使用 AdventureWorks 示例数据库。 已对该数据库中的表应用所有更新。 您可以还原 AdventureWorks 数据库。  
  
### <a name="a-updating-a-record"></a>A. 更新记录  
 以下 Updategram 将 AdventureWorks 数据库的 Person.Contact 表中的雇员姓氏更新为 Fuller。 Updategram 不指定任何映射架构，因此，Updategram 使用默认映射。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
   <Person.Contact ContactID="1" />  
</updg:before>  
<updg:after>  
   <Person.Contact LastName="Abel-Achong" />  
</updg:after>  
</updg:sync>  
</ROOT>  
```  
  
 块中所述的记录 **\<before>** 表示数据库中的当前记录。 Updategram 使用在块中指定的所有列值 **\<before>** 来搜索记录。 在此 updategram 中， **\<before>** 块仅提供 ContactID 列; 因此，updategram 只使用值来搜索记录。 如果要将 LastName 值添加到该块，则 Updategram 会同时使用 ContactID 和 LastName 值执行搜索。  
  
 在此 updategram 中， **\<after>** 块仅提供 LastName 列值，因为这是唯一要更改的值。  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  复制上面的 Updategram 模板，并将它粘贴到文本文件中。 将该文件另存为 UpdateLastName.xml。  
  
2.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  

     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
### <a name="b-updating-multiple-records-by-using-the-updgid-attribute"></a>B. 使用 updg:id 属性更新多个记录  
 在该示例中，Updategram 对 AdventureWorks 数据库中的 HumanResources.Shift 表执行两次更新：  
  
-   它将 7:00AM 开始的原始白班的名称从“Day”更改为“Early Morning”。  
  
-   它插入名为“Late Morning”的从 10:00AM 开始的新班。  
  
 在 updategram 中， **updg： id** 属性在和块中的元素之间创建关联 **\<before>** **\<after>** 。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
       <HumanResources.Shift updg:id="x" Name="Day" />  
    </updg:before>  
    <updg:after>  
      <HumanResources.Shift updg:id="y" Name="Late Morning"   
                            StartTime="1900-01-01 10:00:00.000"  
                            EndTime="1900-01-01 18:00:00.000"  
                            ModifiedDate="2004-06-01 00:00:00.000"/>  
      <HumanResources.Shift updg:id="x" Name="Early Morning" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 请注意， **updg： id** 属性如何将块中的元素的第一个实例 \<HumanResources.Shift> **\<before>** 与 \<HumanResources.Shift> 块中元素的第二个实例配对 **\<after>** 。  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  复制上面的 Updategram 模板，并将它粘贴到文本文件中。 将该文件另存为 UpdateMultipleRecords.xml。  
  
2.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
### <a name="c-specifying-multiple-before-and-after-blocks"></a>C. 指定多个 \<before> 和 \<after> 块  
 若要避免多义性，可以通过使用多个 **\<before>** 和块对来编写示例 B 中的 updategram **\<after>** 。 指定 **\<before>** 和 **\<after>** 对是指定多个更新的一种方法，其中至少有混淆。 此外，如果每个 **\<before>** 和 **\<after>** 块最多指定一个元素，则无需使用 **updg： id** 属性。  
  
> [!NOTE]  
>  若要形成一对， **\<after>** 标记必须紧跟在其对应的 **\<before>** 标记之后。  
  
 在以下 updategram 中，第一个 **\<before>** 和 **\<after>** 对将更新当天的 shift 名称。 第二对将插入新的轮班记录。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
       <HumanResources.Shift ShiftID="1" Name="Day" />  
    </updg:before>  
    <updg:after>  
      <HumanResources.Shift Name="Early Morning" />  
    </updg:after>  
    <updg:before>  
    </updg:before>  
    <updg:after>  
      <HumanResources.Shift Name="Late Morning"   
                            StartTime="1900-01-01 10:00:00.000"  
                            EndTime="1900-01-01 18:00:00.000"  
                            ModifiedDate="2004-06-01 00:00:00.000"/>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  复制上面的 Updategram 模板，并将它粘贴到文本文件中。 将该文件另存为 UpdateMultipleBeforeAfter.xml。  
  
2.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
### <a name="d-specifying-multiple-sync-blocks"></a>D. 指定多个 \<sync> 块  
 可以 **\<sync>** 在 updategram 中指定多个块。 指定的每个 **\<sync>** 块都是独立的事务。  
  
 在下面的 updategram 中，第一个 **\<sync>** 块更新 Customer 表中的一条记录。 出于简化原因，Updategram 仅指定必需的列值：标识值 (CustomerID) 和要更新的值 (SalesPersonID)。  
  
 第二个 **\<sync>** 块将两个记录添加到 SalesOrderHeader 表中。 对于该表，SalesOrderID 是 IDENTITY 类型的列。 因此，updategram 不会在每个元素中指定 SalesOrderID 的值 \<Sales.SalesOrderHeader> 。  
  
 指定多个 **\<sync>** 块很有用，因为如果第二个 **\<sync>** 块 (事务) 未能将记录添加到 SalesOrderHeader 表中，则第一个 **\<sync>** 块仍可更新 customer 表中的 customer 记录。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
      <Sales.Customer CustomerID="1" SalesPersonID="280" />  
    </updg:before>  
    <updg:after>  
      <Sales.Customer CustomerID="1" SalesPersonID="283" />  
    </updg:after>  
  </updg:sync>  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
   <Sales.SalesOrderHeader   
             CustomerID="1"  
             RevisionNumber="1"  
             OrderDate="2004-07-01 00:00:00.000"  
             DueDate="2004-07-13 00:00:00.000"  
             OnlineOrderFlag="0"  
             ContactID="378"  
             BillToAddressID="985"  
             ShipToAddressID="985"  
             ShipMethodID="5"  
             SubTotal="24643.9362"  
             TaxAmt="1971.5149"  
             Freight="616.0984"  
             rowguid="01010101-2222-3333-4444-556677889900"  
             ModifiedDate="2004-07-08 00:00:00.000" />  
   <Sales.SalesOrderHeader  
             CustomerID="1"  
             RevisionNumber="1"  
             OrderDate="2004-07-01 00:00:00.000"  
             DueDate="2004-07-13 00:00:00.000"  
             OnlineOrderFlag="0"  
             ContactID="378"  
             BillToAddressID="985"  
             ShipToAddressID="985"  
             ShipMethodID="5"  
             SubTotal="1000.0000"  
             TaxAmt="0.0000"  
             Freight="0.0000"  
             rowguid="10101010-2222-3333-4444-556677889900"  
             ModifiedDate="2004-07-09 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  复制上面的 Updategram 模板，并将它粘贴到文本文件中。 将文件另存为 UpdateMultipleSyncs.xml。  
  
2.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
### <a name="e-using-a-mapping-schema"></a>E. 使用映射架构  
 在此示例中，updategram 通过使用 **映射架构** 特性来指定映射架构。 （没有默认映射；就是说，由映射架构提供所需的映射，将 Updategram 中的元素和属性映射到数据库表和列。）  
  
 在 Updategram 中指定的元素和属性将引用映射架构中的元素和属性。  
  
 下面的 XSD 映射架构具有 **\<Customer>** 、 **\<Order>** 和 **\<OD>** 元素，这些元素映射到数据库中的 SalesOrderHeader 表和 SalesOrderDetail 表。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustomerOrder"  
          parent="Sales.Customer"  
          parent-key="CustomerID"  
          child="Sales.SalesOrderHeader"  
          child-key="CustomerID" />  
  
    <sql:relationship name="OrderOD"  
          parent="Sales.SalesOrderHeader"  
          parent-key="SalesOrderID"  
          child="Sales.SalesOrderDetail"  
          child-key="SalesOrderID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customer" sql:relation="Sales.Customer" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="Order"   
                     sql:relation="Sales.SalesOrderHeader"  
                     sql:relationship="CustomerOrder" >  
           <xsd:complexType>  
              <xsd:sequence>  
                <xsd:element name="OD"   
                             sql:relation="Sales.SalesOrderDetail"  
                             sql:relationship="OrderOD" >  
                 <xsd:complexType>  
                  <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
                  <xsd:attribute name="ProductID" type="xsd:integer" />  
                  <xsd:attribute name="UnitPrice" type="xsd:decimal" />  
                  <xsd:attribute name="OrderQty" type="xsd:integer" />  
                  <xsd:attribute name="UnitPriceDiscount" type="xsd:decimal" />   
                 </xsd:complexType>  
                </xsd:element>  
              </xsd:sequence>  
              <xsd:attribute name="CustomerID" type="xsd:string" />  
              <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
              <xsd:attribute name="OrderDate" type="xsd:date" />  
           </xsd:complexType>  
        </xsd:element>  
      </xsd:sequence>  
      <xsd:attribute name="CustomerID"   type="xsd:string" />   
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 以下 Updategram 指定该映射架构 (UpdategramMappingSchema.xml)。 该 Updategram 在 Sales.SalesOrderDetail 表中为特定顺序添加顺序细节项。 Updategram 包括嵌套元素： **\<OD>** 嵌套在元素内的元素 **\<Order>** 。 在映射架构中指定了这两个元素之间的主键/外键关系。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync mapping-schema="UpdategramMappingSchema.xml" >  
    <updg:before>  
       <Order SalesOrderID="43659" />  
    </updg:before>  
    <updg:after>  
      <Order SalesOrderID="43659" >  
          <OD ProductID="776" UnitPrice="2329.0000"  
              OrderQty="2" UnitPriceDiscount="0.0" />  
      </Order>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  复制上面的映射架构，并将它粘贴到文本文件中。 将该文件另存为 UpdategramMappingSchema.xml。  
  
2.  复制上面的 Updategram 模板，并将它粘贴到文本文件中。 在用于保存映射架构 (UpdategramMappingSchema.xml) 的相同文件夹中将该文件另存为 UpdateWithMappingSchema.xml。  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 有关使用映射架构的 updategram 的更多示例，请参阅 [在 Updategram &#40;SQLXML 4.0&#41;中指定带批注的映射架构 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md)。  
  
### <a name="f-using-a-mapping-schema-with-idrefs-attributes"></a>F. 将 IDREFS 属性与映射架构一起使用  
 该示例说明 Updategram 如何在映射架构中使用 IDREFS 属性以更新多个表中的记录。 对于该示例，假定数据库由以下表组成：  
  
-   Student(StudentID, LastName)  
  
-   Course(CourseID, CourseName)  
  
-   Enrollment(StudentID, CourseID)  
  
 由于学生可以注册参加很多课程，而且一种课程可以有很多学生，因此需要用第三个表 Enrollment 表以表示该 M:N 关系。  
  
 下面的 XSD 映射架构通过使用 **\<Student>** 、和元素提供表的 XML 视图 **\<Course>** **\<Enrollment>** 。 映射架构中的 **IDREFS** 特性指定这些元素之间的关系。 元素上的 **StudentIDList** 属性 **\<Course>** 是一个 **IDREFS** 类型属性，该属性引用注册表中的 StudentID 列。 同样，元素上的 **EnrolledIn** 属性 **\<Student>** 是一个 **IDREFS** 类型属性，该属性引用注册表中的 CourseID 列。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="StudentEnrollment"  
          parent="Student"  
          parent-key="StudentID"  
          child="Enrollment"  
          child-key="StudentID" />  
  
    <sql:relationship name="CourseEnrollment"  
          parent="Course"  
          parent-key="CourseID"  
          child="Enrollment"  
          child-key="CourseID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Course" sql:relation="Course"   
                             sql:key-fields="CourseID" >  
    <xsd:complexType>  
    <xsd:attribute name="CourseID"  type="xsd:string" />   
    <xsd:attribute name="CourseName"   type="xsd:string" />   
    <xsd:attribute name="StudentIDList" sql:relation="Enrollment"  
                 sql:field="StudentID"  
                 sql:relationship="CourseEnrollment"   
                                     type="xsd:IDREFS" />  
  
    </xsd:complexType>  
  </xsd:element>  
  <xsd:element name="Student" sql:relation="Student" >  
    <xsd:complexType>  
    <xsd:attribute name="StudentID"  type="xsd:string" />   
    <xsd:attribute name="LastName"   type="xsd:string" />   
    <xsd:attribute name="EnrolledIn" sql:relation="Enrollment"  
                 sql:field="CourseID"  
                 sql:relationship="StudentEnrollment"   
                                     type="xsd:IDREFS" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 一旦在 Updategram 中指定该架构，并在 Course 表中插入记录，则 Updategram 将在 Course 表中插入新的课程记录。 如果为 StudentIDList 属性指定一个或多个新学生 ID，则 Updategram 还将在 Enrollment 表中为每个新学生插入记录。 该 Updategram 将确保不向 Enrollment 表添加重复项。  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  在虚拟根目录中所指定的数据库内创建这些表：  
  
    ```  
    CREATE TABLE Student(StudentID varchar(10) primary key,   
                         LastName varchar(25))  
    CREATE TABLE Course(CourseID varchar(10) primary key,   
                        CourseName varchar(25))  
    CREATE TABLE Enrollment(StudentID varchar(10)   
                                      references Student(StudentID),  
                           CourseID varchar(10)   
                                      references Course(CourseID))  
    ```  
  
2.  添加以下示例数据：  
  
    ```  
    INSERT INTO Student VALUES ('S1','Davoli')  
    INSERT INTO Student VALUES ('S2','Fuller')  
  
    INSERT INTO Course VALUES  ('CS101', 'C Programming')  
    INSERT INTO Course VALUES  ('CS102', 'Understanding XML')  
  
    INSERT INTO Enrollment VALUES ('S1', 'CS101')  
    INSERT INTO Enrollment VALUES ('S1', 'CS102')  
    ```  
  
3.  复制上面的映射架构，并将它粘贴到文本文件中。 将该文件另存为 SampleSchema.xml。  
  
4.  将 Updategram (SampleUpdategram) 保存到上一步中用于保存映射架构的相同文件夹中。 （该 Updategram 从 CS102 课程中删除 StudentID="1" 的学生。）  
  
    ```  
    <ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
      <updg:sync mapping-schema="SampleSchema.xml" >  
        <updg:before>  
            <Student updg:id="x" StudentID="S1" LastName="Davolio"  
                                 EnrolledIn="CS101 CS102" />  
        </updg:before>  
        <updg:after >  
            <Student updg:id="x" StudentID="S1" LastName="Davolio"  
                                 EnrolledIn="CS101" />  
        </updg:after>  
      </updg:sync>  
    </ROOT>  
    ```  
  
5.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
6.  按照前面描述的步骤，保存并执行以下 Updategram。 该 Updategram 在 Enrollment 表中添加记录，从而将 StudentID="1" 的学生重新添加到 CS102 课程中。  
  
    ```  
    <ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
      <updg:sync mapping-schema="SampleSchema.xml" >  
        <updg:before>  
            <Student updg:id="x" StudentID="S1" LastName="Davolio"  
                                 EnrolledIn="CS101" />  
        </updg:before>  
        <updg:after >  
            <Student updg:id="x" StudentID="S1" LastName="Davolio"  
                                 EnrolledIn="CS101 CS102" />  
        </updg:after>  
      </updg:sync>  
    </ROOT>  
    ```  
  
7.  按照前面描述的步骤，保存并执行下一个 Updategram。 该 Updategram 插入三个新学生，并将他们注册到 CS101 课程。 同样，IDREFS 关系将在 Enrollment 表中插入记录。  
  
    ```  
    <ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
      <updg:sync mapping-schema="SampleSchema.xml" >  
        <updg:before>  
           <Course updg:id="y" CourseID="CS101"   
                               CourseName="C Programming" />  
        </updg:before>  
        <updg:after >  
           <Student updg:id="x1" StudentID="S3" LastName="Leverling" />  
           <Student updg:id="x2" StudentID="S4" LastName="Pecock" />  
           <Student updg:id="x3" StudentID="S5" LastName="Buchanan" />  
           <Course updg:id="y" CourseID="CS101"  
                               CourseName="C Programming"  
                               StudentIDList="S3 S4 S5" />  
        </updg:after>  
      </updg:sync>  
    </ROOT>  
    ```  
  
 这是等效的 XDR 架构：  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:dt="urn:schemas-microsoft-com:datatypes"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <ElementType name="Enrollment" sql:relation="Enrollment" sql:key-fields="StudentID CourseID">  
    <AttributeType name="StudentID" dt:type="id" />  
    <AttributeType name="CourseID" dt:type="id" />  
  
    <attribute type="StudentID" />  
    <attribute type="CourseID" />  
  </ElementType>  
  <ElementType name="Course" sql:relation="Course" sql:key-fields="CourseID">  
    <AttributeType name="CourseID" dt:type="id" />  
    <AttributeType name="CourseName" />  
  
    <attribute type="CourseID" />  
    <attribute type="CourseName" />  
  
    <AttributeType name="StudentIDList" dt:type="idrefs" />  
    <attribute type="StudentIDList" sql:relation="Enrollment" sql:field="StudentID" >  
        <sql:relationship  
                key-relation="Course"  
                key="CourseID"  
                foreign-relation="Enrollment"  
                foreign-key="CourseID" />  
    </attribute>  
  
  </ElementType>  
  <ElementType name="Student" sql:relation="Student">  
    <AttributeType name="StudentID" dt:type="id" />  
     <AttributeType name="LastName" />  
  
    <attribute type="StudentID" />  
    <attribute type="LastName" />  
  
    <AttributeType name="EnrolledIn" dt:type="idrefs" />  
    <attribute type="EnrolledIn" sql:relation="Enrollment" sql:field="CourseID" >  
        <sql:relationship  
                key-relation="Student"  
                key="StudentID"  
                foreign-relation="Enrollment"  
                foreign-key="StudentID" />  
    </attribute>  
  
    <element type="Enrollment" sql:relation="Enrollment" >  
        <sql:relationship key-relation="Student"  
                          key="StudentID"  
                          foreign-relation="Enrollment"  
                          foreign-key="StudentID" />  
    </element>  
  </ElementType>  
  
</Schema>  
```  
  
 有关使用映射架构的 updategram 的更多示例，请参阅 [在 Updategram &#40;SQLXML 4.0&#41;中指定带批注的映射架构 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;SQLXML 4.0&#41;的 Updategram 安全注意事项 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
