---
title: " (XQuery) 为 true 函数 |Microsoft Docs"
description: 了解返回布尔值 True () XQuery 函数 true。
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:true function
- true function
ms.assetid: 318e370d-0444-4812-afe4-307df7ef9f3b
author: rothja
ms.author: jroth
ms.openlocfilehash: 779e3fa093d851e99e2869053f155fb70570f67c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340500"
---
# <a name="boolean-constructor-functions---true-xquery"></a>布尔构造函数 - true (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  返回 xs:boolean 值 True。 这等效于 `xs:boolean("1")`。  
  
## <a name="syntax"></a>语法  
  
```  
fn:true() as xs:boolean  
```  
  
## <a name="examples"></a>示例  
 本主题提供了对存储在 AdventureWorks 数据库的各种 **xml** 类型列中的 xml 实例的 XQuery 示例。  
  
### <a name="a-using-the-true-xquery-boolean-function"></a>A. 使用 XQuery 布尔函数 true()  
 下面的示例将查询一个非类型化的 **xml** 变量。 如果 "aaa" 是属性值，则 **值 ()** 方法中的表达式将返回布尔值 **true ()** 。 **Xml** 数据类型的 **值 ()** 方法将布尔值转换为位并将其返回。  
  
```  
DECLARE @x XML  
SET @x= '<ROOT><elem attr="aaa">bbb</elem></ROOT>'  
select @x.value(' if ( (/ROOT/elem/@attr)[1] eq "aaa" ) then fn:true() else fn:false() ', 'bit')  
go  
-- result = 1  
```  
  
 在下面的示例中，对类型化的 **xml** 列指定了查询。 `if`表达式检查 <> 元素的类型化布尔值， `ROOT` 并相应地返回构造的 XML。 该示例执行以下操作：  
  
-   创建定义 `ROOT` xs： boolean 类型的 <> 元素的 XML 架构集合。  
  
-   使用 XML 架构集合创建包含类型化 **xml** 列的表。  
  
-   将 XML 实例保存在列中并查询它。  
  
```  
-- Drop table if exist  
--DROP TABLE T  
--go  
DROP XML SCHEMA COLLECTION SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="QNameXSD" >  
      <element name="ROOT" type="boolean" nillable="true"/>  
</schema>'  
go  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- following OK  
insert into T values ('<ROOT xmlns="QNameXSD">true</ROOT>')  
 go  
-- Retrieve the local name.   
SELECT xmlCol.query('declare namespace a="QNameXSD";   
   if (/a:ROOT[1] eq true()) then  
       <result>Found boolean true</result>  
   else  
       <result>Found boolean false</result>')  
  
FROM T  
-- result = <result>Found boolean true</result>  
-- Clean up  
DROP TABLE T  
go  
DROP XML SCHEMA COLLECTION SC  
go  
```  
  
## <a name="see-also"></a>另请参阅  
 [布尔构造函数 &#40;XQuery&#41;](./xquery-functions-against-the-xml-data-type.md)  
  
