---
description: CREATE SEQUENCE (Transact-SQL)
title: CREATE SEQUENCE (Transact-SQL)
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SEQUENCE
- CREATE SEQUENCE
- SEQUENCE_TSQL
- CREATE_SEQUENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE SEQUENCE statement
- sequence number object, creating
- sequence object
- number, sequence
ms.assetid: 419f907b-8a72-4d6c-80cb-301df44c24c1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0fb687f42e65bb1f1ec74046927282dc90563ccd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188502"
---
# <a name="create-sequence-transact-sql"></a>CREATE SEQUENCE (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  创建一个序列对象并指定其属性。 序列是用户定义的绑定到架构的对象，该对象可根据创建序列所依据的规范来生成数值序列。 这组数值以定义的间隔按升序或降序生成，并且可配置为用尽时重新启动（循环）。 序列不与特定表相关联，这一点与标识列不同。 应用程序将引用某一序列对象以便检索其下一个值。 序列与表之间的关系由应用程序控制。 用户应用程序可以引用一个序列对象，并跨多个行和表协调值。  
  
 与在插入行时生成的标识列值不同，应用程序可以获得下一个序列号，而不必通过调用 [NEXT VALUE FOR 函数](../../t-sql/functions/next-value-for-transact-sql.md)来插入行。 使用 [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) 同时获取多个序列号。  
  
 有关同时使用 **CREATE SEQUENCE** 和 **NEXT VALUE FOR** 函数的信息，请参阅 [序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE SEQUENCE [schema_name . ] sequence_name  
    [ AS [ built_in_integer_type | user-defined_integer_type ] ]  
    [ START WITH <constant> ]  
    [ INCREMENT BY <constant> ]  
    [ { MINVALUE [ <constant> ] } | { NO MINVALUE } ]  
    [ { MAXVALUE [ <constant> ] } | { NO MAXVALUE } ]  
    [ CYCLE | { NO CYCLE } ]  
    [ { CACHE [ <constant> ] } | { NO CACHE } ]  
    [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
sequence_name  
指定数据库中标识序列的唯一名称。 类型为 sysname。  
  
[ built_in_integer_type | user-defined_integer_type  
序列可定义为任何整数类型。 允许使用下面的类型。  
  
-   **tinyint** - 范围为 0 到 255  
-   **smallint** - 范围为 -32,768 到 32,767  
-   **int** - 范围为 -2,147,483,648 到 2,147,483,647  
-   **bigint** - 范围为 -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807  
-   **decimal** 或 **numeric**，小数位数为 0。  
-   基于这些允许类型之一的任何用户定义数据类型（别名类型）。  
  
如果未提供任何数据类型，则将 bigint 数据类型用作默认数据类型。  
  
START WITH \<constant>  
序列对象返回的第一个值。 START 值必须小于或等于序列对象的最大值并大于或等于其最小值。 新序列对象的默认起始值是升序序列对象的最小值和降序序列对象的最大值。  
  
INCREMENT BY \<constant>  
每次调用 NEXT VALUE FOR 函数时序列对象值递增（如果为负数，则为递减）的值。 如果增量是负值，则序列对象为降序，否则为升序。 增量不能为 0。 新序列对象的默认增量为 1。  
  
[ MINVALUE \<constant> | NO MINVALUE ]  
指定序列对象的边界。 一个新序列对象的默认最小值是该序列对象的数据类型的最小值。 对于 **tinyint** 数据类型，此值为零，对于所有其他数据类型则为负数。  
  
[ MAXVALUE \<constant> | NO MAXVALUE  
指定序列对象的边界。 一个新序列对象的默认最大值是该序列对象的数据类型的最大值。  
  
[ CYCLE | NO CYCLE ]  
此属性指定当超过序列对象的最小值或最大值时，序列对象是应从最小值（对于降序序列对象，则为最大值）重新开始，还是应引发异常。 新序列对象的默认循环选项是 NO CYCLE。  
  
> [!NOTE]
> 循环 SEQUENCE 从最小值或最大值（而不是从起始值）重新开始。  
  
[ CACHE [\<constant> ] | NO CACHE ]  
通过最大限度地减少生成序列编号所需的磁盘 IO 数，可以提高使用序列对象的应用程序的性能。 默认值为 CACHE。  
  
例如，如果选择的缓存大小为 50，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并不会缓存 50 个单个值。 它只是缓存当前值和缓存中保留的值数。 这意味着，存储缓存所需的内存量始终为序列对象的数据类型的两个实例。  
  
> [!NOTE]  
> 如果启用缓存选项而不指定缓存大小，数据库引擎将选择一个大小。 但是，所选的大小并不是一成不变。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 可能会更改计算缓存大小的方法而不另行通知。  
  
当使用 CACHE 选项创建时，意外关机（如电源故障）可能导致缓存中保留的序列号丢失。  
  
## <a name="general-remarks"></a>一般备注  
 序列号在当前事务的作用域之外生成。 无论提交还是回滚使用序列号的事务，都会占用序列号。 只有在记录被完全填充后，才会发生重复验证。 在某些情况下，这可能会导致在创建过程中将相同数字用于多个记录，但随后被识别为重复。 如果发生这种情况并且其他自动编号值已应用于后续记录，则可能会导致自动编号值与预期行为之间存在差距。
  
### <a name="cache-management"></a>缓存管理  
 为了提高性能，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会预分配 CACHE 参数所指定数量的序列号。  
  
 例如，用起始值 1 和缓存大小 15 创建一个新序列。 在需要第一个值时，便可从内存中使用值 1 到 15。 最后缓存的值 (15) 将写入磁盘上的系统表中。 使用完所有的 15 个数字后，下一个请求（数字 16）将导致重新分配缓存。 新的最后缓存值 (30) 将写入系统表中。  
  
 如果在使用了 22 个数字后[!INCLUDE[ssDE](../../includes/ssde-md.md)]停止，则会将内存中的下一个预期序列号 (23) 写入系统表，替换以前存储的数字。  
  
 在 SQL Server 重新启动并且需要一个序列号之后，将从系统表中读取起始编号 (23)。 会为内存分配 15 个数字 (23-38) 的缓存量，并将下一个非缓存编号 (39) 写入到系统表。  
  
 如果[!INCLUDE[ssDE](../../includes/ssde-md.md)]因某事件（如电源故障）异常停止，序列将使用从系统表中读取的编号 (39) 重新开始。 分配给内存（但用户或应用程序永远不会请求）的任何序列号都将丢失。 此功能可能会留有间隙，但可以保证不会两次为单个序列对象发出同一个值，除非该对象定义为 CYCLE 或手动重新启动。  
  
 将通过跟踪当前值（发出的最后一个值）和缓存中留下的值数量来在内存中保留缓存。 因此，缓存所使用的内存量始终为序列对象的数据类型的两个实例。  
  
 如果将缓存参数设置为 NO CACHE，则每次使用序列时都会将当前的序列值写入系统表。 这样会增加磁盘访问，从而降低性能，但可以减少出现意外间隙的机会。 如果使用 NEXT VALUE FOR 或 sp_sequence_get_range 函数请求编号，但之后不使用该编号或在未提交的事务中使用该编号，那么仍然会出现间隙 。  
  
 当序列对象使用 CACHE 选项时，如果重新启动该序列对象或改变 INCREMENT、CYCLE、MINVALUE、MAXVALUE 或缓存大小属性，则会导致在更改发生之前将缓存写入系统表    。 然后，从当前值（即没有跳过任何数字）开始重新加载缓存。 更改缓存大小将立即生效。  
  
 **缓存的值可用时的 CACHE 选项**  
  
 每当请求序列对象以便为 CACHE 选项生成下一个值时，如果在序列对象的内存中缓存中提供未使用的值，则会发生以下过程。  
  
1.  计算序列对象的下一个值。  
  
2.  在内存中更新该序列对象的新当前值。  
  
3.  将计算的值返回给调用语句。  
  
**缓存用尽时的 CACHE 选项**  
  
 每当请求序列对象以便为 CACHE 选项生成下一个值时，如果缓存已用完，都会发生以下过程：  
  
1.  计算序列对象的下一个值。  
  
2.  计算新缓存的最后一个值。  
  
3.  锁定序列对象的系统表行，将在步骤 2 中计算的值（最后一个值）写入系统 表。 触发缓存已用完的 xevent，向用户通知新的持久值。  
  
**NO CACHE 选项**  
  
 每当请求序列对象以便为 NO CACHE 选项生成下一个值时，都会发生以下过程：  
  
1.  计算序列对象的下一个值。  
  
2.  将序列对象的新当前值写入系统表。  
  
3.  将计算的值返回给调用语句。  
  
## <a name="metadata"></a>元数据  
 有关序列的信息，请查询 [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md)。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求对 SCHEMA 拥有 **CREATE SEQUENCE**、 **ALTER** 或 **CONTROL** 权限。  
  
-   db_owner 和 db_ddladmin 固定数据库角色的成员可以创建、更改和删除序列对象。  
  
-   db_owner 和 db_datawriter 固定数据库角色的成员可以通过使序列对象生成数字来对其进行更新。  
  
 以下示例向用户 AdventureWorks\Larry 授予在 Test 架构中创建序列的权限。  
  
```sql  
GRANT CREATE SEQUENCE ON SCHEMA::Test TO [AdventureWorks\Larry]  
```  
  
 可以使用 ALTER AUTHORIZATION 语句转让序列对象的所有权。  
  
 如果序列使用用户定义的数据类型，则序列的创建者必须对该类型拥有 REFERENCES 权限。  
  
### <a name="audit"></a>审核  
 若要审核 CREATE SEQUENCE，请监视 SCHEMA_OBJECT_CHANGE_GROUP 。  
  
## <a name="examples"></a>示例  
 有关创建序列和使用 NEXT VALUE FOR 函数生成序列号的示例，请参阅[序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)。  
  
 下面的大多数示例都在名为 Test 的架构中创建序列对象。  
  
 若要创建 Test 架构，请执行以下语句。  
  
```sql  
CREATE SCHEMA Test ;  
GO  
```  
  
### <a name="a-creating-a-sequence-that-increases-by-1"></a>A. 创建按 1 递增的序列  
 在以下示例中，Thierry 创建一个名为 CountBy1 的序列，每次使用该序列时将增加 1。  
  
```sql  
CREATE SEQUENCE Test.CountBy1  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="b-creating-a-sequence-that-decreases-by-1"></a>B. 创建按 1 递减的序列  
 以下示例从 0 开始，每次使用时递减 1。  
  
```sql  
CREATE SEQUENCE Test.CountByNeg1  
    START WITH 0  
    INCREMENT BY -1 ;  
GO  
```  
  
### <a name="c-creating-a-sequence-that-increases-by-5"></a>C. 创建按 5 递增的序列  
 以下示例创建一个每次使用时增加 5 的序列。  
  
```sql  
CREATE SEQUENCE Test.CountBy1  
    START WITH 5  
    INCREMENT BY 5 ;  
GO  
```  
  
### <a name="d-creating-a-sequence-that-starts-with-a-designated-number"></a>D. 创建以指定数字开始的序列  
 在导入表后，Thierry 注意到所使用的最高 ID 号是 24,328。 Thierry 需要将生成从 24,329 开始的数字的序列。 下面的代码将创建一个从 24,329 开始、增量为 1 的序列。  
  
```sql  
CREATE SEQUENCE Test.ID_Seq  
    START WITH 24329  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="e-creating-a-sequence-using-default-values"></a>E. 使用默认值创建序列  
 以下示例将使用默认值创建一个序列。  
  
```sql  
CREATE SEQUENCE Test.TestSequence ;  
```  
  
 执行以下语句可查看序列的属性。  
  
```sql  
SELECT * FROM sys.sequences WHERE name = 'TestSequence' ;  
```  
  
 输出的部分列表演示了默认值。  
  
| 输出 | 默认值|  
|-|-|  
|`start_value`|`-9223372036854775808`|  
|`increment`|`1`|  
|`mimimum_value`|`-9223372036854775808`|  
|`maximum_value`|`9223372036854775807`|  
|`is_cycling`|`0`|  
|`is_cached`|`1`|  
|`current_value`|`-9223372036854775808`|  
  
### <a name="f-creating-a-sequence-with-a-specific-data-type"></a>F. 创建具有特定数据类型的序列

以下示例使用 smallint 数据类型（范围为 -32,768 到 32,767）创建一个序列。  
  
```sql  
CREATE SEQUENCE SmallSeq 
    AS smallint ;  
```  
  
### <a name="g-creating-a-sequence-using-all-arguments"></a>G. 使用所有参数创建序列  
 以下示例使用 decimal 数据类型（范围为 0 到 255）创建一个名为 DecSeq 的序列。 序列以 125 开始，每次生成数字时递增 25。 因为该序列配置为可循环，所以，当值超过最大值 200 时，序列将从最小值 100 重新开始。  
  
```sql  
CREATE SEQUENCE Test.DecSeq  
    AS decimal(3,0)   
    START WITH 125  
    INCREMENT BY 25  
    MINVALUE 100  
    MAXVALUE 200  
    CYCLE  
    CACHE 3  
;  
```  
  
 执行以下语句可查看第一个值；`START WITH` 选项为 125。  
  
```sql  
SELECT NEXT VALUE FOR Test.DecSeq;  
```  
  
 将该语句再执行三次，以返回 150、175 和 200。  
  
 再次执行该语句，以查看起始值如何循环回到 `MINVALUE` 选项值 100。  
  
 执行以下代码，以确认缓存大小并查看当前值。  
  
```sql  
SELECT cache_size, current_value   
FROM sys.sequences  
WHERE name = 'DecSeq' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER SEQUENCE (Transact-SQL)](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [DROP SEQUENCE (Transact-SQL)](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR (Transact-SQL)](../../t-sql/functions/next-value-for-transact-sql.md)   
 [序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
