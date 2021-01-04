---
title: 扩展性框架 API
titleSuffix: SQL Server Language Extensions
description: 可以使用 Extensibility Framework 为 SQL Server 编写编程语言扩展插件。 适用于 Microsoft SQL Server 的 Extensibility Framework API 是一种 API，语言扩展插件可使用它来与 SQL Server 交互和交换数据。
author: dphansen
ms.author: davidph
ms.date: 10/09/2020
ms.topic: reference
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 244a8ce680704b0b25f9d0499e1a7802661d56cd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471688"
---
# <a name="extensibility-framework-api-for-sql-server"></a>适用于 SQL Server 的 Extensibility Framework API
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

可以使用 Extensibility Framework 为 SQL Server 编写编程语言扩展插件。 适用于 Microsoft SQL Server 的 Extensibility Framework API 是一种 API，语言扩展插件可使用它来与 SQL Server 交互和交换数据。

作为语言扩展插件作者，你可以将此参考资料与开放源代码语言扩展插件结合使用，以了解如何使用 API 编写自己的语言扩展插件。 你可以在 [aka.ms/mssql-lang-extensions](https://aka.ms/mssql-lang-extensions) 中找到语言扩展插件的源代码。

在下面查找有关所有 API 函数的语法和参数信息。

## <a name="return-value"></a>返回值

所有函数都返回 SQLRETURN 参数  。 如果该值是 SQL_SUCCESS 以外的任何值，则将被视为错误，并且脚本的执行将停止  。

## <a name="standard-output"></a>标准输出

标准输出或错误流扩展插件的任何输出都将追溯到会话的日志文件，并最终追溯到 SQL Server（类似于“SSMS 消息”选项卡中显示的任何内容）。


## <a name="init"></a>Init

此函数仅调用一次，用于初始化运行时以执行。 

### <a name="syntax"></a>语法

```cpp
SQLRETURN Init(
    SQLCHAR *ExtensionParams,
    SQLULEN ExtensionParamsLength,
    SQLCHAR *ExtensionPath,
    SQLULEN ExtensionPathLength,
    SQLCHAR *PublicLibraryPath,
    SQLULEN PublicLibraryPathLength,
    SQLCHAR *PrivateLibraryPath,
    SQLULEN PrivateLibraryPathLength
);
```

### <a name="arguments"></a>参数

ExtensionParams   
\[输入\] 以 NULL 结尾的字符串，其中包含在 [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) 或 [ALTER EXTERNAL LANGUAGE](../../t-sql/statements/alter-external-language-transact-sql.md) 期间提供的 `PARAMETERS` 值。

ExtensionParamsLength   
\[输入\] ExtensionParams 的长度（以字节为单位，不包含 NULL 结尾字符）  。

ExtensionPath   
\[输入\] 以 NULL 结尾的 UTF-8 字符串，其中包含扩展插件安装目录的绝对路径。

ExtensionPathLength   
\[输入\] ExtensionPath 的长度（以字节为单位，不包含 NULL 结尾字符）  。

PublicLibraryPath   
\[输入\] 以 NULL 结尾的 UTF-8 字符串，其中包含此外部语言的公共外部库目录的绝对路径。

PublicLibraryPathLength   
\[输入\] PublicLibraryPath 的长度（以字节为单位，不包含 NULL 结尾字符）  。

PrivateLibraryPath   
\[输入\] 以 NULL 结尾的 UTF-8 字符串，其中包含此用户和此外部语言的专用外部库目录的绝对路径。

PrivateLibraryPathLength   
\[输入\] PrivateLibraryPath 的长度（以字节为单位，不包含 NULL 结尾字符）  。

## <a name="initsession"></a>InitSession

每个会话调用此函数一次，用于初始化会话特定的设置。

### <a name="syntax"></a>语法

```cpp
SQLRETURN InitSession(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLCHAR*        Script,
    SQLULEN         ScriptLength,
    SQLUSMALLINT    InputSchemaColumnsNumber,
    SQLUSMALLINT    ParametersNumber
    SQLCHAR*        InputDataName,
    SQLUSMALLINT    InputDataNameLength,
    SQLCHAR*        OutputDataName,
    SQLUSMALLINT    OutputDataNameLength
);
```

### <a name="arguments"></a>参数

*SessionId*  
\[输入\] 唯一标识此脚本会话的 GUID。

*TaskId*  
\[输入\] 唯一标识此执行过程的整数。

当 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中包含 `@parallel = 1` 时，此值的范围从 0 到查询的并行度。

*脚本*  
\[输入\] 以 NULL 结尾的 UTF-8 字符串，其中包含 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@script`。

ScriptLength   
\[输入\] ScriptScript 的长度（以字节为单位，不包含 NULL 结尾字符）  。

InputSchemaColumnsNumber   
\[输入\] 结果集中的列数，该结果集来自 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@input_data_1`。

ParametersNumber   
\[输入\] 输入参数的数目，这些输入参数来自 [sp_execute_external_script ](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@params`。

InputDataName   
\[输入\] 以 NULL 结尾的 UTF-8 字符串，其中包含 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@input_data_1_name`。

InputDataNameLength   
\[输入\] InputDataName 的长度（以字节为单位，不包含 NULL 结尾字符）  。

OutputDataName   
\[输入\] 以 NULL 结尾的 UTF-8 字符串，其中包含 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@output_data_1_name`。

OutputDataNameLength   
\[输入\] OutputDataName 的长度（以字节为单位，不包含 NULL 结尾字符）  。

## <a name="initcolumn"></a>InitColumn

初始化有关特定会话的给定列的信息。

将对结果集中的每一列调用此函数，该结果集来自 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@input_data_1`。

此结果集的列结构称为“输入架构”  。

### <a name="syntax"></a>语法

```cpp
SQLRETURN InitColumn(
    SQLGUID       SessionId,
    SQLUSMALLINT  TaskId,
    SQLUSMALLINT  ColumnNumber,
    SQLCHAR*      ColumnName,
    SQLSMALLINT   ColumnNameLength,
    SQLSMALLINT   DataType,
    SQLULEN       ColumnSize,
    SQLSMALLINT   DecimalDigits,
    SQLSMALLINT   Nullable,
    SQLSMALLINT   PartitionByNumber,
    SQLSMALLINT   OrderByNumber
);
```

### <a name="arguments"></a>参数

*SessionId*  
\[输入\] 唯一标识此脚本会话的 GUID。

*TaskId*  
\[输入\] 唯一标识此执行过程的整数。

当 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中包含 `@parallel = 1` 时，此值的范围从 0 到查询的并行度。

ColumnNumber   
\[输入\] 一个整数，标识此列在输入架构中的索引。 列按从 0 开始的递增顺序进行编号。

*ColumnName*  
\[输入\] 以 NULL 结尾的 UTF-8 字符串，其中包含列名称。

ColumnNameLength   
\[输入\] ColumnName 的长度（以字节为单位，不包含 NULL 结尾字符）  。

*DataType*  
\[输入\] 标识此列数据类型的 ODBC C 类型。

ColumnSize   
\[输入\] 此列中基础数据的最大大小（以字节为单位）。

对于 SQL_C_CHAR、SQL_C_WCHAR 和 SQL_C_BINARY 数据类型，大于 8000 的值表明此列表示 LOB 对象，最大大小为 2 GB。

DecimalDigits   
\[输入\] 此列中基础数据的小数位数，由[小数位数](../../odbc/reference/appendixes/decimal-digits.md)定义。

*可以为 Null*  
\[输入\] 一个值，表明此列是否可以包含 NULL 值。 可能的值：

- SQL_NO_NULLS：列不能包含 NULL 值。
- SQL_NULLABLE：列可以包含 NULL 值。

PartitionByNumber   
\[输入\] 一个值，指示此列在 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@input_data_1_partition_by_columns` 序列中的索引。 列按从 0 开始的递增顺序进行编号。 如果序列中不包含此列，则值为 -1。

OrderByNumber   
\[输入\] 一个值，指示此列在 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@input_data_1_order_by_columns` 序列中的索引。 列按从 0 开始的递增顺序进行编号。 如果序列中不包含此列，则值为 -1。

## <a name="initparam"></a>InitParam

初始化有关特定会话的给定输入参数的信息。

将对来自 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@params` 的每一个参数调用此函数。

### <a name="syntax"></a>语法

```cpp
SQLRETURN InitParam(
    SQLGUID      SessionId,
    SQLUSMALLINT TaskId,
    SQLUSMALLINT ParamNumber,
    SQLCHAR*     ParamName,
    SQLSMALLINT  ParamNameLength,
    SQLSMALLINT  DataType,
    SQLULEN      ParamSize,
    SQLSMALLINT  DecimalDigits,
    SQLPOINTER   ParamValue,
    SQLINTEGER   StrLen_or_Ind,
    SQLSMALLINT  InputOutputType
);
```

### <a name="arguments"></a>参数

*SessionId*  
\[输入\] 唯一标识此脚本会话的 GUID。

*TaskId*  
\[输入\] 唯一标识此执行过程的整数。

当 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中包含 `@parallel = 1` 时，此值的范围从 0 到查询的并行度。

ParamNumber   
\[输入\] 一个整数，标识此参数的索引。 参数按从 0 开始的递增顺序进行编号。

ParamName   
\[输入\] 以 NULL 结尾的 UTF-8 字符串，其中包含参数名称。

ParamNameLength   
\[输入\] ParamName 的长度（以字节为单位，不包含 NULL 结尾字符）  。

*DataType*  
\[输入\] 标识此参数数据类型的 ODBC C 类型。

ParamSize   
\[输入\] 此参数中基础数据的最大大小（以字节为单位）。

对于 SQL_C_CHAR、SQL_C_WCHAR 和 SQL_C_BINARY 数据类型，大于 8000 的值表明此参数表示 LOB 对象，最大大小为 2 GB。

DecimalDigits   
\[输入\] 此参数中基础数据的小数位数，由[小数位数](../../odbc/reference/appendixes/decimal-digits.md)定义。

ParamValue   
\[输入\] 一个指针，指向包含参数值的缓冲区。

StrLen_or_Ind   
\[输入\] 一个整数值，指示 ParamValue 的长度（以字节为单位），或指示数据为 NULL 的 SQL_NULL_DATA  。

如果列不可为 null，且不表示以下任何一个数据类型，则可以忽略 StrLen_or_Ind\[col\]：SQL_C_CHAR、SQL_C_WCHAR 和 SQL_C_BINARY、SQL_C_NUMERIC 或 SQL_C_TYPE_TIMESTAMP。 否则，它将指向具有 \[RowsNumber\] 元素的有效数组，其中每个元素包含其长度或 NULL 指示器数据。

InputOutputType   
\[输入\] 参数的类型。 InputOutputType 参数为以下其中一个值  ：

- SQL_PARAM_INPUT
- SQL_PARAM_INPUT_OUTPUT

## <a name="execute"></a>执行

执行 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@script`。

可多次调用此函数。 将对 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@input_data_1_partition_by_columns` 中的每个流块和每个分区调用一次。


### <a name="syntax"></a>语法

```cpp
SQLRETURN Execute(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN         RowsNumber,
    SQLPOINTER*     Data,
    SQLINTEGER**    StrLen_or_Ind,
    SQLUSMALLINT*   OutputSchemaColumnsNumber
);
```

### <a name="arguments"></a>参数

*SessionId*  
\[输入\] 唯一标识此脚本会话的 GUID。

*TaskId*  
\[输入\] 唯一标识此执行过程的整数。

当 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中包含 `@parallel = 1` 时，此值的范围从 0 到查询的并行度。

RowsNumber   
\[输入\] Data 中的行数  。

*数据*  
\[输入\] 一个二维数组，其中包含 `@input_data_1` n [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 的结果集。

列总数为在 [InitSession](#initsession) 调用中收到的 InputSchemaColumnsNumber  。 每一列都包含 RowsNumber 元素，这些元素应根据 [InitColumn](#initcolumn) 中的列类型进行解释  。

StrLen_or_Ind 中指示为 NULL 的元素不保证有效，应忽略  。

StrLen_or_Ind   
\[输入\] 一个二维数组，其中包含“数据”中每个值的长度/NULL 指示器。 每个单元格的可能值：

- n，其中 n > 0。 指示数据的长度（以字节为单位）
- SQL_NULL_DATA，指示 NULL 值。

列总数为在 [InitSession](#initsession) 调用中收到的 InputSchemaColumnsNumber  。 每一列都包含 RowsNumber 元素，这些元素应根据 [InitColumn](#initcolumn) 中的列类型进行解释  。

如果列不可为 NULL 且不表示以下任何一个数据类型，则可以忽略 StrLen_or_Ind\[col\]：SQL_C_CHAR、SQL_C_WCHAR 和 SQL_C_BINARY、SQL_C_NUMERIC 或 SQL_C_TYPE_TIMESTAMP。 否则，它将指向具有 RowsNumber 元素的有效数组，每个元素包含其长度或 NULL 指示器数据  。

OutputSchemaColumnsNumber   
\[输出\] 指向缓冲区的指针，该缓冲区用于在 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@script` 的预期结果集中返回列数。

## <a name="getresultcolumn"></a>GetResultColumn

检索有关特定会话的给定输出列的信息。

将对结果集中的每一列调用此函数，该结果集来自 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@script`。
此结果集的列结构成为“输出架构”。

### <a name="syntax"></a>语法

```cpp
SQLRETURN GetResultColumn(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLUSMALLINT    ColumnNumber,
    SQLSMALLINT*    DataType,
    SQLINTEGER*     ColumnSize,
    SQLSMALLINT*    DecimalDigits,
    SQLSMALLINT*    Nullable
);
```

### <a name="arguments"></a>参数

*SessionId*  
\[输入\] 唯一标识此脚本会话的 GUID。

*TaskId*  
\[输入\] 唯一标识此执行过程的整数。

当 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中包含 `@parallel = 1` 时，此值的范围从 0 到查询的并行度。

ColumnNumber   
\[输入\] 一个整数，标识此列在输出架构中的索引。 列按从 0 开始的递增顺序进行编号。

*DataType*  
\[输出\] 指向缓冲区的指针，该缓冲区包含用于标识此列数据类型的 ODBC C 类型。

ColumnSize   
\[输出\] 指向缓冲区的指针，该缓冲区包含此列中基础数据的最大大小（以字节为单位）。

DecimalDigits   
\[输出\] 指向缓冲区的指针，该缓冲区包含此列中基础数据的小数位数，由[小数位数](../../odbc/reference/appendixes/decimal-digits.md)定义。 如果小数位数不能确定或不适用，则将放弃该值。

*可以为 Null*  
\[输出\] 指向缓冲区的指针，该缓冲区包含指示此列是否可以包含 NULL 值的值。 可能的值：

- SQL_NO_NULLS：列不能包含 NULL 值。
- SQL_NULLABLE：列可以包含 NULL 值。

如果传递了其他值，则将停止执行。

## <a name="getresults"></a>GetResults

检索执行 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@script` 返回的结果集。

可多次调用此函数。 将对 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@input_data_1_partition_by_columns` 中的每个流块和每个分区调用一次。


### <a name="syntax"></a>语法

```cpp
SQLRETURN GetResults(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN*        RowsNumber,
    SQLPOINTER**    Data,
    SQLINTEGER**_   StrLen_or_Ind
);
```

### <a name="arguments"></a>自变量

SessionId  
\[输入\] 唯一标识此脚本会话的 GUID。

*TaskId*  
\[输入\] 唯一标识此执行过程的整数。

当 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中包含 `@parallel = 1` 时，此值的范围从 0 到查询的并行度。

RowsNumber   
\[输出\] 指向缓冲区的指针，该缓冲区包含 Data 中的行数  。

*数据*  
\[输出\] 指向二维数组的指针，该二维数组由包含 `@script` n [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 的结果集的扩展插件分配。

列总数应为在 [Execute](#execute) 调用中检索到的 OutputSchemaColumnsNumber  。 每一列都应包含 RowsNumber 元素，这些元素应根据 [GetResultColumn](#getresultcolumn) 中的列类型进行解释  。

StrLen_or_Ind   
\[输出\] 指向二维数组的指针，该二维数组由包含“数据”中每个值的长度/NULL 指示器的扩展插件分配。 每个单元格的可能值：

- n，其中 n > 0。 指示数据的长度（以字节为单位）
- SQL_NULL_DATA，指示 NULL 值。

列总数应为在 [Execute](#execute) 调用中接收到的 OutputSchemaColumnsNumber  。 每一列都包含 RowsNumber 元素，这些元素应根据 [GetResultColumn](#getresultcolumn) 中的列类型进行解释  。

如果列不可为 NULL，且不表示以下任何一个数据类型，则将忽略 StrLen_or_Ind\[col\]：SQL_C_CHAR、SQL_C_WCHAR 和 SQL_C_BINARY [添加日期]。 否则，它将指向具有 RowsNumber 元素的有效数组，每个元素包含其长度或 NULL 指示器数据  。

## <a name="getoutputparam"></a>GetOutputParam

检索有关特定会话的给定输出参数的信息。

将对 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中的 `@params` 中的每一个标记为 OUTPUT 的参数调用此函数。

### <a name="syntax"></a>语法

```cpp
SQLRETURN GetOutputParam(
    SQLGUID        SessionId,
    SQLUSMALLINT   ParamNumber,
    SQLPOINTER*    ParamValue,
    SQLINTEGER*    StrLen_or_Ind
);
```

### <a name="arguments"></a>参数

*SessionId*  
\[输入\] 唯一标识此脚本会话的 GUID。

ParamValue   
\[输出\] 一个指针，指向包含参数值的缓冲区。

StrLen_or_Ind \[输出\] 指向缓冲区的指针，该缓冲区包含指示 ParamValue 长度（以字节为单位）的整数值，或指示数据为 NULL 的 SQL_NULL_DATA   。

## <a name="getinterfaceversion"></a>GetInterfaceVersion

检索接口版本。
此函数返回表示扩展插件接口版本的整数。 支持的值：
1. 版本 1 是初始 API 版本。 在 SQL Server 2019 RTM 中受支持。
1. 版本 2 添加了对 InstallExternalLibrary 和 UninstallExternalLibrary API 的支持，并且在 SQL Server 2019 CU3 中支持。                            

### <a name="syntax"></a>语法

```cpp
SQLUSMALLINT GetInterfaceVersion();
```

## <a name="cleanupsession"></a>CleanupSession

清除每个会话的信息。

### <a name="syntax"></a>语法

```cpp
SQLRETURN CleanupSession(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId
);
```

### <a name="arguments"></a>参数

*SessionId*  
\[输入\] 唯一标识此脚本会话的 GUID。

*TaskId*  
\[输入\] 唯一标识此执行过程的整数。

当 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中包含 `@parallel = 1` 时，此值的范围从 0 到查询的并行度。

## <a name="cleanup"></a>清理

清理全局共享信息（如 JVM）。

### <a name="syntax"></a>语法

```cpp
SQLRETURN Cleanup();
```

## <a name="gettelemetryresults"></a>GetTelemetryResults

检索扩展插件中的遥测（键值对）数据。 该函数是可选的，不需要实现。 遥测由 `dm_db_external_script_execution_stats` 动态管理视图 (DMV) 公开。

有一个名为 script_executions 的计数器，由框架发送。 扩展插件不应使用该名称。

每个遥测项都是一个键值对。 键为字符串，值为 64 位整数 - 计数器。 因此，输出包括两个逻辑数组：名称及其相应的计数器。 每个数组都是输出。

每个数组的长度是 RowsNumber，这是一个输出  。 第一个逻辑输出包含指向字符串的指针，因此，它由两个数组表示：CounterNames（实际字符串数据）和 CounterNamesLength（每个字符串的长度）   。 第二个逻辑输出存储在 CounterValues 指针中  。

### <a name="syntax"></a>语法

```cpp
SQLRETURN GetTelemetryResults(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId,
    SQLUINTEGER    *RowsNumber,
    SQLCHAR        ***CounterNames,
    SQLINTEGER      **CounterNamesLength,
    SQLBIGINT       **CounterValues
);
```

### <a name="arguments"></a>参数

*SessionId*  
\[输入\] 唯一标识此脚本会话的 GUID。

*TaskId*  
\[输入\] 唯一标识此执行过程的整数。

当 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中包含 `@parallel = 1` 时，此值的范围从 0 到查询的并行度。

RowsNumber   
\[输出\] 键值对的数目。

CounterNames   
\[输出\] 包含键的字符串数据。

CounterNamesLength   
\[输出\] 每个键字符串的长度。

CounterValues   
\[输出\] 包含这些值的 64 位整数数据。

## <a name="installexternallibrary"></a>InstallExternalLibrary

安装库。 该函数是可选的，不需要实现。 默认实现是将库的内容（请参阅 [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md)）复制到位于正确位置的文件。 文件名为库名称。

### <a name="syntax"></a>语法

```cpp
SQLRETURN InstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryFile,
    SQLINTEGER LibraryFileLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>参数

SetupSessionId   
\[输入\] 唯一标识此脚本会话的 GUID。

LibraryName   
\[输入\] 库名称。

LibraryNameLength   
\[输入\] 库名称的长度。

LibraryFile   
\[输入\] 库文件的路径（作为字符串），该库文件包含由 [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) 指定的二进制内容。

LibraryFileLength   
\[输入\] LibraryFile 字符串的长度。

LibraryInstallDirectory   
\[输入\] 安装库的根目录。

LibraryInstallDirectoryLength   
\[输入\] LibraryInstallDirectory 字符串的长度。

LibraryError   
\[输出\] 可选的输出参数。 如果库安装过程中出现错误，LibraryError 将指向描述错误的字符串。

LibraryErrorLength   
\[输出\] LibraryError 字符串的长度。

## <a name="uninstallexternallibrary"></a>UninstallExternalLibrary

卸载库。 该函数是可选的，不需要实现。 默认实现是撤消 InstallExternalLibrary 的默认实现完成的工作。 默认实现会删除 LibraryInstallDirectory 下的 LibraryName 文件的内容   。

### <a name="syntax"></a>语法

```cpp
SQLRETURN UninstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>参数

SetupSessionId   
\[输入\] 唯一标识此脚本会话的 GUID。

LibraryName   
\[输入\] 库名称

LibraryNameLength   
\[输入\] 库名称的长度

LibraryFile   
\[输入\] 库文件的路径（作为字符串），该库文件包含由 CREATE EXTERNAL LIBRARY 指定的二进制内容

LibraryFileLength   
\[输入\] LibraryFile 字符串的长度

LibraryInstallDirectory   
\[输入\] 安装库的根目录

LibraryInstallDirectoryLength   
\[输入\] LibraryInstallDirectory 字符串的长度。

LibraryError   
\[输出\] 库错误字符串。

LibraryErrorLength   
\[输出\] LibraryError 字符串的长度。

## <a name="next-steps"></a>后续步骤

- [适用于 SQL Server 的Microsoft Java Extensibility SDK](../how-to/extensibility-sdk-java-sql-server.md)
- [Python 自定义运行时](../../machine-learning/install/custom-runtime-python.md)
- [R 自定义运行时](../../machine-learning/install/custom-runtime-r.md)。