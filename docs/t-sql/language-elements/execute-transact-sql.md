---
description: EXECUTE (Transact-SQL)
title: EXECUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXEC
- EXECUTE_TSQL
- EXECUTE
- EXEC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote stored procedures [SQL Server]
- command strings [SQL Server]
- extended stored procedures [SQL Server], executing
- stored procedures [SQL Server], executing
- Transact-SQL statements, executing
- strings [SQL Server], executing
- statements [SQL Server], executing
- context switching [SQL Server], execution context
- user-defined functions [SQL Server], executing
- character strings [SQL Server], executing
- switching execution context
- EXECUTE statement
ms.assetid: bc806b71-cc55-470a-913e-c5f761d5c4b7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8713080c2b234c55fb81227e9a7f8823abf66691
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342603"
---
# <a name="execute-transact-sql"></a>EXECUTE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理中的命令字符串、字符串或执行下列模块之一：系统存储过程、用户定义存储过程、CLR 存储过程、标量值用户定义函数或扩展存储过程。 EXECUTE 语句可用于向链接服务器发送传递命令。 此外，还可以显式设置执行字符串或命令的上下文。 可以使用 WITH RESULT SETS 选项定义结果集的元数据。
  
> [!IMPORTANT]  
>  在使用字符串调用 EXECUTE 之前，请先验证该字符串。 永远不要执行由未经验证的用户输入构造的命令。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  

::: moniker range=">=sql-server-ver15"
以下代码块显示 SQL Server 2019 中的语法。 或者，可以参阅 [SQL Server 2017 及更早版本中的语法](execute-transact-sql.md?view=sql-server-2017&preserve-view=true)。 

```syntaxsql
-- Syntax for SQL Server 2019
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name [ ;number ] | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH <execute_option> [ ,...n ] ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS { LOGIN | USER } = ' name ' ]  
[;]  
  
Execute a pass-through command against a linked server  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'command_string [ ? ]' } [ + ...n ]  
        [ { , { value | @variable [ OUTPUT ] } } [ ...n ] ]  
    )   
    [ AS { LOGIN | USER } = ' name ' ]  
    [ AT linked_server_name ]  
    [ AT DATA_SOURCE data_source_name ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML   
}  
```  
::: moniker-end

::: moniker range=">=sql-server-2016"

以下代码块显示 SQL Server 2017 及更早版本中的语法。 或者，可以参阅 [SQL Server 2019 中的语法](execute-transact-sql.md?view=sql-server-ver15&preserve-view=true)。


```syntaxsql
-- Syntax for SQL Server 2017 and earleir  
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name [ ;number ] | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH <execute_option> [ ,...n ] ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS { LOGIN | USER } = ' name ' ]  
[;]  
  
Execute a pass-through command against a linked server  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'command_string [ ? ]' } [ + ...n ]  
        [ { , { value | @variable [ OUTPUT ] } } [ ...n ] ]  
    )   
    [ AS { LOGIN | USER } = ' name ' ]  
    [ AT linked_server_name ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML   
}  
```  
::: moniker-end

  
```syntaxsql
-- In-Memory OLTP   

Execute a natively compiled, scalar user-defined function  
[ { EXEC | EXECUTE } ]   
    {   
      [ @return_status = ]   
      { module_name | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable   
                           | [ DEFAULT ]   
                           }  
        ]   
      [ ,...n ]   
      [ WITH <execute_option> [ ,...n ] ]   
    }  
<execute_option>::=  
{  
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}  
```  
  
```syntaxsql
-- Syntax for Azure SQL Database   
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name  | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH RECOMPILE ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS {  USER } = ' name ' ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML  
  
```

  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

-- Execute a stored procedure  
[ { EXEC | EXECUTE } ]  
    procedure_name   
        [ { value | @variable [ OUT | OUTPUT ] } ] [ ,...n ] }  
[;]  
  
