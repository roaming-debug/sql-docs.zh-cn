---
description: 适用于 SQL Server 的 Microsoft OLE DB 提供程序概述
title: 适用于 SQL Server 的 Microsoft OLE DB 提供程序 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], OLE DB provider for SQL Server
- OLE DB provider for SQL Server [ADO]
- SQLOLEDB [ADO]
ms.assetid: 99bc40c4-9181-4ca1-a06f-9a1a914a0b7b
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b6a62e8946415cc9c1e869a173de43b98ba14f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029251"
---
# <a name="microsoft-ole-db-provider-for-sql-server-overview"></a>适用于 SQL Server 的 Microsoft OLE DB 提供程序概述
适用于 SQL Server 的 Microsoft OLE DB 提供程序，SQLOLEDB 允许 ADO 访问 Microsoft SQL Server。

> [!IMPORTANT]
> 适用于 SQL Server 的 Microsoft OLE DB 提供程序 (SQLOLEDB) 保持不推荐使用，不建议用于新的开发工作。 相反，请使用新的 [Microsoft OLE DB Driver for SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL)，其将使用最新的服务器功能进行更新。

## <a name="connection-string-parameters"></a>连接字符串参数
 若要连接到该提供程序，请将 *提供程序* 参数设置为 [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) 属性，以：

```vb
SQLOLEDB
```

 还可以使用 [Provider](../../reference/ado-api/provider-property-ado.md) 属性设置或读取此值。

## <a name="typical-connection-string"></a>典型连接字符串
 此提供程序的典型连接字符串是：

```vb
"Provider=SQLOLEDB;Data Source=serverName;"
Initial Catalog=databaseName;
User ID=MyUserID;Password=MyPassword;"
```

 字符串包含以下关键字：

