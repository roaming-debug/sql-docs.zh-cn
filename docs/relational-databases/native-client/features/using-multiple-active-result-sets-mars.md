---
title: 使用多重活动结果集 (MARS) | Microsoft Docs
description: SQL Server 支持多个活动的结果集。 每个连接的应用程序可以有多个挂起的请求和活动的默认结果集。
ms.custom: ''
ms.date: 08/08/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, MARS
- SQLNCLI, MARS
- data access [SQL Server Native Client], MARS
- Multiple Active Result Sets
- SQL Server Native Client, MARS
- MARS [SQL Server]
- SQL Server Native Client ODBC driver, MARS
ms.assetid: ecfd9c6b-7d29-41d8-af2e-89d7fb9a1d83
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 468de09dbbf49b14f1794de5956df7e6125407fa
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461978"
---
# <a name="using-multiple-active-result-sets-mars-in-sql-server-native-client"></a>在 SQL Server Native Client (MARS) 使用多个活动的结果集

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 在访问 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的应用程序中引入了对多重活动结果集 (MARS) 的支持。 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的早期版本中，数据库应用程序无法在单个连接上保持多个活动语句。 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认结果集时，应用程序必须先处理或取消从某一批处理生成的所有结果集，然后才能对该连接执行任何其他批处理。 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 引入了新的连接属性，该属性允许应用程序在每个连接上使用多个待定请求，具体而言，每个连接可以具有多个活动的默认结果集。  
  
 MARS 通过以下新功能简化了应用程序设计：  
  
-   应用程序可以同时打开多个默认结果集，并且交错读取它们。  
  
-   应用程序可以在默认结果集打开的同时执行其他语句（例如 INSERT、UPDATE、DELETE 和存储过程调用）。  
  
 下列指南对使用 MARS 的应用程序很有帮助：  
  
-   默认结果集应该用于使用单个 SQL 语句（SELECT、带 OUTPUT 的 DML、RECEIVE、READ TEXT 等）生成的短期或较小结果集。  
  
-   服务器游标应该用于使用单个 SQL 语句生成的长期或较大结果集。  
  
-   对于过程请求（不论它们是否返回结果）以及返回多个结果的批处理，应始终读取到它们的结果的末尾。  
  
-   尽可能使用 API 调用（而不是 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句）更改连接属性和管理事务。  
  
-   在 MARS 中，有多个批处理并发运行时禁止会话范围内的模拟。  