-- Execute a SQL string  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'tsql_string' } [ +...n ] )  
[;]  
```  


  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 @*return_status*  
 可选的整型变量，存储模块的返回状态。 这个变量在用于 EXECUTE 语句前，必须在批处理、存储过程或函数中声明过。  
  
 在用于调用标量值用户定义函数时，@*return_status* 变量可以为任意标量数据类型。  
  
 module_name  
 是要调用的存储过程或标量值用户定义函数的完全限定或者不完全限定名称。 模块名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 无论服务器的排序规则如何，扩展存储过程的名称总是区分大小写。  
  
 用户可以执行在另一数据库中创建的模块，只要运行模块的用户拥有此模块或具有在该数据库中执行该模块的适当权限。 用户可以在另一台运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的服务器中执行模块，只要该用户有相应的权限使用该服务器（远程访问），并能在数据库中执行该模块。 如果指定了服务器名称但没有指定数据库名称，则 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]会在用户的默认数据库中查找该模块。  
  
 ;*number*  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
 是可选整数，用于对同名的过程分组。 该参数不能用于扩展存储过程。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 有关过程组的详细信息，请参阅 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)。  
  
 @*module_name_var*  
 是局部定义的变量名，代表模块名称。  
  
 该变量可以包含本机编译的标量用户定义函数的名称。  
  
 @*parameter*  
 *module_name* 的参数，与在模块中定义的相同。 参数名称前必须加上符号 (@)。 在与 @*parameter_name*=*value* 格式一起使用时，参数名和常量不必按它们在模块中定义的顺序提供。 但是，如果对任何参数使用了 @*parameter_name*=*value* 格式，则必须对所有后续参数都使用此格式。  
  
 默认情况下，参数可为空值。  
  
 *value*  
 传递给模块或传递命令的参数值。 如果参数名称没有指定，参数值必须以在模块中定义的顺序提供。  
  
 对链接服务器执行传递命令时，参数值的顺序取决于链接服务器的 OLE DB 访问接口。 大多数 OLE DB 访问接口按从左到右的顺序将值绑定到参数。  
  
 如果参数值是一个对象名、字符串或由数据库名称或架构名称限定，则整个名称必须用单引号括起来。 如果参数值是一个关键字，则该关键字必须用双引号括起来。  
  
如果传递一个不以 `@` 开头且不以问号结尾的单词（例如，如果忘记参数名称中的 `@`），则尽管缺少引号，也会将该单词视为 nvarchar 字符串。

 如果在模块中定义了默认值，用户执行该模块时可以不必指定参数。  
  
 默认值也可以为 NULL。 通常，模块定义会指定当参数值为 NULL 时应该执行的操作。  
  
 @*variable*  
 是用来存储参数或返回参数的变量。  
  
 OUTPUT  
 指定模块或命令字符串返回一个参数。 该模块或命令字符串中的匹配参数也必须已使用关键字 OUTPUT 创建。 使用游标变量作为参数时使用该关键字。  
  
 如果将 *value* 定义为对链接服务器执行的模块的 OUTPUT 参数值，在此模块执行结束时，OLE DB 提供程序对相应 @*parameter* 执行的任何更改都会复制回此变量。  
  
 如果正在使用 OUTPUT 参数，并且使用的目的是在执行调用的批处理或模块内的其他语句中使用其返回值，则此参数的值必须作为变量传递，例如，@*parameter* = @*variable*。 如果一个参数在模块中没有定义为 OUTPUT 参数，则不能通过对该参数指定 OUTPUT 执行模块。 不能使用 OUTPUT 将常量传递给模块；返回参数需要变量名称。 在执行过程之前，必须声明变量的数据类型并赋值。  
  
 当对远程存储过程使用 EXECUTE 或对链接服务器执行传递命令时，OUTPUT 参数不能是任何大型对象 (LOB) 数据类型。  
  
 返回参数可以是 LOB 数据类型之外的任意数据类型。  
  
 DEFAULT  
 根据模块的定义，提供参数的默认值。 当模块需要的参数值没有定义默认值，并且缺少参数或指定了 DEFAULT 关键字，则会发生错误。  
  
 @*string_variable*  
 局部变量的名称。 @*string_variable* 可以为任何 **char**、**varchar**、**nchar** 或 **nvarchar** 数据类型。 其中包括 **(max)** 数据类型。  
  
 [N] '*tsql_string*'  
 常量字符串。 *tsql_string* 可以为任何 **nvarchar** 或 **varchar** 数据类型。 如果包含 N，则字符串将解释为 **nvarchar** 数据类型。  
  
 AS \<context_specification>  
 指定执行语句的上下文。  
  
 LOGIN  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
 指定要模拟的上下文是登录名。 模拟范围为服务器。  
  
 USER  
 指定要模拟的上下文是当前数据库中的用户。 模拟范围只限于当前数据库。 对数据库用户的上下文切换不会继承该用户的服务器级别权限。  
  
> [!IMPORTANT]  
>  当到数据库用户的上下文切换处于活动状态时，任何对数据库外部资源的访问尝试都会导致语句失败。 这包括 USE *database* 语句、分布式查询和使用标识符（由三个部分或四个部分组成）引用其他数据库的查询。  
  
 '*name*'  
 有效的用户或登录名。 *name* 必须是 sysadmin 固定服务器角色成员，或者分别作为 [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) 或 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 中的主体存在。  
  
 *name* 不能为内置帐户，如 NT AUTHORITY\LocalService、NT AUTHORITY\NetworkService 或 NT AUTHORITY\LocalSystem。  
  
 有关详细信息，请参阅本主题后面的[指定用户名或登录名](#_user)。  
  
 [N] '*command_string*'  
 常量字符串，包含要传递给链接服务器的命令。 如果包含 N，则字符串将解释为 **nvarchar** 数据类型。  
  
 [?]  
 指示参数，其值在 EXEC('...', \<arg-list>) AT \<linkedsrv> 语句所使用的传递命令的 \<arg-list> 中提供。  
  
 AT *linked_server_name*  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
 指定对 *linked_server_name* 执行 *command_string*，并将结果（如果有）返回到客户端。 *linked_server_name* 必须引用本地服务器中的现有链接服务器定义。 链接服务器是使用 [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) 定义的。  
  
 WITH \<execute_option>  
 可能的执行选项。 不能在 INSERT…EXEC 语句中指定 RESULT SETS 选项。  
 
AT DATA_SOURCE data_source_name 适用于：[!INCLUDE[sssql19](../../includes/sssql19-md.md)] 及更高版本
  
 指定对 _name 执行 command_string，并将结果（如果有）返回到客户端。  data_source_name 必须引用数据库中现有 EXTERNAL DATA SOURCE 定义。 仅支持指向 SQL Server 的数据源。 此外，对于指向计算池的 SQL Server 大数据群集数据源，支持数据池或存储池。 使用 [CREATE EXTERNAL DATA SOURCE](../statements/create-external-data-source-transact-sql.md) 定义数据源。  
  
 WITH \<execute_option>  
 可能的执行选项。 不能在 INSERT…EXEC 语句中指定 RESULT SETS 选项。  
  
|术语|定义|  
|----------|----------------|  
|RECOMPILE|执行模块后，强制编译、使用和放弃新计划。 如果该模块存在现有查询计划，则该计划将保留在缓存中。<br /><br /> 如果所提供的参数为非典型参数或者数据有很大的改变，使用该选项。 该选项不能用于扩展存储过程。 建议尽量少使用该选项，因为它消耗较多系统资源。<br /><br /> **注意：** 在调用使用 OPENDATASOURCE 语法的存储过程时，不能使用 WITH RECOMPILE。 如果指定由四个部分组成的对象名，则忽略 WITH RECOMPILE 选项。<br /><br /> **注意：** 本机编译的标量用户定义函数不支持 RECOMPILE。 如需重新编译，请使用 [sp_recompile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md)。|  
|**RESULT SETS UNDEFINED**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。<br /><br /> 此选项不保证将返回任何结果（如果有），并且不提供任何定义。 如果返回任何结果，则说明语句正常执行而没有发生错误，否则，不会返回任何结果。 如果未提供 result_sets_option，则 RESULT SETS UNDEFINED 是默认行为。<br /><br /> 对于已解释的标量用户定义函数和本机编译的标量用户定义函数，此选项不可操作，因为这些函数永远不会返回结果集。|  
|RESULT SETS NONE|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。<br /><br /> 保证执行语句不返回任何结果。 如果返回任何结果，则会中止批处理。<br /><br /> 对于已解释的标量用户定义函数和本机编译的标量用户定义函数，此选项不可操作，因为这些函数永远不会返回结果集。|  
|*\<result_sets_definition>*|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。<br /><br /> 保证返回 result_sets_definition 中指定的结果。 对于返回多个结果集的语句，请提供多个 *result_sets_definition* 部分。 将每个 *result_sets_definition* 用括号括上，并以逗号隔开。 有关详细信息情，请参阅本主题后面的 \<result_sets_definition>。<br /><br /> 对于本机编译的标量用户定义函数，此选项总是会导致错误，因为这些函数永远不会返回结果集。|
  
\<result_sets_definition>
适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 描述执行的语句所返回的结果集。 result_sets_definition 的子句具有以下含义  
  
|术语|定义|  
|----------|----------------|  
|{<br /><br /> column_name<br /><br /> data_type<br /><br /> [ COLLATE collation_name]<br /><br /> [NULL &#124; NOT NULL]<br /><br /> }|请参阅下表。|  
|db_name|包含表、视图或表值函数的数据库的名称。|  
|schema_name|拥有表、视图或表值函数的架构的名称。|  
|table_name &#124; view_name &#124; table_valued_function_name|指定返回的列是在命名的表、视图或表值函数中指定的列。 AS 对象语法中不支持表变量、临时表以及同义词。|  
|AS TYPE [schema_name.]table_type_name|指定返回的列是在表类型中指定的列。|  
|AS FOR XML|指定将 EXECUTE 语句调用的语句或存储过程返回的 XML 结果转换为仿佛是由 SELECT …FOR XML ... 语句生成的格式。 来自原始语句中类型指令的所有格式设置都被删除，返回的结果就好像未指定任何类型指令一样。 AS FOR XML 不将所执行的语句和存储过程的非 XML 表格结果转换为 XML。|  
  
|术语|定义|  
|----------|----------------|  
|column_name|每个列的名称。 如果列数不同于结果集，则会发生错误并中止批处理。 如果列名不同于结果集，则将返回的列名设置为定义的名称。|  
|data_type|每个列的数据类型。 如果数据类型不同，则对定义的数据类型执行隐式转换。 如果转换失败，则中止批处理。|  
|COLLATE collation_name|每个列的排序规则。 如果排序规则不匹配，则尝试使用隐式排序规则。 如果该操作失败，则中止批处理。|  
|NULL &#124; NOT NULL|每个列的为 Null 性。 如果定义的为 NULL 性为 NOT NULL，并且返回的数据包含 NULL，则会发生错误并中止批处理。 如果未指定，则默认值符合 ANSI_NULL_DFLT_ON 和 ANSI_NULL_DFLT_OFF 选项的设置。|  
  
 执行期间返回的实际结果集在以下方面可能不同于使用 WITH RESULT SETS 子句定义的结果：结果集数、列数、列名、为 Null 性以及数据类型。 如果结果集数不同，则会发生错误并中止批处理。  
  
## <a name="remarks"></a>备注  
 可通过使用 *value* 或使用 @*parameter_name*=*value* 来提供参数。 来提供参数。参数不是事务的一部分；因此，如果在以后回退的事务中更改了参数，则此参数的值不会恢复为以前的值。 返回给调用方的值总是模块返回时的值。  
  
 当一个模块调用其他模块或通过引用公共语言运行时 (CLR) 模块、用户定义类型或聚合执行托管代码时，将出现嵌套。 当开始执行调用模块或托管代码引用时，嵌套级别将增加，而当调用模块或托管代码引用完成时，嵌套级别将减少。 嵌套级别最高为 32 级，超过 32 级时，会导致整个调用链失败。 当前的嵌套级别存储在 @@NESTLEVEL 系统函数中。  
  
 因为远程存储过程和扩展存储过程不在事务的范围内（除非在 BEGIN DISTRIBUTED TRANSACTION 语句中发出或者是和不同的配置选项一起使用），所以通过调用执行的命令不能回滚。 有关详细信息，请参阅[系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md) 和 [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)。  
  
 当使用游标变量时，如果执行的过程传递一个分配有游标的游标变量，就会出错。  
  
 在执行模块时，如果语句是批处理中的第一个语句，则不一定要指定 EXECUTE 关键字。  
  
 有关 CLR 存储过程特定的其他信息，请参阅 CLR 存储过程。  
  
## <a name="using-execute-with-stored-procedures"></a>在存储过程中使用 EXECUTE  
 在执行存储过程时，如果语句是批处理中的第一个语句，则不一定要指定 EXECUTE 关键字。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统存储过程以字符 sp_ 开头。 这些存储过程物理上存储在[资源数据库](../../relational-databases/databases/resource-database.md)中，但逻辑上出现在每个系统数据库和用户定义数据库的 sys 架构中。 在批处理或模块（如用户定义存储过程或函数）中执行系统存储过程时，建议使用 sys 架构名称限定存储过程名称。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统扩展存储过程以字符 xp_ 开头，这些存储过程包含在 master 数据库的 dbo 架构中。 在批处理或模块（如用户定义存储过程或函数）内执行系统扩展存储过程时，建议使用 master.dbo 限定存储过程名称。  
  
 在批处理或模块（如用户定义存储过程或函数）内执行用户定义存储过程时，建议使用架构名限定存储过程名称。 建议不要使用与系统存储过程相同的名称命名用户定义存储过程。 有关执行存储过程的详细信息，请参阅[执行存储过程](../../relational-databases/stored-procedures/execute-a-stored-procedure.md)。  
  
## <a name="using-execute-with-a-character-string"></a>使用带字符串的 EXECUTE 命令  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本中，字符串限制为 8,000 字节。 这要求连接长字符串，以便动态执行。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，可以指定 **varchar(max)** 和 **nvarchar(max)** 数据类型，它们允许字符串使用多达 2 GB 数据。  
  
 数据库上下文的更改只在 EXECUTE 语句结束前有效。 例如，运行下面这条语句中的 `EXEC` 之后，数据库上下文将为 master。  
  
```sql  
USE master; EXEC ('USE AdventureWorks2012; SELECT BusinessEntityID, JobTitle FROM HumanResources.Employee;');  
```  
  
## <a name="context-switching"></a>上下文切换  
 可以使用 `AS { LOGIN | USER } = ' name '` 子句切换动态语句的执行上下文。 当将上下文切换指定为 `EXECUTE ('string') AS <context_specification>` 时，上下文切换的持续时间限制为执行查询的范围。  
  
###  <a name="specifying-a-user-or-login-name"></a><a name="_user"></a>指定用户名或登录名  
 `AS { LOGIN | USER } = ' name '` 中指定的用户名或登录名必须分别为 sys.database_principals 或 sys.server_principals 的主体，否则该语句将失败。 此外，还必须为该主体授予 IMPERSONATE 权限。 除非调用方是数据库所有者或 sysadmin 固定服务器角色的成员，否则，即使在用户通过 Windows 组成员身份访问数据库或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，也必须存在该主体。 例如，假设条件如下：  
  
-   CompanyDomain\SQLUsers 组具有对 Sales 数据库的访问权限。  
  
-   CompanyDomain\SqlUser1 是 SQLUsers 的成员，因此具有对 Sales 数据库的隐式访问权限。  
  
 尽管 CompanyDomain\SqlUser1 可以通过 SQLUsers 组的成员身份访问数据库，但 `EXECUTE @string_variable AS USER = 'CompanyDomain\SqlUser1'` 语句仍会失败，因为 `CompanyDomain\SqlUser1` 不是数据库中的主体。  
  
### <a name="best-practices"></a>最佳实践  
 指定具有执行语句或模块中定义的操作所需的最低权限的登录名或用户。 例如，如果只需数据库级别权限，则不要指定拥有服务器级别权限的登录名；如果不需要相应权限，也不要指定数据库所有者帐户。  
  
## <a name="permissions"></a>权限  
 运行 EXECUTE 语句无需权限。 但是，需要对 EXECUTE 字符串内引用的安全对象具有权限。 例如，如果字符串包含 INSERT 语句，则 EXECUTE 语句的调用方对目标表必须具有 INSERT 权限。 在遇到 EXECUTE 语句时，即使 EXECUTE 语句包含于模块内，也将检查权限。  
  
 模块的 EXECUTE 权限默认授予该模块的所有者，该所有者可以将此权限转让给其他用户。 当运行一个执行字符串的模块时，系统会在执行该模块的用户上下文中而不是在创建该模块的用户上下文中检查权限。 但是，如果同一用户拥有调用模块和被调用模块，则不对后者执行 EXECUTE 权限检查。  
  
 如果模块访问其他数据库对象，则当拥有对该模块的 EXECUTE 权限并且以下任一情况存在时，执行将成功：  
  
-   模块被标记为 EXECUTE AS USER 或 SELF，并且模块所有者对被引用对象具有相应权限。 有关模块内模拟的详细信息，请参阅 [EXECUTE AS 子句 (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)。  
  
-   模块被标记为 EXECUTE AS CALLER，并且您对对象具有相应权限。  
  
-   模块被标记为 EXECUTE AS *user_name*，并且 *user_name* 对对象具有相应权限。  
  
### <a name="context-switching-permissions"></a>上下文切换权限  
 若要对某登录名指定 EXECUTE AS，调用方必须具有对所指定登录名的 IMPERSONATE 权限。 若要对某数据库用户指定 EXECUTE AS，调用方必须具有对所指定用户名的 IMPERSONATE 权限。 如果未指定执行上下文或指定了 EXECUTE AS CALLER，则无需 IMPERSONATE 权限。  
  
## <a name="examples-sql-server"></a>示例：SQL Server
  
### <a name="a-using-execute-to-pass-a-single-parameter"></a>A. 使用 EXECUTE 传递单个参数  
 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `uspGetEmployeeManagers` 存储过程需要一个参数 (`@EmployeeID`)。 以下示例执行`uspGetEmployeeManagers` 存储过程，以 `Employee ID 6` 作为参数值。  
  
```sql    
EXEC dbo.uspGetEmployeeManagers 6;  
GO  
```  
  
 在执行过程中变量可以显式命名：  
  
```sql    
EXEC dbo.uspGetEmployeeManagers @EmployeeID = 6;  
GO  
```  
  
 如果以下语句是批处理、**osql** 或 **sqlcmd** 脚本中的第一个语句，则无需 EXEC。  
  
```sql    
dbo.uspGetEmployeeManagers 6;  
GO  
--Or  
dbo.uspGetEmployeeManagers @EmployeeID = 6;  
GO  
```  
  
### <a name="b-using-multiple-parameters"></a>B. 使用多个参数  
 下面的示例在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中执行 `spGetWhereUsedProductID` 存储过程。 该存储过程将传递两个参数：第一个参数为产品 ID (`819`)，第二个参数 `@CheckDate,` 是 `datetime` 值。  
  
```sql    
DECLARE @CheckDate DATETIME;  
SET @CheckDate = GETDATE();  
EXEC dbo.uspGetWhereUsedProductID 819, @CheckDate;  
GO  
```  
  
### <a name="c-using-execute-tsql_string-with-a-variable"></a>C. 使用带变量的 EXECUTE 'tsql_string' 语句  
 下面的示例说明了 `EXECUTE` 如何处理动态生成的包含变量的字符串。 该示例创建 `tables_cursor` 游标以保存 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中所有用户定义表的列表，然后使用该列表重新生成对表的全部索引。  
  
```sql    
DECLARE tables_cursor CURSOR  
   FOR  
   SELECT s.name, t.name   
   FROM sys.objects AS t  
   JOIN sys.schemas AS s ON s.schema_id = t.schema_id  
   WHERE t.type = 'U';  
