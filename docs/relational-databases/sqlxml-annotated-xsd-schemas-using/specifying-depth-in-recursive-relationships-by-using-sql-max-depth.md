---
title: 设置递归深度关系与 sql：最大深度
description: 了解如何使用 XQuery 中的 sql：最大深度批注在查询具有递归关系的表时指定深度。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- max-depth annotation
- XPath queries [SQLXML], recursive relationships
- depth in recursive relationships [SQLXML]
- annotated XSD schemas, recursive relationships
- relationships [SQLXML], recursive relationships
- self joins
- recursive relationships [SQLXML]
- recursion [SQLXML]
- sql:max-depth
- recursive joins [SQLXML]
ms.assetid: 0ffdd57d-dc30-44d9-a8a0-f21cadedb327
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cbc39f5ea259007f19a57e77601d2528730ae88f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439698"
---
# <a name="specifying-depth-in-recursive-relationships-by-using-sqlmax-depth"></a>使用 sql:max-depth 指定递归关系中的深度
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  在关系数据库中，当表涉及与其自身的关系时，将它称为递归关系。 例如，在监督者和被监督者关系中，存储雇员记录的表涉及与其自身的关系。 在这种情况下，雇员表在关系的一端扮演监督者的角色，而在另一端则扮演被监督者的角色。  
  
 映射架构可以包含元素及其祖先属于相同类型的递归关系。  
  
## <a name="example-a"></a>示例 A  
 考虑以下表：  
  
```  
Emp (EmployeeID, FirstName, LastName, ReportsTo)  
```  
  
 在该表中，ReportsTo 列存储经理的雇员 ID。  
  
 假设您想生成一个雇员 XML 层次结构，其中经理雇员位于层次结构的顶部，而向该经理报告的雇员出现在对应的层次结构中，如以下 XML 片段示例所示。 此片段显示的内容是员工1的 *递归树* 。  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
     <Emp FirstName="Andrew" EmployeeID="2" LastName="Fuller" />   
     <Emp FirstName="Janet" EmployeeID="3" LastName="Leverling">  
        <Emp FirstName="Margaret" EmployeeID="4" LastName="Peacock">  
          <Emp FirstName="Steven" EmployeeID="5" LastName="Devolio">  
