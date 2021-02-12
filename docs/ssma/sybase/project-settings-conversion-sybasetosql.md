---
description: 项目设置（转换）(SybaseToSQL)
title: " (转换的项目设置)  (SybaseToSQL) |Microsoft Docs"
ms.custom: ''
ms.date: 10/19/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: eeb80fa5-f530-4f21-beee-25f5a4b8ace6
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 66d1ce573f464f14e0b738fb4c461be031db64ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070052"
---
# <a name="project-settings-conversion-sybasetosql"></a>项目设置（转换）(SybaseToSQL)

"**项目设置**" 对话框的 "**转换**" 页包含用于自定义 SSMA 如何将 SAP 自适应服务器企业 (ASE) 语法转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure SQL 语法的设置。

"**项目设置**" 和 "**默认项目设置**" 对话框中提供了 "**转换**" 窗格：

- 如果要指定所有 SSMA 项目的设置，请在 " **工具** " 菜单上，选择 " **默认项目设置**"，单击左侧窗格底部的 " **常规** "，然后单击 " **转换**"。

- 若要指定当前项目的设置，请在 " **工具** " 菜单上选择 " **项目设置**"，单击左侧窗格底部的 " **常规** "，然后单击 " **转换**"。

## <a name="miscellaneous-section"></a>其他部分

### <a name="error"></a>@@ERROR

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL 和 ASE 使用不同的错误代码。

使用此设置来指定消息的类型 (警告或错误) 当在 ASE 代码中遇到对的引用时，SSMA 在输出或错误列表窗格中显示 `@@ERROR` 。

- 如果选择 " **转换" 并标记** 为 "警告"，则 SSMA 将转换这些语句，并将其标记为警告注释。
- 如果选择 " **标记并出错**"，则 SSMA 将跳过转换，并将语句标记为错误注释。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|转换并标记警告|
|乐观|转换并标记警告|
|完全|标记有错误|

### <a name="conversion-of-like-operator"></a>LIKE 运算符的转换

指定是否将 `LIKE` 操作数转换为与 SAP ASE 行为匹配。 此时，ASE 会以类似模式修整尾随空白。 解决方法是将右侧表达式强制转换为固定长度的数据类型，并具有最大精度。
  
- 选择 " **简单转换** " 可在不进行任何更正的情况下转换表达式。
- 若要使用 ASE 行为，请选择 " **强制转换为固定长度"。**
  
在 "模式" 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|简单转换|
|乐观|简单转换|
|完全|强制转换为固定长度|

### <a name="convert-or-cast-empty-strings-to-numeric-types"></a>将空字符串转换或强制转换为数值类型

指定如何在或表达式中处理空或空字符串， `CONVERT` `CAST` 并将数值类型作为 datatype 参数。 此设置可使用以下选项：

- 选择 " **简单转换** " 可在不进行任何更正的情况下转换表达式。
- 如果选择 **空字符串作为零数字** ，则字符串参数 `{s}` 将替换为 `CASE ltrim(rtrim({s})) WHEN "" THEN 0 else {s} END` expression。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|简单转换|
|乐观|简单转换|
|完全|空字符串（零数字）|

### <a name="concatenation-of-null"></a>空值的串联

此设置指定如何转换字符串与的连接 `NULL` 。 可以为此特定设置设置以下选项：

- 如果选择了 " **带 ISNULL 函数的 Wrap** " 选项，则串联中的每个非常量都将 `string_expression` 用 `ISNULL(string_expression)` `NULL` 空字符串替换，并将替换为空字符串。
- **保留当前语法** 将保留原始语法。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|保留当前语法|
|乐观|保留当前语法|
|完全|用 ISNULL 函数换行|

### <a name="conversion-of-empty-strings"></a>空字符串的转换

此设置指定如何转换空字符串。 可以为此特定设置设置以下选项：

- **将所有字符串表达式替换为空格**
- **用空格替换空字符串常量**

若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 行为，请选择 " **保留当前语法**"。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|保留当前语法|
|乐观|保留当前语法|
|完全|将所有字符串表达式替换为空格|

### <a name="convert-and-cast-binary-string-conversion"></a>转换和强制转换二进制字符串转换

二进制值到数字的转换可能会在不同的平台上返回不同的值。 例如，在 x86 处理器上， `CONVERT(integer, 0x00000100)` 在 `65536` ASE 中返回，但 `256` 在中返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 ASE 还根据字节顺序返回不同的值。

