---
description: SELECT - FOR 子句 (Transact-SQL)
title: FOR Clause (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/08/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FOR
- FOR CLAUSE
- FOR_TSQL
- FOR_CLAUSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- XML option [SQL Server]
- BROWSE option
- FOR clause [Transact-SQL]
ms.assetid: 08a6f084-8f73-4f2a-bae4-3c7513dc99b9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 99b47c3d47e7403e04cfff4b62ff5c1d78bb6071
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207551"
---
# <a name="select---for-clause-transact-sql"></a>SELECT - FOR 子句 (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

使用 FOR 子句指定查询结果的以下查询结果之一。
  
-   允许在查看浏览模式游标中的查询结果时通过指定 FOR BROWSE 来更新。  
  
-   通过指定 FOR XML，将查询结果格式化为 XML。  
  
-   通过指定 FOR JSON，将查询结果格式化为 JSON 。  

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
[ FOR { BROWSE | <XML> | <JSON>} ]  
  
<XML> ::=  
XML   
{   
    { RAW [ ( 'ElementName' ) ] | AUTO }   
    [   
        <CommonDirectivesForXML>   
        [ , { XMLDATA | XMLSCHEMA [ ( 'TargetNameSpaceURI' ) ] } ]   
        [ , ELEMENTS [ XSINIL | ABSENT ]   
    ]  
  | EXPLICIT   
    [   
        <CommonDirectivesForXML>   
        [ , XMLDATA ]   
    ]  
  | PATH [ ( 'ElementName' ) ]   
    [  
        <CommonDirectivesForXML>   
        [ , ELEMENTS [ XSINIL | ABSENT ] ]  
    ]  
}   
  
<CommonDirectivesForXML> ::=   
[ , BINARY BASE64 ]  
[ , TYPE ]  
[ , ROOT [ ( 'RootName' ) ] ]  
  
<JSON> ::=  
JSON   
{   
    { AUTO | PATH }   
    [   
        [ , ROOT [ ( 'RootName' ) ] ]  
        [ , INCLUDE_NULL_VALUES ]  
        [ , WITHOUT_ARRAY_WRAPPER ]  
    ]  
  
}
```
  
## <a name="for-browse"></a>FOR BROWSE

 BROWSE  
 指定可以在查看 DB-Library 浏览模式游标中的数据时进行更新。 如果表包括 timestamp 列，并且表有唯一索引，而且在发送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的 SELECT 语句中 FOR BROWSE 选项位于末尾，则可以在应用程序中浏览该表。  
  
> [!NOTE]
> 不能在包括 FOR BROWSE 选项的 SELECT 语句中使用 \<lock_hint> HOLDLOCK。
  
 FOR BROWSE 不能出现在由 UNION 运算符所联接的 SELECT 语句中。  
  
> [!NOTE]
> 如果表的唯一索引键列可为空，并且表在外部联接的内侧，则浏览模式不支持索引。  
  
 使用浏览模式可以扫描 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的行并逐行更新表中的数据。 若要以浏览模式访问应用程序中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表，必须使用下列两个选项之一：  
  
-   用于访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表的数据的 SELECT 语句必须以 FOR BROWSE 关键字结尾。 启用 FOR BROWSE 选项以使用浏览模式时，将创建时态表。  
  
-   必须运行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，以使用 NO_BROWSETABLE 选项打开浏览模式：  
  
    ```sql
    SET NO_BROWSETABLE ON  
    ```  
  
     启用 NO_BROWSETABLE 选项时，所有 SELECT 语句的行为就像在这些语句后追加了 FOR BROWSE 选项一样 。 但是，NO_BROWSETABLE 选项不会创建 FOR BROWSE 选项通常用于向应用程序发送结果的时态表 。  
  
 当使用涉及外部联接语句的 SELECT 查询尝试以浏览模式访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的数据，并且当对外部联接语句内侧已存在的表定义唯一索引时，浏览模式不支持此唯一索引。 仅当所有唯一索引键列可接受 Null 值时，浏览模式才支持唯一索引。 如果满足下列条件，浏览模式不支持唯一索引：  
  
-   尝试使用包含外部联接语句的 SELECT 查询在浏览模式中访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表的数据。  
  
-   对外部联接语句内侧已存在的表定义唯一索引。  
  
 若要在浏览模式下重现此行为，请按下列步骤操作：  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，创建一个名为 SampleDB 的数据库。  
  
2.  在 SampleDB 数据库中，创建均包含名为 c1 的列的 tleft 表和 tright 表。 对 tleft 表中的 c1 列定义唯一索引，并将此列设置为接受 Null 值。 为此，请在相应的查询窗口中运行下列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```sql
    CREATE TABLE tleft(c1 INT NULL UNIQUE) ;  
    GO   
    CREATE TABLE tright(c1 INT NULL) ;  
    GO  
    ```  
  
3.  在 tleft 表和 tright 表中插入多个值。 请确保在 tleft 表中插入一个 Null 值。 为此，请在查询窗口中运行下列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```sql
    INSERT INTO tleft VALUES(2) ;  
    INSERT INTO tleft VALUES(NULL) ;  
    INSERT INTO tright VALUES(1) ;  
    INSERT INTO tright VALUES(3) ;  
    INSERT INTO tright VALUES(NULL) ;  
    GO  
    ```  
  
4.  开启 NO_BROWSETABLE 选项。 为此，请在查询窗口中运行下列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```sql
    SET NO_BROWSETABLE ON ;  
    GO  
    ```  
  
5.  通过在 SELECT 查询中使用外部联接语句来访问 tleft 表和 tright 表中的数据。 请确保 tleft 表位于外部联接语句的内侧。 为此，请在查询窗口中运行下列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
    ```sql
    SELECT tleft.c1   
    FROM tleft   
    RIGHT JOIN tright   
    ON tleft.c1 = tright.c1   
    WHERE tright.c1 <> 2 ;
    ```  
  
     请注意“结果”窗格中的以下输出：  
  
     c1  
  
     ---\-  
  
     Null  
  
     Null  
  
 在运行 SELECT 查询以便以浏览模式访问表之后，对于 tleft 表中的 c1 列，SELECT 查询的结果集包含两个 Null 值，这是因为在右外部联接语句中定义了该列。 因此，您无法在结果集中区分源自该表的 Null 值和右外部联接语句引入的 Null 值。 如果必须忽略结果集的 Null 值，则可能收到错误结果。  
  
> [!NOTE]
> 如果唯一索引中包含的列不接受 Null 值，则结果集中的所有 Null 值都是由右外部联接语句引入的。  
  
## <a name="for-xml"></a>FOR XML

 XML  
 指定以 XML 文档返回查询的结果。 必须指定下列 XML 模式之一：RAW、AUTO、EXPLICIT。 有关 XML 数据和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的详细信息，请参阅 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)。  
  
 RAW [ ('ElementName') ]  
 获得查询结果并将结果集内的每一行转换为以一般标识符 \<row /> 作为元素标记的 XML 元素。 （可选）可以为该行元素指定名称。 产生的 XML 输出将指定的 ElementName 用作每行生成的行元素。 有关详细信息，请参阅 [将 RAW 模式与 FOR XML 一起使用](../../relational-databases/xml/use-raw-mode-with-for-xml.md)。
  
 AUTO  
 以简单的嵌套 XML 树返回查询结果。 FROM 子句中每个在 SELECT 子句中至少列出一次的表都被表示为一个 XML 元素。 SELECT 子句中列出的列映射到适当的元素属性。 有关详细信息，请参阅 [将 AUTO 模式与 FOR XML 一起使用](../../relational-databases/xml/use-auto-mode-with-for-xml.md)。  
  
 EXPLICIT  
 指定显式定义产生的 XML 树的形状。 使用该模式要求必须以一种特定的方式编写查询，即显式指定与想要的嵌套有关的其他信息。 有关详细信息，请参阅 [将 EXPLICIT 模式与 FOR XML 一起使用](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)。  
  
 XMLDATA  
 返回内联 XDR 架构，但不将根元素添加到结果中。 如果指定了 XMLDATA，则 XDR 架构将被追加到文档末尾。  
  
> [!IMPORTANT]
> XMLDATA 指令已弃用。 如果是 RAW 和 AUTO 模式，请使用 XSD 生成。 在 EXPLICIT 模式下，没有 XMLDATA 指令的替代项。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  

禁止显示不需要的换行符：可以使用 SQL Server Management Studio (SSMS) 来发出使用 FOR XML 子句的查询。 有时会返回大量 XML 并在一个网格单元格中显示。 XML 字符串可能比 SSMS 网格单元在单个行上可以容纳的长度更长。 在这些情况下，SSMS 可能会在整个 XML 字符串的长段之间插入换行符。 此类换行可能发生在子某个字符串的中间，不应跨行拆分该子字符串。 可以使用强制转换 AS XMLDATA 来阻止换行符。 使用 FOR JSON PATH 时，此解决方案也适用。 该技巧在 Stack Overflow 上进行了介绍，并显示在以下 Transact-SQL 示例 SELECT 语句中：

- [使用 SQL Server FOR XML：将结果数据类型转换为文本/varchar/字符串等？](https://stackoverflow.com/questions/5655332/using-sql-server-for-xml-convert-result-datatype-to-text-varchar-string-whate/5658758#5658758)

    ```sql
    SELECT CAST(
        (SELECT column1, column2
            FROM my_table
            FOR XML PATH('')
        )
            AS VARCHAR(MAX)
    ) AS XMLDATA ;
    ```

<!-- The preceding Stack Overflow example is per MicrosoftDocs/sql-docs Issue 1501.  2019-01-06 -->

 XMLSCHEMA [ ('TargetNameSpaceURI') ]  
 返回内联 XSD 架构。 如果指定该指令（用于返回架构中指定的命名空间），则可以选择指定目标命名空间 URI。 有关详细信息，请参阅 [生成内联 XSD 架构](../../relational-databases/xml/generate-an-inline-xsd-schema.md)。  
  
 ELEMENTS  
 指定列作为子元素返回。 否则，列将映射到 XML 属性。 只在 RAW、AUTO 和 PATH 模式中支持该选项。 有关详细信息，请参阅 [将 RAW 模式与 FOR XML 一起使用](../../relational-databases/xml/use-raw-mode-with-for-xml.md)。  
  
 XSINIL  
 指定为 NULL 列值创建其 xsi:nil 属性设置为 True 的元素 。 该选项只能与 ELEMENTS 指令一起指定。 有关详细信息，请参阅：

- [使用 XSINIL 参数生成 NULL 值对应的元素](../../relational-databases/xml/generate-elements-for-null-values-with-the-xsinil-parameter.md)。
- [SELECT 语句上的 FOR XML](../../relational-databases/xml/for-xml-sql-server.md)
  
 ABSENT  
 指示对于空列值，将不在 XML 结果中添加对应的 XML 元素。 该选项只能与 ELEMENTS 一起指定。  
  
 PATH [ ('ElementName') ]  
 为结果集中的每一行生成 \<row> 元素包装器。 可以根据需要为 \<row> 元素包装器指定元素名称。 如果提供了空字符串（例如 FOR XML PATH ('')），则不生成包装器元素。 使用 PATH 可能为使用 EXPLICIT 指令所编写的查询提供更简单的代替方案。 有关详细信息，请参阅 [将 PATH 模式与 FOR XML 一起使用](../../relational-databases/xml/use-path-mode-with-for-xml.md)。  
  
 BINARY BASE64  
 指定查询返回二进制 base64 编码格式的二进制数据。 使用 RAW 和 EXPLICIT 模式检索二进制数据时，必须指定该选项。 这是 AUTO 模式中的默认值。  
  
 TYPE  
 指定查询以 xml 类型返回结果。 有关详细信息，请参阅 [TYPE Directive in FOR XML Queries](../../relational-databases/xml/type-directive-in-for-xml-queries.md)。  
  
 ROOT [ ('RootName') ]  
 指定将一个顶级元素添加到结果 XML 中。 可以选择指定要生成的根元素名称。 如果不指定可选的根名称，则添加默认的 \<root> 元素。  
  
 有关详细信息，请参阅 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)。  
  
 **FOR XML 示例**  
  
 以下示例指定具有 `FOR XML AUTO` 和 `TYPE` 选项的 `XMLSCHEMA`。 由于有 `TYPE` 选项，因此结果集将作为 xml 类型返回到客户端。 `XMLSCHEMA` 选项指定在所返回的 XML 数据中包括内联 XSD 架构，而 `ELEMENTS` 选项指定 XML 结果是以元素为中心的。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.BusinessEntityID, FirstName, LastName, PhoneNumber AS Phone  
FROM Person.Person AS p  
JOIN Person.PersonPhone AS pph ON p.BusinessEntityID  = pph.BusinessEntityID  
WHERE LastName LIKE 'G%'  
ORDER BY LastName, FirstName   
FOR XML AUTO, TYPE, XMLSCHEMA, ELEMENTS XSINIL;  
```  
  
## <a name="for-json"></a>对于 JSON

 JSON  
 指定 FOR JSON，返回已格式化为 JSON 文本的查询结果。 还必须指定以下某项 JSON 模式：AUTO 或 PATH。 有关 FOR JSON 子句的详细信息，请参阅[借助 FOR JSON 将查询结果格式化为 JSON &#40;SQL Server&#41;](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)。  
  
 AUTO  
 根据 SELECT 语句的结构  
            通过指定 FOR JSON AUTO 自动格式化 JSON 输出。 有关详细信息和示例，请参阅[在 AUTO 模式下自动格式化 JSON 输出 (SQL Server)](../../relational-databases/json/format-json-output-automatically-with-auto-mode-sql-server.md)。  
  
 PATH  
 通过指定   
            FOR JSON PATH 获取对 JSON 输出格式的完全控制。 借助 **PATH** 模式，你可以创建包装器对象，并嵌套复杂属性。 有关详细信息和示例，请参阅[在 PATH 模式下设置嵌套的 JSON 输出格式 (SQL Server)](../../relational-databases/json/format-nested-json-output-with-path-mode-sql-server.md)。  
  
 INCLUDE_NULL_VALUES  
 通过使用 FOR JSON 子句指定 INCLUDE_NULL_VALUES 选项，在 JSON 输出中包括 NULL 值 。 如果没有指定此选项，输出不会在查询结果中包括 NULL 值的 JSON 属性。 有关详细信息和示例，请参阅[使用 INCLUDE_NULL_VALUES 选项将 NULL 值包含在 JSON 输出中 &#40;SQL Server&#41;](../../relational-databases/json/include-null-values-in-json-include-null-values-option.md)。  
  
 ROOT [ ('RootName') ]  
 通过使用 FOR JSON 子句指定 ROOT 选项，将一个顶层元素添加到 JSON 输出中 。 如果没有指定 **ROOT** 选项，则 JSON 输出不会包括根元素。 有关详细信息和示例，请参阅[使用 ROOT 选项将根节点添加到 JSON 输出中 &#40;SQL Server&#41;](../../relational-databases/json/add-a-root-node-to-json-output-with-the-root-option-sql-server.md)。  
  
 WITHOUT_ARRAY_WRAPPER  
 通过使用 FOR JSON 子句指定 WITHOUT_ARRAY_WRAPPER 选项，删除默认括住 JSON 输出的方括号 。 如果不指定此选项，JSON 输出将括在方括号中。 使用 WITHOUT_ARRAY_WRAPPER 选项可以生成单个 JSON 对象作为输出。  有关详细信息，请参阅 [使用 WITHOUT_ARRAY_WRAPPER 选项从 JSON 输出中删除方括号 (SQL Server)](../../relational-databases/json/remove-square-brackets-from-json-without-array-wrapper-option.md)。  
  
 有关详细信息，请参阅[借助 FOR JSON 将查询结果格式化为 JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)。  
  
## <a name="see-also"></a>另请参阅

 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)

