---
description: sp_sproc_columns (Transact-SQL)
title: sp_sproc_columns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_sproc_columns
- sp_sproc_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_sproc_columns
ms.assetid: 62c18c21-35c5-4772-be0d-ffdcc19c97ab
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 180d57f7daacb1f7aeb6b638dba03f893340c1d2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97404150"
---
# <a name="sp_sproc_columns-transact-sql"></a>sp_sproc_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为当前环境中的单个存储过程或用户定义函数返回列信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_sproc_columns [[@procedure_name = ] 'name']   
    [ , [@procedure_owner = ] 'owner']   
    [ , [@procedure_qualifier = ] 'qualifier']   
    [ , [@column_name = ] 'column_name']  
    [ , [@ODBCVer = ] 'ODBCVer']  
    [ , [@fUsePattern = ] 'fUsePattern']  
```  
  
## <a name="arguments"></a>自变量  
`[ @procedure_name = ] 'name'` 用于返回目录信息的过程的名称。 *name* 为 **nvarchar (** 390 **)**，默认值为%，表示当前数据库中的所有表。 支持通配符模式匹配。  
  
`[ @procedure_owner = ] 'owner'` 过程的所有者的名称。 *所有者* 为 **nvarchar (** 384 **)**，默认值为 NULL。 支持通配符模式匹配。 如果未指定 *owner* ，则应用基础 DBMS 的默认过程可见性规则。  
  
 如果当前用户拥有具有指定名称的过程，则返回有关该过程的信息。 如果未指定 *owner*，并且当前用户没有具有指定名称的过程，则 **sp_sproc_columns** 将查找数据库所有者拥有的具有指定名称的过程。 如果存在该过程，则返回有关该过程的列信息。  
  
`[ @procedure_qualifier = ] 'qualifier'` 过程限定符的名称。 *限定符* 的值为 **sysname**，默认值为 NULL。 各种 DBMS 产品支持表的三部分命名 (*qualifier.owner.name*) 。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，此参数表示数据库名称。 在某些产品中，该列表示表所在的数据库环境的服务器名。  
  
`[ @column_name = ] 'column_name'` 是单个列，只需要目录信息的一列时使用。 *column_name* 为 **nvarchar (** 384 **)**，默认值为 NULL。 如果省略 *column_name* ，则返回所有列。 支持通配符模式匹配。 为了达到最佳的互操作性，网关客户端应只采用 ISO 标准模式匹配（% 和 _ 通配符）。  
  
`[ @ODBCVer = ] 'ODBCVer'` 所使用的 ODBC 的版本。 *ODBCVer* 的值为 **int**，默认值为2，指示 ODBC 版本2.0。 有关 ODBC 版本2.0 和 ODBC 版本3.0 之间的差异的详细信息，请参阅 odbc **SQLProcedureColumns** 规范 for odbc version 3。0  
  
`[ @fUsePattern = ] 'fUsePattern'` 确定下划线 (_) 、百分比 (% ) 和方括号 ( [] ) 字符是否解释为通配符。 有效值为 0（模式匹配为关闭状态）和 1（模式匹配为打开状态）。 *fUsePattern* 的值为 **bit**，默认值为1。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**PROCEDURE_QUALIFIER**|**sysname**|过程限定符名称。 该列可以为 NULL。|  
|**PROCEDURE_OWNER**|**sysname**|过程所有者名称。 该列始终返回值。|  
|**PROCEDURE_NAME**|**nvarchar (** 134 **)**|过程名。 该列始终返回值。|  
|**COLUMN_NAME**|**sysname**|返回的每个 **TABLE_NAME** 列的列名。 该列始终返回值。|  
|**COLUMN_TYPE**|**smallint**|该字段始终返回值：<br /><br /> 0 = SQL_PARAM_TYPE_UNKNOWN<br /><br /> 1 = SQL_PARAM_TYPE_INPUT<br /><br /> 2 = SQL_PARAM_TYPE_OUTPUT<br /><br /> 3 = SQL_RESULT_COL<br /><br /> 4 = SQL_PARAM_OUTPUT<br /><br /> 5 = SQL_RETURN_VALUE|  
|**DATA_TYPE**|**smallint**|ODBC 数据类型的整数代码。 如果此数据类型无法映射到 ISO 类型，则值为 NULL。 本机数据类型名称在 **TYPE_NAME** 列中返回。|  
|**TYPE_NAME**|**sysname**|数据类型的字符串表示形式。 这是由基础 DBMS 表示的数据类型名称。|  
|**PRECISION**|**int**|有效数字位数。 **PRECISION** 列的返回值以10为底。|  
|**LENGTH**|**int**|数据的传输大小。|  
|**纵向**|**smallint**|小数点右边的数字位数。|  
|**RADIX**|**smallint**|数值类型的基数。|  
|**可以为 NULL**|**smallint**|指定为空性：<br /><br /> 1 = 可创建允许空值的数据类型。<br /><br /> 0 = 不允许空值。|  
|**备注**|**varchar (** 254 **)**|对过程列的说明。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不为此列返回值。|  
|**COLUMN_DEF**|**nvarchar (** 4000 **)**|列的默认值。|  
|**SQL_DATA_TYPE**|**smallint**|SQL 数据类型在描述符的 **type** 字段中显示的值。 此列与 DATA_TYPE 列相同，datetime 和 ISO interval 数据类型除外    。 该列始终返回值。|  
|**SQL_DATETIME_SUB**|**smallint**|如果 SQL_DATA_TYPE 的值为 SQL_DATETIME 或 SQL_INTERVAL，则为 datetime ISO interval 子代码      。 对于 **日期时间** 和 ISO **间隔** 以外的数据类型，此字段为 NULL。|  
|**CHAR_OCTET_LENGTH**|**int**|**字符** 或 **二进制** 数据类型列的最大长度（以字节为单位）。 对于所有其他数据类型，此列返回 NULL。|  
|**ORDINAL_POSITION**|**int**|列在表中的序号位置。 表中的第一列为 1。 该列始终返回值。|  
|**IS_NULLABLE**|**varchar (254)**|表中的列的为 Null 性。 根据 ISO 规则确定为 Null 性。 遵从 ISO 标准的 DBMS 不能返回空字符串。<br /><br /> 如果列可包含 NULL，则显示 YES；如果列不能包含 NULL，则显示 NO。<br /><br /> 如果为 Null 性为未知，该列将返回零长度字符串。<br /><br /> 该列返回的值与 NULLABLE 列返回的值不同。|  
|**SS_DATA_TYPE**|**tinyint**|扩展存储过程使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。 有关详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。|  
  
## <a name="remarks"></a>备注  
 **sp_sproc_columns** 等效于 ODBC 中的 **SQLProcedureColumns** 。 返回的结果按 **PROCEDURE_QUALIFIER**、 **PROCEDURE_OWNER**、 **PROCEDURE_NAME** 和参数在过程定义中的显示顺序进行排序。  
  
## <a name="permissions"></a>权限  
 需要对架构的 SELECT 权限。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的目录存储过程 ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