OPEN tables_cursor;  
DECLARE @schemaname sysname;  
DECLARE @tablename sysname;  
FETCH NEXT FROM tables_cursor INTO @schemaname, @tablename;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN;  
   EXECUTE ('ALTER INDEX ALL ON ' + @schemaname + '.' + @tablename + ' REBUILD;');  
   FETCH NEXT FROM tables_cursor INTO @schemaname, @tablename;  
END;  
PRINT 'The indexes on all tables have been rebuilt.';  
CLOSE tables_cursor;  
DEALLOCATE tables_cursor;  
GO  
  
```  
  
### <a name="d-using-execute-with-a-remote-stored-procedure"></a>D. 对远程存储过程使用 EXECUTE 语句  
 以下示例在远程服务器 `uspGetEmployeeManagers` 上执行 `SQLSERVER1` 存储过程，然后在 `@retstat` 中存储指示成功或失败的返回状态。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
```sql    
DECLARE @retstat INT;  
EXECUTE @retstat = SQLSERVER1.AdventureWorks2012.dbo.uspGetEmployeeManagers @BusinessEntityID = 6;  
```  
  
### <a name="e-using-execute-with-a-stored-procedure-variable"></a>E. 使用带存储过程变量的 EXECUTE 语句  
 以下示例创建一个代表存储过程名称的变量。  
  
```sql  
DECLARE @proc_name VARCHAR(30);  
SET @proc_name = 'sys.sp_who';  
EXEC @proc_name;  
  
