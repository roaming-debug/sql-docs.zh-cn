---
title: " (XQuery) 顺序和 QNames |Microsoft Docs"
description: 在 XQuery 中了解序列和 QNames 的基本概念。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence [XQuery]
- XQuery, sequence
- QName [XQuery]
- predefined namespaces [XML in SQL Server]
ms.assetid: 3593ac26-dd78-4bf0-bb87-64fbcac5f026
author: rothja
ms.author: jroth
ms.openlocfilehash: 10caff2486f8dae470a4778c6da649b7bba5ddba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339921"
---
# <a name="sequence-and-qnames-xquery"></a>序列和 QName (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  本主题介绍以下 XQuery 基本概念：  
  
-   序列  
  
-   QNames 和预定义命名空间  
  
## <a name="sequence"></a>序列  
 在 XQuery 中，表达式的结果是由一系列 XML 节点和 XSD 原子类型的实例组成的序列。 序列中的单个项称为一项。 序列中的项可以是下列之一：  
  
-   一个节点，如元素、属性、文本、处理指令、注释或文档  
  
-   一个原子值，如 XSD 简单类型的实例  
  
 例如，下列查询构造了一个由两个元素节点项组成的序列：  
  
```  
SELECT Instructions.query('  
     <step1> Step 1 description goes here</step1>,  
     <step2> Step 2 description goes here </step2>  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7;  
  
```  
  
 结果如下：  
  
```  
<step1> Step 1 description goes here </step1>  
<step2> Step 2 description goes here </step2>   
```  
  
 在上一个查询中，`,` 构造结尾的逗号 (`<step1>`) 是序列构造符，是必需的。 结果中添加的空格只为说明使用，并且包含在本文档的所有示例结果中。  
  
 下面是应该了解的有关序列的其他信息：  
  
-   如果某个查询导致了包含其他序列的一个序列，则被包含的序列将简化为容器序列。 例如，在数据模型中将序列 ((1,2, (3,4,5)),6) 简化成 (1, 2, 3, 4, 5, 6)。  
  
    ```  
    DECLARE @x xml;  
    SET @x = '';  
    SELECT @x.query('(1,2, (3,4,5)),6');  
    ```  
  
-   空序列是不包含任何项的序列。 它被表示为“()”。  
  
-   只包含一项的序列可视为原子值，反之亦然。 也就是说，(1) = 1。  
  
 在这样的实现中，序列必须是同类的。 也就是说，您或者具有一个原子值序列，或者具有一个节点序列。 例如，下面是有效的序列：  
  
```  
DECLARE @x xml;  
SET @x = '';  
-- Expression returns a sequence of 1 text node (singleton).  
SELECT @x.query('1');  
-- Expression returns a sequence of 2 text nodes  
SELECT @x.query('"abc", "xyz"');  
-- Expression returns a sequence of one atomic value. data() returns  
-- typed value of the node.  
SELECT @x.query('data(1)');  
-- Expression returns a sequence of one element node.   
-- In the expression XML construction is used to construct an element.  
SELECT @x.query('<x> {1+2} </x>');  
```  
  
 下面的查询将返回一个错误，因为不支持异类序列。  
  
```  
SELECT @x.query('<x>11</x>, 22');  
```  
  
## <a name="qname"></a>QName  
 XQuery 中的每个标识符都是一个 QName。 QName 由一个命名空间前缀和一个本地名称组成。 在这样的实现中，XQuery 中的变量名是 QNames，它们不能带有前缀。  
  
 请考虑以下示例，其中针对非类型化的 **xml** 变量指定了查询：  
  
```  
DECLARE @x xml;  
SET @x = '<Root><a>111</a></Root>';  
SELECT @x.query('/Root/a');  
```  
  
 在表达式 (`/Root/a`) 中，`Root` 和 `a` 是 QNames。  
  
 在下面的示例中，对类型化的 **xml** 列指定了一个查询。 查询将循环访问 \<step> 第一个 workcenter 位置的所有元素。  
  
```  
SELECT Instructions.query('  
   declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $Step in /AWMI:root/AWMI:Location[1]/AWMI:step  
      return  
           string($Step)   
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7;  
```  
  
 在查询表达式中，注意下列事项：  
  
-   `AWMI root`、`AWMI:Location`、`AWMI:step` 和 `$Step` 都是 QNames。 `AWMI` 是一个前缀，`root`、`Location` 和 `Step` 都是本地名称。  
  
-   `$step` 变量是一个 QName 并且没有前缀。  
  
 已经预定义了下列命名空间，以便与 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的 XQuery 支持一起使用。  
  
|前缀|URI|  
|------------|---------|  
|xs|http://www.w3.org/2001/XMLSchema|  
|xsi|http://www.w3.org/2001/XMLSchema-instance|  
|xdt|http://www.w3.org/2004/07/xpath-datatypes|  
|fn|http://www.w3.org/2004/07/xpath-functions|  
|（无前缀）|`urn:schemas-microsoft-com:xml-sql`|  
|sqltypes|`https://schemas.microsoft.com/sqlserver/2004/sqltypes`|  
|xml|`http://www.w3.org/XML/1998/namespace`|  
|（无前缀）|`https://schemas.microsoft.com/sqlserver/2004/SOAP`|  
  
 你创建的每个数据库都有 **sys** XML 架构集合。 它包含着这些架构，以便可以从任何用户创建的 XML 架构集合中访问这些架构。  
  
> [!NOTE]  
>  此实现不支持前缀， `local` 如的 XQuery 规范中所述 http://www.w3.org/2004/07/xquery-local-functions 。  
  
## <a name="see-also"></a>另请参阅  
 [XQuery 基础知识](../xquery/xquery-basics.md)  
  
  
