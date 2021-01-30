---
description: sp_batch_params (Transact-SQL)
title: sp_batch_params (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_batch_params
- sp_batch_params_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_batch_params
ms.assetid: 7b92fe9e-e755-4b7a-8a15-822c58a813d3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e7ba37a2e9559cd48da23c5ed7cb7758b0405e09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206616"
---
# <a name="sp_batch_params-transact-sql"></a>sp_batch_params (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回一个行集，其中包含有关批中包含的参数的信息 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 **sp_batch_params** 仅分析指定的批处理，并返回有关嵌入的参数值的信息。 此命令不会执行批处理，也不会修改执行环境。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_batch_params [ [ @tsqlbatch = ] 'tsqlbatch' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @tsqlbatch = ] 'tsqlbatch'` 一个 Unicode 字符串，其中包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 所需的参数信息的语句或批处理。 *tsqlbatch* 是 **nvarchar (max)** 或可隐式转换为 **nvarchar (max)**。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**PARAMETER_NAME**|**sysname**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在批处理中找到的参数的名称。|  
|**COLUMN_TYPE**|**smallint**|此字段返回下列值之一：<br /><br /> 0 = SQL_PARAM_TYPE_UNKNOWN<br /><br /> 1 = SQL_PARAM_TYPE_INPUT<br /><br /> 2 = SQL_PARAM_TYPE_OUTPUT<br /><br /> 3 = SQL_RESULT_COL<br /><br /> 4 = SQL_PARAM_OUTPUT<br /><br /> 5 = SQL_RETURN_VALUE<br /><br /> 此列始终为 0。|  
|**DATA_TYPE**|**smallint**|参数的数据类型（ODBC 数据类型的整数代码）。 如果此数据类型无法映射到 ISO 类型，则值为 NULL。 本机数据类型名称在 **TYPE_NAME** 列中返回。 此值始终为 NULL。|  
|**TYPE_NAME**|**sysname**|数据类型的字符串表示形式，通过基础 DBMS 提供。 此值为 NULL。|  
|**PRECISION**|**int**|有效数字位数。 **PRECISION** 列的返回值以10为底。|  
|**LENGTH**|**int**|数据的传输大小。 此值为 NULL。|  
|**纵向**|**smallint**|小数点右边的数字位数。 此值为 NULL。|  
|**RADIX**|**smallint**|数值类型的基数。 此值为 NULL。|  
|**可以为 NULL**|**smallint**|指定为空性：<br /><br /> 1 = 可以将参数数据类型创建为允许空值。<br /><br /> 0 = 不允许空值。<br /><br /> 此值为 NULL。|  
|**SQL_DATA_TYPE**|**smallint**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型的值，在说明符的 TYPE 字段中显示。 此列与 DATA_TYPE 列相同，datetime 和 ISO interval 数据类型除外    。 该列始终返回值。 此值为 NULL。|  
|**SQL_DATETIME_SUB**|**smallint**|如果 **SQL_DATA_TYPE** 的值 SQL_DATETIME 或 SQL_INTERVAL，则为 **datetime** 或 ISO **interval** 子代码。 对于 datetime  和 ISO interval  以外的数据类型，该列为 NULL。 此值为 NULL。|  
|**CHAR_OCTET_LENGTH**|**int**|**字符** 或 **二进制** 数据类型参数的最大长度（以字节为单位）。 对于所有其他数据类型，此列返回 NULL。 此值始终为 NULL。|  
|**ORDINAL_POSITION**|**int**|参数在批处理中的顺序位置。 如果参数名称重复多次，则此列使用第一次出现的序号。 第一个参数的序号为 1。 该列始终返回值。|  
  
## <a name="permissions"></a>权限  
 向 **public** 授予执行 **sp_batch_params** 的权限。  
  
## <a name="examples"></a>示例  
 以下示例显示了传递给 `sp_batch_params` 的一个查询。 结果集枚举了一组嵌入参数值。  
  
```  
DECLARE @SQLString nvarchar(500);  
/* Build the SQL string */  
SET @SQLString =  
     N'SELECT * FROM AdventureWorks2012.HumanResources.Employee   
     WHERE BusinessEntityID = @BusinessEntityID';  
EXECUTE sp_batch_params @SQLString;  
```  
  
## <a name="see-also"></a>另请参阅  
 [运行存储过程](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [运行存储过程操作指南主题 &#40;ODBC&#41;](../native-client-odbc-how-to/running-stored-procedures-call-stored-procedures.md)   
 [运行存储过程 &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/stored-procedures-running.md)  
  
