---
description: Microsoft Jet 的 microsoft OLE DB 提供程序概述
title: Microsoft Jet 的 microsoft OLE DB 提供程序 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Jet provider for OLE DB [ADO]
- providers [ADO], OLE DB provider for Microsoft Jet
- OLE DB provider for Microsoft Jet [ADO]
ms.assetid: fd956da1-5203-40af-aa7e-fc13a6c6581f
author: rothja
ms.author: jroth
ms.openlocfilehash: 87d9e159a9a97a0a929bec72d03118a927aad56e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029221"
---
# <a name="microsoft-ole-db-provider-for-microsoft-jet-overview"></a>Microsoft Jet 的 microsoft OLE DB 提供程序概述
Microsoft Jet 的 OLE DB 提供程序允许 ADO 访问 Microsoft Jet 数据库。

## <a name="connection-string-parameters"></a>连接字符串参数
 若要连接到该提供程序，请将 [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md)属性的 *provider* 参数设置为以下属性：

```vb
Microsoft.Jet.OLEDB.4.0
```

 读取 [提供程序](../../reference/ado-api/provider-property-ado.md) 属性也会返回此字符串。

## <a name="typical-connection-string"></a>典型连接字符串
 此提供程序的典型连接字符串是：

```vb
"Provider=Microsoft.Jet.OLEDB.4.0;Data Source=databaseName;User ID=MyUserID;Password=MyPassword;"
```

 字符串包含以下关键字：

|关键字|说明|
|-------------|-----------------|
|**提供程序**|指定 Microsoft Jet 的 OLE DB 提供程序。|
|**数据源**|指定数据库路径和文件名 (例如 `c:\Northwind.mdb`) 。|
|**用户 ID**|指定用户名。 如果未指定此关键字，则 `admin` 默认情况下使用字符串 ""。|
|**密码**|指定用户密码。 如果未指定此关键字，则默认情况下将使用空字符串 ( "" ) 。|

> [!NOTE]
>  如果要连接到支持 Windows 身份验证的数据源提供程序，应在连接字符串中指定 **Trusted_Connection = yes** 或 **集成安全性 = SSPI** 而不是用户 ID 和密码信息。

## <a name="provider-specific-connection-parameters"></a>Provider-Specific 连接参数
 除 ADO 定义的属性外，Microsoft Jet 的 OLE DB 提供程序还支持多个特定于提供程序的动态属性。 与所有其他 **连接** 参数一样，可以使用 **连接** 对象的 **Properties** 集合或连接字符串的一部分来设置这些参数。

 下表列出了这些属性，并在括号中列出了相应的 OLE DB 属性名称。

