---
title: 内存优化表的查询处理
description: 了解在 SQL Server 的内存中 OLTP 中针对内存优化表和本机编译存储过程的查询处理。
ms.custom: seo-dt-2019
ms.date: 05/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 065296fe-6711-4837-965e-252ef6c13a0f
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eed83fcd8a8b861f102c90fbc73d28d51bd5fa56
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465478"
---
# <a name="a-guide-to-query-processing-for-memory-optimized-tables"></a>内存优化表查询处理指南
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  内存中 OLTP 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中引入内存优化的表和本机编译的存储过程。 本文简单介绍针对内存优化表和本机编译存储过程的查询处理。  
  
 本文档介绍如何编译和执行对内存优化表的查询，包括：  
  
-   基于磁盘的表的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的查询处理管道。  
  
-   查询优化；内存优化的表统计信息的作用以及有关处理有错的查询计划的准则。  
  
-   使用解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问内存优化表。  
  
-   有关优化访问内存优化表的查询的注意事项。  
  
-   本机编译存储过程编译和处理。  
  
-   优化器用来估计开销的统计信息。  
  
-   修复问题查询计划的方式。  
  
## <a name="example-query"></a>示例查询  
 我们将用下面两个示例说明本文中讨论的查询处理概念。  
  
 我们考虑 Customer 和 Order 这两个表。 以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本按（传统）基于磁盘的形式定义了这两个表和关联的索引：  
  
```sql  
CREATE TABLE dbo.[Customer] (  
  CustomerID nchar (5) NOT NULL PRIMARY KEY,  
  ContactName nvarchar (30) NOT NULL   
)  
GO  
  
CREATE TABLE dbo.[Order] (  
  OrderID int NOT NULL PRIMARY KEY,  
  CustomerID nchar (5) NOT NULL,  
  OrderDate date NOT NULL  
)  
GO  
CREATE INDEX IX_CustomerID ON dbo.[Order](CustomerID)  
GO  
CREATE INDEX IX_OrderDate ON dbo.[Order](OrderDate)  
GO  
```  
  
 为构造本文中所示的查询计划，这两个表是用来自 Northwind 示例数据库的示例数据填充的，你可以从 [SQL Server 2000 的 Northwind 和 pubs 示例数据库](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs)下载相关数据库。  
  
 考虑以下查询，这些查询联接 Customer 和 Order 表，并返回订单 ID 和相关客户信息：  
  
```sql  
SELECT o.OrderID, c.* FROM dbo.[Customer] c INNER JOIN dbo.[Order] o ON c.CustomerID = o.CustomerID  
```  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 显示的估计的执行计划如下  
  
 ![用于联接基于磁盘的表的查询计划。](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-1.png "用于联接基于磁盘的表的查询计划。")  
用于联接基于磁盘的表的查询计划。  
  
 关于此查询计划：  
  
-   来自 Customer 表的行从聚集索引检索，聚集索引是主数据结构并且有完整的表数据。  
  
-   Order 表的数据是使用 CustomerID 列的非聚集索引检索的。 此索引包含 CustomerID 列（用于联接）和主键列 OrderID（返回给用户）。 返回 Order 表的其他列需要查找 Order 表的聚集索引。  
  
-   逻辑运算符 **Inner Join** 是通过物理运算符 **Merge Join** 实现的。 其他物理联接类型为 **Nested Loops** 和 **Hash Join**。 **Merge Join** 运算符利用两个索引都按联接列 CustomerID 排序这一事实。  
  
 考虑一个与此查询稍有不同的查询，它返回 Order 表的所有行，而不仅是 OrderID：  
  
```sql  
SELECT o.*, c.* FROM dbo.[Customer] c INNER JOIN dbo.[Order] o ON c.CustomerID = o.CustomerID  
```  
  
 此查询的估计的计划为：  
  
 ![哈希联接基于磁盘的表的查询计划。](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-2.png "哈希联接基于磁盘的表的查询计划。")  
哈希联接基于磁盘的表的查询计划。  
  
 在此查询中，Order 表的行是使用聚集索引检索的。 **Hash Match** 物理运算符现在用于 **Inner Join**。 Order 的聚集索引不是按 CustomerID 排序的，因此 **Merge Join** 需要一个排序运算符，这会影响性能。 请注意 **Hash Match** 运算符的相对开销 (75%) 和上一示例中 **Merge Join** 运算符的开销 (46%) 之间的比较。 优化器在上一示例中也考虑了 **Hash Match** 运算符，但结论是 **Merge Join** 运算符可以提供更好的性能。  
  
