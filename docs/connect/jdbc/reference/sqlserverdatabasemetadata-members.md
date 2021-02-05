---
description: SQLServerDatabaseMetaData 成员
title: SQLServerDatabaseMetaData 成员| Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 327ba0bc-438a-494c-b119-1cd4a096bb58
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4fe11f6a3377d0a26941138b8a581f642a6b0755
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172791"
---
# <a name="sqlserverdatabasemetadata-members"></a>SQLServerDatabaseMetaData 成员
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  下表列出了由 [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md) 类公开的成员。  
  
## <a name="constructors"></a>构造函数  
 无。  
  
## <a name="fields"></a>字段  
 无。  
  
## <a name="inherited-fields"></a>继承的字段  
  
|名称|说明|  
|----------|-----------------|  
|java.sql.DatabaseMetaData|attributeNoNulls、attributeNullable、attributeNullableUnknown、bestRowNotPseudo、bestRowPseudo、 bestRowSession、bestRowTemporary、bestRowTransaction、bestRowUnknown、columnNoNulls、columnNullable、 columnNullableUnknown、importedKeyCascade、importedKeyInitiallyDeferred、importedKeyInitiallyImmediate、 importedKeyNoAction、importedKeyNotDeferrable、importedKeyRestrict、importedKeySetDefault、 importedKeySetNull、procedureColumnIn、procedureColumnInOut、procedureColumnOut、procedureColumnResult、 procedureColumnReturn、procedureColumnUnknown、procedureNoNulls、procedureNoResult、procedureNullable、 procedureNullableUnknown、procedureResultUnknown、procedureReturnsResult、sqlStateSQL、sqlStateSQL99、 sqlStateXOpen、tableIndexClustered、tableIndexHashed、tableIndexOther、tableIndexStatistic、typeNoNulls、 typeNullable、typeNullableUnknown、typePredBasic、typePredChar、typePredNone、typeSearchable、 versionColumnNotPseudo、versionColumnPseudo、versionColumnUnknown|  
  
## <a name="methods"></a>方法  
  
