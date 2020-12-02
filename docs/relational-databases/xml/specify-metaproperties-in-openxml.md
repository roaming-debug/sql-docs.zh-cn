---
title: 在 OPENXML 中指定元属性 | Microsoft Docs
description: 了解如何在 OPENXML 语句中指定元属性特性，以便从 XML 节点提取信息。
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- overflow in XML document [SQL Server]
- metaproperties [XML in SQL Server]
- unconsumed data
- extracting information of XML nodes [SQL Server]
- OPENXML statement, metaproperties
ms.assetid: 29bfd1c6-3f9a-43c4-924a-53d438e442f4
author: MightyPen
ms.author: genemi
ms.openlocfilehash: fd34411b00bfa89c5c69b0d71073ee1c0d4d2280
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130825"
---
# <a name="specify-metaproperties-in-openxml"></a>在 OPENXML 中指定元属性
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  XML 文档中的元属性特性用于描述 XML 项（例如元素节点、属性节点或其他任何 DOM 节点）的属性。 这些特性并不实际存在于 XML 文档文本中。 但是，OPENXML 将为所有 XML 项提供这些元属性。 通过这些元属性可以提取 XML 节点的信息（例如本地定位和命名空间信息）。 这些信息将提供比文字表现形式更加详细的信息。  
  
 可以使用 *ColPattern* 参数将这些元属性映射到 OPENXML 语句中的行集列。 这些列将包含它们所映射到的元属性的值。 有关 OPENXML 语法的详细信息，请参见 [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)。  
  
 若要访问元属性特性，应提供特定于 SQL Server 的命名空间。 此命名空间 ( **urn:schemas-microsoft-com:xml-metaprop** ) 使用户能够访问元属性特性。 如果 OPENXML 查询结果以边缘表的格式返回，则每个元属性特性在边缘表中都有相应的一列（ **xmltext** 元属性除外）。  
  
 有些元属性特性用于处理目的。 例如， **xmltext** 元属性特性用于溢出处理。 溢出处理针对文档中未用完/未处理的数据。 可以将 OPENXML 所生成的行集中的一列标识为溢出列。 可以通过使用 **ColPattern** 参数将该列映射到 *xmltext* 元属性来执行此操作。 然后，该列将接收溢出数据。 *flags* 参数用于确定该列是包含所有数据还是只包含未用完的数据。  
  
 下表列出了每个经过分析的 XML 元素所具有的元属性特性。 可以使用命名空间 **urn:schemas-microsoft-com:xml-metaprop** 来访问这些元属性特性。 用户使用这些元属性在 XML 文档中直接设置的任何值均会被忽略。  
  
> [!NOTE]  
>  不能在任何 XPath 导航中引用这些元属性。  
  
|元属性特性|说明|  
|----------------------------|-----------------|  
|**\@mp:id**|提供由系统生成的、文档范围的 DOM 节点标识符。 只要文档未被重新分析，此 ID 就会引用同一个 XML 节点。<br /><br /> XML ID 为 **0** 表明该元素是根元素。 其父 XML ID 为 NULL。|  
|**\@mp:localname**|存储节点名的本地部分。 与前缀及命名空间 URI 一起用于命名元素节点或属性节点。|  
|**\@mp:namespaceuri**|提供当前元素的命名空间 URI。 如果此特性的值为 NULL，则表明不存在命名空间。|  
|**\@mp:prefix**|存储当前元素名的命名空间前缀。<br /><br /> 如果不存在前缀 (NULL) 且给定了 URI，则表明指定的命名空间为默认命名空间。 如果没有给定 URI，则表明没有附加命名空间。|  
|**\@mp:prev**|存储相对于节点的前一个同级元素。 此特性将提供有关元素在文档中的排序顺序的信息。<br /><br /> \@mp:prev 包含父元素相同的上一个同级元素的 XML ID。 如果元素是同级列表的首个元素，\@mp:prev 为空。|  
|**\@mp:xmltext**|用于处理目的。 它是元素及其属性以及 OPENXML 溢出处理中所使用的子元素的文本序列化。|  
  
 下表显示了使您得以检索关于层次结构的信息的其他父属性。  
  
|父元属性特性|说明|  
|-----------------------------------|-----------------|  
|**\@mp:parentid**|对应于 ../\@mp:id|  
|**\@mp:parentlocalname**|对应于 ../\@mp:localname|  
|**\@mp:parentnamespacerui**|对应于 ../\@mp:namespaceuri|  
|**\@mp:parentprefix**|对应于 ../\@mp:prefix|  
  
## <a name="examples"></a>示例  
 下列示例说明了如何使用 OPENXML 来创建不同的行集视图。  
  
### <a name="a-mapping-the-openxml-rowset-columns-to-the-metaproperties"></a>A. 将 OPENXML 行集列映射到元属性  
 此示例使用 OPENXML 创建该示例 XML 文档的行集视图。 它专门显示了如何使用 *ColPattern* 参数将各种元属性特性映射到 OPENXML 语句中的行集列。  
  
 OPENXML 语句说明了以下信息：  
  
