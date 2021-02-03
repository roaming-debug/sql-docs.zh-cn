---
description: 插入 (XML DML)
title: 插入 (XML DML)
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
- inserting nodes
- insert keyword [XML DML]
- insert XML DML statement
ms.assetid: 0c95c2b3-5cc2-4c38-9e25-86493096c442
author: MightyPen
ms.author: genemi
ms.openlocfilehash: d5eae7b06bb10c356bbb1c3975b05dd7cd267708
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199333"
---
# <a name="insert-xml-dml"></a>插入 (XML DML)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  将 Expression1 标识的一个或多个节点作为 Expression2 标识的节点的子节点或同级节点插入 。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
insert Expression1 (  
{AS first | AS last} INTO | AFTER | BEFORE  
Expression2  
)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 Expression1  
 标识要插入的一个或多个节点。 这可以是一个常量 XML 实例、对应用修改方法的相同 XML 架构集合的类型化 XML 数据类型实例的引用、使用单独的 sql:column()/sql:variable() 函数的非类型化 XML 数据类型实例或者是一个 XQuery 表达式 。 该表达式可以得出节点、文本节点或一组有序的节点。 但它无法解得根 (/) 节点。 如果该表达式得出一个值或一组值，则这些值作为单个文本节点插入，各值之间以空格分隔开。 如果将多个节点指定为常量，则这些节点用括号括住，并以逗号分隔开。 但无法插入异构序列（如一组元素、属性或值）。 如果 Expression1 解析到一个空序列，不发生插入操作且不返回任何错误。  
  
 更改为  
 Expression1 标识的节点作为 Expression2 标识的节点的直接后代（子节点）插入 。 如果 Expression2 中的节点已有一个或多个子节点，则必须使用 as first 或 as last 来指定所需的新节点添加位置 。 例如，分别在子列表的开头或末尾。 插入属性时忽略 as first 和 as last 关键字 。  
  
 after  
 Expression1 标识的节点作为 Expression2 标识的节点的同级节点直接在其后面插入 。 after 关键字不能用于插入属性。 例如，它不能用于插入属性构造函数或从 XQuery 返回属性。  
  
 before  
 Expression1 标识的节点作为 Expression2 标识的节点的同级节点直接在其前面插入 。 before 关键字不能用于插入属性。 例如，它不能用于插入属性构造函数或从 XQuery 返回属性。  
  
 Expression2  
 标识节点。 Expression1 标识的节点是相对于 Expression2 标识的节点插入的 。 这可以是 XQuery 表达式，返回当前被引用的文档中现有节点的引用。 如果返回多个节点，则插入失败。 如果 Expression2 返回一个空序列，不发生插入操作且不返回任何错误。 如果 Expression2 在静态时不是单一实例，将返回静态错误。 Expression2 不能为处理指令、注释或属性。 请注意，Expression2 必须是文档中现有节点的引用，而不是构造的节点。  
  
## <a name="examples"></a>示例  
  
### <a name="a-inserting-element-nodes-into-the-document"></a>A. 将元素节点插入文档中  
 以下示例说明了如何将元素插入文档中。 首先，将 XML 文档分配给 xml 类型的变量。 然后，通过几个 insert XML DML 语句，该示例说明如何将元素节点插入文档中。 每次插入后，SELECT 语句都会显示结果。  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;         
SET @myDoc = '<Root>         
    <ProductDescription ProductID="1" ProductName="Road Bike">         
        <Features>         
        </Features>         
    </ProductDescription>         