|名称|说明|  
|----------|-----------------|  
|[allProceduresAreCallable](../../../connect/jdbc/reference/allproceduresarecallable-method-sqlserverdatabasemetadata.md)|检索当前用户是否有权调用 [getProcedures](../../../connect/jdbc/reference/getprocedures-method-sqlserverdatabasemetadata.md) 方法返回的所有过程。|  
|[allTablesAreSelectable](../../../connect/jdbc/reference/alltablesareselectable-method-sqlserverdatabasemetadata.md)|检索当前用户是否有权使用 SELECT 语句中的 [getTables](../../../connect/jdbc/reference/gettables-method-sqlserverdatabasemetadata.md) 方法返回的所有表。|  
|[autoCommitFailureClosesAllResultSets](../../../connect/jdbc/reference/autocommitfailureclosesallresultsets-method-sqlserverdatabasemetadata.md)|启用自动提交并引发异常时，指示 JDBC 驱动程序是否关闭所有打开的结果集，包括可保持的结果集。|  
|[dataDefinitionCausesTransactionCommit](../../../connect/jdbc/reference/datadefinitioncausestransactioncommit-method-sqlserverdatabasemetadata.md)|检索事务内的数据定义语句是否强制事务提交。|  
|[dataDefinitionIgnoredInTransactions](../../../connect/jdbc/reference/datadefinitionignoredintransactions-method-sqlserverdatabasemetadata.md)|检索此数据库是否忽略事务内的数据定义语句。|  
|[deletesAreDetected](../../../connect/jdbc/reference/deletesaredetected-method-sqlserverdatabasemetadata.md)|检索是否可通过调用 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 类的 [rowDeleted](../../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md) 方法检测到可见行删除。|  
|[doesMaxRowSizeIncludeBlobs](../../../connect/jdbc/reference/doesmaxrowsizeincludeblobs-method-sqlserverdatabasemetadata.md)|检索 [getMaxRowSize](../../../connect/jdbc/reference/getmaxrowsize-method-sqlserverdatabasemetadata.md) 方法的返回值是否包括 SQL 数据类型 LONGVARCHAR 和 LONGVARBINARY。|  
|[getAttributes](../../../connect/jdbc/reference/getattributes-method-sqlserverdatabasemetadata.md)|检索给定架构和目录中可用的用户定义类型的给定类型的给定属性的说明。|  
|[getBestRowIdentifier](../../../connect/jdbc/reference/getbestrowidentifier-method-sqlserverdatabasemetadata.md)|检索表中唯一标识一行的最佳列集的说明。|  
|[getCatalogs](../../../connect/jdbc/reference/getcatalogs-method-sqlserverdatabasemetadata.md)|检索在连接的服务器中可用的目录名称。|  
|[getCatalogSeparator](../../../connect/jdbc/reference/getcatalogseparator-method-sqlserverdatabasemetadata.md)|检索此数据库用作目录和表名之间的分隔符的字符串。|  
|[getCatalogTerm](../../../connect/jdbc/reference/getcatalogterm-method-sqlserverdatabasemetadata.md)|检索数据库供应商有关“目录”的首选术语。|  
|[getClientInfoProperties](../../../connect/jdbc/reference/getclientinfoproperties-method-sqlserverdatabasemetadata.md)|检索驱动程序支持的客户端信息属性的列表。|  
|[getColumnPrivileges](../../../connect/jdbc/reference/getcolumnprivileges-method-sqlserverdatabasemetadata.md)|检索关于表中各列的访问权限的说明。|  
|[getColumns](../../../connect/jdbc/reference/getcolumns-method-sqlserverdatabasemetadata.md)|检索指定目录中可用的表列的说明。|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatabasemetadata.md)|检索生成此元数据对象的连接。|  
|[getCrossReference](../../../connect/jdbc/reference/getcrossreference-method-sqlserverdatabasemetadata.md)|检索给定外键表中的外键列的说明，该外键表引用给定主键表的主键列。|  
|[getDatabaseMajorVersion](../../../connect/jdbc/reference/getdatabasemajorversion-method-sqlserverdatabasemetadata.md)|检索基础数据库的主版本号。|  
|[getDatabaseMinorVersion](../../../connect/jdbc/reference/getdatabaseminorversion-method-sqlserverdatabasemetadata.md)|检索基础数据库的次版本号。|  
|[getDatabaseProductName](../../../connect/jdbc/reference/getdatabaseproductname-method-sqlserverdatabasemetadata.md)|检索此数据库产品的名称。|  
|[getDatabaseProductVersion](../../../connect/jdbc/reference/getdatabaseproductversion-method-sqlserverdatabasemetadata.md)|检索此数据库产品的版本号。|  
|[getDefaultTransactionIsolation](../../../connect/jdbc/reference/getdefaulttransactionisolation-method-sqlserverdatabasemetadata.md)|检索此数据库的默认事务隔离级别。|  
|[getDriverMajorVersion](../../../connect/jdbc/reference/getdrivermajorversion-method-sqlserverdatabasemetadata.md)|检索此 JDBC 驱动程序的主版本号。|  
|[getDriverMinorVersion](../../../connect/jdbc/reference/getdriverminorversion-method-sqlserverdatabasemetadata.md)|检索此 JDBC 驱动程序的次版本号。|  
|[getDriverName](../../../connect/jdbc/reference/getdrivername-method-sqlserverdatabasemetadata.md)|检索此 JDBC 驱动程序的名称。|  
|[getDriverVersion](../../../connect/jdbc/reference/getdriverversion-method-sqlserverdatabasemetadata.md)|检索此 JDBC 驱动程序的版本号。|  
|[getExportedKeys](../../../connect/jdbc/reference/getexportedkeys-method-sqlserverdatabasemetadata.md)|检索引用给定表主键列的外键列说明。|  
|[getExtraNameCharacters](../../../connect/jdbc/reference/getextranamecharacters-method-sqlserverdatabasemetadata.md)|检索可以用于未加引号的标识符名称的所有其他字符，例如在 a-z、A-Z、0-9 和 _ 之外的字符。|  
|[getFunctions](../../../connect/jdbc/reference/getfunctions-method-sqlserverdatabasemetadata.md)|检索系统函数和用户函数的说明。|  
|[getFunctionColumns](../../../connect/jdbc/reference/getfunctioncolumns-method-sqlserverdatabasemetadata.md)|检索关于指定目录的系统函数或用户函数参数和返回类型的说明。|  
|[getIdentifierQuoteString](../../../connect/jdbc/reference/getidentifierquotestring-method-sqlserverdatabasemetadata.md)|检索用于为 SQL 标识符加引号的字符串。|  
|[getImportedKeys](../../../connect/jdbc/reference/getimportedkeys-method-sqlserverdatabasemetadata.md)|检索由表的外键列引用的主键列的说明。|  
|[getIndexInfo](../../../connect/jdbc/reference/getindexinfo-method-sqlserverdatabasemetadata.md)|检索给定表的索引和统计信息的说明。|  
|[getJDBCMajorVersion](../../../connect/jdbc/reference/getjdbcmajorversion-method-sqlserverdatabasemetadata.md)|检索此驱动程序的 JDBC 主版本号。|  
|[getJDBCMinorVersion](../../../connect/jdbc/reference/getjdbcminorversion-method-sqlserverdatabasemetadata.md)|检索此驱动程序的 JDBC 次版本号。|  
|[getMaxBinaryLiteralLength](../../../connect/jdbc/reference/getmaxbinaryliterallength-method-sqlserverdatabasemetadata.md)|检索此数据库在内联二进制文本中允许的最大十六进制字符数。|  
|[getMaxCatalogNameLength](../../../connect/jdbc/reference/getmaxcatalognamelength-method-sqlserverdatabasemetadata.md)|检索此数据库在目录名称中允许的最大字符数。|  
|[getMaxCharLiteralLength](../../../connect/jdbc/reference/getmaxcharliterallength-method-sqlserverdatabasemetadata.md)|检索此数据库在字符文本中允许的最大字符数。|  
|[getMaxColumnNameLength](../../../connect/jdbc/reference/getmaxcolumnnamelength-method-sqlserverdatabasemetadata.md)|检索此数据库在列名中允许的最大字符数。|  
|[getMaxColumnsInGroupBy](../../../connect/jdbc/reference/getmaxcolumnsingroupby-method-sqlserverdatabasemetadata.md)|检索此数据库在 GROUP BY 子句中允许的最大列数。|  
|[getMaxColumnsInIndex](../../../connect/jdbc/reference/getmaxcolumnsinindex-method-sqlserverdatabasemetadata.md)|检索此数据库在索引中允许的最大列数。|  
|[getMaxColumnsInOrderBy](../../../connect/jdbc/reference/getmaxcolumnsinorderby-method-sqlserverdatabasemetadata.md)|检索此数据库在 ORDER BY 子句中允许的最大列数。|  
|[getMaxColumnsInSelect](../../../connect/jdbc/reference/getmaxcolumnsinselect-method-sqlserverdatabasemetadata.md)|检索此数据库在 SELECT 列表中允许的最大列数。|  
|[getMaxColumnsInTable](../../../connect/jdbc/reference/getmaxcolumnsintable-method-sqlserverdatabasemetadata.md)|检索此数据库在表中允许的最大列数。|  
|[getMaxConnections](../../../connect/jdbc/reference/getmaxconnections-method-sqlserverdatabasemetadata.md)|检索可能连接到此数据库的最大并发连接数。|  
|[getMaxCursorNameLength](../../../connect/jdbc/reference/getmaxcursornamelength-method-sqlserverdatabasemetadata.md)|检索此数据库在游标名称中允许的最大字符数。|  
|[getMaxIndexLength](../../../connect/jdbc/reference/getmaxindexlength-method-sqlserverdatabasemetadata.md)|检索此数据库允许的最大索引（包括索引的所有部分）字节数。|  
|[getMaxProcedureNameLength](../../../connect/jdbc/reference/getmaxprocedurenamelength-method-sqlserverdatabasemetadata.md)|检索此数据库在过程名称中允许的最大字符数。|  
|[getMaxRowSize](../../../connect/jdbc/reference/getmaxrowsize-method-sqlserverdatabasemetadata.md)|检索此数据库在单行中允许的最大字节数。|  
|[getMaxSchemaNameLength](../../../connect/jdbc/reference/getmaxschemanamelength-method-sqlserverdatabasemetadata.md)|检索此数据库在架构名称中允许的最大字符数。|  
|[getMaxStatementLength](../../../connect/jdbc/reference/getmaxstatementlength-method-sqlserverdatabasemetadata.md)|检索此数据库允许的 SQL 语句字符数上限。|  
|[getMaxStatements](../../../connect/jdbc/reference/getmaxstatements-method-sqlserverdatabasemetadata.md)|检索此数据库可同时打开的活动语句的最大数目。|  
|[getMaxTableNameLength](../../../connect/jdbc/reference/getmaxtablenamelength-method-sqlserverdatabasemetadata.md)|检索此数据库在表名中允许的最大字符数。|  
|[getMaxTablesInSelect](../../../connect/jdbc/reference/getmaxtablesinselect-method-sqlserverdatabasemetadata.md)|检索此数据库允许的 SELECT 语句中的最大表数。|  
|[getMaxUserNameLength](../../../connect/jdbc/reference/getmaxusernamelength-method-sqlserverdatabasemetadata.md)|检索此数据库在用户名中允许的最大字符数。|  
|[getNumericFunctions](../../../connect/jdbc/reference/getnumericfunctions-method-sqlserverdatabasemetadata.md)|检索可用于此数据库的以逗号分隔的数学函数列表。|  
|[getPrimaryKeys](../../../connect/jdbc/reference/getprimarykeys-method-sqlserverdatabasemetadata.md)|检索给定表的主键列的说明。|  
|[getProcedureColumns](../../../connect/jdbc/reference/getprocedurecolumns-method-sqlserverdatabasemetadata.md)|检索存储过程参数和结果列的说明。|  
|[getProcedures](../../../connect/jdbc/reference/getprocedures-method-sqlserverdatabasemetadata.md)|检索在给定目录、架构或存储过程名称模式中可用的存储过程的说明。|  
|[getProcedureTerm](../../../connect/jdbc/reference/getprocedureterm-method-sqlserverdatabasemetadata.md)|检索与此数据库中的“过程”对应的首选术语。|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverdatabasemetadata.md)|检索此数据库的结果集的默认保持能力。|  
|[getRowIdLifetime](../../../connect/jdbc/reference/getrowidlifetime-method-sqlserverdatabasemetadata.md)|返回一种状态，该状态指示 SQL RowId 数据类型是否受支持。 如果受支持，则返回 RowId 对象保持有效的生存期。|  
|[getSchemas](../../../connect/jdbc/reference/getschemas-method.md)|检索当前数据库中可用的架构名称。|  
|[getSchemaTerm](../../../connect/jdbc/reference/getschematerm-method-sqlserverdatabasemetadata.md)|检索与此数据库中的“架构”对应的首选术语。|  
|[getSearchStringEscape](../../../connect/jdbc/reference/getsearchstringescape-method-sqlserverdatabasemetadata.md)|检索可用于转义通配符的 String 值。|  
|[getSQLKeywords](../../../connect/jdbc/reference/getsqlkeywords-method-sqlserverdatabasemetadata.md)|检索此数据库的所有 SQL 关键字（但并非 SQL92 关键字）的以逗号分隔的列表。|  
|[getSQLStateType](../../../connect/jdbc/reference/getsqlstatetype-method-sqlserverdatabasemetadata.md)|指示 SQLException.getSQLState 方法返回的 SQLSTATE 是否为 X/Open（现称为 Open Group）、SQL CLI、SQL99 (JDBC 3.0) 或 SQL:2003 (JDBC 4.0)。|  
|[getStringFunctions](../../../connect/jdbc/reference/getstringfunctions-method-sqlserverdatabasemetadata.md)|检索可用于此数据库的 String 函数以逗号分隔的列表。|  
|[getSuperTables](../../../connect/jdbc/reference/getsupertables-method-sqlserverdatabasemetadata.md)|检索在此数据库的特定架构中定义的表层次结构的说明。|  
|[getSuperTypes](../../../connect/jdbc/reference/getsupertypes-method-sqlserverdatabasemetadata.md)|检索此数据库的特定架构中由用户定义的类型层次结构的说明。|  
|[getSystemFunctions](../../../connect/jdbc/reference/getsystemfunctions-method-sqlserverdatabasemetadata.md)|检索可用于此数据库的系统函数的以逗号分隔的列表。|  
|[getTablePrivileges](../../../connect/jdbc/reference/gettableprivileges-method-sqlserverdatabasemetadata.md)|检索可用于给定目录、架构或表名称模式的各表的访问权限的说明。|  
|[getTables](../../../connect/jdbc/reference/gettables-method-sqlserverdatabasemetadata.md)|检索可用于给定目录、架构或表名称模式的各表的说明。|  
|[getTableTypes](../../../connect/jdbc/reference/gettabletypes-method-sqlserverdatabasemetadata.md)|检索当前数据库中可用的表类型。|  
|[getTimeDateFunctions](../../../connect/jdbc/reference/gettimedatefunctions-method-sqlserverdatabasemetadata.md)|检索可用于此数据库的时间和日期函数的以逗号分隔的列表。|  
|[getTypeInfo](../../../connect/jdbc/reference/gettypeinfo-method-sqlserverdatabasemetadata.md)|检索当前数据库支持的所有标准 SQL 类型的说明。|  
|[getUDTs](../../../connect/jdbc/reference/getudts-method-sqlserverdatabasemetadata.md)|检索特定架构中定义的由用户定义的类型的说明。|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatabasemetadata.md)|检索此数据库的 URL。|  
|[getUserName](../../../connect/jdbc/reference/getusername-method-sqlserverdatabasemetadata.md)|检索此数据库可识别的用户名。|  
|[getVersionColumns](../../../connect/jdbc/reference/getversioncolumns-method-sqlserverdatabasemetadata.md)|检索在某行内的任何值更新时会随之自动更新的表列的说明。|  
|[insertsAreDetected](../../../connect/jdbc/reference/insertsaredetected-method-sqlserverdatabasemetadata.md)|检索是否可通过调用 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 类的 [rowInserted](../../../connect/jdbc/reference/rowinserted-method-sqlserverresultset.md) 方法检测可见的行插入。|  
|[isCatalogAtStart](../../../connect/jdbc/reference/iscatalogatstart-method-sqlserverdatabasemetadata.md)|检索目录是否出现在完全限定表名的开始位置。|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverdatabasemetadata.md)|检索此数据库是否处于只读模式。|  
|[locatorsUpdateCopy](../../../connect/jdbc/reference/locatorsupdatecopy-method-sqlserverdatabasemetadata.md)|指示是将对 LOB 的更新应用到副本还是直接应用到 LOB。|  
|[nullPlusNonNullIsNull](../../../connect/jdbc/reference/nullplusnonnullisnull-method-sqlserverdatabasemetadata.md)|指示此数据库是否支持 NULL 值与非 NULL 值串联为 NULL。|  
|[nullsAreSortedAtEnd](../../../connect/jdbc/reference/nullsaresortedatend-method-sqlserverdatabasemetadata.md)|检索 NULL 值是否始终排在最后，无论排序顺序如何。|  
|[nullsAreSortedAtStart](../../../connect/jdbc/reference/nullsaresortedatstart-method-sqlserverdatabasemetadata.md)|检索 NULL 值是否始终排在最前，无论排序顺序如何。|  
|[nullsAreSortedHigh](../../../connect/jdbc/reference/nullsaresortedhigh-method-sqlserverdatabasemetadata.md)|检索 NULL 值是否在排序中位置较高。|  
|[nullsAreSortedLow](../../../connect/jdbc/reference/nullsaresortedlow-method-sqlserverdatabasemetadata.md)|检索 NULL 值是否在排序中位置较低。|  
|[othersDeletesAreVisible](../../../connect/jdbc/reference/othersdeletesarevisible-method-sqlserverdatabasemetadata.md)|检索由其他人所做删除是否可见。|  
|[othersInsertsAreVisible](../../../connect/jdbc/reference/othersinsertsarevisible-method-sqlserverdatabasemetadata.md)|检索其他人执行的插入操作是否可见。|  
|[othersUpdatesAreVisible](../../../connect/jdbc/reference/othersupdatesarevisible-method-sqlserverdatabasemetadata.md)|检索其他人执行的更新操作是否可见。|  
|[ownDeletesAreVisible](../../../connect/jdbc/reference/owndeletesarevisible-method-sqlserverdatabasemetadata.md)|检索结果集自身的删除是否可见。|  
|[ownInsertsAreVisible](../../../connect/jdbc/reference/owninsertsarevisible-method-sqlserverdatabasemetadata.md)|检索结果集自身的插入是否可见。|  
|[ownUpdatesAreVisible](../../../connect/jdbc/reference/ownupdatesarevisible-method-sqlserverdatabasemetadata.md)|检索结果集自身的更新是否可见。|  
|[storesLowerCaseIdentifiers](../../../connect/jdbc/reference/storeslowercaseidentifiers-method-sqlserverdatabasemetadata.md)|检索此数据库是否将未用双引号引起来的大小写混合的 SQL 标识符视为不区分大小写，并以小写方式存储它们。|  
|[storesLowerCaseQuotedIdentifiers](../../../connect/jdbc/reference/storeslowercasequotedidentifiers-method-sqlserverdatabasemetadata.md)|检索此数据库是否将用双引号引起来的大小写混合的 SQL 标识符视为不区分大小写，并以小写方式存储它们。|  
|[storesMixedCaseIdentifiers](../../../connect/jdbc/reference/storesmixedcaseidentifiers-method-sqlserverdatabasemetadata.md)|检索此数据库是否将未用双引号引起来的大小写混合的 SQL 标识符视为不区分大小写，并以混合大小写方式存储它们。|  
|[storesMixedCaseQuotedIdentifiers](../../../connect/jdbc/reference/storesmixedcasequotedidentifiers-method-sqlserverdatabasemetadata.md)|检索此数据库是否将用双引号引起来的大小写混合的 SQL 标识符视为不区分大小写，并以混合大小写方式存储它们。|  
|[storesUpperCaseIdentifiers](../../../connect/jdbc/reference/storesuppercaseidentifiers-method-sqlserverdatabasemetadata.md)|检索此数据库是否将未用双引号引起来的大小写混合的 SQL 标识符视为不区分大小写，并以大写方式存储它们。|  
|[storesUpperCaseQuotedIdentifiers](../../../connect/jdbc/reference/storesuppercasequotedidentifiers-method-sqlserverdatabasemetadata.md)|检索此数据库是否将用双引号引起来的大小写混合的 SQL 标识符视为不区分大小写，并以大写方式存储它们。|  
|[supportsAlterTableWithAddColumn](../../../connect/jdbc/reference/supportsaltertablewithaddcolumn-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持带有添加列的 ALTER TABLE。|  
|[supportsAlterTableWithDropColumn](../../../connect/jdbc/reference/supportsaltertablewithdropcolumn-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持带有删除列的 ALTER TABLE。|  
|[supportsANSI92EntryLevelSQL](../../../connect/jdbc/reference/supportsansi92entrylevelsql-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 ANSI92 入门级 SQL 语法。|  
|[supportsANSI92FullSQL](../../../connect/jdbc/reference/supportsansi92fullsql-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 ANSI92 完整 SQL 语法。|  
|[supportsANSI92IntermediateSQL](../../../connect/jdbc/reference/supportsansi92intermediatesql-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 ANSI92 中级 SQL 语法。|  
|[supportsBatchUpdates](../../../connect/jdbc/reference/supportsbatchupdates-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持批更新。|  
|[supportsCatalogsInDataManipulation](../../../connect/jdbc/reference/supportscatalogsindatamanipulation-method-sqlserverdatabasemetadata.md)|检索目录名称能否用于数据操作语句。|  
|[supportsCatalogsInIndexDefinitions](../../../connect/jdbc/reference/supportscatalogsinindexdefinitions-method-sqlserverdatabasemetadata.md)|检索目录名称能否用于索引定义语句。|  
|[supportsCatalogsInPrivilegeDefinitions](../../../connect/jdbc/reference/supportscatalogsinprivilegedefinitions-method-sqlserverdatabasemetadata.md)|检索目录名称能否用于特权定义语句。|  
|[supportsCatalogsInProcedureCalls](../../../connect/jdbc/reference/supportscatalogsinprocedurecalls-method-sqlserverdatabasemetadata.md)|检索目录名称能否用于过程调用语句。|  
|[supportsCatalogsInTableDefinitions](../../../connect/jdbc/reference/supportscatalogsintabledefinitions-method-sqlserverdatabasemetadata.md)|检索目录名称能否用于表定义语句。|  
|[supportsColumnAliasing](../../../connect/jdbc/reference/supportscolumnaliasing-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持列名使用别名。|  
|[supportsConvert](../../../connect/jdbc/reference/supportsconvert-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持在 SQL 类型之间使用 CONVERT 函数。|  
|[supportsCoreSQLGrammar](../../../connect/jdbc/reference/supportscoresqlgrammar-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 ODBC 核心 SQL 语法。|  
|[supportsCorrelatedSubqueries](../../../connect/jdbc/reference/supportscorrelatedsubqueries-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持相关子查询。|  
|[supportsDataDefinitionAndDataManipulationTransactions](../../../connect/jdbc/reference/supportsdatadefinitionanddatamanipulationtransactions-method.md)|检索此数据库是否可在一个事务内同时支持数据定义和数据操作语句。|  
|[supportsDataManipulationTransactionsOnly](../../../connect/jdbc/reference/supportsdatamanipulationtransactionsonly-method-sqlserverdatabasemetadata.md)|检索此数据库是否在一个事务内仅支持数据操作语句。|  
|[supportsDifferentTableCorrelationNames](../../../connect/jdbc/reference/supportsdifferenttablecorrelationnames-method-sqlserverdatabasemetadata.md)|检索在支持表相关名称时，这些名称是否必须与表名不同。|  
|[supportsExpressionsInOrderBy](../../../connect/jdbc/reference/supportsexpressionsinorderby-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 ORDER BY 列表中的表达式。|  
|[supportsExtendedSQLGrammar](../../../connect/jdbc/reference/supportsextendedsqlgrammar-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 ODBC 扩展的 SQL 语法。|  
|[supportsFullOuterJoins](../../../connect/jdbc/reference/supportsfullouterjoins-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持完整的嵌套外部联接。|  
|[supportsGetGeneratedKeys](../../../connect/jdbc/reference/supportsgetgeneratedkeys-method-sqlserverdatabasemetadata.md)|检索在执行某语句之后可否检索自动生成的键。|  
|[supportsGroupBy](../../../connect/jdbc/reference/supportsgroupby-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持某种形式的 GROUP BY 子句。|  
|[supportsGroupByBeyondSelect](../../../connect/jdbc/reference/supportsgroupbybeyondselect-method-sqlserverdatabasemetadata.md)|检索在 SELECT 语句中的所有列均包含在 GROUP BY 子句中的情况下，此数据库是否支持使用 GROUP BY 子句中的 SELECT 语句不包含的列。|  
|[supportsGroupByUnrelated](../../../connect/jdbc/reference/supportsgroupbyunrelated-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持使用 GROUP BY 子句中的 SELECT 语句不包含的列。|  
|[supportsIntegrityEnhancementFacility](../../../connect/jdbc/reference/supportsintegrityenhancementfacility-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 SQL 完整性增强功能。|  
|[supportsLikeEscapeClause](../../../connect/jdbc/reference/supportslikeescapeclause-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持指定 LIKE 转义子句。|  
|[supportsLimitedOuterJoins](../../../connect/jdbc/reference/supportslimitedouterjoins-method-sqlserverdatabasemetadata.md)|检索此数据库是否可为外部联接提供有限支持。|  
|[supportsMinimumSQLGrammar](../../../connect/jdbc/reference/supportsminimumsqlgrammar-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 ODBC 最小 SQL 语法。|  
|[supportsMixedCaseIdentifiers](../../../connect/jdbc/reference/supportsmixedcaseidentifiers-method-sqlserverdatabasemetadata.md)|检索此数据库是否将未用双引号引起来的大小写混合的 SQL 标识符视为不区分大小写，并以混合大小写方式存储它们。|  
|[supportsMixedCaseQuotedIdentifiers](../../../connect/jdbc/reference/supportsmixedcasequotedidentifiers-method-sqlserverdatabasemetadata.md)|检索此数据库是否将用双引号引起来的大小写混合的 SQL 标识符视为不区分大小写，并以混合大小写方式存储它们。|  
|[supportsMultipleOpenResults](../../../connect/jdbc/reference/supportsmultipleopenresults-method-sqlserverdatabasemetadata.md)|检索 [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) 对象能否同时返回多个 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。|  
|[supportsMultipleResultSets](../../../connect/jdbc/reference/supportsmultipleresultsets-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持通过一次调用 [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) 类的 [execute](../../../connect/jdbc/reference/execute-method.md) 方法获取多个 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。|  
|[supportsMultipleTransactions](../../../connect/jdbc/reference/supportsmultipletransactions-method-sqlserverdatabasemetadata.md)|检索此数据库是否允许在不同连接上同时打开多个事务。|  
|[supportsNamedParameters](../../../connect/jdbc/reference/supportsnamedparameters-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持可调用语句中的命名参数。|  
|[supportsNonNullableColumns](../../../connect/jdbc/reference/supportsnonnullablecolumns-method-sqlserverdatabasemetadata.md)|检索此数据库中的列是否可定义为不可为 Null 的值。|  
|[supportsOpenCursorsAcrossCommit](../../../connect/jdbc/reference/supportsopencursorsacrosscommit-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持提交时保持打开游标。|  
|[supportsOpenCursorsAcrossRollback](../../../connect/jdbc/reference/supportsopencursorsacrossrollback-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持回滚时保持打开游标。|  
|[supportsOpenStatementsAcrossCommit](../../../connect/jdbc/reference/supportsopenstatementsacrosscommit-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持提交时保持打开语句。|  
|[supportsOpenStatementsAcrossRollback](../../../connect/jdbc/reference/supportsopenstatementsacrossrollback-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持回滚时保持打开语句。|  
|[supportsOrderByUnrelated](../../../connect/jdbc/reference/supportsorderbyunrelated-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持使用 ORDER BY 子句中的 SELECT 语句不包含的列。|  
|[supportsOuterJoins](../../../connect/jdbc/reference/supportsouterjoins-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持某种形式的外部联接。|  
|[supportsPositionedDelete](../../../connect/jdbc/reference/supportspositioneddelete-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持定位的 DELETE 语句。|  
|[supportsPositionedUpdate](../../../connect/jdbc/reference/supportspositionedupdate-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持定位的 UPDATE 语句。|  
|[supportsResultSetConcurrency](../../../connect/jdbc/reference/supportsresultsetconcurrency-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持与给定结果集类型结合的给定并发类型。|  
|[supportsResultSetHoldability](../../../connect/jdbc/reference/supportsresultsetholdability-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持给定结果集保持能力。|  
|[supportsResultSetType](../../../connect/jdbc/reference/supportsresultsettype-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持给定结果集类型。|  
|[supportsSavepoints](../../../connect/jdbc/reference/supportssavepoints-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持保存点。|  
|[supportsSchemasInDataManipulation](../../../connect/jdbc/reference/supportsschemasindatamanipulation-method-sqlserverdatabasemetadata.md)|检索架构名称能否用于数据操作语句。|  
|[supportsSchemasInIndexDefinitions](../../../connect/jdbc/reference/supportsschemasinindexdefinitions-method-sqlserverdatabasemetadata.md)|检索架构名称能否用于索引定义语句。|  
|[supportsSchemasInPrivilegeDefinitions](../../../connect/jdbc/reference/supportsschemasinprivilegedefinitions-method-sqlserverdatabasemetadata.md)|检索架构名称能否用于特权定义语句。|  
|[supportsSchemasInProcedureCalls](../../../connect/jdbc/reference/supportsschemasinprocedurecalls-method-sqlserverdatabasemetadata.md)|检索架构名称能否用于过程调用语句。|  
|[supportsSchemasInTableDefinitions](../../../connect/jdbc/reference/supportsschemasintabledefinitions-method-sqlserverdatabasemetadata.md)|检索架构名称能否用于表定义语句。|  
|[supportsSelectForUpdate](../../../connect/jdbc/reference/supportsselectforupdate-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 SELECT FOR UPDATE 语句。|  
|[supportsStatementPooling](../../../connect/jdbc/reference/supportsstatementpooling-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持语句池。|  
|[supportsStoredFunctionsUsingCallSyntax](../../../connect/jdbc/reference/supportsstoredfunctionsusingcallsyntax-method-sqlserverdatabasemetadata.md)|指示当前数据库是否支持通过使用存储过程转义语法调用用户或供应商定义的函数。|  
|[supportsStoredProcedures](../../../connect/jdbc/reference/supportsstoredprocedures-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持使用存储过程转义语法的存储过程调用。|  
|[supportsSubqueriesInComparisons](../../../connect/jdbc/reference/supportssubqueriesincomparisons-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持比较表达式中的子查询。|  
|[supportsSubqueriesInExists](../../../connect/jdbc/reference/supportssubqueriesinexists-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 EXISTS 表达式中的子查询。|  
|[supportsSubqueriesInIns](../../../connect/jdbc/reference/supportssubqueriesinins-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 IN 语句中的子查询。|  
|[supportsSubqueriesInQuantifieds](../../../connect/jdbc/reference/supportssubqueriesinquantifieds-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持限定表达式中的子查询。|  
|[supportsTableCorrelationNames](../../../connect/jdbc/reference/supportstablecorrelationnames-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持表相关名称。|  
|[supportsTransactionIsolationLevel](../../../connect/jdbc/reference/supportstransactionisolationlevel-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持给定事务隔离级别。|  
|[supportsTransactions](../../../connect/jdbc/reference/supportstransactions-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持事务。|  
|[supportsUnion](../../../connect/jdbc/reference/supportsunion-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 SQL UNION。|  
|[supportsUnionAll](../../../connect/jdbc/reference/supportsunionall-method-sqlserverdatabasemetadata.md)|检索此数据库是否支持 SQL UNION ALL。|  
|[updatesAreDetected](../../../connect/jdbc/reference/updatesaredetected-method-sqlserverdatabasemetadata.md)|检索是否可通过调用 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 类的 [rowUpdated](../../../connect/jdbc/reference/rowupdated-method-sqlserverresultset.md) 方法检测到可见行更新。|  
|[usesLocalFilePerTable](../../../connect/jdbc/reference/useslocalfilepertable-method-sqlserverdatabasemetadata.md)|检索此数据库是否为每个表使用一个文件。|  
|[usesLocalFiles](../../../connect/jdbc/reference/useslocalfiles-method-sqlserverdatabasemetadata.md)|检索此数据库是否将表存储在本地文件中。|  
  
## <a name="inherited-methods"></a>继承的方法  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|java.lang.Object|clone、equals、finalize、getClass、hashCode、notify、notifyAll、toString、wait|  
|java.sql.Wrapper|isWrapperFor、unwrap|  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
