---
description: 填充全文索引
title: 填充全文索引 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- index populations [full-text search]
- incremental populations [full-text search]
- populations [full-text search]
- full-text search [SQL Server], populations
- crawls [full-text search]
- automatic full-text index updates
- change tracking-based populations [full-text search]
- manual updates [full-text search]
- manual populations [full-text search]
- auto populations [full-text search]
- full-text indexes [SQL Server], key column
- full populations [full-text search]
- full-text indexes [SQL Server], populations
ms.assetid: 76767b20-ef55-49ce-8dc4-e77cb8ff618a
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ac09b96cf93144ebf2b615bbd73db36c673eb5ca
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479458"
---
# <a name="populate-full-text-indexes"></a>填充全文索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  创建和维护全文索引涉及使用称为“填充”（也称为“爬网”）的过程填充索引。  
  
##  <a name="types-of-population"></a><a name="types"></a> Types of population  
全文索引支持以下填充类型：
-   **完全** 填充
-   基于 **更改跟踪** 的自动或手动填充
-   基于 **时间戳** 的增量填充
  
## <a name="full-population"></a>完全填充  
 在完全填充期间，为表或索引视图的所有行生成索引条目。 全文索引的完全填充为基表或索引视图的所有行生成索引条目。  
  
默认情况下，一旦创建新的全文索引， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 便会对其进行完全填充。
-   另一方面，完全填充会占用大量的资源。 因此，当在高峰期创建全文索引时，最佳做法通常是将完全填充推迟到非高峰时段，当全文索引的基表非常大时更应如此。
-   另一方面，索引所属的全文目录在填充其所有全文索引之后才可使用。

若要创建全文索引但不立即填充该索引，请在 `CREATE FULLTEXT INDEX` 语句中指定 `CHANGE_TRACKING OFF, NO POPULATION` 子句。 如果指定 `CHANGE_TRACKING MANUAL`，则在你使用 `START FULL POPULATION` 或 `START INCREMENTAL POPULATION` 子句执行 `ALTER FULLTEXT INDEX` 语句之前，全文引擎不会填充新的全文索引。 

### <a name="example---create-a-full-text-index-without-running-a-full-population"></a>示例 - 创建全文索引但不运行完全填充  
 以下示例对 `Production.Document` 示例数据库的 `AdventureWorks` 表创建全文索引。 此示例使用 `WITH CHANGE_TRACKING OFF, NO POPULATION` 来延迟初始完全填充。  
  
```sql
CREATE UNIQUE INDEX ui_ukDoc ON Production.Document(DocumentID);  
CREATE FULLTEXT CATALOG AW_Production_FTCat;  
CREATE FULLTEXT INDEX ON Production.Document  
(  
    Document                         --Full-text index column name   
        TYPE COLUMN FileExtension    --Name of column that contains file type information  
        Language 1033                 --1033 is LCID for the English language  
)  
    KEY INDEX ui_ukDoc  
    ON AW_Production_FTCat  
    WITH CHANGE_TRACKING OFF, NO POPULATION;  
GO  
  
```  
  
### <a name="example---run-a-full-population-on-a-table"></a>示例 - 对表运行完全填充  
 以下示例对 `Production.Document` 示例数据库的 `AdventureWorks` 表运行完全填充。  
  
```sql
ALTER FULLTEXT INDEX ON Production.Document  
   START FULL POPULATION;  
```  
   
## <a name="population-based-on-change-tracking"></a>基于更改跟踪的填充
 或者，您可以在对全文索引进行初始完全填充之后使用更改跟踪对其进行维护。 将出现与更改跟踪关联的较小开销，因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 维护它用来跟踪自上次填充后对基表所做更改的表。 当你使用更改跟踪时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会维护基表或索引视图中已通过更新、删除或插入进行过修改的行的记录。 通过 WRITETEXT 和 UPDATETEXT 所做的数据更改不会反映到全文索引中，也不能使用更改跟踪方法拾取。  
  
> [!NOTE]  
>  对于包含 **timestamp** 列的表，可以使用增量填充来代替更改跟踪。  
  
 如果你在创建索引期间启用更改跟踪，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在新全文索引创建之后立即对其进行完全填充。 其后，将跟踪更改并将更改传播到全文索引。