|参数|说明|
|---------------|-----------------|
|Jet OLEDB：压缩回收的空间量 (DBPROP_JETOLEDB_COMPACTFREESPACESIZE) |指示可通过压缩数据库来回收的空间量的估计值（以字节为单位）。 只有建立了数据库连接后，此值才有效。|
|Jet OLEDB：连接控制 (DBPROP_JETOLEDB_CONNECTIONCONTROL) |指示用户是否可以连接到数据库。|
|Jet OLEDB：创建系统数据库 (DBPROP_JETOLEDB_CREATESYSTEMDATABASE) |指示创建新的数据源时是否应创建系统数据库。|
|Jet OLEDB：数据库锁定模式 (DBPROP_JETOLEDB_DATABASELOCKMODE) |指示此数据库的锁定模式。 打开数据库的第一个用户确定数据库打开时使用的模式。|
|Jet OLEDB：数据库密码 (DBPROP_JETOLEDB_DATABASEPASSWORD) |指示数据库密码。|
|Jet OLEDB：不要 DBPROP_JETOLEDB_COMPACT_DONTCOPYLOCALE Compact (上复制区域设置) |指示在压缩数据库时 Jet 是否应复制区域设置信息。|
|Jet OLEDB：加密数据库 (DBPROP_JETOLEDB_ENCRYPTDATABASE) |指示是否应对压缩的数据库进行加密。 如果未设置此属性，则在原始数据库也已加密的情况下，压缩的数据库将被加密。|
|Jet OLEDB：引擎类型 (DBPROP_JETOLEDB_ENGINE) |指示用于访问当前数据存储的存储引擎。|
|Jet OLEDB：独占异步延迟 (DBPROP_JETOLEDB_EXCLUSIVEASYNCDELAY) |表示以毫秒为单位的最大时间长度，当数据库以独占方式打开时，Jet 可以将异步写入延迟到磁盘中。<br /><br /> 除非 **JET OLEDB：刷新事务超时** 设置为0，否则将忽略此属性。|
|Jet OLEDB：刷新事务超时 (DBPROP_JETOLEDB_FLUSHTRANSACTIONTIMEOUT) |指示在缓存中存储用于异步写入的数据写入磁盘之前要等待的时间量。 此设置将替代 " **JET oledb：共享异步延迟** " 和 " **Jet Oledb：独占异步延迟**" 的值。|
|Jet OLEDB：全局批量事务 (DBPROP_JETOLEDB_GLOBALBULKNOTRANSACTIONS) |指示是否对 SQL 大容量事务进行事务处理。|
|Jet OLEDB：全局部分大容量 Ops (DBPROP_JETOLEDB_GLOBALBULKPARTIAL) |指示用于打开数据库的密码。|
|Jet OLEDB：隐式提交同步 (DBPROP_JETOLEDB_IMPLICITCOMMITSYNC) |指示是否在同步或异步模式下编写在内部隐式事务中所做的更改。|
|Jet OLEDB：锁定延迟 (DBPROP_JETOLEDB_LOCKDELAY) |指示在上一次尝试失败后尝试获取锁定之前等待的毫秒数。|
|Jet OLEDB：锁定重试 (DBPROP_JETOLEDB_LOCKRETRY) |指示重复访问锁定页面的次数。|
|Jet OLEDB：最大缓冲区大小 (DBPROP_JETOLEDB_MAXBUFFERSIZE) |指示在开始刷新磁盘更改之前，Jet 可以使用的最大内存量（以 kb 为单位）。|
|Jet OLEDB：每个文件的最大锁 (DBPROP_JETOLEDB_MAXLOCKSPERFILE) |指示 Jet 可对数据库进行的最大锁定数。 默认值为9500。|
|Jet OLEDB：新的数据库密码 (DBPROP_JETOLEDB_NEWDATABASEPASSWORD) |指示要为此数据库设置的新密码。 旧密码存储在 **JET OLEDB：数据库密码** 中。|
|Jet OLEDB： ODBC 命令超时 (DBPROP_JETOLEDB_ODBCCOMMANDTIMEOUT) |指示从 Jet 发出的远程 ODBC 查询超时前等待的毫秒数。|
|Jet OLEDB：页锁到表锁 (DBPROP_JETOLEDB_PAGELOCKSTOTABLELOCK) |指示在 Jet 尝试将锁升级到表锁之前，必须在事务中锁定多少页。 如果此值为0，则永远不会升级锁。|
|Jet OLEDB： DBPROP_JETOLEDB_PAGETIMEOUT (的页面超时) |指示在检查数据库文件的缓存是否过期之前，Jet 将等待的毫秒数。|
|Jet OLEDB：循环 Long-Valued 页面 (DBPROP_JETOLEDB_RECYCLELONGVALUEPAGES) |指示 Jet 是否应在释放 BLOB 页时主动尝试回收它们。|
|Jet OLEDB：注册表路径 (DBPROP_JETOLEDB_REGPATH) |指示包含 Jet 数据库引擎值的 Windows 注册表项。|
|Jet OLEDB：重置 ISAM Stats (DBPROP_JETOLEDB_RESETISAMSTATS) |指示架构 **记录集** DBSCHEMA_JETOLEDB_ISAMSTATS 在返回性能信息后是否应重置其性能计数器。|
|Jet OLEDB：共享的异步延迟 (DBPROP_JETOLEDB_SHAREDASYNCDELAY) |指示在多用户模式下打开数据库时，Jet 可以将异步写入延迟到磁盘的最大时间（以毫秒为单位）。|
|Jet OLEDB：系统数据库 (DBPROP_JETOLEDB_SYSDBPATH) |指示工作组信息文件 (系统数据库) 的路径和文件名。|
|Jet OLEDB：事务提交模式 (DBPROP_JETOLEDB_TXNCOMMITMODE) |指示在提交事务时 Jet 是同步还是异步地将数据写入磁盘。|
|Jet OLEDB：用户提交同步 (DBPROP_JETOLEDB_USERCOMMITSYNC) |指示是否以同步模式或异步模式写入事务中所做的更改。|