## <a name="ssnoversion-query-processing-for-disk-based-tables"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 基于磁盘的表的查询处理  
 下图显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中针对即席查询的查询处理流程：  
  
 ![SQL Server 查询处理管道。](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-3.png "SQL Server 查询处理管道。")  
SQL Server 查询处理管道。  
  
 在本方案中：  
  
1.  用户发出查询。  
  
2.  分析器和 algebrizer 根据用户提交的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 文本采用逻辑运算符构造查询树。  
  
3.  优化器创建一个包含物理运算符的查询优化计划（例如，嵌套循环联接）。 优化后，该计划存储在计划高速缓存中。 如果计划高速缓存中已经包含针对此查询的计划，则跳过此步骤。  
  
4.  查询执行引擎处理查询计划的解释。  
  
5.  对于每个索引查找、索引扫描和表扫描运算符，执行引擎都会从 Access Methods 请求来自相应索引和表结构的行。  
  
6.  Access Methods 根据需要从缓冲池中的索引和数据页检索行并将页面从磁盘加载到缓冲池。  

 对于第一个示例查询，执行引擎从 Access Methods 请求 Customer 聚集索引中的行和 Order 非聚集索引中的行。 Access Methods 遍历 B 树索引结构以检索请求的行。 在本例中检索所有行，因为计划需要全部索引扫描。  
  
## <a name="interpreted-tsql-access-to-memory-optimized-tables"></a>使用解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问内存优化表  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 即席批处理和存储过程也称为解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)]。 解释型是指这样一个事实，即对于查询计划中的每个运算符，查询计划都由查询执行引擎进行解释。 执行引擎读取运算符及其参数并执行运算。  
  
 解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 可用于访问内存优化表和基于磁盘的表。 下图举例说明对解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问内存优化表的查询处理：  
  
 ![用于解释型 tsql 的查询处理管道。](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-4.png "用于解释型 tsql 的查询处理管道。")  
有关解释型 Transact-SQL 访问内存优化表的查询处理管道。  
  
 如图所示，查询处理管道基本保持不变：  
  
-   分析器和 algebrizer 构造查询树。  
  
-   优化器创建执行计划。  
  
-   查询执行引擎解释执行计划。  
  
 与传统查询处理管道（图 2）的主要不同之处在于并非使用访问方法从缓冲池检索内存优化表的行。 而是通过内存中 OLTP 引擎从内存中数据结构检索行。 数据结构上的差异导致优化器在某些情况下选取不同的计划，如下例所示。  
  
 下面的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本包含 Order 和 Customer 表的内存优化版本，其中使用哈希索引：  
  
```sql  
CREATE TABLE dbo.[Customer] (  
  CustomerID nchar (5) NOT NULL PRIMARY KEY NONCLUSTERED,  
  ContactName nvarchar (30) NOT NULL   
) WITH (MEMORY_OPTIMIZED=ON)  
GO  
  
CREATE TABLE dbo.[Order] (  
  OrderID int NOT NULL PRIMARY KEY NONCLUSTERED,  
  CustomerID nchar (5) NOT NULL INDEX IX_CustomerID HASH(CustomerID) WITH (BUCKET_COUNT=100000),  
  OrderDate date NOT NULL INDEX IX_OrderDate HASH(OrderDate) WITH (BUCKET_COUNT=100000)  
) WITH (MEMORY_OPTIMIZED=ON)  
GO  
```  
  
 考虑对内存优化表执行相同的查询：  
  
```sql  
SELECT o.OrderID, c.* FROM dbo.[Customer] c INNER JOIN dbo.[Order] o ON c.CustomerID = o.CustomerID  
```  
  
 估计的计划如下：  
  
 ![用于联接内存优化表的查询计划。](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-5.png "用于联接内存优化表的查询计划。")  
用于联接内存优化表的查询计划。  
  
 观察该计划与基于磁盘的表的相同查询计划（图 1）的以下不同：  
  
-   此计划包含针对 Customer 表的表扫描，而不是聚集索引扫描：  
  
    -   表定义不包含聚集索引。  
  
    -   内存优化表不支持聚集索引。 相反，每个内存优化表必须至少有一个非聚集索引，因此内存优化表上的所有索引可高效访问表中的所有列，而不必将其存储在索引中或引用聚集索引。  
  
-   此计划包含 **Hash Match** 而不是 **Merge Join**。 Order 和 Customer 表的索引均为哈希索引，因此没有顺序。 **Merge Join** 会要求排序运算符，这会降低性能。  
  