...  
...  
</root>  
```  
  
 在该片段中，雇员 5 向雇员 4 报告，雇员 4 向雇员 3 报告，雇员 3 和 2 向雇员 1 报告。  
  
 若要产生该结果，可以使用以下 XSD 架构，并指定针对它的 XPath 查询。 此架构描述 **\<Emp>** 类型为 EmployeeType 的元素，该元素包含 **\<Emp>** 一个相同类型的子元素 EmployeeType。 此即属于递归关系（元素和其祖先属于相同类型）。 此外，架构使用 **\<sql:relationship>** 来描述监控器和被监督者之间的父子关系。 请注意，在这 **\<sql:relationship>** 种情况下，Emp 既是父级又是子表。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"  
                                  parent="Emp"  
                                  parent-key="EmployeeID"  
                                  child="Emp"  
                                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp" type="EmployeeType"   
                          sql:relation="Emp"   
                          sql:key-fields="EmployeeID"   
                          sql:limit-field="ReportsTo" />  
  <xsd:complexType name="EmployeeType">  
    <xsd:sequence>  
      <xsd:element name="Emp" type="EmployeeType"   
                              sql:relation="Emp"   
                              sql:key-fields="EmployeeID"  
                              sql:relationship="SupervisorSupervisee"  
                              sql:max-depth="6" />  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:ID" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 因为该关系是递归关系，所以需要以某种方式指定在架构中的递归深度。 否则，结果将是无穷递归的（雇员向雇员报告而该雇员又向雇员报告，以此类推）。 **Sql：最大深度** 批注允许指定递归中的深度。 在此特定示例中，若要指定 **sql：最大深度** 值，你必须了解管理层次结构在公司中的深度。  
  
> [!NOTE]  
>  该架构指定 **sql： limit 字段** 批注，但未指定 **sql： limit-值** 注释。 这会使得所生成的层次结构的顶级节点仅为那些不向任何人报告的雇员。  (上级为 NULL。 ) 指定 **sql： limit 字段** ，而不指定 **sql： limit-值** (默认为 NULL) 注释即可完成此操作。 如果希望生成的 XML 包含每个可能的报表树 (表中每个雇员的报表树) ，请从该架构中删除 **sql： limit 字段** 批注。  
  
> [!NOTE]  
>  以下过程使用 tempdb 数据库。  
  
#### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  在虚拟根目录所指向的 tempdb 数据库中创建称为 Emp 的示例表。  
  
    ```  
    USE tempdb  
    CREATE TABLE Emp (  
           EmployeeID int primary key,   
           FirstName  varchar(20),   
           LastName   varchar(20),   
           ReportsTo int)  
    ```  
  
2.  添加以下示例数据：  
  
    ```  
    INSERT INTO Emp values (1, 'Nancy', 'Devolio',NULL)  
    INSERT INTO Emp values (2, 'Andrew', 'Fuller',1)  
    INSERT INTO Emp values (3, 'Janet', 'Leverling',1)  
    INSERT INTO Emp values (4, 'Margaret', 'Peacock',3)  
    INSERT INTO Emp values (5, 'Steven', 'Devolio',4)  
    INSERT INTO Emp values (6, 'Nancy', 'Buchanan',5)  
    INSERT INTO Emp values (7, 'Michael', 'Suyama',6)  
    ```  
  
3.  复制上面的架构代码，并将它粘贴到文本文件中。 将该文件另存为 maxDepth.xml。  
  
4.  复制以下模板，并将它粘贴到文本文件中。 该文件另存为 maxDepthT.xml，保存在与 maxDepth.xml 相同的目录中。 模板中的查询将返回 Emp 表中的所有雇员。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="maxDepth.xml">  
        /Emp  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (maxDepth.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\MyDir\maxDepth.xml"  
    ```  
  
5.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。 有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  

 结果如下：  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
  <Emp FirstName="Andrew" EmployeeID="2" LastName="Fuller" />   
    <Emp FirstName="Janet" EmployeeID="3" LastName="Leverling">  
      <Emp FirstName="Margaret" EmployeeID="4" LastName="Peacock">  
        <Emp FirstName="Steven" EmployeeID="5" LastName="Devolio">  
          <Emp FirstName="Nancy" EmployeeID="6" LastName="Buchanan">  
            <Emp FirstName="Michael" EmployeeID="7" LastName="Suyama" />   
          </Emp>  
        </Emp>  
      </Emp>  
    </Emp>  
  </Emp>  
</root>  
```  
  
> [!NOTE]  
>  若要在结果中产生不同深度的层次结构，请更改架构中 **sql：最大深度** 批注的值，并在每次更改后再次执行模板。  
  
 在前面的架构中，所有 **\<Emp>** 元素都具有完全相同的属性集 (**雇员 id**、 **FirstName** 和 **LastName**) 。 已对以下架构进行了略微修改，为向经理报告的所有元素返回其他 **上级** 属性 **\<Emp>** 。  
  
 例如，该 XML 片段显示雇员 1 的下级：  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
<Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
  <Emp FirstName="Andrew" EmployeeID="2"   
       ReportsTo="1" LastName="Fuller" />   
  <Emp FirstName="Janet" EmployeeID="3"   
       ReportsTo="1" LastName="Leverling">  
...  
...  
```  
  
 这是修改后的架构：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:documentation>  
      Customer-Order-Order Details Schema  
      Copyright 2000 Microsoft. All rights reserved.  
    </xsd:documentation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"   
                  parent="Emp"  
                  parent-key="EmployeeID"  
                  child="Emp"  
                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp"   
                   type="EmpType"   
                   sql:relation="Emp"   
                   sql:key-fields="EmployeeID"   
                   sql:limit-field="ReportsTo" />  
  <xsd:complexType name="EmpType">  
    <xsd:sequence>  
       <xsd:element name="Emp"   
                    type="EmpType"   
                    sql:relation="Emp"   
                    sql:key-fields="EmployeeID"  
                    sql:relationship="SupervisorSupervisee"  
                    sql:max-depth="6"/>  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:int" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
    <xsd:attribute name="ReportsTo" type="xsd:int" />  
  </xsd:complexType>  