## <a name="provider-specific-recordset-and-command-properties"></a>Provider-Specific 记录集和命令属性
 Jet 提供程序还支持多个特定于提供程序的 **记录集** 和 **命令** 属性。 这些属性通过 **Recordset** 或 **Command** 对象的 **properties** 集合进行访问和设置。 该表列出了 ADO 属性名称及其对应的 OLE DB 属性名称。

|属性名称|说明|
|-------------------|-----------------|
|Jet OLEDB：批量事务 (DBPROP_JETOLEDB_BULKNOTRANSACTIONS) |指示是否对 SQL 大容量操作进行事务处理。 由于资源延迟，大型大容量操作在事务处理时可能会失败。|
|Jet OLEDB： DBPROP_JETOLEDB_ENABLEFATCURSOR 启用 Fat 游标 () |指示在为远程行源填充记录集时，Jet 是否应缓存多个行。|
|Jet OLEDB： Fat Cursor 缓存大小 (DBPROP_JETOLEDB_FATCURSORMAXROWS) |指示使用远程数据存储行缓存时要缓存的行数。 除非 **JET OLEDB： Enable Fat** Cursor 为 True，否则将忽略此值。|
|Jet OLEDB：不一致的 (DBPROP_JETOLEDB_INCONSISTENT) |指示查询结果是否允许不一致的更新。|
|Jet OLEDB：锁定粒度 (DBPROP_JETOLEDB_LOCKGRANULARITY) |指示是否使用行级锁定打开表。|
|Jet OLEDB： ODBC Pass-Through 语句 (DBPROP_JETOLEDB_ODBCPASSTHROUGH) |指示 Jet 应将 **命令** 对象中的 SQL 文本原封不动地传递给后端。|
|Jet OLEDB：部分大容量 Ops (DBPROP_JETOLEDB_BULKPARTIAL) |指示 SQL DML 操作失败时 Jet 的行为。|
|Jet OLEDB：通过查询 Bulk-Op (DBPROP_JETOLEDB_PASSTHROUGHBULKOP) |指示是否将未返回 **记录集** 的查询原封不动地传递到数据源。|
|Jet OLEDB：通过查询连接字符串 (DBPROP_JETOLEDB_ODBCPASSTHROUGHCONNECTSTRING) |指示用于连接到远程数据存储的 Jet connect 字符串。 除非 **JET OLEDB： ODBC Pass-Through 语句** 为 True，否则将忽略此值。|
|Jet OLEDB：存储查询 (DBPROP_JETOLEDB_STOREDQUERY) |指示是否应将命令文本解释为存储查询而不是 SQL 命令。|
|Jet OLEDB：验证 Set (DBPROP_JETOLEDB_VALIDATEONSET 的规则) |指示在设置列数据或将更改提交到数据库时，是否计算 Jet 验证规则。|

 默认情况下，Microsoft Jet 的 OLE DB 提供程序在读/写模式下打开 Microsoft Jet 数据库。 若要以只读模式打开数据库，请将 ADO **连接** 对象的 [mode](../../reference/ado-api/mode-property-ado.md)属性设置为 **adModeRead**。

## <a name="command-object-usage"></a>命令对象使用情况
 [Command](../../reference/ado-api/command-object-ado.md)对象中的命令文本使用 MICROSOFT Jet SQL 方言。 您可以在命令文本中指定返回行的查询、操作查询和表名称;但是，不支持存储过程，且不应指定。

## <a name="recordset-behavior"></a>记录集行为
 Microsoft Jet 数据库引擎不支持动态游标。 因此，Microsoft Jet 的 OLE DB 提供程序不支持 **adLockDynamic** 游标类型。 请求动态游标时，提供程序将返回键集游标并重置 [CursorType](../../reference/ado-api/cursortype-property-ado.md) 属性，以指示返回的 [记录集](../../reference/ado-api/recordset-object-ado.md) 的类型。 此外，如果请求可更新的 **记录集** (**LockType** 为 **adLockOptimistic**、 **adLockBatchOptimistic** 或 **adLockPessimistic**) 提供程序也将返回键集游标并重置 **CursorType** 属性。