## <a name="natively-compiled-stored-procedures"></a>本机编译的存储过程  
 本机编译存储过程是编译为机器代码的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程，而不是由查询执行引擎解释。 以下脚本创建一个本机编译存储过程来运行示例查询（来自“示例查询”部分）。  
  
```sql  
CREATE PROCEDURE usp_SampleJoin  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH   
(  TRANSACTION ISOLATION LEVEL = SNAPSHOT,  
  LANGUAGE = 'english')  
  
  SELECT o.OrderID, c.CustomerID, c.ContactName   
FROM dbo.[Order] o INNER JOIN dbo.[Customer] c   
  ON c.CustomerID = o.CustomerID  
  
END  
```  
  
 本机编译存储过程在创建时编译，而解释型存储过程在首次执行时编译。 （部分编译（特别是分析和 algebrization）发生在创建时。 但是，对于解释型存储过程，将在首次执行时优化查询计划。）重新编译逻辑相似。 如果服务器已重新启动，本机编译的存储过程将在该过程首次执行时重新编译。 解释型存储过程在计划不再存在于计划高速缓存中时重新编译。 下表对本机编译存储过程和解释型存储过程的编译和重新编译进行了总结：  
  
|编译类型|本机编译|使用解释型|  
|-|-----------------------|-----------------|  
|初始编译|创建时。|首次执行时。|  
|自动重新编译|在数据库或服务器重新启动后首次执行该过程时。|服务器重新启动时。 或者从计划高速缓存中逐出时（通常是由于架构或状态更改，或者内存压力）。|  
|手动重新编译|使用 **sp_recompile**。|使用 **sp_recompile**。 您可以手动将计划逐出高速缓存，例如通过 DBCC FREEPROCCACHE。 也可以创建存储过程 WITH RECOMPILE，存储过程将在每次执行时重新编译。|  
  
### <a name="compilation-and-query-processing"></a>编译和查询处理  
 下图说明本机编译存储过程的编译流程：  
  
 ![存储过程的本机编译。](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-6.png "存储过程的本机编译。")  
存储过程的本机编译。  
  
 该过程如下，  
  
1.  用户向 **发出** CREATE PROCEDURE [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]语句。  
  
2.  分析器和 algebrizer 为该过程创建处理流程，并为存储过程中的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询创建查询树。  
  
3.  优化器为存储过程中的所有查询创建优化的查询执行计划。  
  
4.  内存中 OLTP 编译器通过嵌入的优化查询计划接管处理流程，并生成一个 DLL，其中包含执行存储过程的机器代码。  
  
5.  生成的 DLL 加载到内存中。  
  
 本机编译存储过程的调用转换为对 DLL 中函数的调用。  
  
 ![本机编译存储过程的执行。](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-7.png "本机编译存储过程的执行。")  
本机编译存储过程的执行。  
  
 本机编译存储过程的调用如下所述：  
  
1.  用户发出一条“EXEC usp_myproc”语句。  
  
2.  分析器提取名称和存储过程参数。  
  
     如果语句已准备就绪（例如使用 **sp_prep_exec**），则分析器执行时不需要提取过程名称和参数。  
  
3.  内存中 OLTP 运行时查找存储过程的 DLL 入口点。  
  
4.  DLL 中的机器代码将执行，结果会返回到客户端。  
  
 **参数截取**  
  
 解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程在首次执行时编译，而本机编译存储过程在创建时编译。 由于调用编译解释型存储过程时，优化器使用为此调用提供的参数值生成执行计划。 这种编译期间的参数用法称为参数截取。  
  
 参数截取不适用于编译本机编译存储过程。 此类存储过程的所有参数都视为具有 UNKNOWN 值。 与解释型存储过程不同，本机编译存储过程还支持 **OPTIMIZE FOR** 提示。 有关详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  
  
### <a name="retrieving-a-query-execution-plan-for-natively-compiled-stored-procedures"></a>为本机编译存储过程检索查询执行计划  
 可使用 **中的** 估计的执行计划 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]，或使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的 SHOWPLAN_XML 选项，检索本机编译的存储过程的查询执行计划。 例如：  
  
```sql  
SET SHOWPLAN_XML ON  
GO  
EXEC dbo.usp_myproc  
GO  
SET SHOWPLAN_XML OFF  
GO  
```  
  
 查询优化器生成的执行计划由树组成，树的节点和叶是查询运算符。 树的结构确定运算符之间的交互（行从一个运算符到另一个运算符的流动）。 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]图形视图中，是从右向左流动的。 例如，图 1 中的查询计划包含两个索引扫描运算符，它们向合并联接运算符提供行。 合并联接运算符向选择运算符提供行。 最后，选择运算符将行返回客户端。  
  
