---
description: sp_execute_remote（Azure SQL 数据库）
title: Azure SQL Database (sp_execute_remote) |Microsoft Docs
ms.custom: ''
ms.date: 02/01/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- sp_execute_remote
- sp_execute_remote_TSQL
helpviewer_keywords:
- remote execution
- queries, remote execution
ms.assetid: ca89aa4c-c4c1-4c46-8515-a6754667b3e5
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current
ms.openlocfilehash: 2ab1c51c53282b5f245cf7da0d33cf4f797bf53a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439459"
---
# <a name="sp_execute_remote-azure-sql-database"></a>sp_execute_remote（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  [!INCLUDE[tsql](../../includes/tsql-md.md)]对单个远程 AZURE SQL 数据库或在水平分区方案中用作分片的一组数据库执行语句。  
  
 存储过程是弹性查询功能的一部分。  请参阅 [AZURE SQL 数据库弹性数据库查询概述](/azure/azure-sql/database/elastic-query-overview) 和 [适用于分片的弹性数据库查询 (水平分区) ](/azure/azure-sql/database/elastic-query-horizontal-partitioning)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_execute_remote [ @data_source_name = ] datasourcename  
[ , @stmt = ] statement  
[   
  { , [ @params = ] N'@parameter_name data_type [,...n ]' }   
     { , [ @param1 = ] 'value1' [ ,...n ] }  
]  
```  
  
## <a name="arguments"></a>自变量  
 [ \@ data_source_name =]   
 标识执行语句的外部数据源。 请参阅 [CREATE EXTERNAL DATA SOURCE &#40;transact-sql&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)。 外部数据源的类型可以是 "RDBMS" 或 "SHARD_MAP_MANAGER"。  
  
 [ \@ stmt =] *语句*  
 是包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或批处理的 Unicode 字符串。 \@stmt 必须是 Unicode 常量或 Unicode 变量。 不允许使用更复杂的 Unicode 表达式（例如使用 + 运算符连接两个字符串）。 不允许使用字符常量。 如果指定了 Unicode 常量，则必须使用 **N** 作为前缀。例如，Unicode 常量 **N "sp_who"** 有效，但字符常量 **"sp_who"** 不是。 字符串的大小仅受可用数据库服务器内存限制。 在64位服务器上，字符串的大小限制为 2 GB，最大值为 **nvarchar (max)**。  
  
> [!NOTE]  
>  \@stmt 可以包含与变量名称格式相同的参数，例如： `N'SELECT * FROM HumanResources.Employee WHERE EmployeeID = @IDParameter'`  
  
 Stmt 中包含的每个参数都 \@ 必须在 " \@ params 参数定义" 列表和 "参数值" 列表中都有相应的条目。  
  
 [ \@ params =] N ' \@*parameter_name * * data_type* [,.。。 *n* ] "  
 一个字符串，其中包含在 stmt 中嵌入的所有参数的定义 \@ 。字符串必须是 Unicode 常量或 Unicode 变量。 每个参数定义由参数名称和数据类型组成。 *n* 是表示附加参数定义的占位符。 在 stmtmust 中指定的每个参数 \@ 都可在 params 中定义 \@ 。 如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] stmt 中的语句或批处理不 \@ 包含参数， \@ 则无需参数。 该参数的默认值为 NULL。  
  
 [ \@ param1 =] '*value1*'  
 参数字符串中定义的第一个参数的值。 该值可以是 Unicode 常量，也可以是 Unicode 变量。 对于 stmt 中包含的每个参数，都必须提供一个参数值 \@ 。如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] stmt 中的语句或批处理没有参数，则不需要这些值 \@ 。  
  
 *n*  
 附加参数值的占位符。 这些值只能为常量或变量， 不能是很复杂的表达式（例如函数）或使用运算符生成的表达式。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或非零（失败）  
  
## <a name="result-sets"></a>结果集  
 返回第一个 SQL 语句的结果集。  
  
## <a name="permissions"></a>权限  
 需要 `ALTER ANY EXTERNAL DATA SOURCE` 权限。  
  
## <a name="remarks"></a>备注  
 `sp_execute_remote` 必须按照上述语法部分中所述，按特定顺序输入参数。 如果这些参数的输入顺序不正确，则会显示一条错误消息。  
  
 `sp_execute_remote` 与执行的行为相同， [&#40;transact-sql&#41;](../../t-sql/language-elements/execute-transact-sql.md) 与批处理和名称的范围有关。 在执行 sp_execute_remote 语句之前，不会编译 sp_execute_remote *\@ stmt* 参数中的 transact-sql 语句或批处理。  
  
 `sp_execute_remote` 将附加列添加到名为 "$ShardName" 的结果集，该结果集包含生成该行的远程数据库的名称。  
  
 `sp_execute_remote` 可以与 [sp_executesql &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md)类似。  
  
## <a name="examples"></a>示例  
### <a name="simple-example"></a>简单示例  
 下面的示例在远程数据库上创建和执行简单的 SELECT 语句。  
  
```sql  
EXEC sp_execute_remote  
    N'MyExtSrc',  
    N'SELECT COUNT(w_id) AS Count_id FROM warehouse'   
```  
  
### <a name="example-with-multiple-parameters"></a>具有多个参数的示例  
在用户数据库中创建数据库范围的凭据，并为 master 数据库指定管理员凭据。 创建一个外部数据源，该数据源指向 master 数据库，并指定数据库作用域凭据。 接下来，在用户数据库中，执行 `sp_set_firewall_rule` master 数据库中的过程。 此 `sp_set_firewall_rule` 过程需要3个参数，并要求 `@name` 参数为 Unicode。

```
EXEC sp_execute_remote @data_source_name  = N'PointToMaster', 
@stmt = N'sp_set_firewall_rule @name, @start_ip_address, @end_ip_address', 
@params = N'@name nvarchar(128), @start_ip_address varchar(50), @end_ip_address varchar(50)',
@name = N'TempFWRule', @start_ip_address = '0.0.0.2', @end_ip_address = '0.0.0.2';
```

## <a name="see-also"></a>另请参阅：

[CREATE DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)  
[CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)  
