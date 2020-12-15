---
title: 通过 sql：加码 (SQLXML) 获取对 BLOB 数据的 URL 引用
description: 了解如何通过在 SQLXML 4.0 中指定 sql：编码批注来请求对 BLOB 数据的 URL 引用。
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sql:encode
- encode annotation
- URL references to BLOB data [SQLXML]
- references to BLOB data [SQLXML]
- annotated XSD schemas, URL references to BLOB data
- requesting URL references to BLOB data
- BLOBs, URL references
- Base 64-encoded format
ms.assetid: 2f8cd93b-c636-462b-8291-167197233ee0
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 432b1b888392b345038d14bb18909ba90b4d86a7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461748"
---
# <a name="requesting-url-references-to-blob-data-using-sqlencode-sqlxml-40"></a>使用 sql:encode 请求 BLOB 数据的 URL 引用 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  在带批注的 XSD 架构中，将属性（或元素）映射为 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 BLOB 列时，将在 XML 中以 Base 64 编码格式返回数据。  
  
 如果希望返回对数据的引用 (可供稍后用来检索二进制格式的 BLOB 数据的 URI) ，请指定 **sql：编码** 批注。 可以在简单类型的属性或元素上指定 **sql：编码** 。  
  
 指定 **sql：编码** 批注以指示应返回字段的 URL，而不是字段的值。 **sql：编码** 依赖于在 URL 中生成单一实例选择的主键。 可以使用 **sql：键字段** 批注指定主键。  
  
 可以为 **sql：编码** 批注分配 "url" 或 "默认" 值。 值为“default”将返回 Base 64 编码格式的数据。  
  
 Sql： **use-cdata** 或 ID、IDREF、IDREFS、NMTOKEN 或 NMTOKENS 属性类型不能使用 sql：**加码** 批注。 它也不能用于 XSD **固定** 属性。  
  
> [!NOTE]  
>  BLOB 类型的列不能用作键或外键的一部分。  
  
## <a name="examples"></a>示例  
 若要创建使用以下示例的工作示例，必须满足某些要求。 有关详细信息，请参阅 [运行 SQLXML 示例的要求](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)。  
  
### <a name="a-specifying-sqlencode-to-obtain-a-url-reference-to-blob-data"></a>A. 指定 sql:encode 以获取对 BLOB 数据的 URL 引用  
 在此示例中，映射架构对 **LargePhoto** 属性指定了 **sql：加码**，以检索对特定产品照片 (的 URI 引用，而不是检索以64编码格式) 的二进制数据。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  
  <xsd:element name="ProductPhoto" sql:relation="Production.ProductPhoto"   
               sql:key-fields="ProductPhotoID" >  
   <xsd:complexType>  
      <xsd:attribute name="ProductPhotoID"  type="xsd:int"  />  
     <xsd:attribute name="LargePhoto" type="xsd:string" sql:encode="url" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>针对架构测试示例 XPath 查询  
  
1.  复制上面的架构代码，并将它粘贴到文本文件中。 将文件另存为 sqlEncode.xml。  
  
2.  复制以下模板，并将它粘贴到文本文件中。 在保存 sqlEncode.xml 的相同目录中将该文件另存为 sqlEncodeT.xml。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="sqlEncode.xml">  
            /ProductPhoto[@ProductPhotoID=100]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     为映射架构 (sqlEncode.xml) 指定的目录路径是相对于模板保存目录的相对路径。 也可以指定绝对路径，例如：  
  
    ```  
    mapping-schema="C:\SqlXmlTest\sqlEncode.xml"  
    ```  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 执行该模板。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
 结果如下：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
   <ProductPhoto ProductPhotoID="100"  
                 LargePhoto="dbobject/Production.ProductPhoto[@ProductPhotoID="100"]/@LargePhoto" />   
</ROOT>  
```  
  
  