</xsd:schema>  
```  
  
## <a name="sqlmax-depth-annotation"></a>sql:max-depth 批注  
 在由递归关系组成的架构中，必须在架构中显式指定递归的深度。 若要成功生成可返回所请求的结果的相应 FOR XML EXPLICIT 查询，则必须这样做。  
  
 使用架构中的 **sql： max 深度** 批注指定架构中描述的递归关系中的递归深度。 **Sql：最大深度** 批注的值为一个 (1 到 50) 的正整数，指示递归的数目：值为1时，将在指定了 **sql：最大深度** 批注的元素处停止递归;如果值为2，则将从指定了 **sql： max 深度** 的元素停止下一个级别的递归;依此类推。  
  
> [!NOTE]  
>  在基础实现中，针对映射架构指定的 XPath 查询将转换为 SELECT .。。FOR XML EXPLICIT 查询。 该查询需要您指定一个有限的递归深度。 为 **sql：最大深度** 指定的值越大，生成的 FOR XML EXPLICIT 查询就越大。 这可能会使检索时间变长。  
  
> [!NOTE]  
>  Updategram 和 XML 大容量加载将忽略最大深度批注。 这意味着，不管为最大深度指定了什么值，都将发生递归更新或插入。  
  
## <a name="specifying-sqlmax-depth-on-complex-elements"></a>在复杂元素上指定 sql:max-depth  
 可以在任何复杂内容元素上指定 **sql：最大深度** 批注。  
  
### <a name="recursive-elements"></a>递归元素  
 如果在递归关系中的父元素和子元素上都指定了 **sql： max 深度** ，则优先使用父元素上指定的 **sql：最大深度** 批注。 例如，在下面的架构中， **sql：最大深度** 批注同时在父和子 employee 元素上指定。 在这种情况下，将优先使用在父元素上指定的、 (扮演监察员) 角色的 **sql： max-depth = 4** **\<Emp>** 。 将忽略 (扮演被监督者) 角色的子元素上指定的 **sql： max 深度** **\<Emp>** 。  
  
#### <a name="example-b"></a>示例 B  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"  
                                  parent="Emp"  
                                  parent-key="EmployeeID"  
                                  child="Emp"  
                                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp" type="EmployeeType"   
                          sql:relation="Emp"   
                          sql:key-fields="EmployeeID"   
                          sql:limit-field="ReportsTo"   
                          sql:max-depth="3" />  
  <xsd:complexType name="EmployeeType">  
    <xsd:sequence>  
      <xsd:element name="Emp" type="EmployeeType"   
                              sql:relation="Emp"   
                              sql:key-fields="EmployeeID"  
                              sql:relationship="SupervisorSupervisee"  
                              sql:max-depth="2" />  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:ID" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 若要测试此架构，请按照本主题前面的示例 A 中提供的步骤进行操作。  
  
### <a name="nonrecursive-elements"></a>非递归元素  
 如果在不引发任何递归的架构中的某个元素上指定 **sql： max 深度** 批注，则将忽略该批注。 在下面的架构中， **\<Emp>** 元素由 **\<Constant>** 子元素组成，后者又具有 **\<Emp>** 子元素。  
  
 在此架构中，将忽略在元素上指定的 **sql：最大深度** 批注， **\<Constant>** 因为 **\<Emp>** 父元素和子元素之间不存在递归 **\<Constant>** 。 但在 **\<Emp>** 祖先和子级之间存在递归 **\<Emp>** 。 架构指定了这两个上的 **sql：最大深度** 批注。 因此，在主管角色中对上级 (指定的 **sql：最深层** 批注 **\<Emp>**) 优先。  
  
#### <a name="example-c"></a>示例 C  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"   
                  parent="Emp"   
                  child="Emp"   
                  parent-key="EmployeeID"   
                  child-key="ReportsTo"/>  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp"   
               sql:relation="Emp"   
               type="EmpType"  
               sql:limit-field="ReportsTo"  
               sql:max-depth="1" />  
    <xsd:complexType name="EmpType" >  
      <xsd:sequence>  
       <xsd:element name="Constant"   
                    sql:is-constant="1"   
                    sql:max-depth="20" >  
         <xsd:complexType >  
           <xsd:sequence>  
            <xsd:element name="Emp"   
                         sql:relation="Emp" type="EmpType"  
                         sql:relationship="SupervisorSupervisee"   
                         sql:max-depth="3" />  
         </xsd:sequence>  
         </xsd:complexType>  
         </xsd:element>  
      </xsd:sequence>  
      <xsd:attribute name="EmployeeID" type="xsd:int" />  
    </xsd:complexType>  
</xsd:schema>  
```  
  
 若要测试该架构，请执行为本主题前面的示例 A 提供的步骤。  
  