### <a name="query-operators-in-natively-compiled-stored-procedures"></a>本机编译存储过程中的查询运算符  
 下表对本机编译存储过程中支持的查询运算符进行了总结：  
  
|操作员|示例查询|说明|  
|--------------|------------------|-----------|  
|SELECT|`SELECT OrderID FROM dbo.[Order]`||  
|INSERT|`INSERT dbo.Customer VALUES ('abc', 'def')`||  
|UPDATE|`UPDATE dbo.Customer SET ContactName='ghi' WHERE CustomerID='abc'`||  
|DELETE|`DELETE dbo.Customer WHERE CustomerID='abc'`||  
|Compute Scalar|`SELECT OrderID+1 FROM dbo.[Order]`|此运算符用于内部函数和类型转换。 不是所有函数和类型转换在本机编译存储过程中都受支持。|  
|Nested Loops Join|`SELECT o.OrderID, c.CustomerID FROM dbo.[Order] o INNER JOIN dbo.[Customer] c`|Nested Loops 是本机编译存储过程内唯一支持的联接运算符。 所有包含联接的计划都将使用 Nested Loops 运算符，即使以解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 执行的同一查询计划包含哈希或合并联接也是如此。|  
|排序|`SELECT ContactName FROM dbo.Customer ORDER BY ContactName`||  
|TOP|`SELECT TOP 10 ContactName FROM dbo.Customer`||  
|Top-sort|`SELECT TOP 10 ContactName FROM dbo.Customer  ORDER BY ContactName`|**TOP** 表达式（要返回的行的数量）不能超过 8,000 行。 如果查询中还有联接和聚合运算符，数量会更少。 与基表的行数相比，联接和聚合通常可减少要排序的行数。|  
|Stream Aggregate|`SELECT count(CustomerID) FROM dbo.Customer`|请注意，聚合不支持 Hash Match 运算符。 因此，本机编译存储过程中的所有聚合都使用 Stream Aggregate 运算符，即使解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中针对同一查询计划使用 Hash Match 运算符也是如此。|  
  
## <a name="column-statistics-and-joins"></a>列统计信息和联接  

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在索引键列值中维护统计信息，以帮助估计特定操作的开销，如索引扫描和索引查找。 （如果显式创建非索引键列，或者查询优化器在对于带谓词的查询提供的响应中创建非索引键列，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 也会对这些列创建统计信息。）开销估计的主要度量是一个运算符处理的行数。 请注意，对于基于磁盘的表，某一特定运算符处理的页数对于开销估计非常重要。 但是，由于页数对于内存优化表并不重要（始终为零），因此本次讨论重点在于行数。 从计划中的索引查找和扫描运算符开始估计，然后扩展到包含其他运算符，如联接运算符。 对联接运算符要处理的行数的估计基于对基础索引、搜索和扫描运算符的估计。 对于内存优化表的解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问，您可以通过实际执行计划查看计划中运算符的估计行数和实际行计数之差。  
  
对于图 1 中的示例，  
  
- 对 Customer 的聚集索引扫描的估计行数为 91，实际为 91。  
- 对 CustomerID 的非聚集索引扫描的估计行数为 830，实际为 830。  
- Merge Join 运算符的估计值为 815，实际为 830。  
  
索引扫描的估计值非常准确。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 维护基于磁盘的表的行计数。 整个表和索引扫描的估计值始终很准确。 联接的估计值也相当准确。  
  
如果这些估计值更改，不同备选计划的开销考虑也会发生更改。 例如，如果联接的一端的估计行计数为 1 或几行，则使用嵌套循环联接开销较小。 请考虑下列查询：  
  
```sql
SELECT o.OrderID, c.* FROM dbo.[Customer] c INNER JOIN dbo.[Order] o ON c.CustomerID = o.CustomerID  
```  
  
删除 `Customer` 表中的所有行后，将生成以下查询计划：  
  
![列统计信息和联接。](../../relational-databases/in-memory-oltp/media/hekaton-query-plan-9.png "列统计信息和联接。")  
  
关于此查询计划：  
  
- 用 Nested Loops 物理联接运算符替换了 Hash Match。  
- 用索引查找替换了对 IX_CustomerID 的全文检索扫描。 这样只需扫描 5 行，而全文检索扫描需要扫描 830 行。  
  
## <a name="see-also"></a>另请参阅  
 [Memory-Optimized Tables](./sample-database-for-in-memory-oltp.md)  
  
