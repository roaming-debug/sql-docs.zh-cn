---
description: ALTER FULLTEXT INDEX (Transact-SQL)
title: ALTER FULLTEXT INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER FULLTEXT INDEX
- ALTER_FULLTEXT_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- modifying full-text indexes
- full-text indexes [SQL Server], modifying
- search property lists [SQL Server], associating with full-text indexes
- ALTER FULLTEXT INDEX statement
ms.assetid: b6fbe9e6-3033-4d1b-b6bf-1437baeefec3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 17981c3c04c17b0c0291714a245dfa1856a69af7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204227"
---
# <a name="alter-fulltext-index-transact-sql"></a>ALTER FULLTEXT INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中更改全文检索的属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER FULLTEXT INDEX ON table_name  
   { ENABLE   
   | DISABLE  
   | SET CHANGE_TRACKING [ = ] { MANUAL | AUTO | OFF }  
   | ADD ( column_name   
           [ TYPE COLUMN type_column_name ]   
           [ LANGUAGE language_term ]  
           [ STATISTICAL_SEMANTICS ]  
           [,...n]   
         )  
     [ WITH NO POPULATION ]  
   | ALTER COLUMN column_name  
     { ADD | DROP } STATISTICAL_SEMANTICS  
     [ WITH NO POPULATION ]  
   | DROP ( column_name [,...n] )  
     [ WITH NO POPULATION ]   
   | START { FULL | INCREMENTAL | UPDATE } POPULATION  
   | {STOP | PAUSE | RESUME } POPULATION   
   | SET STOPLIST [ = ] { OFF| SYSTEM | stoplist_name }  
     [ WITH NO POPULATION ]   
   | SET SEARCH PROPERTY LIST [ = ] { OFF | property_list_name }  
     [ WITH NO POPULATION ]   
   }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *table_name*  
 包含全文索引中的一列或多列的表或索引视图的名称。 可以选择指定数据库和表所有者名称。  
  
 ENABLE | DISABLE  
 通知 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是否收集 table_name 的全文检索数据。 ENABLE 激活全文索引；DISABLE 关闭全文索引。 禁用索引时，表不支持全文查询。  
  
 通过禁用全文索引，您可以关闭更改跟踪但保留全文索引，并且可以随时使用 ENABLE 重新激活全文索引。 如果禁用全文索引，则全文索引元数据将保留在系统表中。 禁用全文索引时，如果 CHANGE_TRACKING 处于启用状态（自动或手动更新），则索引状态将冻结，任何正在进行的爬网将停止，并且不会跟踪对表数据进行的新更改，也不会将这些更改传播到索引。  
  
 SET CHANGE_TRACKING {MANUAL | AUTO | OFF}  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是否会将对全文检索所覆盖的表列的更改（更新、删除或插入）传播到全文检索。 通过 WRITETEXT 和 UPDATETEXT 所做的数据更改不会反映到全文索引中，也不能使用更改跟踪方法拾取。  
  