> [!NOTE]
> 默认情况下，驱动程序不启用 MARS 功能。 若要在通过 Native Client 连接到时使用 MARS [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，必须在连接字符串中专门启用 mars。 但是，如果应用程序检测到驱动程序支持 MARS，某些应用程序可能会默认启用 MARS。 对于这些应用程序，可以根据需要在连接字符串中禁用 MARS。 有关详细信息，请参阅本主题下文中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序等章节。

 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 不限制某个连接上的活动语句的数量。  
  
 不需要同时执行多个多语句批处理或存储过程的典型应用程序将受益于 MARS，而不必了解如何实现 MARS。 不过，具有较复杂要求的应用程序确实需要考虑到这一点。  
  
 MARS 支持在单一连接中交错执行多个请求。 即：它允许运行批处理，并且在执行过程中还允许执行其他请求。 不过请注意，MARS 是从交错执行而不是从并行执行的角度定义的。  
  
 MARS 基础结构允许以交错方式执行多个批处理，尽管只能在定义完善的时间点切换执行。 此外，多数语句必须在同一批处理内以原子方式运行。 在将行发送到客户端时，允许向客户端返回行的语句（有时称为 " *生成点*"）在完成前交错执行，例如：  
  
-   SELECT  
  
-   FETCH  
  
-   RECEIVE  
  
 作为存储过程或批处理的一部分执行的任何其他语句必须运行完毕，之后才能切换到执行其他 MARS 请求。  
  
 多个批处理交错执行的确切方式受若干因素影响，很难预测包含收获点的多个批处理中的命令的确切执行顺序。 注意避免交错执行此类复杂批处理所产生的意外负面影响。  
  
 要避免这些问题，可使用 API 调用而不是 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句来管理连接状态（SET、USE）和事务（BEGIN TRAN、COMMIT、ROLLBACK），方法是不在同样包含收获点的多语句批处理中包括这些语句，以及通过使用或取消所有结果来顺序执行此类批处理。  
  
> [!NOTE]  
>  启用 MARS 时，启动手动或隐式事务的批处理或存储过程必须完成该事务，之后批处理才能退出。 如果不是这样，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将在批处理完成时回滚该事务所做的所有更改。 这种事务由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 作为批范围的事务管理。 这是 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 新引入的事务类型，用于在启用 MARS 时支持使用功能良好的现有存储过程。 若要详细了解批范围内的事务，请参阅[事务语句 &#40;Transact-SQL&#41;](~/t-sql/statements/statements.md)。  
  
 有关使用 ADO 的示例，请参阅将 [ado 用于 SQL Server Native Client](../../../relational-databases/native-client/applications/using-ado-with-sql-server-native-client.md)。  
  
## <a name="in-memory-oltp"></a>内存中 OLTP  
 内存中 OLTP 支持在查询和本机编译的存储过程中使用 MARS。 使用 MARS，可以请求从多个查询获取数据，而无需在发送请求从新结果集中提取行前完全检索每个结果集。 若要成功读取多个打开的结果集，必须使用启用 MARS 的连接。  
  
 MARS 默认处于禁用状态，因此必须通过将 `MultipleActiveResultSets=True` 添加到连接字符串来显式启用它。 下面的示例展示了如何连接到 SQL Server 实例，以及如何指定 MARS 已启用：  
  
```  
Data Source=MSSQL; Initial Catalog=AdventureWorks; Integrated Security=SSPI; MultipleActiveResultSets=True  
```  
  
 具有内存中 OLTP 的 MARS 与 SQL 引擎的其余部分中的 MARS 本质上是相同的。 下面列出了在内存优化表和本机编译的存储过程中使用 MARS 时的区别。  
  
 **MARS 和内存优化表**  
  
 下面列出了在使用已启用 MARS 的连接时基于磁盘的表和内存优化表的区别：  
  
-   两个语句可以修改同一个目标对象中的数据，但如果这两个语句同时尝试修改相同的记录，那么写/写冲突会导致新操作失败。 不过，如果两个操作修改不同的记录，操作就会成功。  
  
-   每个语句都是在 SNAPSHOT 隔离下运行，因此新操作看不到现有语句所做的更改。 即使并发语句在同一个事务下执行，SQL 引擎也会为相互隔离的每个语句创建批范围内的事务。 不过，批范围内的事务仍绑定在一起，因此回滚一个批范围内的事务会影响同一批中的其他事务。  
  
-   不允许在用户事务中执行 DDL 操作，因此这些操作会立即失败。  
  
 **MARS 和本机编译的存储过程**  
  
 本机编译的存储过程可以在已启用 MARS 的连接中运行，并且仅在遇到转让点时才能将执行机会转让给另一个语句。 转让点需要 SELECT 语句，它是本机编译的存储过程中唯一可以将执行机会转让给另一个语句的语句。 如果过程中没有 SELECT 语句，它就不会转让执行机会，而是一直运行完，然后其他语句才能开始执行。  
  
 **MARS 和内存中 OLTP 事务**  
  
 由交错的语句和 ATOMIC 块所做的更改相互隔离。 例如，如果一个语句或 ATOMIC 块进行了一些更改，然后将执行机会转让给另一个语句，那么新语句就看不到第一个语句所做的更改。 此外，当第一个语句继续执行时，它也看不到其他任何语句所做的任何更改。 语句只会看到在语句开始之前完成和提交的更改。  
  
 使用 BEGIN TRANSACTION 语句可以在当前用户事务中启动新的用户事务-此操作仅在互操作模式下受支持，因此只能从 T-sql 语句调用 BEGIN TRANSACTION，而不能从本机编译的存储过程中调用。您可以使用 SAVE TRANSACTION 或对 transaction 的 API 调用在事务中创建一个保存点。保存 (save_point_name) 回滚到保存点。 此功能也只能从 T-SQL 语句中启用，而不能从本机编译的存储过程中启用。  
  
 **MARS 和列存储索引**  
  
 SQL Server（自 2016 起）支持结合使用 MARS 和列存储索引。 SQL Server 2014 将 MARS 用于具有列存储索引的表的只读连接。    但是，SQL Server 2014 不支持将 MARS 用于具有列存储索引的表中的并发数据操作语言 (DML) 操作。 发生这种情况时，SQL Server 会终止连接并中止事务。   SQL Server 2012 包含的是只读列存储索引，MARS 不适用于它们。  
  
## <a name="sql-server-native-client-ole-db-provider"></a>SQL Server Native Client OLE DB 访问接口  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序通过添加 SSPROP_INIT_MARSCONNECTION 数据源初始化属性（在 DBPROPSET_SQLSERVERDBINIT 属性集中实现）来支持 MARS。 此外，还添加了新的连接字符串关键字 MarsConn  。 它接受值 true  或 false  ；默认值为 false  。  
  
 数据源属性 DBPROP_MULTIPLECONNECTIONS 默认为 VARIANT_TRUE。 这意味着访问接口将生成多个连接以支持多个并发命令和行集对象。 启用 MARS 后， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 可以在单个连接上支持多个命令和行集对象，因此默认情况下 MULTIPLE_CONNECTIONS 设置为 VARIANT_FALSE。  
  
 若要详细了解 DBPROPSET_SQLSERVERDBINIT 属性集的增强，请参阅[初始化和授权属性](../../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md)。  
  
### <a name="sql-server-native-client-ole-db-provider-example"></a>SQL Server Native Client OLE DB 访问接口示例  
 在此示例中，数据源对象是使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 本机 OLE DB 提供程序创建的，并且在创建 session 对象之前使用 DBPROPSET_SQLSERVERDBINIT 属性集启用了 MARS。  
  
```cpp
#include <sqlncli.h>  
  
IDBInitialize *pIDBInitialize = NULL;  
IDBCreateSession *pIDBCreateSession = NULL;  
IDBProperties *pIDBProperties = NULL;  
  
// Create the data source object.  
hr = CoCreateInstance(CLSID_SQLNCLI10, NULL,  
   CLSCTX_INPROC_SERVER,  
   IID_IDBInitialize,   
    (void**)&pIDBInitialize);  
  
hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void**)&pIDBProperties);  
  
// Set the MARS property.  
DBPROP rgPropMARS;  
  
// The following is necessary since MARS is off by default.  
rgPropMARS.dwPropertyID = SSPROP_INIT_MARSCONNECTION;  
rgPropMARS.dwOptions = DBPROPOPTIONS_REQUIRED;  
rgPropMARS.dwStatus = DBPROPSTATUS_OK;  
rgPropMARS.colid = DB_NULLID;  
V_VT(&(rgPropMARS.vValue)) = VT_BOOL;  
V_BOOL(&(rgPropMARS.vValue)) = VARIANT_TRUE;  
  
// Create the structure containing the properties.  
DBPROPSET PropSet;  
PropSet.rgProperties = &rgPropMARS;  
PropSet.cProperties = 1;  
PropSet.guidPropertySet = DBPROPSET_SQLSERVERDBINIT;  
  
// Get an IDBProperties pointer and set the initialization properties.  
pIDBProperties->SetProperties(1, &PropSet);  
pIDBProperties->Release();  
  
// Initialize the data source object.  
hr = pIDBInitialize->Initialize();  
  
//Create a session object from a data source object.  
IOpenRowset * pIOpenRowset = NULL;  
hr = IDBInitialize->QueryInterface(IID_IDBCreateSession, (void**)&pIDBCreateSession));  
hr = pIDBCreateSession->CreateSession(  
   NULL,             // pUnkOuter  
   IID_IOpenRowset,  // riid  
  &pIOpenRowset ));  // ppSession  
  
// Create a rowset with a firehose mode cursor.  
IRowset *pIRowset = NULL;  
DBPROP rgRowsetProperties[2];  
  
// To get a firehose mode cursor request a   
// forward only read only rowset.  
rgRowsetProperties[0].dwPropertyID = DBPROP_IRowsetLocate;  
rgRowsetProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
rgRowsetProperties[0].dwStatus = DBPROPSTATUS_OK;  
rgRowsetProperties[0].colid = DB_NULLID;  
VariantInit(&(rgRowsetProperties[0].vValue));  
rgRowsetProperties[0].vValue.vt = VARIANT_BOOL;  
rgRowsetProperties[0].vValue.boolVal = VARIANT_FALSE;  
  
rgRowsetProperties[1].dwPropertyID = DBPROP_IRowsetChange;  
rgRowsetProperties[1].dwOptions = DBPROPOPTIONS_REQUIRED;  
rgRowsetProperties[1].dwStatus = DBPROPSTATUS_OK;  
rgRowsetProperties[1].colid = DB_NULLID;  
VariantInit(&(rgRowsetProperties[1].vValue));  
rgRowsetProperties[1].vValue.vt = VARIANT_BOOL;  
rgRowsetProperties[1].vValue.boolVal = VARIANT_FALSE;  
  
DBPROPSET rgRowsetPropSet[1];  
rgRowsetPropSet[0].rgProperties = rgRowsetProperties  
rgRowsetPropSet[0].cProperties = 2  
rgRowsetPropSet[0].guidPropertySet = DBPROPSET_ROWSET;  
  
hr = pIOpenRowset->OpenRowset (NULL,  
   &TableID,  
   NULL,  
   IID_IRowset,  
   1,  
   rgRowsetPropSet  
   (IUnknown**)&pIRowset);  
```  
  
## <a name="sql-server-native-client-odbc-driver"></a>SQL Server Native Client ODBC 驱动程序  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序通过向[SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)和[SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md)函数添加内容支持 MARS。 添加了 SQL_COPT_SS_MARS_ENABLED 以接受 SQL_MARS_ENABLED_YES 或 SQL_MARS_ENABLED_NO，默认值为 SQL_MARS_ENABLED_NO。 此外，还添加了一个新的连接字符串关键字 **Mars_Connection**。 它接受值 "yes" 或 "no"；默认值为 "no"。  
  
### <a name="sql-server-native-client-odbc-driver-example"></a>SQL Server Native Client ODBC 驱动程序示例  
 在此示例中， **SQLSetConnectAttr** 函数用于在调用 **SQLDriverConnect** 函数连接数据库之前启用 MARS。 建立连接后，将调用两个 **SQLExecDirect** 函数，以便在同一连接上创建两个单独的结果集。  
  
```cpp
#include <sqlncli.h>  
  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_MARS_ENABLED, SQL_MARS_ENABLED_YES, SQL_IS_UINTEGER);  
SQLDriverConnect(hdbc, hwnd,   
   "DRIVER=SQL Server Native Client 10.0;  
   SERVER=(local);trusted_connection=yes;", SQL_NTS, szOutConn,   
   MAX_CONN_OUT, &cbOutConn, SQL_DRIVER_COMPLETE);  
  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt1);  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt2);  
  
// The 2nd execute would have failed with connection busy error if  
// MARS were not enabled.  
SQLExecDirect(hstmt1, L"SELECT * FROM Authors", SQL_NTS);  
SQLExecDirect(hstmt2, L"SELECT * FROM Titles", SQL_NTS);  
  
// Result set processing can interleave.  
SQLFetch(hstmt1);  
SQLFetch(hstmt2);  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [使用 SQL Server 默认结果集](../../../relational-databases/native-client-odbc-cursors/implementation/using-sql-server-default-result-sets.md)  
  
  