```  
  
### <a name="f-using-execute-with-default"></a>F. 使用带 DEFAULT 的 EXECUTE  
 以下示例创建了一个存储过程，第一个和第三个参数具有默认值。 当运行该过程时，如果调用时没有传递值或者指定了默认值，这些默认值就会赋给第一个和第三个参数。 请注意，`DEFAULT` 关键字有多种使用方法。  
  
```sql    
IF OBJECT_ID(N'dbo.ProcTestDefaults', N'P')IS NOT NULL  
   DROP PROCEDURE dbo.ProcTestDefaults;  
GO  
-- Create the stored procedure.  
CREATE PROCEDURE dbo.ProcTestDefaults (  
@p1 SMALLINT = 42,   
@p2 CHAR(1),   
@p3 VARCHAR(8) = 'CAR')  
AS   
   SET NOCOUNT ON;  
   SELECT @p1, @p2, @p3  
;  
GO  
```  
  
 `Proc_Test_Defaults` 存储过程可使用多种组合执行。  
  
```sql    
-- Specifying a value only for one parameter (@p2).  
EXECUTE dbo.ProcTestDefaults @p2 = 'A';  
-- Specifying a value for the first two parameters.  
EXECUTE dbo.ProcTestDefaults 68, 'B';  
-- Specifying a value for all three parameters.  
EXECUTE dbo.ProcTestDefaults 68, 'C', 'House';  
-- Using the DEFAULT keyword for the first parameter.  
EXECUTE dbo.ProcTestDefaults @p1 = DEFAULT, @p2 = 'D';  
-- Specifying the parameters in an order different from the order defined in the procedure.  
EXECUTE dbo.ProcTestDefaults DEFAULT, @p3 = 'Local', @p2 = 'E';  
-- Using the DEFAULT keyword for the first and third parameters.  
EXECUTE dbo.ProcTestDefaults DEFAULT, 'H', DEFAULT;  
EXECUTE dbo.ProcTestDefaults DEFAULT, 'I', @p3 = DEFAULT;  
  