使用此设置可控制 SSMA 转换 `CONVERT` 和 `CAST` 包含二进制值的表达式的方式：

- 选择 " **简单转换** " 可以转换表达式，而无需任何警告或更正。 如果你知道 ASE 服务器的字节顺序不需要二进制值的任何更改，请使用此设置。
- 选择 " **转换" 和 "更正** "，让 SSMA 转换和更正表达式以便在上使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 文本常量中的字节顺序将会反转。 所有其他二进制值 (如二进制变量和列) 将被标记为有错误。 如果你知道 ASE 服务器的字节顺序需要更改二进制值，请使用此值。

选择 " **转换" 并标记** 为 "警告"，让 SSMA 转换和更正表达式，并将所有已转换的表达式标记为警告注释。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|转换并标记警告|
|乐观|简单转换|
|完全|转换和更正|

### <a name="dynamic-sql"></a>动态 SQL

使用此设置来指定消息的类型 (警告或错误) 在 ASE 代码中遇到动态 SQL 时，SSMA 在 **输出** 或 **错误列表** 窗格中显示。

- 如果选择 " **转换" 并标记** 为 "警告"，则 SSMA 将转换动态 SQL，并将语句标记为警告注释。
- 如果选择 " **标记并出错**"，则 SSMA 将跳过转换，并将语句标记为错误注释。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|转换并标记警告|
|乐观|转换并标记警告|
|完全|标记有错误|

### <a name="equality-check-conversion"></a>相等性检查转换

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 中，如果 `ANSI_NULLS` 设置为 on，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `UNKNOWN` 当任何相等比较包含一个值时，/Azure SQL 将返回 `NULL` 。 如果 `ANSI_NULLS` 为 off，则包含值的相等比较 `NULL` 在两个比较的列和表达式或两个表达式均为 true 时返回 true `NULL` 。 默认情况下 (`ANSINULL OFF`) SAP ASE 相等比较的行为类似于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL with `ANSI_NULLS OFF` 。

- 如果选择 **简单转换**，则 SSMA 会将 ASE 代码转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL 语法，而不会对值进行额外检查 `NULL` 。 如果 `ANSI_NULLS` 在/AZURE SQL 中，则使用此设置， `OFF` [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果要基于每个大小修订相等性比较，则使用此设置。
- 如果选择 " **考虑 NULL 值**"，则 SSMA 将 `NULL` 使用和子句添加对值的检查 `IS NULL` `IS NOT NULL` 。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|简单转换|
|乐观|简单转换|
|完全|考虑 NULL 值|

### <a name="format-strings"></a>格式字符串

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL 不再支持 `format_string` 和语句中的 `PRINT` 参数 `RAISERROR` 。 `format_string`允许在字符串中直接放置可替换参数，然后在运行时替换参数的参数。 相反， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 需要使用字符串文字或使用变量生成的字符串的完整字符串。 有关详细信息，请参阅 [PRINT ([!INCLUDE[tsql](../../includes/tsql-md.md)]) ](../../t-sql/language-elements/print-transact-sql.md) 主题。

当 SSMA 遇到 `format_string` 参数时，它可以使用变量生成字符串文本，或创建新变量并使用该变量生成字符串。

- 若要将字符串文本用于 `PRINT` 和 `RAISERROR` 函数，请选择 " **创建新字符串**"。

    在此模式下，如果 PRINT 或 RAISERROR 语句未使用占位符和局部变量，则语句将保持不变。 %% (%% ) 的双百分比字符将更改为打印字符串文本中的单个百分号字符%。

    如果 PRINT 或 RAISERROR 语句使用占位符和一个或多个局部变量，如以下示例中所示：

    ```sql
    PRINT 'Total: %1!%%', @percent
    ```

    SSMA 会将其转换为以下语法：

    ```sql
    PRINT 'Total: '+ CAST(@percent AS varchar(max)) + '%'
    ```

    如果 `format_string` 是变量，如以下语句中所示：  

    ```sql
    PRINT @fmt, @arg1, @arg2
    ```

    SSMA 无法执行简单的字符串转换，必须创建新的变量：

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 =
        REPLACE (@fmt, '%%', '%')
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        CAST (@arg1 AS varchar(max)))
    SET @print_format_1 =
        REPLACE (@print_format_1, '%2!',
        CAST (@arg2 AS varchar(max)))
    PRINT @print_format_1
    ```

    如果它使用 " **创建新字符串** " 模式，则 SSMA [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会假设选项 `CONCAT_NULL_YIELDS_NULL` 为 `OFF` 。 因此，SSMA 不检查空参数。

- 若要让 SSMA 为每个 and 语句生成一个新变量 `PRINT` `RAISERROR` ，然后将该变量用于字符串值，请选择 " **创建新变量**"。

    在此模式下，如果 `PRINT` 或 `RAISERROR` 语句不使用占位符和局部变量，则 SSMA 会将所有双精度百分比)  (字符替换为 `%%` 单百分号字符，以符合 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 语法。

    如果 `PRINT` 或 `RAISERROR` 语句使用占位符和一个或多个局部变量，如以下示例中所示：

    ```sql
    PRINT 'Total: %1!%%', @percent
    ```

    SSMA 会将其转换为以下语法：

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 = 'Total: %1!%'
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        ISNULL(CAST (@percent AS VARCHAR(max)), ''))
    PRINT @print_format_1
    ```

    如果 `format_string` 是变量，如以下语句中所示：

    ```sql
    PRINT @fmt, @arg1, @arg2
    ```

    SSMA 创建一个新变量，如下所示，检查每个参数中是否有 null 值：

    ```sql
    DECLARE @print_format_1 varchar(max)
    SET @print_format_1 =
        REPLACE (@fmt, '%%', '%')
    SET @print_format_1 =
        REPLACE (@print_format_1, '%1!',
        ISNULL(CAST (@arg1 AS varchar(max)),''))
    SET @print_format_1 =
        REPLACE (@print_format_1, '%2!',
        ISNULL(CAST (@arg2 AS varchar(max)),''))
    PRINT @print_format_1
    ```

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|创建新字符串|
|乐观|创建新字符串|
|完全|创建新变量|

