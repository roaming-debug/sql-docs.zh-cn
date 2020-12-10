---
title: 定义 XML 数据的序列化 | Microsoft Docs
description: 了解在 SQL Server 中序列化 xml 数据时使用的规则。
ms.custom: ''
ms.date: 12/07/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- entitization rules [XML in SQL Server]
- serialization
- reparsing serialized XML structures
- encoding [XML in SQL Server]
- XML [SQL Server], serialization
- xml data type [SQL Server], serialization
- typed XML
ms.assetid: 42b0b5a4-bdd6-4a60-b451-c87f14758d4b
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 67201804cc1f93a9595ff46c02a57da7ea6e6109
ms.sourcegitcommit: 68063a1857f40487e6a2028de25990728419e3a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96749699"
---
# <a name="define-the-serialization-of-xml-data"></a>定义 XML 数据的序列化
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  将 xml 数据类型显式或隐式转换为 SQL 字符串或二进制类型时，将根据本主题中所述的规则对 xml 数据类型的内容进行序列化。  
  
## <a name="serialization-encoding"></a>序列化编码  
 如果 SQL 目标类型是 VARBINARY，则将使用前面具有 UTF-16 字节顺序标记且没有 XML 声明的 UTF-16 对结果进行序列化。 如果目标类型太小，将会引发错误。  
  
 例如：  
  
```sql
select CAST(CAST(N'<Δ/>' as XML) as VARBINARY(MAX))  
```  
  
 结果如下：  
  
```console
0xFFFE3C0094032F003E00  
```  
  
 如果 SQL 目标类型是 NVARCHAR 或 NCHAR，则使用前面没有字节顺序标记且没有 XML 声明的 UTF-16 对结果进行序列化。 如果目标类型太小，将会引发错误。  
  
 例如：  
  
```sql
select CAST(CAST(N'<Δ/>' as XML) as NVARCHAR(MAX))  
```  
  
 结果如下：  
  
```console
<Δ/>  
```  
  
 如果 SQL 目标类型是 VARCHAR 或 CHAR，则使用与数据库的排序规则代码页对应的编码（既没有字节顺序标记，也没有 XML 声明）对结果进行序列化。 如果目标类型太小或无法将值映射到目标排序规则代码页，将产生错误。  
  
 例如：  
  
```sql
select CAST(CAST(N'<Δ/>' as XML) as VARCHAR(MAX))  
```  
  
 如果当前排序规则的代码页无法表示 Unicode 字符  ，这可能导致错误，或者，它将用特定编码来表示该字符。  
  
 将 XML 结果返回到客户端时，将用 UTF-16 编码发送数据。 然后，客户端提供程序将根据 API 规则显示该数据。  
  
## <a name="serialization-of-the-xml-structures"></a>XML 结构的序列化  
 用通常的方式对 **xml** 数据类型的内容进行序列化。 确切地说，就是将元素节点映射到元素标记，将文本节点映射到文本内容。 不过，对字符进行实体化的环境以及对类型化的原子值进行序列化的方法将在下面两节中进行介绍。  
  
## <a name="entitization-of-xml-characters-during-serialization"></a>序列化期间 XML 字符的实体化  
 应该能够对每个序列化的 XML 结构进行重新分析。 因此，必须以实体化方式对某些字符进行序列化，从而在整个 XML 分析器的规范化阶段保留这些字符的往返能力。 不过，还必须对某些字符进行实体化，以便文档的格式正确并能够被分析。 下面是序列化期间应用的实体化规则：  
  
-   如果字符 &、\<, and > 出现在属性值或元素内容中，始终将它们分别实体化为 `&amp;`、`&lt;` 和 `&gt;`。  
  
-   因为 SQL Server 使用引号 (U+0022) 来闭合属性值，所以将属性值中的引号实体化为 `&quot;`。  
  
-   仅在服务器上进行转换时，将代理项对实体化为单个数字字符引用。 例如，将代理项对 U+D800 U+DF00 实体化为数字字符引用 `&#x00010300;`。  
  
-   为了防止 TAB (U+0009) 和换行符 (LF, U+000A) 在分析期间被规范化，在属性值内，将它们分别实体化为它们的数字字符引用 `&#x9;` 和 `&#xA;`。  
  
-   为了防止回车符 (CR, U+000D) 在分析期间被规范化，在属性值和元素内容中，将它实体化为它的数字字符引用 `&#xD;`。  
  
-   为了保护仅包含空格的文本节点，将其中一个空格字符（通常是最后一个）实体化为它的数字字符引用。 这样，重新分析将保留空格文本节点，而不考虑分析期间空格处理的设置。  
  
 例如：  
  
```sql
declare @u NVARCHAR(50)  
set @u = N'<a a="  
    '+NCHAR(0xD800)+NCHAR(0xDF00)+N'>">   '+NCHAR(0xA)+N'</a>'  
select CAST(CONVERT(XML,@u,1) as NVARCHAR(50))  
```  
  
 结果如下：  
  
```console
<a a="  
    𐌀>">     
</a>  
```  
  
 如果不想应用最后空格保护规则，可以在将 **xml** 转换为字符串或二进制类型时使用显式 CONVERT 选项 1。 例如，为避免实体化，可以执行以下语句：  
  
```sql
select CONVERT(NVARCHAR(50), CONVERT(XML, '<a>   </a>', 1), 1)  
```  
  
 请注意， [query() 方法（xml 数据类型）](../../t-sql/xml/query-method-xml-data-type.md) 会产生 xml 数据类型实例。 因此，将根据前面介绍的规则对转换为字符串或二进制类型的 **query()** 方法的任何结果进行实体化。 如果要获取不经过实体化的字符串值，则应改用 [value() 方法（xml 数据类型）](../../t-sql/xml/value-method-xml-data-type.md) 。 下面是使用 **query()** 方法的示例：  
  
```sql
declare @x xml  
set @x = N'<a>This example contains an entitized char: <.</a>'  
select @x.query('/a/text()')  
```  
  
 结果如下：  
  
```console
This example contains an entitized char: <.  
```  
  
 下面是使用 **value()** 方法的示例：  
  
```sql
select @x.value('(/a/text())[1]', 'nvarchar(100)')  
```  
  
 结果如下：  
  
```console
This example contains an entitized char: <.  
```  
  
## <a name="serializing-a-typed-xml-data-type"></a>对类型化的 xml 数据类型进行序列化  
 类型化的 **xml** 数据类型实例包括根据其 XML 架构类型类型化的值。 根据这些值的 XML 架构类型对这些值进行序列化，使用的格式与 XQuery 转换为 xs:string 所产生的格式相同。 有关详细信息，请参阅 [XQuery 中的类型转换规则](../../xquery/type-casting-rules-in-xquery.md)。  
  
 例如，下面的示例显示了将 xs:double 值 1.34e1 序列化为 13.4：  
  
```sql
declare @x xml  
set @x =''  
select CAST(@x.query('1.34e1') as nvarchar(50))  
```  
  
 将返回字符串值 13.4。  
  
## <a name="see-also"></a>另请参阅  
 [XQuery 中的类型转换规则](../../xquery/type-casting-rules-in-xquery.md)   
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
 