## <a name="dynamic-properties"></a>动态属性
 用于 Microsoft Jet 的 OLE DB 提供程序将多个动态属性插入未打开的 [连接](../../reference/ado-api/connection-object-ado.md)、[记录集](../../reference/ado-api/recordset-object-ado.md)和 [命令](../../reference/ado-api/command-object-ado.md)对象的 **properties** 集合。

 下表是每个动态属性的 ADO 和 OLE DB 名称的交叉索引。 OLE DB 程序员引用通过术语 "Description" 引用 ADO 属性名称。 可以在 OLE DB 程序员参考中找到有关这些属性的详细信息。

## <a name="connection-dynamic-properties"></a>连接动态属性
 将以下属性添加到 **连接** 对象的 **properties** 集合。

|ADO 属性名称|OLE DB 属性名称|
|-----------------------|--------------------------|
|活动会话|DBPROP_ACTIVESESSIONS|
|可异步中止|DBPROP_ASYNCTXNABORT|
|可异步提交|DBPROP_ASYNCTNXCOMMIT|
|自动提交隔离级别|DBPROP_SESS_AUTOCOMMITISOLEVELS|
|目录位置|DBPROP_CATALOGLOCATION|
|目录术语|DBPROP_CATALOGTERM|
|列定义|DBPROP_COLUMNDEFINITION|
|当前目录|DBPROP_CURRENTCATALOG|
|数据源|DBPROP_INIT_DATASOURCE|
|数据源名称|DBPROP_DATASOURCENAME|
|数据源对象线程模型|DBPROP_DSOTHREADMODEL|
|DBMS 名称|DBPROP_DBMSNAME|
|DBMS 版本|DBPROP_DBMSVER|
|按支持分组|DBPROP_GROUPBY|
|异类表支持|DBPROP_HETEROGENEOUSTABLES|
|标识符区分大小写|DBPROP_IDENTIFIERCASE|
|隔离级别|DBPROP_SUPPORTEDTXNISOLEVELS|
|隔离保持|DBPROP_SUPPORTEDTXNISORETAIN|
|区域设置标识符|DBPROP_INIT_LCID|
|最大索引大小|DBPROP_MAXINDEXSIZE|
|最大行大小|DBPROP_MAXROWSIZE|
|最大行大小包括 BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|
|选择中的最大表数|DBPROP_MAXTABLESINSELECT|
|模型|DBPROP_INIT_MODE|
|多个参数集|DBPROP_MULTIPLEPARAMSETS|
|多个结果|DBPROP_MULTIPLERESULTS|
|多个存储对象|DBPROP_MULTIPLESTORAGEOBJECTS|
|多表更新|DBPROP_MULTITABLEUPDATE|
|NULL 排序顺序|DBPROP_NULLCOLLATION|
|NULL 串联行为|DBPROP_CONCATNULLBEHAVIOR|
|OLE DB 版本|DBPROP_PROVIDEROLEDBVER|
|OLE 对象支持|DBPROP_OLEOBJECTS|
|打开行集支持|DBPROP_OPENROWSETSUPPORT|
|选择列表中的 ORDER BY 列|DBPROP_ORDERBYCOLUMNSINSELECT|
|输出参数可用性|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|通过引用访问器传递|DBPROP_BYREFACCESSORS|
|密码|DBPROP_AUTH_PASSWORD|
|永久性 ID 类型|DBPROP_PERSISTENTIDTYPE|
|准备中止行为|DBPROP_PREPAREABORTBEHAVIOR|
|准备提交行为|DBPROP_PREPARECOMMITBEHAVIOR|
|过程术语|DBPROP_PROCEDURETERM|
|提示|DBPROP_INIT_PROMPT|
|提供程序友好名称|DBPROP_PROVIDERFRIENDLYNAME|
|Provider Name|DBPROP_PROVIDERFILENAME|
|提供程序版本|DBPROP_PROVIDERVER|
|Read-Only 数据源|DBPROP_DATASOURCEREADONLY|
|命令行集转换|DBPROP_ROWSETCONVERSIONSONCOMMAND|
|架构术语|DBPROP_SCHEMATERM|
|架构使用情况|DBPROP_SCHEMAUSAGE|
|SQL 支持|DBPROP_SQLSUPPORT|
|结构化存储|DBPROP_STRUCTUREDSTORAGE|
|子查询支持|DBPROP_SUBQUERIES|
|表术语|DBPROP_TABLETERM|
|事务 DDL|DBPROP_SUPPORTEDTXNDDL|
|用户 ID|DBPROP_AUTH_USERID|
|用户名|DBPROP_USERNAME|
|窗口句柄|DBPROP_INIT_HWND|