### <a name="enable-change-tracking"></a>启用更改跟踪
有两种类型的更改跟踪：
-   自动（`CHANGE_TRACKING AUTO` 选项）。 自动更改跟踪为默认行为。
-   手动（`CHANGE_TRACKING MANUAL` 选项）。   
  
 更改跟踪的类型确定全文索引的填充方式，如下所示：  
  
-   **自动填充**  
  
     默认情况下，或者如果你指定 `CHANGE_TRACKING AUTO`，全文引擎将对全文索引使用自动填充。 完成首次完全填充之后，当修改基表中的数据时将跟踪更改并自动传播跟踪的更改。 不过，由于全文索引是在后台更新的，因此传播的更改可能不会立即反映到索引中。  
  
     **启动使用自动填充的跟踪更改**  
  
    -   [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md) ...WITH CHANGE_TRACKING AUTO  
  
    -   [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) ...SET CHANGE_TRACKING AUTO  
  
    **示例 - 更改全文索引以使用自动更改跟踪**  
    以下示例更改 `HumanResources.JobCandidate` 示例数据库的 `AdventureWorks` 表的全文索引以使用自动填充的更改跟踪。  
  
    ```sql  
    USE AdventureWorks;  
    GO  
    ALTER FULLTEXT INDEX ON HumanResources.JobCandidate SET CHANGE_TRACKING AUTO;  
    GO   
    ```  
  
-   **手动填充**  
  
     如果您指定 CHANGE_TRACKING MANUAL，全文引擎将对全文索引使用手动填充。 完成首次完全填充之后，当修改基表中的数据时将跟踪更改。 但是，这些更改不会传播到全文索引，直至你执行 ALTER FULLTEXT INDEX …START UPDATE POPULATION 语句手动应用更改。 您可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理来定期调用此 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
     **启动使用手动填充的跟踪更改**  
  
    -   [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md) ...WITH CHANGE_TRACKING MANUAL  
  
    -   [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) ...SET CHANGE_TRACKING MANUAL  
  
    **示例 - 使用手动更改跟踪创建全文索引**  
    以下示例对 `HumanResources.JobCandidate` 示例数据库的 `AdventureWorks` 表创建全文索引，该全文索引将使用具有手动填充的更改跟踪。  
  
    ```sql
    USE AdventureWorks;  
    GO  
    CREATE UNIQUE INDEX ui_ukJobCand ON HumanResources.JobCandidate(JobCandidateID);  
    CREATE FULLTEXT CATALOG ft AS DEFAULT;  
    CREATE FULLTEXT INDEX ON HumanResources.JobCandidate(Resume)   
       KEY INDEX ui_ukJobCand   
       WITH CHANGE_TRACKING=MANUAL;  
    GO  
    ```  
  
    **示例 - 运行手动填充**  
    以下示例对 `HumanResources.JobCandidate` 示例数据库的 `AdventureWorks` 表的更改跟踪全文索引运行手动填充。  
  
    ```sql 
    USE AdventureWorks;  
    GO  
    ALTER FULLTEXT INDEX ON HumanResources.JobCandidate START UPDATE POPULATION;  
    GO  
    ```
   
### <a name="disable-change-tracking"></a>禁用更改跟踪 
  
-   [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md) ...WITH CHANGE_TRACKING OFF  
  
-   [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) ...SET CHANGE_TRACKING OFF  
   
  
## <a name="incremental-population-based-on-a-timestamp"></a>基于时间戳的增量填充  
 增量填充是手动填充全文索引的一种替代机制。 如果对表进行大量插入操作，则使用增量填充会较使用手动填充有效。
 
 您可以对 CHANGE_TRACKING 设置为 MANUAL 或 OFF 的全文索引运行增量填充。 
  
 增量填充要求索引表必须具有 **timestamp** 数据类型的列。 如果 **timestamp** 列不存在，则无法执行增量填充。   

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 **timestamp** 列标识自上次填充后发生更改的行。 然后，增量填充在全文索引中更新上次填充的当时或之后添加、删除或修改的行。 在填充结束时，全文引擎将记录新的 **timestamp** 值。 此值是 SQL 收集器找到的最大 **timestamp** 值。 下一次启动增量填充时，将使用此值。  
 
