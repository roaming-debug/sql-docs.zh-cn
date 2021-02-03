---
description: CREATE SELECTIVE XML INDEX (Transact-SQL)
title: CREATE SELECTIVE XML INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 1d769f62-f646-4057-b93a-bf5f90e935ed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d5b2226422e413b2437ab7fc4643740a92378b51
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192675"
---
# <a name="create-selective-xml-index-transact-sql"></a>CREATE SELECTIVE XML INDEX (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  对指定的表和 XML 列创建新的选择性 XML 索引。 选择性 XML 索引通过只对您通常查询的节点子集建立索引，改进 XML 索引和查询的性能。 您还可以创建辅助选择性 XML 索引。 有关信息，请参阅[创建、更改和删除辅助选择性 XML 索引](../../relational-databases/xml/create-alter-and-drop-secondary-selective-xml-indexes.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE SELECTIVE XML INDEX index_name  
    ON <table_object> (xml_column_name)  
    [WITH XMLNAMESPACES (<xmlnamespace_list>)]  
    FOR (<promoted_node_path_list>)  
    [WITH (<index_options>)]  
  
<table_object> ::=  
 { database_name.schema_name.table_name | schema_name.table_name | table_name }  
  
<promoted_node_path_list> ::=   
<named_promoted_node_path_item> [, <promoted_node_path_list>]  
  
<named_promoted_node_path_item> ::=   
<path_name> = <promoted_node_path_item>  
  
<promoted_node_path_item>::=  
<xquery_node_path_item> | <sql_values_node_path_item>  
  
<xquery_node_path_item> ::=   
<node_path> [AS XQUERY <xsd_type_or_node_hint>] [SINGLETON]  
  
<xsd_type_or_node_hint> ::=   
[<xsd_type>] [MAXLENGTH(x)] | node()  
  
<sql_values_node_path_item> ::=  
<node_path> AS SQL <sql_type> [SINGLETON]  
  
<node_path> ::=   
character_string_literal  
  
<xsd_type> ::=   
character_string_literal  
  
<sql_type> ::=   
identifier  
  
<path_name> ::=   
identifier  
  
<xmlnamespace_list> ::=   
<xmlnamespace_item> [, <xmlnamespace_list>]  
  
<xmlnamespace_item> ::=   
<xmlnamespace_uri> AS <xmlnamespace_prefix>  
  
<xml_namespace_uri> ::=   
character_string_literal  
  
<xml_namespace_prefix> ::=   
identifier  
  
<index_options> ::=   
(   
  | PAD_INDEX  = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> 参数  
 index_name  
 要创建的新索引的名称。 索引名称在表中必须唯一，但在数据库中不必唯一。 索引名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)的规则。  
  
 \<table_object> 是包含要建立索引的 XML 列的表。 使用以下格式之一：  
  
-   `database_name.schema_name.table_name`  
  
-   `database_name..table_name`  
  
-   `schema_name.table_name`  
  
-   `table_name`  
  
 xml_column_name  
 XML 列的名称，该列中包含要建立索引的路径。  
  
 [WITH XMLNAMESPACES (\<xmlnamespace_list>) ] 是要建立索引的路径使用的命名空间的列表。 有关 WITH XMLNAMESPACES 子句的语法的信息，请参阅 [WITH XMLNAMESPACES (Transact-SQL)](../../t-sql/xml/with-xmlnamespaces.md)。  
  
 FOR (\<promoted_node_path_list>) 是要使用可选优化提示对其建立索引的路径列表 。 有关可以在 CREATE 或 ALTER 语句中指定的路径和优化提示的信息，请参阅[为选择性 XML 索引指定路径和优化提示](../../relational-databases/xml/specify-paths-and-optimization-hints-for-selective-xml-indexes.md)。  
  
 WITH \<index_options> - 有关索引选项的信息，请参阅 [CREATE XML INDEX（选择性 XML 索引）](../../t-sql/statements/create-xml-index-selective-xml-indexes.md)。  
  
## <a name="best-practices"></a>最佳实践  
 在大多数情况下，通过创建选择性 XML 索引而不是普通 XML 索引，可以改善性能和提高存储效率。 但在以下条件之一成立时，不推荐使用选择性 XML 索引：  
  
-   您需要映射大量节点路径。  
  
-   您需要支持对未知元素或未知位置中元素的查询。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 有关限制和局限的信息，请参阅[选择性 XML 索引 (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md)。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求对表或视图具有 ALTER 权限。 用户必须是 **sysadmin** 固定服务器角色的成员，或者是 **db_ddladmin** 和 **db_owner** 固定数据库角色的成员。  
  
## <a name="examples"></a>示例  
 下面的示例显示了创建选择性 XML 索引的语法。 它还显示了该语法的若干变化形式，以便描述使用可选的优化提示建立索引的路径。  
  
```sql  
CREATE TABLE Tbl ( id INT PRIMARY KEY, xmlcol XML );  
GO  
CREATE SELECTIVE XML INDEX sxi_index  
ON Tbl(xmlcol)  
FOR(  
    pathab   = '/a/b' as XQUERY 'node()',  
    pathabc  = '/a/b/c' as XQUERY 'xs:double',   
    pathdtext = '/a/b/d/text()' as XQUERY 'xs:string' MAXLENGTH(200) SINGLETON,  
    pathabe = '/a/b/e' as SQL NVARCHAR(100)  
);  
```  
  
 下列的示例包含 WITH XMLNAMESPACES 子句。  
  
```sql  
CREATE SELECTIVE XML INDEX on T1(C1)  
WITH XMLNAMESPACES ('https://www.tempuri.org/' as myns)  
FOR ( path1 = '/myns:book/myns:author/text()' );  
```  
  
## <a name="see-also"></a>另请参阅  
 [选择性 XML 索引 (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md)   
 [创建、更改和删除选择性 XML 索引](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md)   
 [为选择性 XML 索引指定路径和优化提示](../../relational-databases/xml/specify-paths-and-optimization-hints-for-selective-xml-indexes.md)  
  
  