-   id 列映射到 \@mp:id 元属性，并指明列中包含元素的系统生成唯一 XML ID。  
  
-   parent 列映射到 \@mp:parentid，并指明列中包含元素的父元素的 XML ID。  
  
-   parentLocalName 列映射到 \@mp:parentlocalname，并指明列中包含父元素的本地名称。  
  
 然后，SELECT 语句将返回由 OPENXML 生成的行集：  
  
```  
DECLARE @idoc int  
DECLARE @doc nvarchar(1000)  
-- Sample XML document  
SET @doc = N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@idoc, '/root/Customer/Order', 9)  
      WITH (id int '@mp:id',   
            oid char(5),   
            date datetime,   
            amount real,   
            parentIDNo int '@mp:parentid',   
            parentLocalName varchar(40) '@mp:parentlocalname')  
EXEC sp_xml_removedocument @idoc  
```  
  
 结果如下：  
  
```  
id   oid         date                amount    parentIDNo  parentLocalName    
--- ------- ---------------------- ---------- ------------ ---------------  
6    O1    1996-01-20 00:00:00.000     3.5         2        Customer  
10   O2    1997-04-30 00:00:00.000     13.4        2        Customer  
19   O3    1999-07-14 00:00:00.000     100.0       15       Customer  
25   O4    1996-01-20 00:00:00.000     10000.0     15       Customer  
```  
  
### <a name="b-retrieving-the-whole-xml-document"></a>B. 检索整个 XML 文档  
 在本例中，使用 OPENXML 来创建示例 XML 文档的单列行集视图。 此列 ( **Col1**) 将映射到 **xmltext** 元属性，并成为一个溢出列。 因此，此列将接收未用完的数据。 在这种情况下，它就是整个文档。  
  
 然后，SELECT 语句将返回整个行集。  
  
```  
DECLARE @idoc int  
DECLARE @doc nvarchar(1000)  
SET @doc = N'<?xml version="1.0"?>  
<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very   
             satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
     <MyTag>Testing to see if all the subelements are returned</MyTag>  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@idoc, '/')  
   WITH (Col1 ntext '@mp:xmltext')  
```  
  
 若要在没有 XML 声明的情况下检索整个文档，可以将查询指定为以下形式：  
  
```  
SELECT *  
FROM OPENXML (@idoc, '/root')  
   WITH (Col1 ntext '@mp:xmltext')  
EXEC sp_xml_removedocument @idoc  
```  
  
 查询将返回带有名称根的根元素以及根元素所包含的数据。  
  
### <a name="c-specifying-the-xmltext-metaproperty-to-retrieve-the-unconsumed-data-in-a-column"></a>C. 指定 xmltext 元属性来检索列中未使用的数据  
 此示例使用 OPENXML 创建该示例 XML 文档的行集视图。 本例显示了如何通过将 **xmltext** 元属性特性映射到 OPENXML 中的行集列来检索未用完的 XML 数据。  
  
 通过将 comment 列映射到 \@mp:xmltext 元属性，把它标识为溢出列。 *flags* 参数将设置为 **9** （XML_ATTRIBUTE 和 XML_NOCOPY）。 这指明了 **attribute-centric** 映射，并指明只有未用完的数据才应当被复制到溢出列中。  
  
 然后，SELECT 语句返回由 OPENXML 生成的行集。  
  
 此示例为 OPENXML 所生成行集中的 ParentLocalName 列设置了 \@mp:parentlocalname 元属性。 因此，此列包含父元素的本地名。  
  
 在行集中另外还指定了两列， **parent** 和 **comment**。 parent 列映射到 \@mp:parentid，并指明列中包含元素的父元素的 XML ID。 通过将 comment 列映射到 \@mp:xmltext 元属性，把它标识为溢出列。  
  
```  
DECLARE @idoc int  
DECLARE @doc nvarchar(1000)  
-- sample XML document  
SET @doc = N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>  
'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@idoc, '/root/Customer/Order', 9)  
      WITH (oid char(5),   
            date datetime,  
            comment ntext '@mp:xmltext')  
EXEC sp_xml_removedocument @idoc  
```  
  
 结果如下： 因为 oid 列和 date 列已经用完，因此它们不会出现在溢出列中。  
  
```  
oid   date                        comment                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            
----- --------------------------- ----------------------------------------  
O1    1996-01-20 00:00:00.000     <Order amount="3.5"/>  
O2    1997-04-30 00:00:00.000     <Order amount="13.4">Customer was very   
                                   satisfied</Order>  
O3    1999-07-14 00:00:00.000     <Order amount="100" note="Wrap it blue   
                                   white red"><Urgency>   
                                   Important</Urgency></Order>  
O4    1996-01-20 00:00:00.000     <Order amount="10000"/>  
```  
  
## <a name="see-also"></a>另请参阅  
 [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)   
 [OPENXML (SQL Server)](../../relational-databases/xml/openxml-sql-server.md)  
  
  