在某些情况下，针对增量填充的请求会导致完全填充。
-   对不含 **timestamp** 列的表请求增量填充会导致完全填充操作。
-   如果全文索引的第一个填充是增量填充，它将对所有行编制索引并使其等效于完全填充。 
-   如果影响表全文索引的任意元数据自上次填充以来发生了变化，则增量填充请求将作为完全填充来执行。 这包括更改任何列、索引或全文索引定义所引起的元数据更改。 

### <a name="run-an-incremental-population"></a>运行增量填充
  
 若要运行增量填充，请使用 `START INCREMENTAL POPULATION` 子句执行 `ALTER FULLTEXT INDEX` 语句。  
  
###  <a name="create-or-change-a-schedule-for-incremental-population"></a><a name="create"></a>创建或更改增量填充计划   
  
1.  在 Management Studio 的对象资源管理器中展开服务器。  
  
2.  展开“数据库”，然后展开包含全文索引的数据库。  
  
3.  展开 **“表”** 。  
  
    右键单击对其定义了全文索引的表，选择“全文索引”，然后在“全文索引”上下文菜单中单击“属性”。 此时将打开“全文索引属性”对话框。  

    > [!IMPORTANT]  
    >  如果基表或视图不包含 **timestamp** 数据类型的列，则无法执行增量填充。
      
1.  在“选择页”窗格中选择“计划”。   
  
     使用此页可以创建或管理 SQL Server 代理作业的计划，该作业用于启动对全文索引基表或索引视图的表增量填充。  

     选项如下：  
  
    -   若要 **创建** 新计划，请单击“新建”。  
  
        此时将打开“新建全文索引表计划”对话框，你可以在此对话框中创建计划。 若要保存计划，请单击 **“确定”** 。  
  
        > [!IMPORTANT]  
        >  在退出“全文索引属性”对话框之后，SQL Server 代理作业（对 *database_name*.*table_name* 启动表增量填充）将与新计划相关联。 如果你针对同一全文索引创建多个计划，这些计划都将使用同一作业。  
  
    -   若要 **更改** 现有的计划，选择该计划，然后单击“编辑”。  
  
         此时将打开“新建全文索引表计划” 对话框，你可以在此对话框中修改计划。  
  
        > [!NOTE]  
        >  有关修改 SQL Server 代理作业的信息，请参阅[修改作业](../../ssms/agent/modify-a-job.md)。  
  
    -   若要 **删除** 现有的计划，选择该计划，然后单击“删除”。  
  
2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]   

##  <a name="troubleshoot-errors-in-a-full-text-population-crawl"></a><a name="crawl"></a>排查全文填充（爬网）错误  
如果在爬网期间发生了错误，全文搜索的爬网日志功能会创建并维护一个爬网日志，该日志是一个纯文本文件。 每个爬网日志都对应于某一个全文目录。 默认情况下，给定实例（在此示例中为默认实例）的爬网日志文件位于 `%ProgramFiles%\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\LOG` 文件夹中。
 
爬网日志文件遵循以下命名方案：  
  
`SQLFT<DatabaseID><FullTextCatalogID>.LOG[<n>]`
  
爬网日志文件名的可变部分如下。
-   <**DatabaseID**> - 数据库的 ID。 \<**dbid**> 是一个带有前导零的五位数。  
-   <**FullTextCatalogID**> - 全文目录 ID。 \<**catid**> 是一个带有前导零的五位数。  
-   <**n**> 是一个整数，指示同一全文目录现有的一个或多个爬网日志。  
  
 例如，`SQLFT0000500008.2` 是一个数据库 ID 为 5、全文目录 ID 为 8 的数据库爬网日志文件。 文件名结尾的 2 指示此数据库/目录对具有两个爬网日志文件。  

## <a name="see-also"></a>另请参阅  
 [sys.dm_fts_index_population (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)   
 [全文搜索入门](../../relational-databases/search/get-started-with-full-text-search.md)   
 [创建和管理全文索引](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
