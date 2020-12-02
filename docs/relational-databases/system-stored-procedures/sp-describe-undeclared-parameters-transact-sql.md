---
description: sp_describe_undeclared_parameters (Transact-SQL)
title: sp_describe_undeclared_parameters (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_describe_undeclared_parameters
- sp_describe_undeclared_parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_undeclared_parameters
ms.assetid: 6f016da6-dfee-4228-8b0d-7cd8e7d5a354
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017||= sqlallproducts-allversions
ms.openlocfilehash: 232ad1cfe65fca719260a9ed8ab87a7f2d7ed3dd
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96443154"
---
# <a name="sp_describe_undeclared_parameters-transact-sql"></a>sp_describe_undeclared_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)] 

  返回一个结果集，其中包含有关在批处理中未声明的参数的元数据 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 考虑在 **\@ tsql** 批处理中使用但未在 **\@ params** 中声明的每个参数。 每个此类参数在返回的结果集中各占一行，并包含推断的参数类型信息。 如果 **\@ tsql** 输入批处理没有参数中声明的 **\@ 参数，则** 该过程返回一个空结果集。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql
  
sp_describe_undeclared_parameters   
    [ @tsql = ] 'Transact-SQL_batch'   
    [ , [ @params = ] N'parameters' data type ] [, ...n]  
```  

> [!Note] 
> 若要在专用 SQL 池中的 Azure Synapse Analytics 中使用此存储过程，请将数据库兼容级别设置为20或更高。   若要选择退出，请将数据库兼容级别更改为10。

## <a name="arguments"></a>参数  
`[ \@tsql = ] 'Transact-SQL\_batch'` 一个或多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 *SQL_batch* 可以是 **nvarchar (** _n_ **)** 或 **nvarchar (max)**。  
  
`[ \@params = ] N'parameters'`\@params 为批处理参数提供声明字符串 [!INCLUDE[tsql](../../includes/tsql-md.md)] ，类似于 sp_executesql 的工作方式。 *参数* 可以为 **nvarchar (** _n_ **)** 或 **nvarchar (max)**。  
  
 一个字符串，其中包含已嵌入到 *SQL_batch* 中的所有参数的定义。 字符串必须是 Unicode 常量或 Unicode 变量。 每个参数定义由参数名称和数据类型组成。 n 是表示附加参数定义的占位符。 如果语句中的 Transact-sql 语句或批处理不包含参数， \@ 则无需参数。 该参数的默认值为 NULL。  
  
 Datatype  
 参数的数据类型。  
  
## <a name="return-code-values"></a>返回代码值  
 **sp_describe_undeclared_parameters** 始终在成功时返回零返回状态。 如果该过程引发错误并且该过程作为 RPC 调用，则返回状态将由 sys.dm_exec_describe_first_result_set 的 error_type 列中所述的错误类型填充。 如果该过程是从 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中调用的，则返回值始终为零，甚至在出现错误情况时也不例外。  
  
## <a name="result-sets"></a>结果集  
 **sp_describe_undeclared_parameters** 返回下面的结果集。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**parameter_ordinal**|**int NOT NULL**|在结果集中包含参数的序号位置。 第一个参数的位置将指定为 1。|  
|name |**sysname 不为 NULL**|包含参数的名称。|  
|**suggested_system_type_id**|**int NOT NULL**|包含 sys.databases 中指定的参数数据类型的 **system_type_id** 。<br /><br /> 对于 CLR 类型，即使 **system_type_name** 列返回 NULL，该列也会返回值240。|  
|**suggested_system_type_name**|**nvarchar (256) NULL**|包含数据类型名称。 包含为参数数据类型指定的参数（例如，length、precision、scale）。 如果数据类型是用户定义的别名类型，则会在此处指定基本系统类型。 如果数据类型是 CLR 用户定义数据类型，则在此列中返回 NULL。 如果无法推断参数类型，则返回 NULL。|  
|**suggested_max_length**|**smallint NOT NULL**|请参阅 sys.databases。 对于 **max_length** 列说明。|  
|**suggested_precision**|**tinyint NOT NULL**|请参阅 sys.databases。 了解有关精度列的说明。|  
|**suggested_scale**|**tinyint NOT NULL**|请参阅 sys.databases。 了解有关小数位数列的说明。|  
|**suggested_user_type_id**|**int NULL**|对于 CLR 和别名类型，包含在 sys.types 中指定的列数据类型的 user_type_id。 否则为 NULL。|  
|**suggested_user_type_database**|**sysname NULL**|对于 CLR 和别名类型，包含在其中定义相应类型的数据库的名称。 否则为 NULL。|  
|**suggested_user_type_schema**|**sysname NULL**|对于 CLR 和别名类型，包含在其中定义相应类型的架构的名称。 否则为 NULL。|  
|**suggested_user_type_name**|**sysname NULL**|对于 CLR 和别名类型，包含类型的名称。 否则为 NULL。|  
|**suggested_assembly_qualified_type_name**|**nvarchar (4000) NULL**|对于 CLR 类型，返回定义类型的程序集和类的名称。 否则为 NULL。|  
|**suggested_xml_collection_id**|**int NULL**|包含 sys.databases 中指定的参数数据类型的 xml_collection_id。 如果返回的类型与 XML 架构集合不关联，则该列将返回 NULL。|  
|**suggested_xml_collection_database**|**sysname NULL**|包含定义与此类型关联的 XML 架构集合的数据库。 如果返回的类型与 XML 架构集合不关联，则该列将返回 NULL。|  
|**suggested_xml_collection_schema**|**sysname NULL**|包含定义与此类型关联的 XML 架构集合的架构。 如果返回的类型与 XML 架构集合不关联，则该列将返回 NULL。|  
|**suggested_xml_collection_name**|**sysname NULL**|包含与此类型关联的 XML 架构集合的名称。 如果返回的类型与 XML 架构集合不关联，则该列将返回 NULL。|  
|**suggested_is_xml_document**|**位非 NULL**|如果要返回的类型为 XML，并且保证该类型是 XML 文档，则返回 1。 否则，返回 0。|  
|**suggested_is_case_sensitive**|**位非 NULL**|如果列为区分大小写的字符串类型，则返回 1；否则，返回 0。|  
|**suggested_is_fixed_length_clr_type**|**位非 NULL**|如果列为固定长度 CLR 类型，则返回 1；否则，返回 0。|  
|**suggested_is_input**|**位非 NULL**|如果在赋值语句左侧以外的任何地方使用参数，则返回 1。 否则，返回 0。|  
|**suggested_is_output**|**位非 NULL**|如果在赋值语句左侧使用参数或将参数传递给存储过程的输出参数，则返回 1。 否则，返回 0。|  
|**formal_parameter_name**|**sysname NULL**|如果参数是存储过程或用户定义的函数的参数，则返回相应形参的名称。 否则，返回 NULL。|  
|**suggested_tds_type_id**|**int NOT NULL**|供内部使用。|  
|**suggested_tds_length**|**int NOT NULL**|供内部使用。|  
  
## <a name="remarks"></a>备注  
 **sp_describe_undeclared_parameters** 始终返回零的返回状态。  
  
 最常见的用途是，为应用程序提供的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句可能包含一些参数，并且必须以某种方式处理这些参数。 一个例子是用户接口（如 ODBCTest 或 RowsetViewer），用户可以在其中使用 ODBC 参数语法提供查询。 应用程序必须动态查找参数数目，并提示用户输入每个参数。  
  
 另一个例子是，在没有用户输入时，应用程序必须循环访问这些参数，并从某个其他位置（例如，表）获取这些参数的数据。 在这种情况下，应用程序不必同时传递所有参数信息。 相反，应用程序可以从提供程序中获取所有参数信息，并从表中获取数据本身。 使用 **sp_describe_undeclared_parameters** 的代码更通用，如果数据结构稍后发生更改，则不太可能需要修改。  
  
 在以下任何情况下， **sp_describe_undeclared_parameters** 都将返回错误。  
  
-   如果输入 \@ tsql 不是有效的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理。 有效性是通过分析和分析该批来决定的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 在确定批处理是否有效时，不考虑批处理在查询优化期间或执行期间引发的任何错误 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。  
  
-   如果参数 \@ 不为 NULL，并且包含一个字符串，该字符串不是参数的语法有效声明字符串，或者包含多次声明任何参数的字符串。  
  
-   如果输入 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批声明一个局部变量，该局部变量与 params 中声明的参数同名 \@ 。  
  
- 如果语句引用临时表，则为。

- 查询包括创建随后要查询的永久表。
  
 如果 \@ tsql 没有参数中声明的参数， \@ 则该过程返回一个空结果集。  
  
## <a name="parameter-selection-algorithm"></a>参数选择算法  
 对于具有未声明的参数的查询，将通过三个步骤推断未声明的参数的数据类型。  
  
 **步骤 1**  
  
 要推断具有未声明的参数的查询的数据类型，第一步骤是查找数据类型不依赖于未声明的参数的所有子表达式的数据类型。 可以确定以下表达式的类型：  
  
-   列、常量、变量以及声明的参数。  
  
-   调用用户定义的函数 (UDF) 的结果。  
  
-   数据类型不依赖于所有输入的未声明参数的表达式。  
  
 例如，考虑查询 `SELECT dbo.tbl(@p1) + c1 FROM t1 WHERE c2 = @p2 + 2`。 表达式 dbo.tbl (\@ p1) + C1 和 c2 具有数据类型，并且 expression \@ p1 和 \@ p2 + 2 不存在。  
  
 在执行此步骤后，如果任何表达式（对 UDF 的调用除外）有两个没有数据类型的参数，类型推断将失败并发生错误。 例如，下面的语句均产生错误：  
  
```sql
SELECT * FROM t1 WHERE @p1 = @p2  
SELECT * FROM t1 WHERE c1 = @p1 + @p2  
SELECT * FROM t1 WHERE @p1 = SUBSTRING(@p2, 2, 3)  
```  
  
 以下示例不产生错误：  
  
```sql
SELECT * FROM t1 WHERE @p1 = dbo.tbl(c1, @p2, @p3)  
```
  
 **步骤 2**  
  
 对于给定的未声明参数 \@ p，类型推导算法将查找包含 p 的最内层表达式 E (\@ p) ，这 \@ 是以下各项之一：  
  
-   比较或赋值运算符的参数。  
  
-   用户定义的函数（包括表值 UDF）、过程或方法的参数。  
  
-   **INSERT** 语句的 **VALUES** 子句的参数。  
  
-   **强制转换** 或 **转换** 的参数。  
  
 类型推导算法查找 E (p) 的目标数据类型 TT (\@ p) \@ 。 上述示例的目标数据类型如下所示：  
  
-   比较或赋值语句的另一侧的数据类型。  
  
-   将此参数传递到的参数的声明数据类型。  
  
-   将该值插入到的列的数据类型。  
  
-   要将语句转换到的数据类型。  
  
 例如，考虑查询 `SELECT * FROM t WHERE @p1 = dbo.tbl(@p2 + c1)`。 然后，E (\@ p1) = \@ P1，e (\@ p2) = \@ P2 + C1，TT (\@ p1) 是 dbo.tbl 的声明返回数据类型，tt (\@ p2) 是 dbo.tbl 的声明的参数数据类型。  
  
 如果 \@ p 不包含在步骤2开头列出的任何表达式中，则类型推导算法将确定 e (\@ p) 是包含 p 的最大标量表达式 \@ ，而类型推导算法不会计算 e (p) 的目标数据类型 TT (\@ p) \@ 。 例如，如果查询为 SELECT， `@p + 2` 则 E (\@ p) = \@ p + 2，并且没有 TT (\@ p) 。  
  
 **步骤 3**  
  
 现在 \@ 已确定 E (p) 和 TT (\@ p) ，类型推导算法使用 \@ 以下两种方式之一推导 p 的数据类型：  
  
-   简单推断  
  
     如果 E (\@ p) = \@ p 和 TT (\@ p) 存在，即，如果 \@ p 直接是第2步开头列出的其中一个表达式的参数，则类型推导算法推导 p 的数据类型为 \@ TT (\@ p) 。 例如：  
  
    ```sql
    SELECT * FROM t WHERE c1 = @p1 AND @p2 = dbo.tbl(@p3)  
    ```  
  
     \@P1、 \@ p2 和 p3 的数据类型 \@ 将为 c1 的数据类型、dbo.tbl 的返回数据类型，以及 dbo.tbl 的参数数据类型。  
  
     作为一种特殊情况，如果 \@ p 是 a、= 运算符的参数，则 \<, > 简单的 \<=, or > 扣缴规则不适用。 类型推断算法使用下一节介绍的一般推断规则。 例如，如果 c1 是 char(30) 数据类型的列，请考虑下面两个查询：  
  
    ```sql
    SELECT * FROM t WHERE c1 = @p  
    SELECT * FROM t WHERE c1 > @p  
    ```  
  
     在第一种情况下，类型推导算法推导 **char (30)** 与 \@ 本主题前面的规则的 p 数据类型相同。 在第二种情况下，根据下一部分中的常规扣缴规则，类型推导算法推导 **varchar (8000)** 。  
  
-   一般推断  
  
     如果简单推断不适用，则为未声明的参数考虑以下数据类型：  
  
    -   整数数据类型 (**位**、 **tinyint**、 **smallint**、 **int**、 **bigint**)   
  
    -   Money 数据类型 (**smallmoney**、 **money**)   
  
    -   浮点数据类型 (**float**， **real**)   
  
    -   **数值 (38，19)** -不考虑其他数值或 decimal 数据类型。  
  
    -   **varchar (8000)**， **varchar (max)**， **nvarchar (4000)** 和 **nvarchar (最大)** 的字符串数据类型 (例如 **文本**、 **char (8000)**、 **nvarchar (30)** 等。  
  
    -   **varbinary (8000)** 和 **varbinary (max)** 其他二进制数据类型不被视为 (，如 **图像**、 **二进制 (8000)**、 **varbinary (30)** 等。  
  
    -   **日期**、 **时间 (7)**、 **smalldatetime**、 **datetime**、 **datetime2 (7)**， **datetimeoffset (7)** 其他日期和时间类型，如 **时间 (4)**，则不考虑这种情况。  
  
    -   **sql_variant**  
  
    -   **xml**  
  
    -   CLR、 **geometry**、 **geography**) **(CLR** 系统定义类型  
  
    -   CLR 用户定义的类型  
  
### <a name="selection-criteria"></a>选择条件  
 在候选数据类型中，将拒绝使查询无效的任何数据类型。 在其余候选数据类型中，类型推断算法将根据以下规则选择一种数据类型。  
  
1.  选择在 E (p) 中生成最小数量的隐式转换的数据类型 \@ 。 如果特定的数据类型为 E (\@ p) （与 TT (p) 不同）生成数据类型 \@ ，则类型推导算法会将此数据类型视为从 E (\@ p) 到 TT (p) 的数据类型的额外隐式转换 \@ 。  
  
     例如：  
  
    ```sql
    SELECT * FROM t WHERE Col_Int = Col_Int + @p  
    ```  
  
     在这种情况下，E (\@ p) Col_Int + \@ p 和 TT (\@ p) 是 **Int**。为 p 选择 **int** ， \@ 因为它不生成隐式转换。 选择任何其他数据类型都会产生至少一次隐式转换。  
  
2.  如果多种数据类型都产生次数最少的转换，则使用具有较高优先级的数据类型。 例如：  
  
    ```sql
    SELECT * FROM t WHERE Col_Int = Col_smallint + @p  
    ```  
  
     在这种情况下， **int** 和 **smallint** 产生一个转换。 每种其他数据类型产生多次转换。 由于 **int** 优先于 **smallint**，因此 **int** 用于 \@ p。 有关数据类型优先级的详细信息，请参阅 [数据类型优先级 &#40;transact-sql&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md)。  
  
     只有在每种数据类型之间的隐式转换次数相同（按照规则 1）并且某种数据类型具有最高优先级时，此规则才适用。 如果没有隐式转换，数据类型推断将失败并发生错误。 例如，在查询中 `SELECT @p FROM t` ，数据类型推导失败，因为 p 的任何数据类型 \@ 都是相同的。 例如，没有从 **int** 到 **xml** 的隐式转换。  
  
3.  如果两个相似的数据类型绑定到规则1下，例如 **varchar (8000)** 和 **varchar (max)**，则选择较小的数据类型 (**varchar (8000)**) 。 同一原则适用于 **nvarchar** 和 **varbinary** 数据类型。  
  
4.  就规则 1 而言，类型推断算法倾向于将某些转换视为比其他转换好。 转换从最好到最坏依次为：  

    1.  不同长度的相同基本数据类型之间的转换。  
  
    2.  相同数据类型的固定长度和可变长度版本之间的转换 (例如， **char** 到 **varchar**) 。  
  
    3.  **NULL** 和 **int** 之间的转换。  
  
    4.  任何其他转换。  
  
 例如，对于查询 `SELECT * FROM t WHERE [Col_varchar(30)] > @p` ，选择 **varchar (8000)** ，因为) 最好使用转换 (。 对于查询 `SELECT * FROM t WHERE [Col_char(30)] > @p` ，仍选择 **varchar (8000)** ，因为这会导致类型 (b) 转换，并且由于 **varchar (4000 (**) )  () 转换类型。  
  
 作为最后一个示例，提供查询时 `SELECT NULL + @p` ，将为 p 选择 **int** ， \@ 因为它会导致类型 (c) 转换。  
  
## <a name="permissions"></a>权限  
 要求具有执行 \@ tsql 参数的权限。  
  
## <a name="examples"></a>示例  
 以下示例返回一些信息，例如，未声明的 `@id` 和 `@name` 参数的预期数据类型。  
  
```sql
sp_describe_undeclared_parameters @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes  
WHERE object_id = @id OR name = @name'  
  
```  
  
 如果 `@id` 参数是作为 `@params` 引用提供的，则会在结果集中省略 `@id` 参数，而仅描述 `@name` 参数。  
  
```sql
sp_describe_undeclared_parameters @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes  
WHERE object_id = @id OR NAME = @name',  
@params = N'@id int'  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_describe_first_result_set &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)