|关键字|说明|
|-------------|-----------------|
|**提供程序**|指定 SQL Server 的 OLE DB 提供程序。|
|**数据源** 或 **服务器**|指定服务器的名称。|
|**初始目录** 或 **数据库**|指定服务器上的数据库的名称。|
|**用户 ID** 或 **uid**|指定 SQL Server 身份验证) 的用户名 (。|
|**Password** 或 **pwd**|为 SQL Server Authentication) 指定用户密码 (。|

> [!NOTE]
>  如果要连接到支持 Windows 身份验证的数据源提供程序，应在连接字符串中指定 **Trusted_Connection = yes** 或 **集成安全性 = SSPI** 而不是用户 ID 和密码信息。

## <a name="provider-specific-connection-parameters"></a>Provider-Specific 连接参数
 提供程序除 ADO 定义的连接参数外，还支持多个特定于提供程序的连接参数。 与 ADO 连接属性一样，这些特定于提供程序的属性可以通过 [连接](../../reference/ado-api/connection-object-ado.md)的 [properties](../../reference/ado-api/properties-collection-ado.md)集合进行设置，也可以设置为 **ConnectionString** 的一部分。

|参数|说明|
|---------------|-----------------|
|Trusted_Connection|指示用户身份验证模式。 这可以设置为 **"是" 或 "** **否**"。 默认值为“否”。 如果将此属性设置为 **"是"**，则 SQLOLEDB 将使用 MICROSOFT Windows NT 身份验证模式来授权用户访问由 **Location** 和 [Datasource](../../reference/ado-api/datasource-property-ado.md) 属性值指定的 SQL Server 数据库。 如果将此属性设置为 " **否**"，则 SQLOLEDB 将使用混合模式来授予用户对 SQL Server 数据库的访问权限。 SQL Server 登录名和密码在 " **用户 Id** " 和 " **密码** " 属性中指定。|
|当前语言|指示 SQL Server 语言名称。 标识用于系统消息选择和格式化的语言。 必须在 SQL Server 上安装该语言，否则打开该连接将失败。|
|网络地址|指示 **Location** 属性指定的 SQL Server 的网络地址。|
|Network Library|指示用于与 SQL Server 通信的网络库 (DLL) 的名称。 该名称不应当包含路径或 .dll 文件扩展名。 默认值由 SQL Server 客户端配置提供。|
|使用准备过程|确定在准备 **好** 的属性 (准备好命令时 SQL Server 是否创建临时存储过程) 。|
|自动翻译|指示是否转换 OEM/ANSI 字符。 此属性可以设置为 **True** 或 **False**。 默认值为 **True**。 如果将此属性设置为 **True**，则当从 SQL Server 检索多字节字符字符串或将其发送到时，SQLOLEDB 将执行 OEM/ANSI 字符转换。 如果将此属性设置为 **False**，则 SQLOLEDB 不会对多字节字符字符串数据执行 OEM/ANSI 字符转换。|
|Packet Size|指示网络数据包大小（以字节为单位）。 "数据包大小" 属性值必须介于512到32767之间。 默认的 SQLOLEDB 网络数据包大小为4096。|
|应用程序名称|指示客户端应用程序的名称。|
|工作站 ID|用于标识工作站的字符串。|

## <a name="command-object-usage"></a>命令对象使用情况
 SQLOLEDB 接受使用 ODBC、ANSI 和 SQL Server 特定的 Transact-sql 作为有效语法的综合。 例如，以下 SQL 语句使用 ODBC SQL 转义序列指定 LCASE 字符串函数：

```sql
SELECT customerid={fn LCASE(CustomerID)} FROM Customers

```

 LCASE 返回一个字符串，将所有大写字母字符转换为相应的小写字母字符。 ANSI SQL 字符串函数 LOWER 执行相同的操作，因此以下 SQL 语句与前面提供的 ODBC 语句等效。

```sql
SELECT customerid=LOWER(CustomerID) FROM Customers

```

 当指定为命令的文本时，SQLOLEDB 成功处理语句的任一形式。

## <a name="stored-procedures"></a>存储过程
 使用 SQLOLEDB 命令执行 SQL Server 存储过程时，请在命令文本中使用 ODBC 过程调用转义序列。 然后，SQLOLEDB 使用 SQL Server 的远程过程调用机制优化命令处理。 例如，以下 ODBC SQL 语句是 Transact-sql 窗体上首选的命令文本：

## <a name="odbc-sql"></a>ODBC SQL

```vb
{call SalesByCategory('Produce', '1995')}

```

## <a name="transact-sql"></a>Transact-SQL

```sql
EXECUTE SalesByCategory 'Produce', '1995'

```

## <a name="sql-server-features"></a>SQL Server 功能
 使用 SQL Server，ADO 可以使用 XML 作为 **命令** 输入，并以 xml 流格式而不是在 **Recordset** 对象中检索结果。 有关详细信息，请参阅将 [流用于命令输入](../data/command-streams.md) 和 [检索流中的结果](../data/retrieving-resultsets-into-streams.md)集。

### <a name="accessing-sql_variant-data-using-mdac-27-mdac-28-or-windows-dac-60"></a>使用 MDAC 2.7、MDAC 2.8 或 Windows DAC 6.0 访问 sql_variant 数据
 Microsoft SQL Server 具有一个名为 **sql_variant** 的数据类型。 与 OLE DB **DBTYPE_VARIANT** 类似， **sql_variant** 数据类型可以存储多种不同类型的数据。 但 **DBTYPE_VARIANT** 和 **sql_variant** 之间存在一些重要差异。 ADO 还会以不同于处理其他数据类型的方式处理存储为 **sql_variant** 值的数据。 以下列表描述了访问 **sql_variant** 类型的列中存储的 SQL Server 数据时要考虑的问题。

-   在 MDAC 2.7、MDAC 2.8 和 Windows 数据访问组件 (Windows DAC) 6.0，SQL Server 的 OLE DB 提供程序支持 **sql_variant** 类型。 ODBC 的 OLE DB 提供程序不是。

-   **Sql_variant** 类型与 **DBTYPE_VARIANT** 数据类型不完全匹配。  **Sql_variant** 类型支持 DBTYPE_VARIANT 不支持的几个新子类型 **，** 包括 **GUID**、 **ANSI** (非 UNICODE) 字符串和 **BIGINT**。 使用前面列出的子类型之外的子类型将能正常工作。

-   **Sql_variant** 子类型 **数值** 与大小 **DBTYPE_DECIMAL** 不匹配。

-   多个数据类型强制将导致类型不匹配。 例如，将具有 **GUID** 子类型的 **sql_variant** 强制为 **DBTYPE_VARIANT** 将 **导致 (字节** 的子类型) 。 将此类型转换回 **sql_variant** 将导致 **数组**)  (字节的新子类型。

-   只有在 **sql_variant** 包含特定子类型时，才能 (封送) 或持久保存包含 **Sql_variant** 数据的 **记录集** 字段。 尝试使用以下不受支持的子类型来远程或保留数据将导致运行时错误， (Microsoft 永久性提供程序中不支持的转换)  (MSPersist) ： **VT_VARIANT**、 **VT_RECORD**、 **VT_ILLEGAL**、 **VT_UNKNOWN**、 **VT_BSTR** 和 **VT_DISPATCH。**

-   MDAC 2.7、MDAC 2.8 和 Windows DAC 6.0 中 SQL Server 的 OLE DB 提供程序有一个名为 " **允许本机变** 体" 的动态属性，如名称所示，允许开发人员访问其本机格式的 **sql_variant** ，而不是 **DBTYPE_VARIANT**。 如果设置此属性，并且使用客户端游标引擎打开 **记录集** (**AdUseClient**) ，则 **记录集。打开** 调用将失败。 如果已设置此属性，并且使用服务器游标打开 **记录集** (**AdUseServer**) ，则 **记录集** 将成功，但访问 **sql_variant** 类型的列将产生错误。

-   在使用 MDAC 2.5 的客户端应用程序中，可以对 Microsoft SQL Server 的查询使用 **sql_variant** 数据。 但 **sql_variant** 数据的值被视为字符串。 此类客户端应用程序应升级到 MDAC 2.7、MDAC 2.8 或 Windows DAC 6.0。

## <a name="recordset-behavior"></a>记录集行为
 SQLOLEDB 不能使用 SQL Server 游标来支持许多命令生成的多个结果。 如果使用者请求需要 SQL Server 游标支持的记录集，则当使用的命令文本生成多个记录集作为其结果时，将发生错误。

 SQL Server 游标支持可滚动的 SQLOLEDB 记录集。 SQL Server 对对数据库的其他用户所做更改敏感的游标施加限制。 具体而言，某些游标中的行无法排序，尝试使用包含 SQL ORDER BY 子句的命令创建记录集可能会失败。

## <a name="dynamic-properties"></a>动态属性
 用于 SQL Server 的 Microsoft OLE DB 提供程序将多个动态属性插入未打开的 [连接](../../reference/ado-api/connection-object-ado.md)、[记录集](../../reference/ado-api/recordset-object-ado.md)和 [命令](../../reference/ado-api/command-object-ado.md)对象的 **properties** 集合。

 下表是每个动态属性的 ADO 和 OLE DB 名称的交叉索引。 OLE DB 程序员参考是指 ADO 属性名称，术语为 "Description"。 可以在 OLE DB 程序员参考中找到有关这些属性的详细信息。 在索引中搜索 OLE DB 属性名称，或者参阅 [附录 C： OLE DB 属性](/previous-versions/windows/desktop/ms723130(v=vs.85))。

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
|连接超时值|DBPROP_INIT_TIMEOUT|
|当前目录|DBPROP_CURRENTCATALOG|
|数据源|DBPROP_INIT_DATASOURCE|
|数据源名称|DBPROP_DATASOURCENAME|
|数据源对象线程模型|DBPROP_DSOTHREADMODEL|
|DBMS 名称|DBPROP_DBMSNAME|
|DBMS 版本|DBPROP_DBMSVER|
|扩展属性|DBPROP_INIT_PROVIDERSTRING|
|按支持分组|DBPROP_GROUPBY|
|异类表支持|DBPROP_HETEROGENEOUSTABLES|
|标识符区分大小写|DBPROP_IDENTIFIERCASE|
|初始目录|DBPROP_INIT_CATALOG|
|隔离级别|DBPROP_SUPPORTEDTXNISOLEVELS|
|隔离保持|DBPROP_SUPPORTEDTXNISORETAIN|
|区域设置标识符|DBPROP_INIT_LCID|
|最大索引大小|DBPROP_MAXINDEXSIZE|
|最大行大小|DBPROP_MAXROWSIZE|
|最大行大小包括 BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|
|选择中的最大表数|DBPROP_MAXTABLESINSELECT|
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
|持久性安全信息|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|
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
|阻止存储对象|DBPROP_BLOCKINGSTORAGEOBJECTS|
|书签类型|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|更改插入的行|DBPROP_CHANGEINSERTEDROWS|
|列特权|DBPROP_COLUMNRESTRICT|
|列集通知|DBPROP_NOTIFYCOLUMNSET|
|命令超时|DBPROP_COMMANDTIMEOUT|
|延迟列|DBPROP_DEFERRED|
|延迟存储对象更新|DBPROP_DELAYSTORAGEOBJECTS|
|向后提取|DBPROP_CANFETCHBACKWARDS|
|保存行|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|固定行|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IrowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IrowsetLocate|DBPROP_IRowsestLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|文本书签|DBPROP_LITERALBOOKMARKS|
|文本行标识|DBPROP_LITERALIDENTITY|
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
|行集提取位置更改通知|DBPROP_NOTIFYROWSETFETCHPOSISIONCHANGE|
|行集发布通知|DBPROP_NOTIFYROWSETRELEASE|
|向后滚动|DBPROP_CANSCROLLBACKWARDS|
|服务器游标|DBPROP_SERVERCURSOR|
|跳过删除的书签|DBPROP_BOOKMARKSKIPPED|
|强行标识|DBPROP_STRONGITDENTITY|
|唯一行|DBPROP_UNIQUEROWS|
|可更新性|DBPROP_UPDATABILITY|
|使用书签|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>命令动态属性
 将以下属性添加到 **命令** 对象的 **properties** 集合。

|ADO 属性名称|OLE DB 属性名称|
|-----------------------|--------------------------|
|访问顺序|DBPROP_ACCESSORDER|
|基路径|SSPROP_STREAM_BASEPATH|
|阻止存储对象|DBPROP_BLOCKINGSTORAGEOBJECTS|
|书签类型|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|更改插入的行|DBPROP_CHANGEINSERTEDROWS|
|列特权|DBPROP_COLUMNRESTRICT|
|列集通知|DBPROP_NOTIFYCOLUMNSET|
|内容类型|SSPROP_STREAM_CONTENTTYPE|
|游标自动提取|SSPROP_CURSORAUTOFETCH|
|延迟列|DBPROP_DEFERRED|
|延迟准备|SSPROP_DEFERPREPARE|
|延迟存储对象更新|DBPROP_DELAYSTORAGEOBJECTS|
|向后提取|DBPROP_CANFETCHBACKWARDS|
|保存行|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|固定行|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IrowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IrowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch|DBPROP_IRowsetResynch|
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
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
|输出编码属性|DBPROP_OUTPUTENCODING|
|输出流属性|DBPROP_OUTPUTSTREAM|
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
|服务器游标|DBPROP_SERVERCURSOR|
|插入时的服务器数据|DBPROP_SERVERDATAONINSERT|
|跳过删除的书签|DBPROP_BOOKMARKSKIP|
|强行标识|DBPROP_STRONGIDENTITY|
|可更新性|DBPROP_UPDATABILITY|
|使用书签|DBPROP_BOOKMARKS|
|XML 根|SSPROP_STREAM_XMLROOT|
|XSL|SSPROP_STREAM_XSL|

 有关 Microsoft SQL Server OLE DB 提供程序的特定实现细节和功能信息，请参阅 [SQL Server 提供程序](/previous-versions/windows/desktop/ms720897(v=vs.85))。

## <a name="see-also"></a>另请参阅
 [ConnectionString 属性 (ado) ](../../reference/ado-api/connectionstring-property-ado.md) [提供程序属性 (Ado) ](../../reference/ado-api/provider-property-ado.md) [Recordset 对象 (ado) ](../../reference/ado-api/recordset-object-ado.md)