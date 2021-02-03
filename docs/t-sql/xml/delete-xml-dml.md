---
description: 删除 (XML DML)
title: 删除 (XML DML)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- XML DML [SQL Server]
- delete keyword
- delete statement [XML DML]
- deleting nodes
ms.assetid: b22c93a4-b84d-4356-af4c-6013322a4b71
author: MightyPen
ms.author: genemi
ms.openlocfilehash: e3f06338515bbd8ad426606a4331c5fc19a1b0ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161056"
---
# <a name="delete-xml-dml"></a>删除 (XML DML)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  删除 XML 实例的节点。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
delete Expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *表达式*  
 识别要删除的节点的 XQuery 表达式。 删除该表达式选择的所有节点，以及所选节点中的所有节点或值。 如 [insert (XML DML)](../../t-sql/xml/insert-xml-dml.md) 中所介绍的，在文档中必须保持对现有节点的引用。 不能是构造的节点。 表达式不能是根 (/) 节点。 如果表达式返回空序列，则不进行删除，不返回错误。  
  
## <a name="examples"></a>示例  
  
### <a name="a-deleting-nodes-from-a-document-stored-in-an-untyped-xml-variable"></a>A. 从存储在非类型化的 xml 变量中的文档中删除节点  
 以下示例说明了如何从文档中删除各种节点。 首先，将 XML 实例分配给 **xml** 类型的变量。 然后，后续的 delete XML DML 语句从文档中删除各种节点。  
  
```sql
DECLARE @myDoc XML  
SET @myDoc = '<?Instructions for=TheWC.exe ?>   
<Root>  
 <!-- instructions for the 1st work center -->  
<Location LocationID="10"   
            LaborHours="1.1"  
            MachineHours=".2" >Some text 1  
<step>Manufacturing step 1 at this work center</step>  
<step>Manufacturing step 2 at this work center</step>  
</Location>  
</Root>'  
SELECT @myDoc  
  
-- delete an attribute  
SET @myDoc.modify('  
  delete /Root/Location/@MachineHours  
')  
SELECT @myDoc  
  
-- delete an element  
SET @myDoc.modify('  
  delete /Root/Location/step[2]  
')  
SELECT @myDoc  
  
-- delete text node (in <Location>  
SET @myDoc.modify('  
  delete /Root/Location/text()  
')  
SELECT @myDoc  
  
-- delete all processing instructions  
SET @myDoc.modify('  
  delete //processing-instruction()  
')  
SELECT @myDoc  
```  
  
### <a name="b-deleting-nodes-from-a-document-stored-in-an-untyped-xml-column"></a>B. 从存储在非类型化的 xml 列中的文档中删除节点  
 在以下示例中，**delete** XML DML 语句从存储在列中的文档中删除 <`Features`> 的第二个子元素。  
  
```sql
CREATE TABLE T (i INT, x XML)  
GO  
INSERT INTO T VALUES(1,'<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>')  
GO
-- verify the contents before delete  
SELECT x.query(' //ProductDescription/Features')  
FROM T  
-- delete the second feature  
UPDATE T  
SET x.modify('delete /Root/ProductDescription/Features/*[2]')  
-- verify the deletion  
SELECT x.query(' //ProductDescription/Features')  
FROM T  
```  
  
 请注意上述查询的以下方面：  
  
-   [modify() 方法（xml 数据类型）](../../t-sql/xml/modify-method-xml-data-type.md)用于指定 **delete** XML DML 关键字。  
  
-   [query() 方法（xml 数据类型）](../../t-sql/xml/query-method-xml-data-type.md)用于查询文档。  
  
### <a name="c-deleting-nodes-from-a-typed-xml-column"></a>C. 从非类型化的 xml 列中删除节点  
 此示例从存储在类型化的 **xml** 列中的生产说明 XML 文档中删除节点。  
  
 在该示例中，首先在 AdventureWorks 数据库中创建带有类型化的 **xml** 列的表 (T)。 然后将生产说明 XML 实例从 ProductModel 表的 Instructions 列复制到表 T 并从文档中删除节点。  
  
```sql
USE AdventureWorks  
GO  
DROP TABLE T  
GO  
CREATE TABLE T(
    ProductModelID INT PRIMARY KEY,   
    Instructions XML (Production.ManuInstructionsSchemaCollection))  
GO  
INSERT T   
SELECT ProductModelID, Instructions  
FROM Production.ProductModel  
WHERE ProductModelID = 7  
GO  
SELECT Instructions  
FROM T  
--1) insert <Location 1000/>. Note: <Root> must be singleton in the query  
UPDATE T  
SET Instructions.modify('  
  DECLARE namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  INSERT <MI:Location LocationID="1000"  LaborHours="1000" >  
           These are manu steps at location 1000.   
           <MI:step>New step1 instructions</MI:step>  
           Instructions for step 2 are here  
           <MI:step>New step 2 instructions</MI:step>  
         </MI:Location>  
  AS first  
  INTO   (/MI:root)[1]  
')  
GO 
SELECT Instructions  
FROM T  
  
-- delete an attribute  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  delete(/MI:root/MI:Location[@LocationID=1000]/@LaborHours)   
')  
GO  
SELECT Instructions  
FROM T  
-- delete text in <location>  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  delete(/MI:root/MI:Location[@LocationID=1000]/text())   
')  
GO  
SET Instructions  
FROM T  
-- delete 2nd manu step at location 1000  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  delete(/MI:root/MI:Location[@LocationID=1000]/MI:step[2])   
')  
GO  
SELECT Instructions  
FROM T  
-- cleanup  
DROP TABLE T  
GO 
```  
  
## <a name="see-also"></a>另请参阅  
 [类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [xml 数据类型方法](../../t-sql/xml/xml-data-type-methods.md)   
 [XML 数据修改语言 (XML DML)](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
