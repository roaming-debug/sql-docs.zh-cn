---
title: " (SQLXML) 带批注的 XSD 架构简介"
description: 了解如何使用 XML 架构定义 (XSD) language (SQLXML 4.0) 创建关系数据的 XML 视图。
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- namespaces [SQLXML], annotated XSD schemas
- mapping schema [SQLXML], about mapping schema
- views [SQLXML]
- valid XSD schemas [SQLXML]
- annotations [SQLXML]
- XSD schemas [SQLXML], creating XML views
- annotated XSD schemas, creating XML views
- minimum XSD schema
- annotated XSD schemas, examples
- XML views [SQLXML]
ms.assetid: 15282db1-65c4-43be-bdb7-e9ef49cb33a2
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d82028477d11cb53034a8ea3f6e40fde17cf205e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467088"
---
# <a name="introduction-to-annotated-xsd-schemas-sqlxml-40"></a>带批注的 XSD 架构简介 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  您可以通过使用 XML 架构定义 (XSD) 语言创建关系数据的 XML 视图。 然后可通过使用 XML Path 语言 (XPath) 查询对这些视图进行查询。 这类似于使用 CREATE VIEW 语句创建视图，然后针对视图指定 SQL 查询。  
  
 XML 架构描述了 XML 文档的结构并且还描述了对文档中数据的各种约束。 针对该架构指定 XPath 查询时，返回的 XML 文档的结构由对其执行 XPath 查询的架构确定。  
  
 在 XSD 架构中， **\<xsd:schema>** 元素包含整个架构; 所有元素声明都必须包含在 **\<xsd:schema>** 元素中。 您可以描述定义架构所在的命名空间的属性，以及架构中用作元素属性的命名空间 **\<xsd:schema>** 。  
  
 有效的 XSD 架构必须包含 **\<xsd:schema>** 定义的元素，如下所示：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<!-- additional schema definitions here -->  
</xsd:schema>  
```  
  
 **\<xsd:schema>** 元素派生自中的 XML 架构命名空间规范 http://www.w3.org/2001/XMLSchema 。  
  
## <a name="annotations-to-the-xsd-schema"></a>XSD 架构的批注  
 您可以对 XSD 架构使用批注来描述数据库映射、查询数据库并返回 XML 文档形式的结果。 使用批注可将 XSD 架构映射到数据库表和列。 可以对 XSD 架构创建的 XML 视图指定 XPath 查询来查询数据库并获取 XML 形式的结果。  
  
> [!NOTE]  
>  在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 中，XSD 架构语言支持随 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 中带批注的 XML-Data Reduced (XDR) 架构语言引入的批注。 SQLXML 4.0 中不推荐使用带批注的 XDR。  
  
 在关系数据库上下文中，将任意 XSD 架构映射到关系存储区很有用。 实现这种映射的一种方法是对 XSD 架构进行批注。 带有批注的 XSD 架构称为 *映射架构*，它提供有关如何将 XML 数据映射到关系存储区的信息。 实际上，映射架构是关系数据的 XML 视图。 使用这些映射能够以 XML 文档形式检索关系数据。  
  
## <a name="namespace-for-annotations"></a>批注的命名空间  
 在 XSD 架构中，批注通过使用命名空间 **urn： schema-microsoft-com： mapping-schema** 来指定。 如下面的示例中所示，指定命名空间的最简单方法是在标记中指定它 **\<xsd:schema>** 。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
...  
</xsd:schema>  
```  
  
 命名空间可采用任意前缀。 在本文档中， **sql** 前缀用于表示批注命名空间，并将此命名空间中的批注与其他命名空间中的批注区分开来。  
  
## <a name="example-of-an-annotated-xsd-schema"></a>带批注的 XSD 架构的示例  
 在下面的示例中，XSD 架构由元素组成 **\<Person.Contact>** 。 **\<Employee>** 元素具有 **ContactID** 特性和 **\<FirstName>** 和 **\<LastName>** 子元素：  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
  <xsd:element name="Contact" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="FName"    
                     type="xsd:string" />   
        <xsd:element name="LName"  
                     type="xsd:string" />  
     </xsd:sequence>  
        <xsd:attribute name="ConID" type="xsd:integer" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 向该 XSD 架构添加批注以将其元素和属性映射到数据库表和列：  
  
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
        <xsd:attribute name="ConID"   
                       sql:field="ContactID"   
                       type="xsd:integer" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 在映射架构中， **\<Contact>** 使用 **sql： relation** 批注将元素映射到示例 AdventureWorks 数据库中的 Contact 表。 属性 ConID、FName 和 LName 映射到 Person 中的 ContactID、FirstName 和 LastName 列。请通过使用 **sql： field** 批注来联系表。  
  
 此带有批注的 XSD 架构提供了关系数据的 XML 视图。 可使用 XPath 语言查询此 XML 视图。 XPath 查询返回的结果是一个 XML 文档，而 SQL 查询返回的结果是行集。  
  
> [!NOTE]  
>  在映射架构中，指定关系值（如表名和列名）是否区分大小写取决于 SQL Server 是否使用区分大小写的排序规则设置。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../../relational-databases/collations/collation-and-unicode-support.md)。  
  
## <a name="other-resources"></a>其他资源  
 有关 XML 架构定义语言 (XSD)、XML Path 语言 (XPath) 和可扩展样式表语言转换 (XSLT) 的详细信息，请访问以下网站：  
  
-   XML 架构第0部分：入门、W3C 建议 (https://www.w3.org/TR/xmlschema-0/)  
  
-   XML 架构第1部分：结构、W3C 建议 (https://www.w3.org/TR/xmlschema-1/)  
  
-   XML 架构第2部分：数据类型、W3C 建议 (https://www.w3.org/TR/xmlschema-2/)  
  
-   XML 路径语言 (XPath)  (https://www.w3.org/TR/xpath)  
  
-    (XSLT) 的 XSL 转换 (https://www.w3.org/TR/xslt)  
  
## <a name="see-also"></a>另请参阅  
 [&#40;SQLXML 4.0 的批注的架构安全注意事项&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/annotated-schema-security-considerations-sqlxml-4-0.md)   
 [SQLXML 4.0 中 &#40;弃用的带批注 XDR 架构&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md)  
  
  
