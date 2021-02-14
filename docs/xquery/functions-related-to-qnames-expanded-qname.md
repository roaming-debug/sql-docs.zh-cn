---
title: 展开-QName (XQuery) |Microsoft Docs
description: 了解如何使用展开的 QName () 函数返回 QName 的命名空间 URI 和本地名称部分。
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
- expanded-QName function
- fn:expanded-QName function
ms.assetid: b8377042-95cc-467b-9ada-fe43cebf4bc3
author: rothja
ms.author: jroth
ms.openlocfilehash: 704cac94b48fe172eaa6a972cdd6c49bad1601fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344932"
---
# <a name="functions-related-to-qnames---expanded-qname"></a>与 QName 相关的函数 - expanded-QName
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  返回 xs： QName 类型的值，该类型具有在 *$paramURI* 中指定的命名空间 URI 和 *$paramLocal* 中指定的本地名称。 如果 *$paramURI* 为空字符串或空序列，则它不表示任何命名空间。  
  
## <a name="syntax"></a>语法  
  
```  
fn:expanded-QName($paramURI as xs:string?, $paramLocal as xs:string?) as xs:QName?  
```  
  
## <a name="arguments"></a>参数  
 *$paramURI*  
 QName 的命名空间 URI。  
  
 *$paramLocal*  
 QName 的本地名称部分。  
  
## <a name="remarks"></a>备注  
 以下内容适用于已 **展开的 QName ()** 函数：  
  
-   如果指定的 *$paramLocal* 值不是 Xs： NCName 类型的正确词法格式，则返回空序列并表示动态错误。  
  
-   在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中不支持从 xs:QName 类型转换为任何其他类型。 因此，不能在 XML 构造中使用 **展开的 QName ()** 函数。 例如，当构造节点（如 `<e> expanded-QName(...) </e>`）时，其值就必须是非类型化的。 这就要求您将 `expanded-QName()` 返回的 xs:QName 类型值转换为 xdt:untypedAtomic。 但是，不支持此操作。 在本主题的后面的示例中给出了一种解决方案。  
  
-   您可以修改或比较现有的 QName 类型值。 例如， `/root[1]/e[1] eq expanded-QName("http://nsURI" "myNS")` 将元素的值（<`e`>）与已 **展开的 qname ()** 函数返回的 QName 进行比较。  
  
## <a name="examples"></a>示例  
 本主题提供针对数据库中各种 **xml** 类型列中存储的 xml 实例的 XQuery 示例 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] 。  
  
### <a name="a-replacing-a-qname-type-node-value"></a>A. 替换 QName 类型节点值  
 此示例说明了如何修改 QName 类型的元素节点的值。 该示例执行以下操作：  
  
-   创建定义 QName 类型的元素的 XML 架构集合。  
  
-   使用 XML 架构集合创建一个具有 **xml** 类型列的表。  
  
-   将 XML 实例保存在表中。  
  
-   使用 xml 数据类型的 **modify ()** 方法修改实例中 QName type 元素的值。 **扩充-QName ()** 函数用于生成新的 QName 类型值。  
  
```  
-- If XML schema collection (if exists)  
-- drop xml schema collection SC  
-- go  
-- Create XML schema collection  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
    xmlns:xs="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="QNameXSD"   
      xmlns:xqo="QNameXSD" elementFormDefault="qualified">  
      <element name="Root" type="xqo:rootType" />  
      <complexType name="rootType">  
            <sequence minOccurs="1" maxOccurs="1">  
                        <element name="ElemQN" type="xs:QName" />  
            </sequence>  
      </complexType>  
</schema>'  
go  
-- Create table.  
CREATE TABLE T( XmlCol xml(SC) )  
-- Insert sample XML instnace  
INSERT INTO T VALUES ('  
<Root xmlns="QNameXSD" xmlns:ns="https://myURI">  
      <ElemQN>ns:someName</ElemQN>  
</Root>')  
go  
-- Verify the insertion  
SELECT * from T  
go  
-- Result  
<Root xmlns="QNameXSD" xmlns:ns="https://myURI">  
  <ElemQN>ns:someName</ElemQN>  
</Root>   
```  
  
 在下面的查询中， `ElemQN` 使用 xml 数据类型的 **modify ()** 方法和 xml DML 的 replace 值替换 <> 元素值，如下所示。  
  