> [!NOTE]  
>  有关详细信息，请参阅[更改跟踪和 NO POPULATION 参数的交互](#change-tracking-no-population)。
  
 MANUAL  
 指定跟踪的更改会手动传播，方法是调用 ALTER FULLTEXT INDEX ...START UPDATE POPULATION [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句（手动填充）。 您可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理来定期调用此 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
 AUTO  
 指定当基表中的数据修改时，所跟踪的更改将会自动传播（自动填充）。 尽管是自动传播更改，但这些更改可能不会立即反映到全文索引中。 默认值为 AUTO。  
  
 OFF  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将不保存对索引数据的更改的列表。  
  
 ADD | DROP column_name  
 指定要添加到全文索引或从全文索引中删除的列。 一个或多个列必须是 char、varchar、nchar、nvarchar、text、ntext、image、xml、varbinary 或 varbinary(max) 类型。  
  
 仅对先前为全文索引启用的列使用 DROP 子句。  
  
 将 TYPE COLUMN 和 LANGUAGE 与 ADD 子句一起使用，以对 column_name 设置这些属性。 添加列后，必须重新填充该表的全文索引，才能使针对此列进行的全文查询生效。  
  
> [!NOTE]  
>  在全文索引中添加或删除某个列后，是否填充全文索引取决于是否启用了更改跟踪以及是否指定了 WITH NO POPULATION。 有关详细信息，请参阅[更改跟踪和 NO POPULATION 参数的交互](#change-tracking-no-population)。
  
 TYPE COLUMN type_column_name  
 指定表列的名称 (type_column_name)，用于存储 varbinary、varbinary(max) 或 image 文档的文档类型。 此列（称为类型列）包含用户提供的文件扩展名（.doc、.pdf、.xls 等）。 类型列必须是 **char**、 **nchar**、 **varchar** 或 **nvarchar** 类型。  
  
 仅当 column_name 指定 varbinary、varbinary(max) 或 image 列（数据作为二进制数据存储在该列中）时，才指定 TYPE COLUMN type_column_name；否则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回错误。  
  
> [!NOTE]  
>  在建立索引时，全文引擎使用每个表行的类型列中的缩写来标识对 column_name 中的文档使用哪个全文搜索筛选器。 筛选器按二进制流加载文档，并删除格式设置信息，然后将文档中的文本发送到断字器组件。 有关详细信息，请参阅 [配置和管理搜索筛选器](../../relational-databases/search/configure-and-manage-filters-for-search.md)。  
  
 LANGUAGE language_term  
 存储在 column_name 中的数据的语言。  
  
 language_term 是可选的，可以将其指定为与语言区域设置标识符 (LCID) 对应的字符串、整数或十六进制值。 如果指定了 language_term，则它表示的语言将应用于搜索条件的所有元素。 如果未指定值，则使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的默认全文语言。  
  
 使用 sp_configure 存储过程访问有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的默认全文语言的信息。  
  
 如果指定为字符串，则 language_term 对应于 syslanguages 系统表中的 alias 列值 。 字符串必须用单引号引起来，如 'language_term'。 如果指定为整数，则 language_term 就是标识该语言的实际 LCID。 如果指定为十六进制值，则 language_term 将以 0x 开头，后跟 LCID 的十六进制值。 十六进制值不能超过八位（包括前导零在内）。  
  
 如果该值是双字节字符集 (DBCS) 格式，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将其转换为 Unicode 格式。  
  
 对于指定为 language_term 的语言，必须启用断字符和词干分析器等资源。 如果这些资源不支持指定的语言， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回错误。  
  
 对于包含多种语言的文本数据的非 BLOB 和非 XML 列，或者列中所存储文本的语言未知时，请使用非特定 (0x0) 语言资源。 对于存储在 XML 或 BLOB 类型列中的文档，在创建索引时，将使用文档内的语言编码。 例如，在 XML 列中，XML 文档中的 xml:lang 属性将标识语言。 在查询时，除非将 language_term 指定为全文查询的一部分，否则将使用以前在 language_term 中指定的值作为全文查询的默认语言。  
  
 STATISTICAL_SEMANTICS  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 创建作为统计语义索引一部分的附加关键短语和文档相似性索引。 有关详细信息，请参阅[语义搜索 (SQL Server)](../../relational-databases/search/semantic-search-sql-server.md)。  
  
 [ **,** _...n_]  
 指示可以为 ADD、ALTER 或 DROP 子句指定多个列。 指定多个列时，请使用逗号分隔这些列。  
  
 WITH NO POPULATION  
 指定在执行 ADD 或 DROP 列操作或 SET STOPLIST 操作之后不填充全文索引。 仅当用户执行 START...POPULATION 命令时，才会填充该索引。  
  
 如果指定了 NO POPULATION，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会填充索引。 仅当用户指定了 ALTER FULLTEXT INDEX...START POPULATION 命令后，才填充此索引。 如果未指定 NO POPULATION， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将填充索引。  
  
 如果启用了 CHANGE_TRACKING 并指定了 WITH NO POPULATION，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回一个错误。 如果启用了 CHANGE_TRACKING，但未指定 WITH NO POPULATION，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将对索引执行完全填充。  
  
> [!NOTE]  
>  有关详细信息，请参阅[更改跟踪和 NO POPULATION 参数的交互](#change-tracking-no-population)。
  
 {ADD | DROP } STATISTICAL_SEMANTICS  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 对指定列启用或禁用统计语义索引。 有关详细信息，请参阅[语义搜索 (SQL Server)](../../relational-databases/search/semantic-search-sql-server.md)。  
  
 START {FULL|INCREMENTAL|UPDATE} POPULATION  
 通知 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 开始填充 table_name 的全文检索。 如果全文检索填充已在执行，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回一个警告，并且不会启动新的填充。  
  
 FULL  
 指定检索表中的每一行以进行全文索引，即使已对这些行进行了索引。  
  
 INCREMENTAL  
 指定仅检索自上次填充以来修改的行以进行全文索引。 仅当表包含一个类型为 **timestamp** 的列时，才能应用 INCREMENTAL。 如果全文目录中的表不包含 timestamp 类型的列，则该表将进行完全填充。  
  
 UPDATE  
 指定对自上次更新更改跟踪索引以来的所有插入、更新或删除进行处理。 必须对表启用更改跟踪填充，但不应打开后台更新索引或自动更改跟踪。  
  
 {STOP | PAUSE | RESUME } POPULATION  
 停止或暂停正在进行的任何填充；或者停止或恢复任何暂停的填充。  
  
 STOP POPULATION 不会停止自动更改跟踪或后台更新索引。 若要停止更改跟踪，请使用 SET CHANGE_TRACKING OFF。  
  
 PAUSE POPULATION 和 RESUME POPULATION 只能用于完全填充。 它们与其他填充类型无关，因为其他填充从爬网停止的位置恢复爬网。  
  
 SET STOPLIST { OFF| SYSTEM | stoplist_name }  
 更改与索引（如果有）关联的全文非索引字表。  
  
 OFF  
 指定没有与全文索引关联的非索引字表。  
  
 SYSTEM  
 指定应对此全文索引使用默认的全文系统 STOPLIST。  
  
 stoplist_name  
 指定要与全文索引关联的非索引字表的名称。  
  
 有关详细信息，请参阅 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)。  
  
 SET SEARCH PROPERTY LIST { OFF | property_list_name } [ WITH NO POPULATION ]  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 更改与索引（如果有）关联的搜索属性列表。  
  
 OFF  
 指定不会将任何属性列表与全文索引相关联。 如果关闭全文索引的搜索属性列表 (ALTER FULLTEXT INDEX ...SET SEARCH PROPERTY LIST OFF)，则不再能够对基表进行属性搜索。  
  
 默认情况下，在关闭现有的搜索时属性列表时，将自动重新填充全文索引。 如果在关闭搜索属性列表时指定 WITH NO POPULATION，则不会自动进行重新填充。 但是，我们建议您在方便的时候对此全文索引运行完全填充。 重新填充全文索引将删除每个删除的搜索属性的属性特定元数据，从而使全文索引更小更有效。  
  
 property_list_name  
 指定要与全文索引关联的搜索属性列表的名称。  
  
 为全文索引添加搜索属性列表需要重新填充索引，以便对为关联的搜索属性列表注册的搜索属性编制索引。 如果在添加搜索属性列表时指定了 WITH NO POPULATION，则需要在适当的时候对索引运行填充。  
  
> [!IMPORTANT]  
>  如果全文索引以前与不同的搜索相关联，则必须重新生成属性列表以使索引保持一致状态。 将立即截断索引，并在运行完全填充之前保留为空。 有关详细信息，请参阅[更改搜索属性列表将导致重新生成索引](#change-search-property-rebuild-index)。 
  
> [!NOTE]  
>  您可以将给定的搜索属性列表与同一数据库中的多个全文索引相关联。  
  
 **查找当前数据库上的搜索属性列表**  
  
-   [sys.registered_search_property_lists](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
  
 有关搜索属性列表的详细信息，请参阅[使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)。  
  
## <a name="interactions-of-change-tracking-and-no-population-parameter"></a><a name="change-tracking-no-population"></a> 更改跟踪和 NO POPULATION 参数的交互  
 是否填充全文索引取决于是否启用了更改跟踪以及在 ALTER FULLTEXT INDEX 语句中是否指定了 WITH NO POPULATION。 下表概述了其交互结果。  
  
|更改跟踪|WITH NO POPULATION|结果|  
|---------------------|------------------------|------------|  
|未启用|未指定|对索引执行完全填充。|  
|未启用|Specified|在发出 ALTER FULLTEXT INDEX...START POPULATION 语句之前，不会进行任何索引填充。|  
|已启用|Specified|引发错误，并且不会更改索引。|  
|已启用|未指定|对索引执行完全填充。|  
  
 有关填充全文检索的详细信息，请参阅[填充全文检索](../../relational-databases/search/populate-full-text-indexes.md)。  
  
## <a name="changing-the-search-property-list-causes-rebuilding-the-index"></a><a name="change-search-property-rebuild-index"></a> 更改搜索属性列表将导致重新生成索引  
 第一次将全文索引与搜索属性列表相关联时，必须重新填充索引以便对属性特定的搜索词编制索引。 不会截断现有的索引数据。  
  
 但是，如果将全文索引与不同的属性列表相关联，则会重新生成索引。 立即重新生成索引将截断全文索引，从而删除所有现有的数据，并且必须重新填充索引。 在进行填充时，针对基表的全文查询仅搜索已通过填充编制索引的表行。 重新填充的索引数据将包括新添加的搜索属性列表的注册属性中的元数据。  
  
 导致重新生成的情况包括：  
  
-   直接切换到不同的搜索属性列表（请参阅本节后面“情况 A”）。  
  
-   关闭搜索属性列表并随后将索引与任何搜索属性列表相关联（请参阅本节后面“情况 B”）。  
  
> [!NOTE]  
>  有关如何通过搜索属性列表执行全文搜索的详细信息，请参阅[使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)。 有关完全填充的信息，请参阅[填充全文检索](../../relational-databases/search/populate-full-text-indexes.md)。  
  
### <a name="scenario-a-switching-directly-to-a-different-search-property-list"></a>情况 A：直接切换到不同的搜索属性列表  
  
1.  在具有搜索属性列表 `spl_1` 的 `table_1` 上创建一个全文检索：  
  
    ```sql  
    CREATE FULLTEXT INDEX ON table_1 (column_name) KEY INDEX unique_key_index   
       WITH SEARCH PROPERTY LIST=spl_1,   
       CHANGE_TRACKING OFF, NO POPULATION;   
    ```  
  
2.  对全文索引运行完全填充：  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 START FULL POPULATION;  
    ```  
  
3.  然后，使用以下语句将全文索引与不同的搜索属性列表 `spl_2` 相关联：  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 SET SEARCH PROPERTY LIST spl_2;  
    ```  
  
     此语句导致进行完全填充（默认行为）。  但是，在开始进行此填充之前，全文引擎将自动截断索引。  
  
### <a name="scenario-b-turning-off-the-search-property-list-and-later-associating-the-index-with-any-search-property-list"></a>情况 B：关闭搜索属性列表并随后将索引与任何搜索属性列表相关联  
  
1.  在具有搜索属性列表 `spl_1` 的 `table_1` 上创建一个全文检索，然后自动进行完全填充（默认行为）：  
  
    ```sql  
    CREATE FULLTEXT INDEX ON table_1 (column_name) KEY INDEX unique_key_index   
       WITH SEARCH PROPERTY LIST=spl_1;   
    ```  
  
2.  将关闭搜索属性列表，如下所示：  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1   
       SET SEARCH PROPERTY LIST OFF WITH NO POPULATION;   
    ```  
  
3.  全文索引将再次与相同或不同的搜索属性列表相关联。  
  
     例如，以下语句重新将全文检索与原始搜索属性列表 `spl_1` 相关联：  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 SET SEARCH PROPERTY LIST spl_1;  
    ```  
  
     此语句启动完全填充（默认行为）。  
  
    > [!NOTE]  
    >  还需要对不同的搜索属性列表进行重新生成，例如 `spl_2`。  
  
## <a name="permissions"></a>权限  
 用户必须对相应表或索引视图拥有 ALTER 权限，或者必须是 sysadmin 固定服务器角色、db_ddladmin 固定数据库角色或 db_owner 固定数据库角色的成员。  
  
 如果指定了 SET STOPLIST，则用户必须对此非索引字表拥有 REFERENCES 权限。 如果指定了 SET SEARCH PROPERTY LIST，则用户必须具有搜索属性列表的 REFERENCES 权限。 如果指定非索引字表或搜索属性列表所有者具有 ALTER FULLTEXT CATALOG 权限，则该所有者可以授予 REFERENCES 权限。  
  
> [!NOTE]  
>  授予 public 对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 随附的默认非索引字表的 REFERENCES 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-setting-manual-change-tracking"></a>A. 设置手动更改跟踪  
 下面的示例为 `JobCandidate` 表的全文索引设置手动更改跟踪。  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate  
   SET CHANGE_TRACKING MANUAL;  
GO  
```  
  
### <a name="b-associating-a-property-list-with-a-full-text-index"></a>B. 将属性列表与全文索引关联  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 下面的示例将 `DocumentPropertyList` 属性列表与 `Production.Document` 表的全文检索相关联。 此 ALTER FULLTEXT INDEX 语句启动完全填充，这是 SET SEARCH PROPERTY LIST 子句的默认行为。  
  
> [!NOTE]  
>  有关创建 `DocumentPropertyList` 属性列表的示例，请参阅 [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md)。  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST DocumentPropertyList;   
GO  
```  
  
### <a name="c-removing-a-search-property-list"></a>C. 删除搜索属性列表  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 下面的示例从 `DocumentPropertyList` 表的全文索引中删除 `Production.Document` 属性列表。 在此示例中，并不急于从索引中删除属性，因此指定了 WITH NO POPULATION 选项。 但是，不再允许针对此全文索引进行属性级别的搜索。  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST OFF WITH NO POPULATION;   
GO  
```  
  
### <a name="d-starting-a-full-population"></a>D. 启动完全填充  
 下面的示例对 `JobCandidate` 表的全文检索启动完全填充。  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate   
   START FULL POPULATION;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.fulltext_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)   
 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [DROP FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/drop-fulltext-index-transact-sql.md)   
 [全文搜索](../../relational-databases/search/full-text-search.md)   
 [填充全文索引](../../relational-databases/search/populate-full-text-indexes.md)  
  
  
