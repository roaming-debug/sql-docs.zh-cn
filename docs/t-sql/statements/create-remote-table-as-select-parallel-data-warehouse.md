---
description: CREATE REMOTE TABLE AS SELECT（并行数据仓库）
title: CREATE REMOTE TABLE AS SELECT（并行数据仓库）
ms.custom: seo-dt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.assetid: 16ef8191-7587-45a3-9ee9-7d99b7088de3
author: ronortloff
ms.author: rortloff
ms.reviewer: jrasnick
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: dc97f307022a28bcb2df00da4335c8676d32342f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471948"
---
# <a name="create-remote-table-as-select-parallel-data-warehouse"></a>CREATE REMOTE TABLE AS SELECT（并行数据仓库）
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  从 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 数据库中选择数据，并将此数据复制到远程服务器上的 SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的新表中。 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 使用包含 MPP 查询进程的所有优势的设备来选择远程副本的数据。 对需要 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能的方案使用此功能。  
  
 要配置远程服务器，请参阅 [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 中的“远程表复制”。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE REMOTE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }  AT ('<connection_string>')  
    [ WITH ( BATCH_SIZE = batch_size ) ]  
    AS <select_statement>  
[;]  
  
<connection_string> ::=   
    Data Source = { IP_address | hostname } [, port ]; User ID = user_name ;Password = password;  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```  
  
## <a name="arguments"></a>参数  
 *database_name*  
 在其中创建远程表的数据库。 database_name 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。 默认情况下，用户使用默认数据库登录到目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
 *schema_name*  
 新表的架构。 默认情况下，用户使用默认架构登录到目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
 *table_name*  
 新表的名称。 有关允许的表名的详细信息，请参阅 [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 中的“对象命名规则”。  
  
 远程表已创建为堆。 没有 CHECK 约束或触发。 远程表列的排序规则与源表列的排序规则相同。 这适用于类型为 char、nchar、varchar 和 nvarchar 的列   。  
  
 connection_string  
 指定用于连接到远程服务器和数据库的 `Data Source`、`User ID` 和 `Password` 参数的字符串。  
  
 连接字符串是由分号分隔的键值对列表。 关键字不区分大小写。 忽略键值对之间的空格。 但是，值可能会区分大小写，具体取决于数据源。  
  
 *数据源*  
 指定名称或 IP 地址和远程 SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 TCP 端口号的参数。  
  
 hostname 或 IP_address   
 远程服务器计算机的名称或远程服务器的 IPv4 地址。 不支持 IPv6 地址。 可以在格式 Computer_Name\Instance_Name 或 IP_address\Instance_Name 中指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 命名实例 。 服务器必须是远程的，因此才不会指定为（本地）。  
  
 TCP 端口号  
 连接的 TCP 端口号。 对于没有在默认端口 1433 上侦听的 SQL Server 实例，可以将其 TCP 端口号指定为 0 到 65535 之间的数字。 例如：ServerA,1450 或 10.192.14.27,1435   
  
> [!NOTE]  
>  建议使用 IP 地址连接到远程服务器。 根据网络配置，使用计算机名称进行连接可能需要额外的步骤，才能使用非设备 DNS 服务器将名称解析为正确的服务器。 使用 IP 地址连接时，不需要此步骤。 有关详细信息，请参阅 [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 中的“使用 DNS 转发器解析非设备 DNS 名称（分析平台系统）”。  
  
 user_name  
 有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录名。 最多包含 128 个字符。  
  
 password  
 登录密码。 最多包含 128 个字符。  
  
 batch_size  
 每批的最大行数。 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 将批处理中的行发送到目标服务器。 Batch_size 为一个大于或等于 0 的正整数。 默认为 0。  
  
 WITH common_table_expression  
 指定临时命名的结果集，这些结果集称为公用表表达式 (CTE)。 有关详细信息，请参阅 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)。  
  
 SELECT \<select_criteria>：指定哪些数据将填充新远程表的查询谓词。 有关 SELECT 语句的信息，请参阅 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 需要：  
  
-   对 SELECT 子句中每个对象的 SELECT 权限。  
  
-   对目标 SMP 数据库的 CREATE TABLE 权限。  
  
-   对目标 SMP 架构的 ALTER、INSERT 和 SELECT 权限。  
  
## <a name="error-handling"></a>错误处理  
 如果未能将数据复制到远程数据库，[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 将终止操作、记录错误并尝试删除远程表。 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 不保证成功清理新表。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 **远程目标服务器**：  
  
-   TCP 是连接到远程服务器所需的默认并仅受支持的协议。  
  
-   目标服务器必须为非设备服务器。 CREATE REMOTE TABLE 不能用于将数据从一个设备复制到另一个设备。  
  
-   CREATE REMOTE TABLE 语句仅创建新表。 因此，不能已经存在新表。 但必须已经存在远程数据库和架构。  
  
-   远程服务器必须具有可用空间，以存储从设备传输到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 远程数据库的数据。  
  
 **SELECT 语句**：  
  
-   选择条件中不支持 ORDER BY 和 TOP 子句。  
  
-   在活动事务内部或会话的 AUTOCOMMIT OFF 设置处于活动状态时，无法运行 CREATE REMOTE TABLE。  
  
 [SET ROWCOUNT (Transact-SQL)](../../t-sql/statements/set-rowcount-transact-sql.md) 对此语句没有影响。 要实现类似的行为，请使用 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)。  
  
## <a name="locking-behavior"></a>锁定行为  
 创建远程表后，直到复制开始，才锁定目标表。 因此，在复制开始前，另一个进程可能会删除已创建的远程表。 当这种情况发生时，[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 会生成错误，且复制失败。  
  
## <a name="metadata"></a>元数据  
 使用 [sys.dm_pdw_dms_workers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql.md) 查看将所选数据复制到远程 SMP 服务器的进度。 PARALLEL_COPY_READER 类型的行包含此信息。  
  
## <a name="security"></a>安全性  
 CREATE REMOTE TABLE 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证连接到远程 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，而不使用 Windows 身份验证。  
  
 除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 端口、管理端口和托管端口外，[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 面向外部的网络必须启用防火墙。  
  
 为了帮助防止数据意外丢失或损坏，用于从设备复制到目标数据库的用户帐户应仅具有对目标数据库的最低要求的权限。  
  
 连接设置允许使用 SSL 保护的用户名和密码数据连接到 SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，但也以明文形式发送未加密的实际数据。 当发生这种情况时，恶意用户可能会截获 CREATE REMOTE TABLE 语句文本，其中包含登录到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户名和密码。 为了避免这种风险，在连接到 SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，请使用数据加密。  
  
##  <a name="examples"></a><a name="Examples"></a> 示例  
  
### <a name="a-creating-a-remote-table"></a>A. 创建远程表  
 此示例在数据库 `OrderReporting` 和架构 `Orders` 上创建名为 `MyOrdersTable` 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SMP 远程表。 `OrderReporting` 数据库在名为 `SQLA` 的服务器上，该服务器侦听默认端口 1433。 连接到服务器时使用用户 `David` 的凭据，密码为 `e4n8@3`。  
  
```sql  
CREATE REMOTE TABLE OrderReporting.Orders.MyOrdersTable  
AT ( 'Data Source = SQLA, 1433; User ID = David; Password = e4n8@3;' )  
AS SELECT <select_criteria>;  
```  
  
### <a name="b-querying-the-sysdm_pdw_dms_workers-dmv-for-remote-table-copy-status"></a>B. 查询 sys.dm_pdw_dms_workers DMV 的远程表复制状态  
 此查询显示了如何查看远程表复制的复制状态。  
  
```sql  
SELECT * FROM sys.dm_pdw_dms_workers   
WHERE type = 'PARALLEL_COPY_READER';  
```  
  
### <a name="c-using-a-query-join-hint-with-create-remote-table"></a>C. 通过 CREATE REMOTE TABLE 使用查询联接提示  
 此查询显示通过 CREATE REMOTE TABLE 使用查询联接提示的基本语法。 将查询提交到控制节点后，在计算节点上运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在生成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询计划时应用哈希联接策略。 有关联接提示以及如何使用 OPTION 子句的详细信息，请参阅 [OPTION 子句 (Transact-SQL)](../../t-sql/queries/option-clause-transact-sql.md)。  
  
```sql  
USE ssawPDW;  
CREATE REMOTE TABLE OrderReporting.Orders.MyOrdersTable  
AT ( 'Data Source = SQLA, 1433; User ID = David; Password = e4n8@3;' )  
    AS SELECT T1.* FROM OrderReporting.Orders.MyOrdersTable T1   
    JOIN OrderReporting.Order.Customer T2  
    ON T1.CustomerID=T2.CustomerID OPTION (HASH JOIN);  
```  
  
  