```  
-- the value.  
UPDATE T   
SET XmlCol.modify('  
  declare default element namespace "QNameXSD";   
  replace value of /Root[1]/ElemQN   
  with expanded-QName("https://myURI", "myLocalName") ')  
go  
-- Verify the result  
SELECT * from T  
go  
```  
  
 结果如下： 请注意， `ElemQN` QName 类型 <> 的元素现在有一个新值：  
  
```  
<Root xmlns="QNameXSD" xmlns:ns="urn">  
  <ElemQN xmlns:p1="https://myURI">p1:myLocalName</ElemQN>  
</Root>  
```  
  
 以下语句删除了用于示例的对象。  
  
```  
-- Cleanup  
DROP TABLE T  
go  
drop xml schema collection SC  
go  
```  
  
### <a name="b-dealing-with-the-limitations-when-using-the-expanded-qname-function"></a>B. 处理使用 expanded-QName() 函数时的限制。  
 无法在 XML 构造中使用 **展开的 QName** 函数。 下面的示例对此进行了演示。 为了消除此限制，该示例首先插入一个节点，然后修改此节点。  
  
```  
-- if exists drop the table T  
--drop table T  
-- go  
-- Create XML schema collection  
-- DROP XML SCHEMA COLLECTION SC  
-- go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="QName" nillable="true"/>  
</schema>'  
go  
 -- Create table T with a typed xml column (using the XML schema collection)  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- Insert an XML instance.  
insert into T values ('<root xmlns:a="https://someURI">a:b</root>')  
 go  
-- Verify  
SELECT *   
FROM T  
```  
  
 以下尝试添加了另一个 <`root`> 元素，但是失败，因为在 XML 构造中不支持扩展的 QName () 函数。  
  
```  
update T SET xmlCol.modify('  
insert <root>{expanded-QName("http://ns","someLocalName")}</root> as last into / ')  
go  
```  
  
 这样做的一种方法是先插入一个实例，其中包含 <`root`> 元素的值，然后对其进行修改。 在此示例中，插入 <> 元素时将使用零初始值 `root` 。 此示例中的 XML 架构集合允许 <> 元素的 nil 值 `root` 。  
  
```  
update T SET xmlCol.modify('  
insert <root xsi:nil="true"/> as last into / ')  
go  
-- now replace the nil value with another QName.  
update T SET xmlCol.modify('  
replace value of /root[last()] with expanded-QName("http://ns","someLocalName") ')  
go  
 -- verify   
SELECT * FROM T  
go  
-- result  
<root>b</root>  
```  
  
 `<root xmlns:a="https://someURI">a:b</root>`  
  
 `<root xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p1="http://ns">p1:someLocalName</root>`  
  
 可以比较 QName 值，如以下查询所示。 查询仅返回 `root` 其值与 **展开的 qname ()** 函数返回的 qname 类型值匹配的 <> 元素。  
  
```  
SELECT xmlCol.query('  
    for $i in /root  
    return  
       if ($i eq expanded-QName("http://ns","someLocalName") ) then  
          $i  
       else  
          ()')  
FROM T  
```  
  
### <a name="implementation-limitations"></a>实现限制  
 有一个限制：已 **展开-QName ()** 函数接受空序列作为第二个参数，并且在第二个参数不正确时，将返回空而不是引发运行时错误。  
  
## <a name="see-also"></a>另请参阅  
 [与 QNames &#40;XQuery 相关的函数&#41;]()  
  