```  
  
### <a name="g-using-execute-with-at-linked_server_name"></a>G. 使用带 AT linked_server_name 的 EXECUTE  
 以下示例将一个命令字符串传递给远程服务器。 先创建一个链接服务器 `SeattleSales`，它指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的另一个实例，然后对该链接服务器执行 DDL 语句 (`CREATE TABLE`)。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
```sql    
EXEC sp_addlinkedserver 'SeattleSales', 'SQL Server'  
GO  
EXECUTE ( 'CREATE TABLE AdventureWorks2012.dbo.SalesTbl   
(SalesID int, SalesName varchar(10)) ; ' ) AT SeattleSales;  
GO  
```  
  
### <a name="h-using-execute-with-recompile"></a>H. 使用 EXECUTE WITH RECOMPILE  
 以下示例执行 `Proc_Test_Defaults` 存储过程，并在执行模块后强制编译、使用和放弃一个新查询计划。  
  
```sql    
EXECUTE dbo.Proc_Test_Defaults @p2 = 'A' WITH RECOMPILE;  
GO  
```  
  
### <a name="i-using-execute-with-a-user-defined-function"></a>I. 对用户定义函数使用 EXECUTE  
 下面的示例在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中执行 `ufnGetSalesOrderStatusText` 标量用户定义函数。 该语句使用 `@returnstatus` 变量存储函数的返回值。 函数需要一个输入参数 `@Status`。 该参数定义为 **tinyint** 数据类型。  
  
```sql    
DECLARE @returnstatus NVARCHAR(15);  
SET @returnstatus = NULL;  
EXEC @returnstatus = dbo.ufnGetSalesOrderStatusText @Status = 2;  
PRINT @returnstatus;  
GO  
```  
  
### <a name="j-using-execute-to-query-an-oracle-database-on-a-linked-server"></a>J. 使用 EXECUTE 查询链接服务器上的 Oracle 数据库  
 以下示例在远程 Oracle 服务器上执行几个 `SELECT` 语句。 示例开始时添加 Oracle 服务器作为链接服务器，并创建链接服务器登录。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
```sql    
-- Setup the linked server.  
EXEC sp_addlinkedserver    
        @server='ORACLE',  
        @srvproduct='Oracle',  
        @provider='OraOLEDB.Oracle',   
        @datasrc='ORACLE10';  
  
EXEC sp_addlinkedsrvlogin   
    @rmtsrvname='ORACLE',  
    @useself='false',   
    @locallogin=null,   
    @rmtuser='scott',   
    @rmtpassword='tiger';  
  
EXEC sp_serveroption 'ORACLE', 'rpc out', true;  
GO  
  
-- Execute several statements on the linked Oracle server.  
EXEC ( 'SELECT * FROM scott.emp') AT ORACLE;  
GO  
EXEC ( 'SELECT * FROM scott.emp WHERE MGR = ?', 7902) AT ORACLE;  
GO  
DECLARE @v INT;   
SET @v = 7902;  
EXEC ( 'SELECT * FROM scott.emp WHERE MGR = ?', @v) AT ORACLE;  
GO   
```  
  
### <a name="k-using-execute-as-user-to-switch-context-to-another-user"></a>K. 使用 EXECUTE AS USER 将上下文切换为其他用户  
 以下示例执行用于创建表的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 字符串并指定 `AS USER` 子句将语句的执行上下文从调用方切换为 `User1`。 当语句运行时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将检查 `User1` 的权限。 `User1` 必须为数据库中的用户，必须具有在 `Sales` 架构中创建表的权限，否则语句将失败。  
  
```sql    
EXECUTE ('CREATE TABLE Sales.SalesTable (SalesID INT, SalesName VARCHAR(10));')  
AS USER = 'User1';  
GO  
```  
  
### <a name="l-using-a-parameter-with-execute-and-at-linked_server_name"></a>L. 以 EXECUTE 和 AT linked_server_name 使用参数  
 以下示例使用问号 (`?`) 占位符作为参数向远程服务器传递命令字符串。 该示例先创建一个链接服务器 `SeattleSales`，它指向另一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，然后对该链接服务器执行 `SELECT` 语句。 `SELECT` 语句使用问号作为 `ProductID` 参数 (`952`)（该参数在语句后提供）的占位符。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
```sql    
-- Setup the linked server.  
EXEC sp_addlinkedserver 'SeattleSales', 'SQL Server'  
GO  
-- Execute the SELECT statement.  
EXECUTE ('SELECT ProductID, Name   
    FROM AdventureWorks2012.Production.Product  
    WHERE ProductID = ? ', 952) AT SeattleSales;  
GO  
```  
  
### <a name="m-using-execute-to-redefine-a-single-result-set"></a>M. 使用 EXECUTE 重新定义单个结果集  
 前面的一些示例执行 `EXEC dbo.uspGetEmployeeManagers 6;`，将返回 7 个列。 以下示例说明如何使用 `WITH RESULT SET` 语法更改返回的结果集的名称和数据类型。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
```sql    
EXEC uspGetEmployeeManagers 16  
WITH RESULT SETS  
(   
   ([Reporting Level] INT NOT NULL,  
    [ID of Employee] INT NOT NULL,  
    [Employee First Name] NVARCHAR(50) NOT NULL,  
    [Employee Last Name] NVARCHAR(50) NOT NULL,  
    [Employee ID of Manager] NVARCHAR(max) NOT NULL,  
    [Manager First Name] NVARCHAR(50) NOT NULL,  
    [Manager Last Name] NVARCHAR(50) NOT NULL )  
);  
  
```  
  
### <a name="n-using-execute-to-redefine-a-two-result-sets"></a>N. 使用 EXECUTE 重新定义两个结果集  
 在执行返回多个结果集的语句时，请定义每个预期结果集。 以下示例在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 中创建一个返回两个结果集的过程。 然后使用 **WITH RESULT SETS** 子句执行该过程并指定两个结果集定义。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
```sql    
--Create the procedure  
CREATE PROC Production.ProductList @ProdName NVARCHAR(50)  
AS  
-- First result set  
SELECT ProductID, Name, ListPrice  
    FROM Production.Product  
    WHERE Name LIKE @ProdName;  
-- Second result set   
SELECT Name, COUNT(S.ProductID) AS NumberOfOrders  
    FROM Production.Product AS P  
    JOIN Sales.SalesOrderDetail AS S  
        ON P.ProductID  = S.ProductID   
    WHERE Name LIKE @ProdName  
    GROUP BY Name;  
GO  
  
-- Execute the procedure   
EXEC Production.ProductList '%tire%'  
WITH RESULT SETS   
(  
    (ProductID INT,   -- first result set definition starts here  
    Name NAME,  
    ListPrice MONEY)  
    ,                 -- comma separates result set definitions  
    (Name NAME,       -- second result set definition starts here  
    NumberOfOrders INT)  
);  
  
```  
  ### <a name="o-using-execute-with-at-data_source-data_source_name-to-query-a-remote-sql-server"></a>O. 将 EXECUTE 用于 AT DATA_SOURCE data_source_name 以查询远程 SQL Server 
  
 以下示例将命令字符串传递给指向 SQL Server 实例的外部数据源。 
  
**适用于**：[!INCLUDE[sssql19](../../includes/sssql19-md.md)] 及更高版本
  
```sql    
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE my_sql_server;  
GO  
```  
  
### <a name="p-using-execute-with-at-data_source-data_source_name-to-query-compute-pool-in-sql-server-big-data-cluster"></a>P. 将 EXECUTE 用于 AT DATA_SOURCE data_source_name 以查询远程 SQL Server 大数据群集中的计算池 

 以下示例将命令字符串传递给指向 SQL Server 大数据群集中计算池的外部数据源。 该示例针对 SQL Server 大数据群集中的计算池创建数据源 `SqlComputePool`，并对该数据源执行 `SELECT` 语句。 
  
**适用于**：[!INCLUDE[sssql19](../../includes/sssql19-md.md)] 及更高版本
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlComputePool 
WITH (LOCATION = 'sqlcomputepool://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlComputePool;  
GO  
```  

### <a name="q-using-execute-with-at-data_source-data_source_name-to-query-data-pool-in-sql-server-big-data-cluster"></a>Q. 将 EXECUTE 用于 AT DATA_SOURCE data_source_name 以查询远程 SQL Server 大数据群集中的数据池 
 以下示例将命令字符串传递给指向 SQL Server 大数据群集中计算池的外部数据源。 该示例针对 SQL Server 大数据群集中的数据池创建数据源 `SqlDataPool`，并对该数据源执行 `SELECT` 语句。 
  
**适用于**：[!INCLUDE[sssql19](../../includes/sssql19-md.md)] 及更高版本
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlDataPool 
WITH (LOCATION = 'sqldatapool://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlDataPool;  
GO  
```

### <a name="r-using-execute-with-at-data_source-data_source_name-to-query-storage-pool-in-sql-server-big-data-cluster"></a>R. 将 EXECUTE 用于 AT DATA_SOURCE data_source_name 以查询远程 SQL Server 大数据群集中的存储池 

 以下示例将命令字符串传递给指向 SQL Server 大数据群集中计算池的外部数据源。 该示例针对 SQL Server 大数据群集中的数据池创建数据源 `SqlStoragePool`，并对该数据源执行 `SELECT` 语句。 
  
**适用于**：[!INCLUDE[sssql19](../../includes/sssql19-md.md)] 及更高版本
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlStoragePool
WITH (LOCATION = 'sqlhdfs://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlStoragePool;  
GO  
```

  
## <a name="examples-azure-synapse-analytics"></a>示例：Azure Synapse Analytics 
  
### <a name="a-basic-procedure-execution"></a>答：基本过程执行  
 执行存储过程：  
  
```sql  
EXEC proc1;  
```  
  
 使用在运行时确定的名称调用存储过程：  
  
```sql    
EXEC ('EXEC ' + @var);  
```  
  
 从存储过程中调用存储过程：  
  
```sql   
CREATE sp_first AS EXEC sp_second; EXEC sp_third;  
```  
  
### <a name="b-executing-strings"></a>B：执行字符串  
 执行 SQL 字符串：  
  
```sql   
EXEC ('SELECT * FROM sys.types');  
```  
  
 执行嵌套字符串：  
  
```sql  
EXEC ('EXEC (''SELECT * FROM sys.types'')');  
```  
  
 执行字符串变量：  
  
```sql  
DECLARE @stringVar NVARCHAR(100);  
SET @stringVar = N'SELECT name FROM' + ' sys.sql_logins';  
EXEC (@stringVar);  
```  
  
### <a name="c-procedures-with-parameters"></a>C:包含参数的过程  

 以下示例创建一个包含参数的过程，并演示执行该过程的三种方法：  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROC ProcWithParameters  
    @name NVARCHAR(50),  
@color NVARCHAR(15)  
AS   
SELECT ProductKey, EnglishProductName, Color FROM [dbo].[DimProduct]  
WHERE EnglishProductName LIKE @name  
AND Color = @color;  
GO  
  
-- Executing using positional parameters  
EXEC ProcWithParameters N'%arm%', N'Black';  
-- Executing using named parameters in order  
EXEC ProcWithParameters @name = N'%arm%', @color = N'Black';  
-- Executing using named parameters out of order  
EXEC ProcWithParameters @color = N'Black', @name = N'%arm%';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [@@NESTLEVEL (Transact-SQL)](../../t-sql/functions/nestlevel-transact-sql.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [EXECUTE AS 子句 (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [osql 实用工具](../../tools/osql-utility.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVERT (Transact-SQL)](../../t-sql/statements/revert-transact-sql.md)   
 [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sqlcmd 实用工具](../../tools/sqlcmd-utility.md)   
 [SUSER_NAME (Transact-SQL)](../../t-sql/functions/suser-name-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [USER_NAME (Transact-SQL)](../../t-sql/functions/user-name-transact-sql.md)   
 [OPENDATASOURCE (Transact-SQL)](../../t-sql/functions/opendatasource-transact-sql.md)   
 [针对内存中 OLTP 的标量用户定义函数](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)  
  
  