</Root>'  ;       
SELECT @myDoc;     
-- insert first feature child (no need to specify as first or as last)         
SET @myDoc.modify('         
INSERT <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>   
INTO (/Root/ProductDescription/Features)[1]') ;  
SELECT @myDoc ;        
-- insert second feature. We want this to be the first in sequence so use 'as first'         
SET @myDoc.modify('         
INSERT <Warranty>1 year parts and labor</Warranty>          
AS first         
INTO (/Root/ProductDescription/Features)[1]         
')  ;       
SELECT @myDoc  ;       
-- insert third feature child. This one is the last child of <Features> so use 'as last'         
SELECT @myDoc         
SET @myDoc.modify('         
INSERT <Material>Aluminium</Material>          
AS last         
INTO (/Root/ProductDescription/Features)[1]         
')         
SELECT @myDoc ;        
-- Add fourth feature - this time as a sibling (and not a child)         
-- 'after' keyword is used (instead of as first or as last child)         
SELECT @myDoc  ;       
SET @myDoc.modify('         
INSERT <BikeFrame>Strong long lasting</BikeFrame>   
AFTER (/Root/ProductDescription/Features/Material)[1]         
')  ;       
SELECT @myDoc;  
GO  
```  
  
 请注意，此示例中的各种路径表达式都指定“[1]”以要求每次只返回单个目标。 这样就确保了只有单个目标节点。  
  
### <a name="b-inserting-multiple-elements-into-the-document"></a>B. 将多个元素插入文档中  
 在以下示例中，首先将文档分配给 xml 类型的变量。 然后，将包括两个元素（代表产品功能）的序列分配给第二个 xml 类型的变量。 再将此序列插入第一个变量。  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc = N'<Root>             
<ProductDescription ProductID="1" ProductName="Road Bike">             
    <Features> </Features>             
</ProductDescription>             
</Root>';  
DECLARE @newFeatures xml;  
SET @newFeatures = N'<Warranty>1 year parts and labor</Warranty>            
<Maintenance>3 year parts and labor extended maintenance is available</Maintenance>';           
-- insert new features from specified variable            
SET @myDoc.modify('             
insert sql:variable("@newFeatures")             
into (/Root/ProductDescription/Features)[1] ')             
SELECT @myDoc;  
GO  
```  
  
### <a name="c-inserting-attributes-into-a-document"></a>C. 将属性插入文档中  
 以下示例说明了如何将属性插入文档中。 首先，将文档分配给 xml 类型变量。 然后，使用一系列 insert XML DML 语句将属性插入文档中。 每次插入属性后，SELECT 语句都会显示结果。  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;            
SET @myDoc =   
'<Root>             
    <Location LocationID="10" >             
        <step>Manufacturing step 1 at this work center</step>             
        <step>Manufacturing step 2 at this work center</step>             
    </Location>             
</Root>' ;  
SELECT @myDoc;          
-- insert LaborHours attribute             
SET @myDoc.modify('             
insert attribute LaborHours {".5" }             
into (/Root/Location[@LocationID=10])[1] ');           
SELECT @myDoc;          
-- insert MachineHours attribute but its value is retrived from a sql variable @Hrs             
DECLARE @Hrs FLOAT;            
SET @Hrs =.2;          
SET @myDoc.modify('             
insert attribute MachineHours {sql:variable("@Hrs") }             
into   (/Root/Location[@LocationID=10])[1] ');            
SELECT @myDoc;             
-- insert sequence of attribute nodes (note the use of ',' and ()              
-- around the attributes.             
SET @myDoc.modify('             
insert (              
           attribute SetupHours {".5" },             
           attribute SomeOtherAtt {".2"}             
        )             
INTO (/Root/Location[@LocationID=10])[1] ');             
SELECT @myDoc;  
GO  
```  
  
### <a name="d-inserting-a-comment-node"></a>D. 插入注释节点  
 在此查询中，首先将 XML 文档分配给 xml 类型的变量。 然后，使用 XML DML 将注释节点插入到第一个 <`step`> 元素后。  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;             
SET @myDoc =   
'<Root>             
    <Location LocationID="10" >             
        <step>Manufacturing step 1 at this work center</step>             
        <step>Manufacturing step 2 at this work center</step>             
    </Location>             
</Root>' ;           
SELECT @myDoc;             
SET @myDoc.modify('             
insert <!-- some comment -->             
after (/Root/Location[@LocationID=10]/step[1])[1] ');            
SELECT @myDoc;  
GO  
```  
  
### <a name="e-inserting-a-processing-instruction"></a>E. 插入处理指令  
 在以下查询中，首先将 XML 文档分配给 xml 类型的变量。 然后，使用 XML DML 关键字将处理指令插入文档的开头。  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>   
    <Location LocationID="10" >   
        <step>Manufacturing step 1 at this work center</step>   
        <step>Manufacturing step 2 at this work center</step>   
    </Location>   
</Root>' ;  
SELECT @myDoc ;  
SET @myDoc.modify('   
insert <?Program = "Instructions.exe" ?>   
before (/Root)[1] ') ;  
SELECT @myDoc ;  
GO  
```  
  
### <a name="f-inserting-data-using-a-cdata-section"></a>F. 使用 CDATA 部分插入数据  
 插入文本时，如果该文本包含在 XML 中无效的字符（如 < 或 >），可以使用 CDATA 部分插入数据，如以下查询中所示。 该查询指定一个 CDATA 部分，但该部分作为文本节点添加进来，其中的无效字符转换成实体。 例如，`<` 另存为 `&lt;`。  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;             
SET @myDoc =   
'<Root>             
    <ProductDescription ProductID="1" ProductName="Road Bike">             
        <Features> </Features>             
    </ProductDescription>             
</Root>' ;            
SELECT @myDoc ;            
SET @myDoc.modify('             
insert <![CDATA[ <notxml> as text </notxml> or cdata ]]>   
into  (/Root/ProductDescription/Features)[1] ') ;   
SELECT @myDoc ;  
GO  
```  
  
 查询将文本节点插入 <`Features`> 元素中：  
  
```  
<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features> &lt;notxml@gt; as text &lt;/notxml&gt; or cdata </Features>  
</ProductDescription>  
</Root>       
```  
  
### <a name="g-inserting-text-node"></a>G. 插入文本节点  
 在此查询中，首先将 XML 文档分配给 xml 类型的变量。 然后，使用 XML DML 将文本节点插入为 <`Root`> 元素的第一个子元素。 并使用文本构造函数指定文本。  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  
</Features>  
</ProductDescription>  
</Root>'  
SELECT @myDoc;  
SET @myDoc.modify('  
 insert text{"Product Catalog Description"}   
 as first into (/Root)[1]  
');  
SELECT @myDoc;  
```  
  
### <a name="h-inserting-a-new-element-into-an-untyped-xml-column"></a>H. 将新元素插入非类型化的 xml 列  
 以下示例应用 XML DML 来更新 xml 类型列中存储的 XML 实例：  
  
```sql
USE AdventureWorks;  
GO  
CREATE TABLE T (i INT, x XML);  
GO  
INSERT INTO T VALUES(1,'<Root>  
    <ProductDescription ProductID="1" ProductName="Road Bike">  
        <Features>  
            <Warranty>1 year parts and labor</Warranty>  
            <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
        </Features>  
    </ProductDescription>  
</Root>');  
GO  
-- insert a new element  
UPDATE T  
SET x.modify('insert <Material>Aluminium</Material> as first  
  into   (/Root/ProductDescription/Features)[1]  
');  
GO  
```  
  
 同样，插入 <`Material`> 元素节点时，路径表达式必须返回单个目标。 这通过在表达式结尾处添加 [1] 来显式指定。  
  
```sql
-- check the update  
SELECT x.query(' //ProductDescription/Features')  
FROM T;  
GO  
```  
  
### <a name="i-inserting-based-on-an-if-condition-statement"></a>I. 根据 if 条件语句进行插入  
 在以下示例中，将 IF 条件语句指定为 insert XML DML 语句中 Expression1 的一部分。 如果条件为 True，则将属性添加到 <`WorkCenter`> 元素中。  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>  
    <Location LocationID="10" LaborHours="1.2" >  
        <step>Manufacturing step 1 at this work center</step>  
    <step>Manufacturing step 2 at this work center</step>  
    </Location>  
</Root>';  
SELECT @myDoc  
SET @myDoc.modify('  
insert  
if (/Root/Location[@LocationID=10])  
then attribute MachineHours {".5"}  
else ()  
    as first into   (/Root/Location[@LocationID=10])[1] ');  
SELECT @myDoc;  
GO  
```  
  
 以下示例与此类似，除了在条件为 True 时 insert XML DML 语句会在文档中插入元素。 也就是说，如果 <`WorkCenter`> 元素的 <`step`> 子元素少于或等于两个，则在文档中插入元素。  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>  
    <Location LocationID="10" LaborHours="1.2" >  
        <step>Manufacturing step 1 at this work center</step>  
        <step>Manufacturing step 2 at this work center</step>  
    </Location>  
</Root>';  
SELECT @myDoc;  
SET @myDoc.modify('  
insert  
if (count(/Root/Location/step) <= 2)  
then element step { "This is a new step" }  
else ()  
    as last into   (/Root/Location[@LocationID=10])[1] ');  
SELECT @myDoc;  
GO  
```  
  
 结果如下：  
  
```  
<Root>  
 <WorkCenter WorkCenterID="10" LaborHours="1.2">  
  <step>Manufacturing step 1 at this work center</step>  
  <step>Manufacturing step 2 at this work center</step>  
  <step>This is a new step</step>  
 </WorkCenter>  
```  
  
### <a name="j-inserting-nodes-in-a-typed-xml-column"></a>J. 将节点插入类型化的 xml 列中  
 此示例将元素和属性插入在类型化的 xml 列存储的生产说明 XML 中。  
  
 在该示例中，首先在 AdventureWorks 数据库中创建带有类型化的 xml 列的表 (T)。 然后将一个生产说明 XML 实例从 ProductModel 表的 Instructions 列复制到表 T 中。随后再对表 T 中的 XML 内容应用插入操作。  
  
```sql
USE AdventureWorks;  
GO            
DROP TABLE T;  
GO             
CREATE TABLE T(
  ProductModelID INT PRIMARY KEY,    
  Instructions XML (Production.ManuInstructionsSchemaCollection));  
GO  
INSERT T              
    SELECT ProductModelID, Instructions             
    FROM Production.ProductModel             
    WHERE ProductModelID=7;  
GO             
SELECT Instructions             
FROM T;  
-- now insertion begins             
--1) insert a new manu. Location. The <Root> specified as              
-- expression 2 in the insert() must be singleton.      
UPDATE T   
SET Instructions.modify('   
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
insert <MI:Location LocationID="1000" >   
           <MI:step>New instructions go here</MI:step>   
         </MI:Location>   
as first   
into   (/MI:root)[1]   
') ;  
  
SELECT Instructions             
FROM T ;  
-- 2) insert attributes in the new <Location>             
UPDATE T             
SET Instructions.modify('             
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";             
insert attribute LaborHours { "1000" }             
into (/MI:root/MI:Location[@LocationID=1000])[1] ');   
GO             
SELECT Instructions             
FROM T ;  
GO             
--cleanup             
DROP TABLE T ;  
GO             
```  
  
## <a name="see-also"></a>另请参阅  
 [类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [xml 数据类型方法](../../t-sql/xml/xml-data-type-methods.md)   
 [XML 数据修改语言 (XML DML)](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
