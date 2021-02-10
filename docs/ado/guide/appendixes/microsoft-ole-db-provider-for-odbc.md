---
description: 适用于 ODBC 的 Microsoft OLE DB 提供程序概述
title: 适用于 ODBC 的 Microsoft OLE DB 提供程序 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB provider for ODBC [ADO]
- providers [ADO], OLE DB provider for ODBC
ms.assetid: 2dc0372d-e74d-4d0f-9c8c-04e5a168c148
author: rothja
ms.author: jroth
ms.openlocfilehash: 5a6cfdda6072f8ed51d55569ed68b113a4148642
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029261"
---
# <a name="microsoft-ole-db-provider-for-odbc-overview"></a>适用于 ODBC 的 Microsoft OLE DB 提供程序概述
对于 ADO 或 RDS 程序员，理想情况下，每个数据源都公开一个 OLE DB 接口，以便 ADO 可以直接调用到数据源。 尽管数据库供应商越来越多地实现 OLE DB 接口，但某些数据源尚未以这种方式公开。 但是，目前使用的大多数 DBMS 系统都可通过 ODBC 访问。

 ODBC 驱动程序可用于当前使用的每个主要 DBMS，其中包括 Microsoft SQL Server、Microsoft Jet 数据库引擎) 和 Microsoft FoxPro (，以及非 Microsoft 数据库产品（如 Oracle）。

 但是，Microsoft ODBC 提供程序允许 ADO 连接到任何 ODBC 数据源。 提供程序已启用自由线程和 Unicode。

 提供程序支持事务，尽管不同 DBMS 引擎提供不同类型的事务支持。 例如，Microsoft Access 支持嵌套事务，最高可达五级。

 这是 ADO 的默认提供程序，并且支持所有与提供程序相关的 ADO 属性和方法。

## <a name="connection-string-parameters"></a>连接字符串参数
 若要连接到该提供程序，请将 [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md)属性的 **provider =** 参数设置为：

```
MSDASQL
```

 读取 [提供程序](../../reference/ado-api/provider-property-ado.md) 属性也会返回此字符串。

## <a name="typical-connection-string"></a>典型连接字符串
 此提供程序的典型连接字符串是：

```
"Provider=MSDASQL;DSN=dsnName;UID=MyUserID;PWD=MyPassword;"
```

 字符串包含以下关键字：

|关键字|说明|
|-------------|-----------------|
|**提供程序**|指定 ODBC 的 OLE DB 提供程序。|
|**DSN**|指定数据源名称。|
|**UID**|指定用户名。|
|**PWD**|指定用户密码。|
|**URL**|指定在 Web 文件夹中发布的文件或目录的 URL。|

 由于这是 ADO 的默认提供程序，如果从连接字符串中省略 **provider =** 参数，则 ADO 将尝试与此提供程序建立连接。

> [!NOTE]
>  如果要连接到支持 Windows 身份验证的数据源提供程序，应在连接字符串中指定 **Trusted_Connection = yes** 或 **集成安全性 = SSPI** 而不是用户 ID 和密码信息。

 除 ADO 定义的参数外，提供程序不支持任何特定的连接参数。 但是，提供程序会将任何非 ADO 连接参数传递到 ODBC 驱动程序管理器。

 由于可以省略 **Provider** 参数，因此可以编写一个与同一数据源的 ODBC 连接字符串相同的 ADO 连接字符串。 在编写 ODBC 连接字符串时， (**DRIVER =**、 **DATABASE =**、 **DSN =** 等) 、值和语法上使用相同的参数名称。 你可以使用或不使用预定义的数据源名称 (DSN) 或 FileDSN 进行连接。

## <a name="syntax-with-a-dsn-or-filedsn"></a>DSN 或 FileDSN 的语法：

```
"[Provider=MSDASQL;] { DSN=name | FileDSN=filename } ;
[DATABASE=database;] UID=user; PWD=password"
```

## <a name="syntax-without-a-dsn-dsn-less-connection"></a>不带 DSN 的语法 (无 dsn 连接) ：

```
"[Provider=MSDASQL;] DRIVER=driver; SERVER=server;
DATABASE=database; UID=MyUserID; PWD=MyPassword"
```

## <a name="remarks"></a>备注
 如果使用 **DSN** 或 **FileDSN**，则必须通过 Windows 控制面板中的 "ODBC 数据源管理器" 进行定义。 在 Microsoft Windows 2000 中，ODBC 管理员位于 "管理工具" 下。 在早期版本的 Windows 中，ODBC 管理员图标名为 **32 位 ODBC** 或只是 **odbc**。

 除了设置 **DSN** 外，还可以指定 ODBC 驱动程序 (**driver =**) ，例如 "SQL Server;" 服务器名称 (**server =**) ;数据库名称 (**database =**) 。

 你还可以指定用户帐户名称 (**UID =**) ，以及用户帐户的密码 (**PWD) =** 在特定于 ODBC 的参数中或在标准 ADO 定义的 *user* 和 *password* 参数中。

 尽管 **dsn** 定义已经指定了数据库，但你也可以指定 *一个**数据库* 参数，以及用于连接到其他数据库的 **dsn** 。 使用 **DSN** 时，最好 *始终包含* *database* 参数。 这将确保你连接到正确的数据库（如果自上次检查 **DSN** 定义后另一个用户更改了默认数据库参数）。

## <a name="provider-specific-connection-properties"></a>Provider-Specific 连接属性
 ODBC 的 OLE DB 提供程序将多个属性添加到 **连接** 对象的 [properties](../../reference/ado-api/properties-collection-ado.md)集合。 下表列出了这些属性，并在括号中列出了相应的 OLE DB 属性名称。

|属性名称|说明|
|-------------------|-----------------|
|可访问过程 (KAGPROP_ACCESSIBLEPROCEDURES) |指示用户是否有权访问存储过程。|
|可访问的表 (KAGPROP_ACCESSIBLETABLES) |指示用户是否有权对数据库表执行 SELECT 语句。|
|活动语句 (KAGPROP_ACTIVESTATEMENTS) |指示 ODBC 驱动程序在连接上可以支持的句柄数。|
|驱动程序名称 (KAGPROP_DRIVERNAME) |指示 ODBC 驱动程序的文件名。|
|驱动程序 ODBC 版本 (KAGPROP_DRIVERODBCVER) |指示此驱动程序支持的 ODBC 版本。|
|文件使用 (KAGPROP_FILEUSAGE) |指示驱动程序如何处理数据源中的文件;作为表或作为目录。|
|Like Escape 子句 (KAGPROP_LIKEESCAPECLAUSE) |指示驱动程序是否支持在 WHERE 子句的 LIKE 谓词中对百分号字符 (% ) 和下划线字符 (_) 使用转义符。|
|Group By 中的最大列 (KAGPROP_MAXCOLUMNSINGROUPBY) |指示 SELECT 语句的 GROUP BY 子句中可列出的最大列数。|
|索引 (KAGPROP_MAXCOLUMNSININDEX 中的最大列数) |指示可包含在索引中的最大列数。|
|按 (KAGPROP_MAXCOLUMNSINORDERBY 排序的最大列) |指示 SELECT 语句的 ORDER BY 子句中可列出的最大列数。|
|Select (KAGPROP_MAXCOLUMNSINSELECT 中的最大列) |指示 SELECT 语句的 SELECT 部分中可列出的最大列数。|
|表 (中的最大列 KAGPROP_MAXCOLUMNSINTABLE) |指示表中允许的最大列数。|
|数值函数 (KAGPROP_NUMERICFUNCTIONS) |指示 ODBC 驱动程序支持哪些数值函数。 有关此位掩码中使用的函数名称和关联值的列表，请参阅 ODBC 文档中的 [附录 E：标量函数](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md)。|
|外部联接功能 (KAGPROP_OJCAPABILITY) |指示提供程序支持的外部联接的类型。|
|外部联接 (KAGPROP_OUTERJOINS) |指示提供程序是否支持外部联接。|
|特殊字符 (KAGPROP_SPECIALCHARACTERS) |指示哪些字符对于 ODBC 驱动程序具有特殊意义。|
|存储过程 (KAGPROP_PROCEDURES) |指示存储过程是否可用于此 ODBC 驱动程序。|
|KAGPROP_STRINGFUNCTIONS 的字符串函数 () |指示 ODBC 驱动程序支持的字符串函数。 有关此位掩码中使用的函数名称和关联值的列表，请参阅 ODBC 文档中的 [附录 E：标量函数](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md)。|
|系统函数 (KAGPROP_SYSTEMFUNCTIONS) |指示 ODBC 驱动程序支持的系统函数。 有关此位掩码中使用的函数名称和关联值的列表，请参阅 ODBC 文档中的 [附录 E：标量函数](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md)。|
|时间/日期函数 (KAGPROP_TIMEDATEFUNCTIONS) |指示 ODBC 驱动程序支持的时间和日期函数。 有关此位掩码中使用的函数名称和关联值的列表，请参阅 ODBC 文档中的 [附录 E：标量函数](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md)。|
|SQL 语法支持 (KAGPROP_ODBCSQLCONFORMANCE) |指示 ODBC 驱动程序支持的 SQL 语法。|

## <a name="provider-specific-recordset-and-command-properties"></a>Provider-Specific 记录集和命令属性
 ODBC 的 OLE DB 提供程序将多个属性添加到 **Recordset** 和 **Command** 对象的 **properties** 集合。 下表列出了这些属性，并在括号中列出了相应的 OLE DB 属性名称。

|属性名称|说明|
|-------------------|-----------------|
|基于查询的更新/删除/插入 (KAGPROP_QUERYBASEDUPDATES) |指示是否可以使用 SQL 查询执行更新、删除和插入操作。|
|ODBC 并发类型 (KAGPROP_CONCURRENCY) |指示用于减少两个用户尝试同时访问数据源中的相同数据而导致的潜在问题的方法。|
| (KAGPROP_BLOBSONFOCURSOR Forward-Only 游标的 BLOB 可访问性) |指示在使用只进游标时是否可以访问 BLOB **字段** 。|
|在 QBU 中包括 SQL_FLOAT、SQL_DOUBLE 和 SQL_REAL，WHERE 子句 (KAGPROP_INCLUDENONEXACT) |指示 SQL_FLOAT、SQL_DOUBLE 和 SQL_REAL 值是否可以包含在 QBU WHERE 子句中。|
|Insert (KAGPROP_POSITIONONNEWROW 后的最后一行上的位置) |指示在表中插入新记录后，表中的最后一行将为当前行。|
|IRowsetChangeExtInfo (KAGPROP_IROWSETCHANGEEXTINFO) |指示 **IRowsetChange** 接口是否提供扩展信息支持。|
|ODBC 游标类型 (KAGPROP_CURSOR) |指示 **记录集** 使用的游标类型。|
|生成可 (KAGPROP_MARSHALLABLE 封送的行集) |指示 ODBC 驱动程序生成可封送的记录集|

## <a name="command-text"></a>命令文本
 使用 [Command](../../reference/ado-api/command-object-ado.md) 对象的方式很大程度取决于数据源以及它将接受何种类型的查询或命令语句。

 ODBC 提供了一个特定的语法来调用存储过程。 对于 **Command** 对象的 [Commandtext](../../reference/ado-api/commandtext-property-ado.md)属性，[连接](../../reference/ado-api/connection-object-ado.md)对象上的 **Execute** 方法的 *commandtext* 参数，或者 [记录集](../../reference/ado-api/recordset-object-ado.md)对象上 **Open** 方法的 *Source* 参数，将传入具有以下语法的字符串：

```
"{ [ ? = ] call procedure [ ( ? [, ? [ , ... ]] ) ] }"
```

 每 **个** 引用 [Parameters](../../reference/ado-api/parameters-collection-ado.md) 集合中的对象。 第一个 **呢？** 引用 **参数** (0) ，下一个 **？** 引用 **参数** (1) 等。

 参数引用是可选的，具体取决于存储过程的结构。 如果要调用不定义任何参数的存储过程，则字符串将如下所示：

```
"{ call procedure }"
```

 如果有两个查询参数，则字符串将类似于以下内容：

```
"{ call procedure ( ?, ? ) }"
```

 如果存储过程返回值，则返回值将被视为另一个参数。 如果没有查询参数，但有返回值，则字符串将类似于以下内容：

```
"{ ? = call procedure }"
```

 最后，如果您有一个返回值和两个查询参数，则您的字符串将如下所示：

```
"{ ? = call procedure ( ?, ? ) }"
```

## <a name="recordset-behavior"></a>记录集行为
 下表列出了使用此提供程序打开的 **记录集** 对象上可用的标准 ADO 方法和属性。

 有关提供程序配置的 **记录集** 行为的详细信息，请运行 [支持](../../reference/ado-api/supports-method.md)方法，并枚举 **记录集** 的 **Properties** 集合，以确定是否存在特定于提供程序的动态属性。

 标准 ADO **记录集** 属性的可用性：

|属性|ForwardOnly|动态|Keyset|Static|
|--------------|-----------------|-------------|------------|------------|
|[AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md)|不可用|不可用|读/写|读/写|
|[AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md)|不可用|不可用|读/写|读/写|
|[ActiveConnection](../../reference/ado-api/activeconnection-property-ado.md)|读/写|读/写|读/写|读/写|
|[BOF](../../reference/ado-api/bof-eof-properties-ado.md)|只读|只读|只读|只读|
|书签 |不可用|不可用|读/写|读/写|
|[CacheSize](../../reference/ado-api/cachesize-property-ado.md)|读/写|读/写|读/写|读/写|
|[CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md)|读/写|读/写|读/写|读/写|
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|读/写|读/写|读/写|读/写|
|[EditMode](../../reference/ado-api/editmode-property.md)|只读|只读|只读|只读|
|[Filter](../../reference/ado-api/filter-property.md)|读/写|读/写|读/写|读/写|
|[LockType](../../reference/ado-api/locktype-property-ado.md)|读/写|读/写|读/写|读/写|
|[MarshalOptions](../../reference/ado-api/marshaloptions-property-ado.md)|读/写|读/写|读/写|读/写|
|[MaxRecords](../../reference/ado-api/maxrecords-property-ado.md)|读/写|读/写|读/写|读/写|
|[PageCount](../../reference/ado-api/pagecount-property-ado.md)|读/写|不可用|只读|只读|
|[PageSize](../../reference/ado-api/pagesize-property-ado.md)|读/写|读/写|读/写|读/写|
|[RecordCount](../../reference/ado-api/recordcount-property-ado.md)|读/写|不可用|只读|只读|
|[Source](../../reference/ado-api/source-property-ado-recordset.md)|读/写|读/写|读/写|读/写|
|[State](../../reference/ado-api/state-property-ado.md)|只读|只读|只读|只读|
|[Status](../../reference/ado-api/status-property-ado-recordset.md)|只读|只读|只读|只读|

 当使用 ADO 与适用于 ODBC 的 Microsoft OLE DB Provider 版本1.0 一起使用时， [AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md) 和 [AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md) 属性是只写的。

 标准 ADO **记录集** 方法的可用性：

|方法|ForwardOnly|动态|Keyset|Static|
|------------|-----------------|-------------|------------|------------|
|[AddNew](../../reference/ado-api/addnew-method-ado.md)|是|是|是|是|
|[取消](../../reference/ado-api/cancel-method-ado.md)|是|是|是|是|
|[CancelBatch](../../reference/ado-api/cancelbatch-method-ado.md)|是|是|是|是|
|[CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md)|是|是|是|是|
|[克隆](../../reference/ado-api/clone-method-ado.md)|否|否|是|是|
|[关闭](../../reference/ado-api/close-method-ado.md)|是|是|是|是|
|[删除](../../reference/ado-api/delete-method-ado-recordset.md)|是|是|是|是|
|[GetRows](../../reference/ado-api/getrows-method-ado.md)|是|是|是|是|
|[移动](../../reference/ado-api/move-method-ado.md)|是|是|是|是|
|[MoveFirst](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|是|是|是|是|
|[MoveLast](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|否|是|是|是|
|[MoveNext](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|是|是|是|是|
|[MovePrevious](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|否|是|是|是|
|[NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)*|是|是|是|是|
|[打开](../../reference/ado-api/open-method-ado-recordset.md)|是|是|是|是|
|[重新](../../reference/ado-api/requery-method.md)|是|是|是|是|
|[重新同步](../../reference/ado-api/resync-method.md)|否|否|是|是|
|[支持](../../reference/ado-api/supports-method.md)|是|是|是|是|
|[Update](../../reference/ado-api/update-method.md)|是|是|是|是|
|[UpdateBatch](../../reference/ado-api/updatebatch-method.md)|是|是|是|是|

 * 不支持 Microsoft Access 数据库。

## <a name="dynamic-properties"></a>动态属性
 适用于 ODBC 的 Microsoft OLE DB 提供程序将多个动态属性插入未打开的 [连接](../../reference/ado-api/connection-object-ado.md)、[记录集](../../reference/ado-api/recordset-object-ado.md)和 [命令](../../reference/ado-api/command-object-ado.md)对象的 **properties** 集合。

 下表是每个动态属性的 ADO 和 OLE DB 名称的交叉索引。 OLE DB 程序员引用通过术语 "Description" 引用 ADO 属性名称。 可以在 OLE DB 程序员参考中找到有关这些属性的详细信息。 在索引中搜索 OLE DB 属性名称，或者参阅 [附录 C： OLE DB 属性](/previous-versions/windows/desktop/ms723130(v=vs.85))。

## <a name="connection-dynamic-properties"></a>连接动态属性
 以下属性将添加到 **连接** 对象的 **properties** 集合中。

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
|位置|DBPROP_INIT_LOCATION|
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
|OLE DB 服务|DBPROP_INIT_OLEDBSERVICES|
|OLE DB 版本|DBPROP_PROVIDEROLEDBVER|
|OLE 对象支持|DBPROP_OLEOBJECTS|
|打开行集支持|DBPROP_OPENROWSETSUPPORT|
|选择列表中的 ORDER BY 列|DBPROP_ORDERBYCOLUMNSINSELECT|
|输出参数可用性|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|密码|DBPROP_AUTH_PASSWORD|
|通过引用访问器传递|DBPROP_BYREFACCESSORS|
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
 以下属性将添加到 **Recordset** 对象的 **properties** 集合中。

|ADO 属性名称|OLE DB 属性名称|
|-----------------------|--------------------------|
|访问顺序|DBPROP_ACCESSORDER|
|阻止存储对象|DBPROP_BLOCKINGSTORAGEOBJECTS|
|书签类型|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|更改插入的行|DBPROP_CHANGEINSERTEDROWS|
|列特权|DBPROP_COLUMNRESTRICT|
|列集通知|DBPROP_NOTIFYCOLUMNSET|
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
|IRowsetResynch||
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
|唯一行|DBPROP_UNIQUEROWS|
|可更新性|DBPROP_UPDATABILITY|
|使用书签|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>命令动态属性
 将以下属性添加到 **命令** 对象的 **properties** 集合。

|ADO 属性名称|OLE DB 属性名称|
|-----------------------|--------------------------|
|访问顺序|DBPROP_ACCESSORDER|
|阻止存储对象|DBPROP_BLOCKINGSTORAGEOBJECTS|
|书签类型|DBPROP_BOOKMARKTYPE|
|Bookmarkable|DBPROP_IROWSETLOCATE|
|更改插入的行|DBPROP_CHANGEINSERTEDROWS|
|列特权|DBPROP_COLUMNRESTRICT|
|列集通知|DBPROP_NOTIFYCOLUMNSET|
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
|IRowsetResynch||
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
|跳过删除的书签|DBPROP_BOOKMARKSKIP|
|强行标识|DBPROP_STRONGIDENTITY|
|可更新性|DBPROP_UPDATABILITY|
|使用书签|DBPROP_BOOKMARKS|

 有关适用于 ODBC 的 Microsoft OLE DB 提供程序的特定实现和功能信息的详细信息，请参阅 MSDN 上的 [OLE DB 程序员参考](/previous-versions/windows/desktop/ms713643(v=vs.85)) 或访问数据访问和存储开发人员中心网站。

## <a name="see-also"></a>另请参阅
 [命令对象 (ado) ](../../reference/ado-api/command-object-ado.md) [COMMANDTEXT 属性 (Ado) ](../../reference/ado-api/commandtext-property-ado.md) [连接对象 (](../../reference/ado-api/connection-object-ado.md) ado) [ConnectionString 属性 (ado) ](../../reference/ado-api/connectionstring-property-ado.md) [Execute 方法](../../reference/ado-api/execute-method-ado-command.md) (Ado 命令) ado 记录 (ado 记录) ado 记录[集 (](../../reference/ado-api/provider-property-ado.md) [](../../reference/ado-api/recordset-object-ado.md) [](../../reference/ado-api/supports-method.md) ado[记录](../../reference/ado-api/open-method-ado-recordset.md)[集)  (](../../reference/ado-api/parameters-collection-ado.md) [ado) ](../../reference/ado-api/properties-collection-ado.md)