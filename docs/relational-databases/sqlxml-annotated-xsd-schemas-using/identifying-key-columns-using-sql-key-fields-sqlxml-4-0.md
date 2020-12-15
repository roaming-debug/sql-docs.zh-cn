---
title: 使用 sql： key 字段 (SQLXML) 标识键列
description: 了解如何通过在 XPath 查询中指定 sql： key 字段注释以标识键列，以确保 SQLXML 4.0 查询结果中的正确嵌套。
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- nesting XML results
- proper nesting in results [SQLXML]
- sql:key-fields
- XSD schemas [SQLXML], key columns
- identifying key columns [SQLXML]
- annotated XSD schemas, key columns
- key columns [SQLXML]
- relationships [SQLXML], key columns
- hierarchical relationships [SQLXML]
- key-fields annotation
ms.assetid: 1a5ad868-8602-45c4-913d-6fbb837eebb0
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9e24eac5a88215a1550beb8c8006852ed7263181
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461778"
---
# <a name="identifying-key-columns-using-sqlkey-fields-sqlxml-40"></a>使用 sql:key-fields 标识键列 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  针对 XSD 架构指定 XPath 查询时，大多数情况下必须有键信息才能获得结果中的正确嵌套。 指定 **sql：键字段** 批注是一种确保生成适当的层次结构的方法。  
  
> [!NOTE]  
>  若要确保正确的嵌套，建议为映射到表的元素指定 **sql：键字段** 。 所生成的 XML 对于基础结果集的排序敏感。 如果未指定 **sql：键字段** ，则生成的 XML 可能未正确生成。  
  
 **Sql： key-字段** 的值标识唯一标识关系中的行 () 的列。 如果需要多个列才能唯一标识某行，则用空格分隔列值。  
  
 当元素包含在 **\<sql:relationship>** 元素和子元素之间定义的，但未提供父元素中指定的表的主键时，必须使用 sql：键字段批注。  
  
## <a name="examples"></a>示例  
 若要创建使用以下示例的工作示例，必须满足某些要求。 有关详细信息，请参阅 [运行 SQLXML 示例的要求](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)。  
  
### <a name="a-producing-the-appropriate-nesting-when-sqlrelationship-does-not-provide-sufficient-information"></a>A. 当 \<sql:relationship> 未提供足够的信息时生成适当的嵌套  
 此示例显示了必须指定 **sql： key 字段** 的位置。  
  
 请考虑以下架构。 该架构指定了和元素之间的层次结构， **\<Order>** **\<Customer>** 其中 **\<Order>** 元素是父级， **\<Customer>** 元素是子元素。  
  
 **\<sql:relationship>** 标记用于指定父子关系。 它将 Sales.SalesOrderHeader 表中的 CustomerID 标识为父键，该父键引用 Sales.Customer 表中的 CustomerID 子键。 中提供的信息不足 **\<sql:relationship>** 以唯一标识父表中的行， (SalesOrderHeader) 。 因此，如果没有 **sql：键字段** 批注，则生成的层次结构不准确。  
  
 对于在上指定的 **sql：键字段** **\<Order>** ，批注唯一标识父 (SalesOrderHeader 表) 中的行，其子元素显示在其父元素之下。  
  
 以下是架构：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrdCust"  
        parent="Sales.SalesOrderHeader"  
        parent-key="CustomerID"  
        child="Sales.Customer"  
        child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader"   
               sql:key-fields="SalesOrderID">  
   <xsd:complexType>  
     <xsd:sequence>  
     <xsd:element name="Customer" sql:relation="Sales.Customer"   
                       sql:relationship="OrdCust"  >  
       <xsd:complexType>  
         <xsd:attribute name="CustID" sql:field="CustomerID" />  
         <xsd:attribute name="SoldBy" sql:field="SalesPersonID" />  
       </xsd:complexType>  
     </xsd:element>  
     </xsd:sequence>  
     <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
     <xsd:attribute name= "CustomerID" type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>创建此架构的工作示例  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 KeyFields1.xml。  
  
2.  复制以下模板，并将它粘贴到文本文件中。 在保存 KeyFields1.xml 的相同目录中将该文件另存为 KeyFields1T.xml。 模板中的 XPath 查询将返回 **\<Order>** CustomerID 小于3的所有元素。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="KeyFields1.xml">  
            /Order[@CustomerID < 3]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (KeyFields1.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\MyDir\KeyFields1.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  

     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 部分结果集如下：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
    <Order SalesOrderID="43860" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    <Order SalesOrderID="44501" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    <Order SalesOrderID="45283" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    .....  
</ROOT>  
```  
  
### <a name="b-specifying-sqlkey-fields-to-produce-proper-nesting-in-the-result"></a>B. 指定 sql:key-fields 以便在结果中生成正确的嵌套  
 在下面的架构中，没有使用指定的层次结构 **\<sql:relationship>** 。 该架构仍需要指定 **sql： key-fields** 批注以唯一标识 HumanResources 表中的雇员。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="HumanResources.Employee" sql:key-fields="EmployeeID" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="Title">  
          <xsd:complexType>  
            <xsd:simpleContent>  
              <xsd:extension base="xsd:string">  
                 <xsd:attribute name="EmployeeID" type="xsd:integer" />  
              </xsd:extension>  
            </xsd:simpleContent>  
          </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
   </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>创建此架构的工作示例  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 KeyFields2.xml。  
  
2.  复制以下模板，并将它粘贴到文本文件中。 在保存 KeyFields2.xml 的相同目录中将该文件另存为 KeyFields2T.xml。 模板中的 XPath 查询将返回所有 **\<HumanResources.Employee>** 元素：  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="KeyFields2.xml">  
        /HumanResources.Employee  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (KeyFields2.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\MyDir\KeyFields2.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 结果如下：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <HumanResources.Employee>  
    <Title EmployeeID="1">Production Technician - WC60</Title>   
  </HumanResources.Employee>  
  <HumanResources.Employee>  
    <Title EmployeeID="2">Marketing Assistant</Title>   
  </HumanResources.Employee>  
  <HumanResources.Employee>  
    <Title EmployeeID="3">Engineering Manager</Title>   
  </HumanResources.Employee>  
  ...  
</ROOT>  
```  
  
  