### <a name="insert-an-explicit-value-into-a-timestamp-column"></a>将显式值插入时间戳列

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL 不支持将显式值插入时间戳列。

- 若要从语句中排除时间戳列 `INSERT` ，请选择 " **排除列**"。
- 若要在每次将 timestamp 列包含在语句中时打印错误消息 `INSERT` ，请选择 " **标记并显示错误**"。 在此模式下， `INSERT` 不会转换语句，并将其标记为错误注释。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|排除列|
|乐观|排除列|
|完全|标记有错误|

### <a name="store-temporary-objects-defined-in-procedures"></a>存储过程中定义的临时对象

此设置指定在转换过程中出现在过程中的临时对象定义是否应存储在源元数据中。

- 选择 **"是"** 以存储到元数据。
- 如果不需要存储对象，请选择 " **否** "。

|模型|值|
|-|-|
|默认|是|
|乐观|是|
|完全|否|

### <a name="proxy-table-conversion"></a>代理表转换

指定 ASE 代理表是否转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 表，或是否不进行转换以及代码是否标记有错误注释。

- 选择 " **转换** " 将代理表转换为常规表。
- 选择 " **标记为错误** "，以便只需将代理表代码标记为 "错误注释"。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|标记有错误|
|乐观|标记有错误|
|完全|标记有错误|

### <a name="raiserror-base-message-number"></a>RAISERROR 基本消息编号

ASE 用户消息存储在每个数据库中。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户消息集中存储，并通过 `sys.messages` 目录视图提供。 此外，ASE 用户消息从开始 `20000` ，但从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 开始的错误消息 `50001` 。

此设置指定要添加到 ASE 用户消息编号的数字，以将其转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户消息。 如果在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目录视图中有用户消息 `sys.messages` ，则可能需要将此数字更改为较高的值。 这是因为，转换后的消息数不会与现有消息号冲突。

注意以下事项：
  
- 范围内的 ASE 消息 `17000` - `19999` 来自 `sysmessages` 系统表，不进行转换。
- 如果语句中引用的消息号 `RAISERROR` 是常量，则 SSMA 会将基本消息号添加到常量，以确定新的用户消息号。
- 如果引用的消息号是变量或表达式，则 SSMA 将创建一个中间局部变量。
- 在 **乐观模式** 下，SSMA 假设 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 选项 `CONCAT_NULL_YIELDS_NULL` 为 `OFF` ，且不会检查 `NULL` 参数。
- 在 **完整模式** 下，SSMA 检查 `NULL` 参数。
- `RAISERROR``arg-list`不转换带参数的。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|30001|
|乐观|30001|
|完全|30001|

### <a name="system-objects"></a>系统对象