## <a name="recordset-dynamic-properties"></a>记录集动态属性
 将以下属性添加到 **Recordset** 对象的 **properties** 集合。

|ADO 属性名称|OLE DB 属性名称|
|-----------------------|--------------------------|
|访问顺序|DBPROP_ACCESSORDER|
|Append-Only 行集|DBPROP_APPENDONLY|
|阻止存储对象|DBPROP_BLOCKINGSTORAGEOBJECTS|
|书签类型|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|书签已排序|DBPROP_ORDEREDBOOKMARKS|
|缓存延迟列|DBPROP_CACHEDEFERRED|
|更改插入的行|DBPROP_CHANGEINSERTEDROWS|
|列特权|DBPROP_COLUMNRESTRICT|
|列集通知|DBPROP_NOTIFYCOLUMNSET|
|列可写|DBPROP_MAYWRITECOLUMN|
|延迟列|DBPROP_DEFERRED|
|延迟存储对象更新|DBPROP_DELAYSTORAGEOBJECTS|
|向后提取|DBPROP_CANFETCHBACKWARDS|
|保存行|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|ILockBytes|DBPROP_ILockBytes|
|固定行|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IrowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IrowsetIndex|DBPROP_IRowsetIndex|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IrowsetLocate|DBPROP_IRowsestLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|IStorage|DBPROP_IStorage|
|IStream|DBPROP_IStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|文本书签|DBPROP_LITERALBOOKMARKS|
|文本行标识|DBPROP_LITERALIDENTITY|
|最大打开行数|DBPROP_MAXOPENROWS|
|最大挂起行数|DBPROP_MAXPENDINGROWS|
|最大行数|DBPROP_MAXROWS|
|内存用量|DBPROP_MEMORYUSAGE|
|通知粒度|DBPROP_NOTIFICATIONGRANULARITY|
|通知阶段|DBPROP_NOTIFICATIONPHASES|
|对象事务处理|DBPROP_TRANSACTEDOBJECT|
|其他可见更改|DBPROP_OTHERUPDATEDELETE|
|其他的插入可见|DBPROP_OTHERINSERT|
|自己的更改可见|DBPROP_OWNUPDATEDELETE|
|自己的可见插入|DBPROP_OWNINSERT|
|中止时保留|DBPROP_ABORTPRESERVE|
|提交时保留|DBPROP_COMMITPRESERVE|
|快速重新启动|DBPROP_QUICKRESTART|
|可重入事件|DBPROP_REENTRANTEVENTS|
|删除删除的行|DBPROP_REMOVEDELETED|
|报告多个更改|DBPROP_REPORTMULTIPLECHANGES|
|返回挂起的插入|DBPROP_RETURNPENDINGINSERTS|
|行删除通知|DBPROP_NOTIFYROWDELETE|
|行首次更改通知|DBPROP_NOTIFYROWFIRSTCHANGE|
|行插入通知|DBPROP_NOTIFYROWINSERT|
|行特权|DBPROP_ROWRESTRICT|
|行重新同步通知|DBPROP_NOTIFYROWRESYNCH|
|行线程模型|DBPROP_ROWTHREADMODEL|
|行撤消更改通知|DBPROP_NOTIFYROWUNDOCHANGE|
|行撤消删除通知|DBPROP_NOTIFYROWUNDODELETE|
|行撤消插入通知|DBPROP_NOTIFYROWUNDOINSERT|
|行更新通知|DBPROP_NOTIFYROWUPDATE|
|行集提取位置更改通知|DBPROP_NOTIFYROWSETFETCHPOSISIONCHANGE|
|行集发布通知|DBPROP_NOTIFYROWSETRELEASE|
|向后滚动|DBPROP_CANSCROLLBACKWARDS|
|跳过删除的书签|DBPROP_BOOKMARKSKIPPED|
|强行标识|DBPROP_STRONGITDENTITY|
|可更新性|DBPROP_UPDATABILITY|
|使用书签|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>命令动态属性
 将以下属性添加到 **命令** 对象的 **properties** 集合。