## <a name="complex-types-derived-by-restriction"></a>由限制派生的复杂类型  
 如果你使用派生复杂类型 **\<restriction>** ，则对应的基本复杂类型的元素无法指定 **sql：最大深度** 批注。 在这些情况下，可以将 **sql：最大深度** 批注添加到派生类型的元素中。  
  
 另一方面，如果您使用派生复杂类型 **\<extension>** ，则对应的基本复杂类型的元素可以指定 **sql：最大深度** 批注。  
  
 例如，以下 XSD 架构将生成错误，因为在基类型上指定了 **sql：最大深度** 批注。 在 **\<restriction>** 从另一种类型派生的类型上不支持此注释。 若要解决此问题，必须更改该架构，并在派生类型的元素上指定 **sql：最大深度** 批注。  
  
#### <a name="example-d"></a>示例 D  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:msdata="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:complexType name="CustomerBaseType">   
    <xsd:sequence>  
       <xsd:element name="CID" msdata:field="CustomerID" />  
       <xsd:element name="CompanyName"/>  
       <xsd:element name="Customers" msdata:max-depth="3">  
         <xsd:annotation>  
           <xsd:appinfo>  
             <msdata:relationship  
                     parent="Customers"  
                     parent-key="CustomerID"  
                     child-key="CustomerID"  
                     child="Customers" />  
           </xsd:appinfo>  
         </xsd:annotation>  
       </xsd:element>  
    </xsd:sequence>  
  </xsd:complexType>  
  <xsd:element name="Customers" type="CustomerType"/>  
  <xsd:complexType name="CustomerType">  
    <xsd:complexContent>  
       <xsd:restriction base="CustomerBaseType">  
          <xsd:sequence>  
            <xsd:element name="CID"   
                         type="xsd:string"/>  
            <xsd:element name="CompanyName"   
                         type="xsd:string"  
                         msdata:field="CName" />  
            <xsd:element name="Customers"   
                         type="CustomerType" />  
          </xsd:sequence>  
       </xsd:restriction>  
    </xsd:complexContent>  
  </xsd:complexType>  
</xsd:schema>   
```  
  
 在架构中，在 **CustomerBaseType** 复杂类型上指定了 **sql： max 深度**。 该架构还指定了 **\<Customer>** **CustomerType** 类型的元素，该元素派生自 **CustomerBaseType**。 在此类架构上指定的 XPath 查询将生成一个错误，因为在限制基类型中定义的元素上不支持 **sql：最大深度** 。  
  
## <a name="schemas-with-a-deep-hierarchy"></a>具有深层次结构的架构  
 您可能有一个包含深层次结构的架构，在该层次结构中，元素包含子元素，而该子元素又包含另一个子元素，以此类推。 如果在此类架构中指定的 **sql：最大深度** 批注生成一个 XML 文档，该文档包含500多个级别的层次结构，而该层次结构的级别为1，其子级别为1，在第2级 (其子元素，依此类推) ，会返回错误。  
  
  
