---
description: 游标 (SQL Server)
title: 游标 (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/11/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- results [SQL Server], cursors
- Transact-SQL cursors, about cursors
- cursors [SQL Server]
- data access [SQL Server], cursors
- result sets [SQL Server], cursors
- requesting cursors
- cursors [SQL Server], about cursors
ms.assetid: e668b40c-bd4d-4415-850d-20fc4872ee72
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a209c033a9218423e298b54bb04809355c9d0507
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485529"
---
# <a name="sql-server-cursors"></a>SQL Server 游标
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
  关系数据库中的操作会对整个行集起作用。 例如，由 `SELECT` 语句返回的行集包括满足该语句的 `WHERE` 子句中条件的所有行。 这种由语句返回的完整行集称为结果集。 应用程序，特别是交互式联机应用程序，并不总能将整个结果集作为一个单元来有效地处理。 这些应用程序需要一种机制以便每次处理一行或一部分行。 游标就是提供这种机制的对结果集的一种扩展。  
  
 游标通过以下方式来扩展结果处理：  
  
-   允许定位在结果集的特定行。  
  
-   从结果集的当前位置检索一行或一部分行。  
  
-   支持对结果集中当前位置的行进行数据修改。  
  
-   为由其他用户对显示在结果集中的数据库数据所做的更改提供不同级别的可见性支持。  
  
-   提供脚本、存储过程和触发器中用于访问结果集中的数据的 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句。  
  
> [!TIP]
> 在某些情况下，如果表上有一个主键，可以使用 `WHILE` 循环代替游标，而不会产生游标的开销。
> 然而，在某些情况下，游标不仅是不可避免的，而且实际上是不可或缺的。 在这种情况下，如果不需要基于游标来更新表，则使用“firehose”游标，即[快进和只读](#forward-only)游标。
  
## <a name="cursor-implementations"></a>游标实现  
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持三种游标实现。  
  
### <a name="transact-sql-cursors"></a>Transact-SQL 游标  
[!INCLUDE[tsql](../includes/tsql-md.md)] 游标基于 `DECLARE CURSOR` 语法，主要用于 [!INCLUDE[tsql](../includes/tsql-md.md)] 脚本、存储过程和触发器。 [!INCLUDE[tsql](../includes/tsql-md.md)] 游标在服务器上实现，由从客户端发送到服务器的 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句管理。 它们还可能包含在批处理、存储过程或触发器中。  
  
### <a name="application-programming-interface-api-server-cursors"></a>应用程序编程接口 (API) 服务器游标  
API 游标支持 OLE DB 和 ODBC 中的 API 游标函数。 API 服务器游标在服务器上实现。 每次客户端应用程序调用 API 游标函数时， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口或 ODBC 驱动程序会把请求传输到服务器，以便对 API 服务器游标进行操作。  
  
### <a name="client-cursors"></a>客户端游标  
客户端游标由 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序和实现 ADO API 的 DLL 在内部实现。 客户端游标通过在客户端高速缓存所有结果集行来实现。 每次客户端应用程序调用 API 游标函数时， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序或 ADO DLL 会对客户端上高速缓存的结果集行执行游标操作。  
  
## <a name="type-of-cursors"></a>游标类型  
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持四种游标类型。 

> [!NOTE]
> 游标可以利用 tempdb 工作表。 就像溢出的聚合或排序操作一样，这些操作在 I/O 中产生，是潜在的性能瓶颈。 `STATIC` 游标从一开始就使用工作表。 有关详细信息，请参阅[查询处理体系结构指南中的工作表](../relational-databases/query-processing-architecture-guide.md#worktables)。

### <a name="forward-only"></a>只进  
只进游标指定为 `FORWARD_ONLY` 和 `READ_ONLY`，不支持滚动。 这些游标也称为“firehose”游标，并且只支持从游标的开始到结束连续提取行  。 行只在从数据库中提取出来后才能检索。 对所有由当前用户发出或由其他用户提交、并影响结果集中的行的 `INSERT`、`UPDATE` 和 `DELETE` 语句，其效果在这些行从游标中提取时是可见的。  
  
 由于游标无法向后滚动，则在提取行后对数据库中的行进行的大多数更改通过游标均不可见。 当值用于确定所修改的结果集（例如更新聚集索引涵盖的列）中行的位置时，修改后的值通过游标可见。  
  
 尽管数据库 API 游标模型将只进游标视为一种游标类型，但是 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不这样。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将只进和滚动视为可应用于静态游标、键集驱动游标和动态游标的选项。 [!INCLUDE[tsql](../includes/tsql-md.md)] 游标支持只进静态游标、键集驱动游标和动态游标。 数据库 API 游标模型则假定静态游标、键集驱动游标和动态游标都是可滚动的。 当数据库 API 游标属性设置为只进时， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将此游标作为只进动态游标实现。  
  
### <a name="static"></a>静态  
 静态游标的完整结果集是打开游标时在 **tempdb** 中生成的。 静态游标总是按照打开游标时的原样显示结果集。 静态游标在滚动期间很少或根本检测不到变化，但消耗的资源相对很少。  
  
游标不反映在数据库中所做的任何影响结果集成员身份的更改，也不反映对组成结果集的行的列值所做的更改。 静态游标不会显示打开游标以后在数据库中新插入的行，即使这些行符合游标 `SELECT` 语句的搜索条件。 如果组成结果集的行被其他用户更新，则新的数据值不会显示在静态游标中。 静态游标会显示打开游标以后从数据库中删除的行。 静态游标中不反映 `UPDATE`、`INSERT` 或者 `DELETE` 操作（除非关闭游标然后重新打开），甚至不反映使用打开游标的同一连接所做的修改。  
  
> [!NOTE]
> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 静态游标始终是只读的。  
  
> [!NOTE]
> 由于静态游标的结果集存储在“tempdb”中的一个工作表中，因此结果集中的行大小不能超过 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 表的最大行大小。  
> 有关详细信息，请参阅[查询处理体系结构指南中的工作表](../relational-databases/query-processing-architecture-guide.md#worktables)。 有关最大行大小的详细信息，请参阅 [SQL Server 最大容量规范](../sql-server/maximum-capacity-specifications-for-sql-server.md)。  
  
[!INCLUDE[tsql](../includes/tsql-md.md)] 称静态游标为不敏感游标。 一些数据库 API 将这类游标识别为快照游标。  
  
### <a name="keyset"></a>Keyset  
打开由键集驱动的游标时，该游标中各行的成员身份和顺序是固定的。 由键集驱动的游标由一组唯一标识符（键）控制，这组键称为键集。 键是根据以唯一方式标识结果集中各行的一组列生成的。 键集是打开游标时来自符合 `SELECT` 语句要求的所有行中的一组键值。 由键集驱动的游标对应的键集是打开该游标时在 **tempdb** 中生成的。  
  
### <a name="dynamic"></a>动态  
动态游标与静态游标相对。 当滚动游标时，动态游标反映结果集中所做的所有更改。 结果集中的行数据值、顺序和成员在每次提取时都会改变。 所有用户做的全部 `UPDATE`、`INSERT` 和 `DELETE` 语句均通过游标可见。 如果使用 API 函数（如“SQLSetPos”）或 [!INCLUDE[tsql](../includes/tsql-md.md)] `WHERE CURRENT OF` 子句通过游标进行更新，它们将立即可见  。 在游标外部所做的更新直到提交时才可见，除非将游标的事务隔离级别设为未提交读。 有关隔离级别的详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。 
 
> [!NOTE]
> 动态游标计划从不使用空间索引。  
  
## <a name="requesting-a-cursor"></a>请求游标  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持两种请求游标的方法：  
  
-   [!INCLUDE[tsql](../includes/tsql-md.md)]  
  
     [!INCLUDE[tsql](../includes/tsql-md.md)] 语言支持在 ISO 游标语法之后制定的用于使用游标的语法。  
  
-   数据库应用程序编程接口（API）游标函数  
  
     [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持以下数据库 API 的游标功能：  
  
    -   ADO（[!INCLUDE[msCoName](../includes/msconame-md.md)] ActiveX 数据对象）  
  
    -   OLE DB  
  
    -   ODBC（开放式数据库连接）  
  
应用程序不能混合使用这两种请求游标的方法。 已经使用 API 指定游标行为的应用程序不能再执行 [!INCLUDE[tsql](../includes/tsql-md.md)] DECLARE CURSOR 语句请求一个 [!INCLUDE[tsql](../includes/tsql-md.md)] 游标。 应用程序只有在将所有的 API 游标特性设置回默认值后，才可以执行 DECLARE CURSOR。  
  
如果既未请求 [!INCLUDE[tsql](../includes/tsql-md.md)] 游标也未请求 API 游标，则默认情况下 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将向应用程序返回一个完整的结果集，这个结果集称为默认结果集。  
  
## <a name="cursor-process"></a>游标进程  
 [!INCLUDE[tsql](../includes/tsql-md.md)] 游标和 API 游标有不同的语法，但下列一般进程适用于所有 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 游标：  
  
1.  将游标与 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句的结果集相关联，并且定义该游标的特性，例如是否能够更新游标中的行。  
  
2.  执行 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句以填充游标。  
  
3.  从游标中检索您想要查看的行。 从游标中检索一行或一部分行的操作称为提取。 执行一系列提取操作以便向前或向后检索行的操作称为滚动。  
  
4.  根据需要，对游标中当前位置的行执行修改操作（更新或删除）。  
  
5.  关闭游标。  
  
## <a name="related-content"></a>相关内容  
[游标行为](../relational-databases/native-client-odbc-cursors/cursor-behaviors.md)    
[如何实现游标](../relational-databases/native-client-odbc-cursors/implementation/how-cursors-are-implemented.md)  
  
## <a name="see-also"></a>另请参阅  
[DECLARE CURSOR (Transact-SQL)](../t-sql/language-elements/declare-cursor-transact-sql.md)   
[游标 (Transact-SQL)](../t-sql/language-elements/cursors-transact-sql.md)   
[游标函数 (Transact-SQL)](../t-sql/functions/cursor-functions-transact-sql.md)   
[游标存储过程 (Transact-SQL)](../relational-databases/system-stored-procedures/cursor-stored-procedures-transact-sql.md)   
[SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../t-sql/statements/set-transaction-isolation-level-transact-sql.md)    

  
