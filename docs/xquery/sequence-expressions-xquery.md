---
title: 序列表达式 (XQuery) |Microsoft Docs
description: 了解用于构造、筛选和组合项序列的 XQuery 序列表达式。
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence [XQuery]
- expressions [XQuery], sequence
- filtering sequences [XQuery]
ms.assetid: 41e18b20-526b-45d2-9bd9-e3b7d7fbce4e
author: rothja
ms.author: jroth
ms.openlocfilehash: ea654a45894791fa22ecfa843c44cdd6214a5181
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351063"
---
# <a name="sequence-expressions-xquery"></a>序列表达式 (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持用于对项序列进行构造、筛选和组合的 XQuery 运算符。 项可以是原子值，也可以是节点。  
  
## <a name="constructing-sequences"></a>构造序列  
 可以使用逗号运算符来构造序列，将多个项连接成一个序列。  
  
 序列可以包含重复的值。 嵌套序列（序列内又有序列）将被折叠。 例如，序列 (1, 2, (3, 4, (5))) 变成 (1, 2, 3, 4, 5)。 下面是构造序列的示例。  
  
### <a name="example-a"></a>示例 A  
 以下查询返回由五个原子值构成的序列：  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2,3,4,5)')  
go  
-- result 1 2 3 4 5  
```  
  
 以下查询返回由两个节点构成的序列：  
  
```  
-- sequence of 2 nodes  
declare @x xml  
set @x=''  
select @x.query('(<a/>, <b/>)')  
go  
-- result  
<a />  
<b />  
```  
  
 以下查询返回错误，因为您构造的序列同时包含原子值和节点。 这是一个异类序列，不支持此序列。  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1, 2, <a/>, <b/>)')  
go  
```  
  
### <a name="example-b"></a>示例 B  
 以下查询通过将四个不同长度的序列组合成一个序列，构造原子值序列。  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2),10,(),(4, 5, 6)')  
go  
-- result = 1 2 10 4 5 6  
```  
  
 可以使用 FLOWR 和 ORDER BY 对该序列进行排序：  
  
```  
declare @x xml  
set @x=''  
select @x.query('for $i in ((1,2),10,(),(4, 5, 6))  
                  order by $i  
                  return $i')  
go  
```  
  
 可以使用 **fn： count ()** 函数对序列中的项进行计数。  
  
```  
declare @x xml  
set @x=''  
select @x.query('count( (1,2,3,(),4) )')  
go  
-- result = 4  
```  
  
### <a name="example-c"></a>示例 C  
 下面的查询是针对 Contact 表中 **xml** 类型的 AdditionalContactInfo 列指定的。 此列存储附加联系信息，如一个或多个附加电话号码、寻呼机号码和地址。 \<telephoneNumber>、 \<pager> 和其他节点可以出现在文档中的任何位置。 查询将构造一个序列，该序列包含 \<telephoneNumber> 上下文节点的所有子级，后跟 \<pager> 子元素。 注意返回表达式 (`($a//act:telephoneNumber, $a//act:pager)`) 中逗号序列运算符的使用。  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
 'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS aci)  
  
SELECT AdditionalContactInfo.query('  
   for $a in /aci:AdditionalContactInfo   
   return ($a//act:telephoneNumber, $a//act:pager)  
') As Result  
FROM Person.Contact  
WHERE ContactID=3  
```  
  
 结果如下：  
  
```  
<act:telephoneNumber xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3333</act:number>  
</act:telephoneNumber>  
<act:telephoneNumber xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3334</act:number>  
</act:telephoneNumber>  
<act:pager xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>999-555-1244</act:number>  
  <act:SpecialInstructions>  
Page only in case of emergencies.  
</act:SpecialInstructions>  
</act:pager>  
```  
  
## <a name="filtering-sequences"></a>筛选序列  
 可以通过向表达式中添加谓词来筛选表达式返回的序列。 有关详细信息，请参阅 [路径表达式 &#40;XQuery&#41;](../xquery/path-expressions-xquery.md)。 例如，下面的查询将返回一个由三个 <`a`> 元素节点组成的序列：  
  
```  
declare @x xml  
set @x = '<root>  
<a attrA="1">111</a>  
<a></a>  
<a></a>  
</root>'  
SELECT @x.query('/root/a')  
```  
  
 结果如下：  
  
```  
<a attrA="1">111</a>  
<a />  
<a />  
```  
  
 若要仅检索 `a` attrA 属性 <> 元素，可以在谓词中指定一个筛选器。 生成的序列将只有一个 <`a`> 元素。  
  
```  
declare @x xml  
set @x = '<root>  
<a attrA="1">111</a>  
<a></a>  
<a></a>  
</root>'  
SELECT @x.query('/root/a[@attrA]')  
```  
  
 结果如下：  
  
```  
<a attrA="1">111</a>  
```  
  
 有关如何在路径表达式中指定谓词的详细信息，请参阅 [在路径表达式步骤中指定谓词](../xquery/path-expressions-specifying-predicates.md)。  
  
 下面的示例生成子树的序列表达式并对该序列应用筛选器。  
  
```  
declare @x xml  
set @x = '  
<a>  
  <c>C under a</c>  
</a>  
<b>    
   <c>C under b</c>  
</b>  
<c>top level c</c>  
<d></d>  
'  
```  
  
 `(/a, /b)` 中的表达式构造具有子树 `/a` 和 `/b` 的序列，并且该表达式将从得到的序列中筛选元素 `<c>`。  
  
```  
SELECT @x.query('  
  (/a, /b)/c  
')  
```  
  
 结果如下：  
  
```  
<c>C under a</c>  
<c>C under b</c>  
```  
  
 下面的示例应用谓词筛选器。 表达式查找 <`a`> 和 `b`> <包含元素 <> 的元素 `c` 。  
  
```  
declare @x xml  
set @x = '  
<a>  
  <c>C under a</c>  
</a>  
<b>    
   <c>C under b</c>  
</b>  
  
<c>top level c</c>  
<d></d>  
'  
SELECT @x.query('  
  (/a, /b)[c]  
')  
```  
  
 结果如下：  
  
```  
<a>  
  <c>C under a</c>  
</a>  
<b>  
  <c>C under b</c>  
</b>  
```  
  
### <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-   不支持 XQuery 范围表达式。  
  
-   必须是同类序列。 确切地说，序列中的所有项必须都是节点，或者都是原子值。 这是通过静态检查来确定的。  
  
-   不支持使用 union、intersect 或 except 运算符组合节点序列。  
  
## <a name="see-also"></a>另请参阅  
 [XQuery 表达式](../xquery/xquery-expressions.md)  
  
  
