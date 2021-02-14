---
title: 本地名称-从-QName (XQuery) |Microsoft Docs
description: 了解如何使用本地名称 from-QName () 函数返回 QName 的本地名称部分。
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:local-name-from-QName function
- local-name-from-QName function
ms.assetid: fafed718-8c3c-403f-93ee-ec51fc157a6e
author: rothja
ms.author: jroth
ms.openlocfilehash: 97852b8479df926a933e34f5a8dddff7eb240e65
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345001"
---
# <a name="functions-related-to-qnames---local-name-from-qname"></a>与 QName 相关的函数 - local-name-from-QName
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  返回一个 xs： NCNAME，它表示 *$arg* 指定的 QName 的本地部分。 如果 *$arg* 是空序列，则结果为空序列。  
  
## <a name="syntax"></a>语法  
  
```  
fn:local-name-from-QName($arg as xs:QName?) as xs:NCName?  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 它是应从中提取本地名称的 QName。  
  
## <a name="examples"></a>示例  
 本主题提供针对数据库中各种 **xml** 类型列中存储的 xml 实例的 XQuery 示例 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] 。  
  
 下面的示例使用 **本地名称 from-qname ()** 函数检索 QName 类型值中的本地名称和命名空间 URI 部分。 该示例执行以下操作：  
  
-   创建 XML 架构集合。  
  
-   创建带有 xml 类型列的表。 xml 类型是使用 XML 架构集合类型化的。  
  
-   将示例 XML 实例存储到表中。 使用 xml 数据类型的 **query ()** 方法时，将执行查询表达式，从实例中检索 QName 类型值的本地名称部分。  
  
```sql
DROP TABLE T  
go  
DROP XML SCHEMA COLLECTION SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="QNameXSD" >  
      <element name="root" type="QName" nillable="true"/>  
</schema>'  
go  
  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- following OK  
insert into T values ('<root xmlns="QNameXSD" xmlns:a="https://someURI">a:someLocalName</root>')  
 go  
-- Retrieve the local name.   
SELECT xmlCol.query('declare default element namespace "QNameXSD"; local-name-from-QName(/root[1])')  
FROM T  
-- Result = someLocalName  
-- You can retrieve namespace URI part from the QName using the namespace-uri-from-QName() function  
SELECT xmlCol.query('declare default element namespace "QNameXSD"; namespace-uri-from-QName(/root[1])')  
FROM T  
-- Result = https://someURI  
```  
  
## <a name="see-also"></a>另请参阅  
 [与 QNames &#40;XQuery 相关的函数&#41;](./functions-related-to-qnames-expanded-qname.md)  
  