使用此设置来指定消息的类型 (警告或错误，在该消息使用 ASE 系统对象时，将在 **输出** 或 **错误列表** 窗格中显示 SSMA) 。

- 如果选择 " **转换" 并标记** 为 "警告"，则 SSMA 会将引用转换为系统对象，并将带有警告注释的语句标记为。
- 如果选择 " **标记并出错**"，则 SSMA 将不会将引用转换为系统对象，并且会将语句标记为带有错误注释。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|转换并标记警告|
|乐观|转换并标记警告|
|完全|标记有错误|

### <a name="unresolved-identifiers"></a>解析的标识符

使用此设置可指定在无法解析标识符时 SSMA 显示在 **输出** 或 **错误列表** 窗格中 (警告或错误) 消息的类型。

- 如果选择 " **转换" 并标记** 为 "警告"，则 SSMA 将尝试将引用转换为未解析的标识符，并将语句标记为带有警告注释。
- 如果选择 " **标记并出错**"，则 SSMA 将不会将引用转换为未解析的标识符，并将标记语句替换为错误注释。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|转换并标记警告|
|乐观|转换并标记警告|
|完全|标记有错误|

## <a name="system-functions-section"></a>系统函数部分

### <a name="charindex-function"></a>CHARINDEX 函数

在 ASE 中 `CHARINDEX` ， `NULL` 仅当所有输入表达式都为时才返回 `NULL` 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`NULL`如果任何输入表达式为，则/AZURE SQL 将返回 `NULL` 。

- 若要使用 ASE 行为，请选择 " **Replace function**"。 对函数的所有调用 `CHARINDEX` 都将替换为对 `CHARINDEX_VARCHAR` 或  `CHARINDEX_NVARCHAR` 用户定义函数的调用，具体取决于在用户数据库中 (创建的参数类型（在架构名称下） `s2ss`) 用于模拟 SAP ASE 行为。
- 若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 行为，请选择 " **保留当前语法**"。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|保留当前语法|
|乐观|保留当前语法|
|完全|Replace 函数|
  
### <a name="datalength-function"></a>DATALENGTH 函数

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`DATALENGTH`当值为单个空格时，/AZURE SQL 和 ASE 的值不同。 在这种情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 返回 `0` 和 ASE 返回 `1` 。

- 若要使用 ASE 行为，请选择 " **Replace function**"。 对 `DATALENGTH` 函数的所有调用都将替换为 `CASE` 表达式来模拟 SAP ASE 行为。
- 若要使用默认的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 行为，请选择 " **保留当前的语法**"。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|保留当前语法|
|乐观|保留当前语法|
|完全|Replace 函数|

### <a name="index_col-function"></a>INDEX_COL 函数

ASE 支持函数的可选 `user_id` 参数 `INDEX_COL` ; 但是， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL 不支持此参数。 如果使用 `user_id` 参数，则此函数不能转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /Azure SQL 语法。

- 若要使用 ASE 行为，请选择 " **转换函数**"。 如果代码包含 `user_id` 参数，则 SSMA 将显示错误。
- 若要在每次遇到时显示错误消息 `INDEX_COL` ，请选择 " **标记并显示错误**"。 SSMA 不会将引用转换为函数，并且会将语句标记为带有错误注释。

|模型|值|
|-|-|
|默认|标记有错误|
|乐观|标记有错误|
|完全|标记有错误|

### <a name="index_colorder-function"></a>INDEX_COLORDER 函数

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL 没有 `INDEX_COLORDER` 系统函数。

- 若要使用 ASE 行为，请选择 " **转换函数**"。 对函数的所有调用 `INDEX_COLORDER` 都将被替换为具有相同名称的用户定义函数的调用， `INDEX_COLORDER` (在用户数据库中创建，该函数将 `s2ss` 模拟 SAP ASE 行为) 。
- 若要在每次遇到时打印错误消息 `INDEX_COLORDER` ，请选择 " **标记并显示错误**"。 SSMA 不会将引用转换为函数，并且会将语句标记为带有错误注释。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|标记有错误|
|乐观|标记有错误|
|完全|标记有错误|

### <a name="left-and-right-functions"></a>左函数和右函数

`LEFT` 和 `RIGHT` ASE 中的函数对于负值长度参数的行为有所不同。

