---
title: FOR XML 查询中的 TYPE 指令 | Microsoft Docs
description: 了解如何将 TYPE 指令与 FOR XML 子句配合使用，以请求将查询结果作为 XML 数据返回。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, TYPE directive
- TYPE directive
ms.assetid: a3df6c30-1f25-45dc-b5a9-bd0e41921293
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 0117b075a821a7e18732675ed432aafab5fafc60
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734718"
---
# <a name="type-directive-in-for-xml-queries"></a>FOR XML 查询中的 TYPE 指令
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持 [xml (Transact-SQL)](../../t-sql/xml/xml-transact-sql.md) 数据类型，这样便可以选择通过指定 TYPE 指令请求将 FOR XML 查询结果作为 xml 数据类型返回。 这样您便可以在服务器上处理 FOR XML 查询的结果。 例如，可以对其指定 Xquery，将结果分配给 **xml** 类型变量，或编写 [嵌套 FOR XML 查询](../../relational-databases/xml/use-nested-for-xml-queries.md)。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将 XML 数据类型实例数据作为不同服务器构造（如使用 TYPE 指令的 FOR XML 查询，或在其中使用 **xml** 数据类型返回 SQL 表列和输出参数中的 XML 实例数据值的 FOR XML 查询）的结果返回到客户端。 在客户端应用程序代码中，ADO.NET 提供程序请求从服务器以二进制编码发送此 XML 数据类型信息。 但是，如果使用的是不带 TYPE 指令的 FOR XML，则 XML 数据将以字符串类型返回。 在任何情况下，客户端访问接口都始终能够处理其中任一种形式的 XML 内容。 请注意，不带 TYPE 指令的顶级 FOR XML 不能与游标一起使用。  
  
## <a name="examples"></a>示例  
 以下示例说明了带 FOR XML 查询的 TYPE 指令的用法。  
  
### <a name="retrieving-for-xml-query-results-as-xml-type"></a>作为 xml 类型检索 FOR XML 查询结果  
 下面的查询检索 `Contacts` 表中的客户联系信息。 由于在 `TYPE` 中指定了 `FOR XML`指令，因此返回的结果的类型为 **xml** 。  
  
```  
USE AdventureWorks2012;  
Go  
SELECT BusinessEntityID, FirstName, LastName  
FROM Person.Person  
ORDER BY BusinessEntityID  
FOR XML AUTO, TYPE;  
```  
  
 下面是部分结果：  
  
 `<Person.Person BusinessEntityID="1" FirstName="Ken" LastName="Sánchez"/>`  
  
 `<Person.Person BusinessEntityID="2" FirstName="Terri" LastName="Duffy"/>`  
  
 `...`  
  
### <a name="assigning-for-xml-query-results-to-an-xml-type-variable"></a>将 FOR XML 查询结果赋给 xml 类型变量  
 在下面的示例中，一个 FOR XML 结果被赋给一个 **xml** 类型变量 `@x`。 该查询从 `BusinessEntityID`xml `FirstName`的 `LastName`列检索联系信息（如 `AdditionalContactInfo` 、 **、** `TYPE`和其他电话号码）。 由于 `FOR XML` 子句指定了 `TYPE` 指令，因此 XML 返回为 **xml** 类型并赋给某个变量。  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @x xml;  
SET @x = (  
   SELECT BusinessEntityID,   
          FirstName,   
          LastName,   
          AdditionalContactInfo.query('  
declare namespace aci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
              //act:telephoneNumber/act:number') as MorePhoneNumbers  
   FROM Person.Person  
   FOR XML AUTO, TYPE);  
SELECT @x;  
GO  
```  
  
### <a name="querying-results-of-a-for-xml-query"></a>查询 FOR XML 查询的结果  
 FOR XML 查询返回 XML。 因此，可以将 **xml** 类型方法（如 **query()** 和 **value()** ）应用于 FOR XML 查询返回的 XML 结果。  
  
 在下面的查询中， `query()` xml **数据类型的** 方法用于查询 `FOR XML` 查询的结果。 有关详细信息，请参阅 [query() 方法（xml 数据类型）](../../t-sql/xml/query-method-xml-data-type.md)。  
  
```  
USE AdventureWorks2012;  
GO  
SELECT (SELECT BusinessEntityID, FirstName, LastName, AdditionalContactInfo.query('  
DECLARE namespace aci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
DECLARE namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
') AS PhoneNumbers  
FROM Person.Person  
FOR XML AUTO, TYPE).query('/Person.Person[1]');  
```  
  
 内部 `SELECT ... FOR XML` 查询返回 **xml** 类型结果，外部 `SELECT` 将 `query()` 方法应用于该 **xml** 类型。 请注意指定的 `TYPE` 指令。  
  
 结果如下：  
  
 `<Person.Person BusinessEntityID="1" FirstName="Ken" LastName="Sánchez">`  
  
 `<PhoneNumbers>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">111-111-1111</act:number>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">112-111-1111</act:number>`  
  
 `</PhoneNumbers>`  
  
 `</Person.Person>`  
  
 在下面的查询中， `value()` xml **数据类型的** 方法用于检索 `SELECT...FOR XML` 查询返回的 XML 结果中的值。 有关详细信息，请参阅 [value() 方法（xml 数据类型）](../../t-sql/xml/value-method-xml-data-type.md)。  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @FirstPhoneFromAdditionalContactInfo varchar(40);  
SELECT @FirstPhoneFromAdditionalContactInfo =   
 ( SELECT BusinessEntityID, FirstName, LastName, AdditionalContactInfo.query('  
declare namespace aci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
   //act:telephoneNumber/act:number  
   ') AS PhoneNumbers  
   FROM Person.Person Contact  
   FOR XML AUTO, TYPE).value('  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
  /Contact[@BusinessEntityID="1"][1]/PhoneNumbers[1]/act:number[1]', 'varchar(40)'  
 )  
SELECT @FirstPhoneFromAdditionalContactInfo;  
```  
  
 `value()` 方法中的 XQuery 路径表达式检索 `BusinessEntityID` 为 `1`的客户联系人的第一个电话号码。  
  
> [!NOTE]  
>  如果未指定 TYPE 指令，则返回的 FOR XML 查询结果的类型为 **nvarchar(max)** 。  
  
### <a name="using-for-xml-query-results-in-insert-update-and-delete-transact-sql-dml"></a>在 INSERT、UPDATE 和 DELETE 中使用 FOR XML 查询结果 (Transact-SQL DML)  
 下面的示例说明了如何在数据操作语言 (DML) 语句中使用 FOR XML 查询。 在此示例中， `FOR XML` 返回 **xml** 类型的实例。 `INSERT` 语句将此 XML 插入表中。  
  
```  
CREATE TABLE T1(intCol int, XmlCol xml);  
GO  
INSERT INTO T1   
VALUES(1, '<Root><ProductDescription ProductModelID="1" /></Root>');  
GO  
  
CREATE TABLE T2(XmlCol xml)  
GO  
INSERT INTO T2(XmlCol)   
SELECT (SELECT XmlCol.query('/Root')   
        FROM T1   
        FOR XML AUTO,TYPE);   
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [FOR XML (SQL Server)](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