|ADO 属性名称|OLE DB 属性名称|
|-----------------------|--------------------------|
|访问顺序|DBPROP_ACCESSORDER|
|Append-Only 行集|DBPROP_APPENDONLY|
|阻止存储对象|DBPROP_BLOCKINGSTORAGEOBJECTS|
|书签类型|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|更改插入的行|DBPROP_CHANGEINSERTEDROWS|
|列特权|DBPROP_COLUMNRESTRICT|
|列集通知|DBPROP_NOTIFYCOLUMNSET|
|延迟列|DBPROP_DEFERRED|
|延迟存储对象更新|DBPROP_DELAYSTORAGEOBJECTS|
|向后提取|DBPROP_CANFETCHBACKWARDS|
|保存行|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|ILockBytes|DBPROP_ILockBytes|
|固定行|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IrowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IrowsetIndex|DBPROP_IRowsetIndex|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IrowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|IStorage|DBPROP_IStorage|
|IStream|DBPROP_IStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|文本书签|DBPROP_LITERALBOOKMARKS|
|文本行标识|DBPROP_LITERALIDENTITY|
|锁定模式|DBPROP_LOCKMODE|
|最大打开行数|DBPROP_MAXOPENROWS|
|最大挂起行数|DBPROP_MAXPENDINGROWS|
|最大行数|DBPROP_MAXROWS|
|通知粒度|DBPROP_NOTIFICATIONGRANULARITY|
|通知阶段|DBPROP_NOTIFICATIONPHASES|
|对象事务处理|DBPROP_TRANSACTEDOBJECT|
|其他可见更改|DBPROP_OTHERUPDATEDELETE|
|其他的插入可见|DBPROP_OTHERINSERT|
|自己的更改可见|DBPROP_OWNUPDATEDELETE|
|自己的可见插入|DBPROP_OWNINSERT|
|中止时保留|DBPROP_ABORTPRESERVE|
|提交时保留|DBPROP_COMMITPRESERVE|
|快速重新启动|DBPROP_QUICKRESTART|
|可重入事件|DBPROP_REENTRANTEVENTS|
|删除删除的行|DBPROP_REMOVEDELETED|
|报告多个更改|DBPROP_REPORTMULTIPLECHANGES|
|返回挂起的插入|DBPROP_RETURNPENDINGINSERTS|
|行删除通知|DBPROP_NOTIFYROWDELETE|
|行首次更改通知|DBPROP_NOTIFYROWFIRSTCHANGE|
|行插入通知|DBPROP_NOTIFYROWINSERT|
|行特权|DBPROP_ROWRESTRICT|
|行重新同步通知|DBPROP_NOTIFYROWRESYNCH|
|行线程模型|DBPROP_ROWTHREADMODEL|
|行撤消更改通知|DBPROP_NOTIFYROWUNDOCHANGE|
|行撤消删除通知|DBPROP_NOTIFYROWUNDODELETE|
|行撤消插入通知|DBPROP_NOTIFYROWUNDOINSERT|
|行更新通知|DBPROP_NOTIFYROWUPDATE|
|行集提取位置更改通知|DBPROP_NOTIFYROWSETFETCHPOSITIONCHANGE|
|行集发布通知|DBPROP_NOTIFYROWSETRELEASE|
|向后滚动|DBPROP_CANSCROLLBACKWARDS|
|插入时的服务器数据|DBPROP_SERVERDATAONINSERT|
|跳过删除的书签|DBPROP_BOOKMARKSKIP|
|强行标识|DBPROP_STRONGIDENTITY|
|可更新性|DBPROP_UPDATABILITY|
|使用书签|DBPROP_BOOKMARKS|

 有关适用于 Microsoft Jet 的 OLE DB 提供程序的具体实现详细信息和功能信息，请参阅 OLE DB 文档中的 [Jet 提供程序](/previous-versions/windows/desktop/ms722791(v=vs.85)) 。