- 若要使用 ASE 行为，请选择 " **Replace Function**"。 然后，将 length 参数替换为 `CASE` 将 `NULL` 为负值返回的表达式。
- 若要使用 SQL Server 行为，请选择 " **保留当前的语法**"。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|保留当前语法|
|乐观|保留当前语法|
|完全|Replace 函数|

> [!NOTE]
> 如果 length 参数是文本值而不是复杂表达式，则始终替换长度值，而不 `NULL` 考虑项目设置。

### <a name="next_identity-function"></a>NEXT_IDENTITY 函数

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Azure SQL 没有 `NEXT_IDENTITY` 系统函数。

- 若要使用 ASE 行为，请选择 " **转换函数**"。 对函数的所有调用 `NEXT_IDENTITY` 都将替换为 `(IDENT_CURRENT(parameter Value) + IDENT_INCR(parameter Value)` 模拟 SAP ASE 行为的 expression。
- 若要在每次遇到时打印错误消息 `NEXT_IDENTITY` ，请选择 " **标记并显示错误**"。 SSMA 不会将引用转换为函数，并且会将语句标记为带有错误注释。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|标记有错误|
|乐观|标记有错误|
|完全|标记有错误|

**默认/乐观/完整模式：** 标记有错误

### <a name="patindex-function"></a>PATINDEX 函数

指定是否转换 `PATINDEX` 函数以匹配 SAP ASE 行为。 此时，ASE 会修剪搜索模式中的尾随空白。 解决方法是将值表达式强制转换为固定长度数据类型，该类型具有最大精度，并将 `rtrim` 函数应用到搜索模式。

- 若要使用 ASE 行为，请选择 " **使用**"。  
- 若要使用默认的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 行为，请选择 "不 **使用**"。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|请勿使用|
|乐观|请勿使用|
|完全|用途|

### <a name="replicate-function"></a>REPLICATE 函数

`REPLICATE`函数将字符串重复指定的次数。 在 ASE 中，如果指定将字符串重复零次，则结果为 `NULL` 。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 中，结果为空字符串。

- 若要使用 ASE 行为，请选择 " **Replace function**"。 对函数的所有调用 `REPLICATE` 都将替换为对 `REPLICATE_VARCHAR` 或 `REPLICATE_NVARCHAR` 用户定义函数的调用，具体取决于在用户数据库中 (创建的参数类型（在架构名称下） `s2ss`) 用于模拟 SAP ASE 行为。
- 若要使用默认的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 行为，请选择 " **Replace Function**"。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|Replace 函数|
|乐观|Replace 函数|
|完全|Replace 函数|

### <a name="trim-ltrim-rtrim-function"></a>TRIM (LTRIM，RTRIM) 函数

此设置指定是将对的调用替换为 `TRIM` `LTRIM` `RTRIM` SAP ASE 等效语法函数还是保留当前语法。 此特定设置有以下选项：

- **Replace 函数**
- **保留当前语法**

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|Replace 函数|
|乐观|Replace 函数|
|完全|Replace 函数|

### <a name="substring-function"></a>SUBSTRING 函数

在 ASE 中， `SUBSTRING(expression, start, length)` `NULL` 如果指定的起始值大于表达式中的字符数，则函数返回; 如果 length 等于零，则返回。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 中，等效表达式返回空字符串。

- 若要使用 ASE 行为，请选择 " **Replace function**"。 对函数的所有调用 `SUBSTRING` 都将替换为对 `SUBSTRING_VARCHAR` 或 `SUBSTRING_NVARCHAR` 或 `SUBSTRING_VARBINARY` 用户定义函数的调用，具体取决于在用户数据库中 (创建的参数类型（在架构名称下） `s2ss`) 用于模拟 SAP ASE 行为。
- 若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] /AZURE SQL 行为，请选择 " **保留当前语法**"。

在 " **模式** " 框中选择转换模式时，SSMA 将应用以下设置：

|模型|值|
|-|-|
|默认|保留当前语法|
|乐观|保留当前语法|
|完全|Replace 函数|

## <a name="tables-section"></a>表部分

### <a name="add-primary-key"></a>添加主密钥

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]如果 SAP ASE 表没有主键或唯一索引，则在或 AZURE SQL 表中创建新的主键。

|模型|值|
|-|-|
|默认|否|
|乐观|否|
|完全|是|

> [!NOTE]
> 连接到 Azure SQL 时，默认为 **"是"** 。

## <a name="see-also"></a>另请参阅

[用户界面参考 (SybaseToSQL)](../../ssma/sybase/user-interface-reference-sybasetosql.md)
