---
title: 使用 sql： use-CDATA (SQLXML) 创建 CDATA 节
description: 了解如何使用 sql： use-CDATA 批注在包含标记字符的文本块中，在 SQLXML 4.0 中创建 CDATA 节。
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- markup characters [SQLXML]
- special characters [SQLXML]
- use-cdata annotation
- plain text [SQLXML]
- CDATA sections
- escaping blocks of text [SQLXML]
- annotated XSD schemas, CDATA sections
- sql:use-cdata
ms.assetid: 26d2b9dc-f857-44ff-bcd4-aaf64ff809d0
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c7237148ffe168299ab20246261969c787a6179a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97415834"
---
# <a name="creating-cdata-sections-using-sqluse-cdata-sqlxml-40"></a>使用 sql:use-cdata 创建 CDATA 节 (SQLXML 4.0)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  在 XML 中，CDATA 节用于对那些所含字符不转义则会识别为标记字符的特定文本块进行转义。  
  
 Microsoft 中的数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 有时可以包含由 XML 分析器视为标记字符的字符; 例如，尖括号 ( # A0 和 >) 、小于号或等于符号 ( # B2 =) ，and 号 ( # A3) 被视为标记字符。 但是，可以在 CDATA 节中对这类特殊字符进行包装，使它们不被视为标记字符。 XML 语法分析程序将 CDATA 节中的文本视为纯文本。  
  
 **Sql： use-cdata** 批注用于指定由返回的数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应包装在 cdata 部分中 (也就是说，它指示由 **sql：字段** 指定的列中的值是否应包含在) 的 cdata 部分中。 只能在映射到数据库列的元素上指定 **sql： use-cdata** 批注。  
  
 **Sql： use-cdata** 批注采用布尔值 (0 = false，1 = true) 。 可接受的值为 0、1、true 和 false。  
  
 此批注不能与 **sql： url 编码** 或 ID、IDREF、IDREFS、NMTOKEN 和 NMTOKENS 属性类型一起使用。  
  
## <a name="examples"></a>示例  
 若要创建使用以下示例的工作示例，必须满足某些要求。 有关详细信息，请参阅 [运行 SQLXML 示例的要求](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)。  
  
### <a name="a-specifying-sqluse-cdata-on-an-element"></a>A. 在元素上指定 sql:use-cdata  
 在下面的架构中， **sql： use-cdata** 在元素内设置为 1 (True) **\<AddressLine1>** **\<Address>** 。 结果，将在 CDATA 节中返回数据。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Address"   
               sql:relation="Person.Address"   
               sql:key-fields="AddressID" >  
   <xsd:complexType>  
        <xsd:sequence>  
          <xsd:element name="AddressID"  type="xsd:string" />  
          <xsd:element name="AddressLine1" type="xsd:string"   
                       sql:use-cdata="1" />  
        </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 UseCData.xml。  
  
2.  复制以下模板，并将它粘贴到文本文件中。 在保存 UseCData.xml 的相同目录中将文件另存为 UseCDataT.xml。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="UseCData.xml">  
            /Address[AddressID < 11]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (UseCData.xml) 指定的目录路径相对于保存模板的目录。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\SqlXmlTest\UseCData.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 部分结果集如下：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">   
  <Address>   
    <AddressID>1</CustomerID>   
    <AddressLine1>   
      <![CDATA[ 1970 Napa Ct.  ]]>   
    </AddressLine1>   
  </Address>  
  <Address>  
    <AddressLine1>   
      <![CDATA[ 9833 Mt. Dias Blv. ]]>   
    </AddressLine1>   
  </Address>  
  ...  
</ROOT>  
```  
  
  
