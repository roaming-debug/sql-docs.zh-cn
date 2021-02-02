---
description: DECLARE CURSOR (Transact-SQL)
title: DECLARE CURSOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECLARE_CURSOR_TSQL
- CURSOR_TSQL
- DECLARE CURSOR
- CURSOR
dev_langs:
- TSQL
helpviewer_keywords:
- DECLARE CURSOR statement
- cursors [SQL Server], attributes
- local cursors [SQL Server]
- nesting cursors
- Transact-SQL cursors, attributes
- global cursors [SQL Server]
ms.assetid: 5a3a27aa-03e8-4c98-a27e-809282379b21
author: cawrites
ms.author: chadam
ms.openlocfilehash: e40722b9330fed0e4be9f48c4463369572659cc9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99120533"
---
# <a name="declare-cursor-transact-sql"></a>DECLARE CURSOR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  定义 [!INCLUDE[tsql](../../includes/tsql-md.md)] 服务器游标的属性，例如游标的滚动行为和用于生成游标所操作的结果集的查询。 `DECLARE CURSOR` 既接受基于 ISO 标准的语法，也接受使用一组 [!INCLUDE[tsql](../../includes/tsql-md.md)] 扩展的语法。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ISO Syntax  
DECLARE cursor_name [ INSENSITIVE ] [ SCROLL ] CURSOR   
     FOR select_statement   
     [ FOR { READ ONLY | UPDATE [ OF column_name [ ,...n ] ] } ]  
[;]  
Transact-SQL Extended Syntax  
DECLARE cursor_name CURSOR [ LOCAL | GLOBAL ]   
     [ FORWARD_ONLY | SCROLL ]   
     [ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]   
     [ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]   
     [ TYPE_WARNING ]   
     FOR select_statement   
     [ FOR UPDATE [ OF column_name [ ,...n ] ] ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 cursor_name  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 服务器游标定义的名称。 cursor_name 必须符合有关标识符的规则。  
  
 INSENSITIVE  
 定义一个游标，以创建将由该游标使用的数据的临时副本。 对游标的所有请求都从 tempdb 中的这一临时表中得到应答；因此，在对该游标进行提取操作时返回的数据中不反映对基表所做的修改，并且该游标不允许修改。 使用 ISO 语法时，如果省略 `INSENSITIVE`，则已提交的（任何用户）对基础表的删除和更新则会反映在后面的提取操作中。  
  
 SCROLL  
 指定所有的提取选项（`FIRST`、`LAST`、`PRIOR`、`NEXT`、`RELATIVE` 和 `ABSOLUTE`）均可用。 如果未在 ISO `DECLARE CURSOR` 中指定 `SCROLL`，则 `NEXT` 是唯一支持的提取选项。 如果还指定了 `FAST_FORWARD`，则无法指定 `SCROLL`。 如果未指定 `SCROLL`，则只有提取选项 `NEXT` 可用，且游标将变为 `FORWARD_ONLY`。
  
 select_statement  
 是定义游标结果集的标准 `SELECT` 语句。 在游标声明的 select_statement 中不允许使用关键字 `FOR BROWSE` 和 `INTO`。  
  
 如果 select_statement 中的子句与所请求的游标类型的功能有冲突，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将游标隐式转换为其他类型。  
  
 READ ONLY  
 禁止通过该游标进行更新。 无法在 `UPDATE` 或 `DELETE` 语句的 `WHERE CURRENT OF` 子句中引用游标。 该选项优先于要更新的游标的默认功能。  
  
 UPDATE [OF column_name [,...n]]  
 定义游标中可更新的列。 如果指定了 OF <column_name> [, <... n>]，则只允许修改所列出的列。 如果指定了 `UPDATE`，但未指定列的列表，则可以更新所有列。  
  
cursor_name  
[!INCLUDE[tsql](../../includes/tsql-md.md)] 服务器游标定义的名称。 cursor_name 必须符合有关标识符的规则。  
  
LOCAL  
指定该游标的范围对在其中创建它的批处理、存储过程或触发器是局部的。 该游标名称仅在这个作用域内有效。 在批处理、存储过程、触发器或存储过程 `OUTPUT` 参数中，该游标可由局部游标变量引用。 `OUTPUT` 参数用于将局部游标传递回调用批处理、存储过程或触发器，它们可在存储过程终止后给游标变量分配参数使其引用游标。 除非 `OUTPUT` 参数将游标传递回来，否则游标将在批处理、存储过程或触发器终止时隐式释放。 如果 `OUTPUT` 参数将游标传递回来，则游标在最后引用它的变量释放或离开作用域时释放。  
  
GLOBAL  
指定该游标范围对连接是全局的。 在由此连接执行的任何存储过程或批处理中，都可以引用该游标名称。 该游标仅在断开连接时隐式释放。  
  
> [!NOTE]  
>  如果 `GLOBAL` 和 `LOCAL` 参数都未指定，则默认值由“默认为本地游标”数据库选项的设置控制。  
  
FORWARD_ONLY  
指定游标只能向前移动，并从第一行滚动到最后一行。 `FETCH NEXT` 是唯一支持的提取选项。 对所有由当前用户发出（或由其他用户提交）并影响结果集中的行的插入、更新和删除语句，其效果在提取这些行时是可见的。 由于游标无法向后滚动，但是，在提取行后对数据库中的行进行的更改通过游标均不可见。 默认只进游标是动态的，这意味着处理当前行时会检测到所有更改。 这可实现更快速的游标打开，并使结果集能够显示对基础表所做的更新。 尽管只进游标不支持向后滚动，但应用程序可通过关闭并重新打开游标返回到结果集的开头。 如果指定了 `FORWARD_ONLY` 而没有指定 `STATIC`、`KEYSET` 和 `DYNAMIC` 关键字，则游标作为动态游标进行操作。 如果未指定 `FORWARD_ONLY` 和 `SCROLL`，则默认为 `FORWARD_ONLY`，除非指定了关键字 `STATIC`、`KEYSET` 或 `DYNAMIC`。 `STATIC`、`KEYSET` 和 `DYNAMIC` 游标默认为 `SCROLL`。 与 ODBC 和 ADO 等数据库 API 不同，`STATIC`、`KEYSET` 和 `DYNAMIC` [!INCLUDE[tsql](../../includes/tsql-md.md)] 游标支持 `FORWARD_ONLY`。  
   
 STATIC  
指定游标始终以第一次打开时的样式显示结果集，并制作数据的临时副本，供游标使用。 对游标的所有请求都通过 tempdb 中的这个临时表进行答复。 因此，对基表所做的插入、更新和删除操作不在对此游标所做的提取操作返回的数据中反映，并且在该游标打开后，不会检测对结果集的成员、顺序或值所做的更改。 尽管不需要，但静态游标可检测其自己的更新、删除和插入。 例如，假定静态游标提取行，然后另一个应用程序将更新该行。 如果应用程序通过静态游标重新提取行，尽管更改由其他应用程序执行，但看到的值将保持不变。 支持所有类型的滚动。 
  
KEYSET  
指定当游标打开时，游标中行的成员身份和顺序已经固定。 对行进行唯一标识的键集内置在 tempdb 内一个称为 keyset 的表中。 此游标在其检测更改的功能方面，提供介于静态和动态游标之间的功能。 比如静态游标，它不会始终检测对结果集的成员身份和顺序的更改。 比如动态游标，它会检测对结果集中的行值的更改。 由键集驱动的游标由一组唯一标识符（键）控制，这组键称为键集。 键是根据以唯一方式标识结果集中各行的一组列生成的。 键集是查询语句返回的所有行中的一组键值。 使用由键集驱动的游标，为游标中的每行生成和保存一个键，并将其存储在客户端工作站或服务器上。 访问每行时，存储的密钥可用来从数据源提取当前数据值。 在由键集驱动的游标中，如果键集完全填充，将冻结结果集成员身份。 此后，在重新打开结果集前，都不会执行影响成员身份的添加或更新操作。 用户滚动浏览结果集时，对数据值的更改（由键集所有者或其他进程执行）是可见的：
-  如果删除某一行，尝试提取该行时将返回为 -2 的 `@@FETCH_STATUS`，因为已删除的行在结果集中将显示为空白。 行键存在于键集中，但行不再存在于结果集中。 
-  游标外所做的插入（由其他进程执行）仅在关闭并重新打开游标后可见。 游标内部所做的插入在结果集的末尾可见。
-  从游标外部更新键值类似于删除旧行后再插入新行。 具有新值的行不可见，且尝试提取具有旧值的行时返回的 `@@FETCH_STATUS` 为 -2。 如果通过指定 `WHERE CURRENT OF` 子句来通过游标执行更新，则新值可见。 

> [!NOTE]  
> 如果查询引用了至少一个无唯一索引的表，则键集游标将转换为静态游标。  
  
DYNAMIC  
定义一个游标，无论更改是发生于游标内部还是由游标外的其他用户执行，在你四处滚动游标并提取新纪录时，该游标均能反映对其结果集中的行所做的所有数据更改。 因此，所有用户做的全部 UPDATE、INSERT 和 DELETE 语句均通过游标可见。 行的数据值、顺序和成员身份在每次提取时都会更改。 动态游标不支持 `ABSOLUTE` 提取选项。 在游标外部所做的更新直到提交时才可见（除非将游标的事务隔离级别设为 `UNCOMMITTED`）。 例如，假设动态游标提取两行，然后另一个应用程序将更新这两行之一并删除另一行。 然后，如果动态游标提取这两行，它将找不到已删除的行，但会显示已更新行的新值。 
  
FAST_FORWARD  
指定已启用了性能优化的 `FORWARD_ONLY` 和 `READ_ONLY` 游标。 如果还指定了 `SCROLL` 或 `FOR_UPDATE`，则无法指定 `FAST_FORWARD`。 此类型的游标不允许从游标内修改数据。  
  
> [!NOTE]  
> 可以在相同的 `DECLARE CURSOR` 语句中使用 `FAST_FORWARD` 和 `FORWARD_ONLY`。  
  
READ_ONLY  
禁止通过该游标进行更新。 无法在 `UPDATE` 或 `DELETE` 语句的 `WHERE CURRENT OF` 子句中引用游标。 该选项优先于要更新的游标的默认功能。  
  
SCROLL_LOCKS  
指定通过游标进行的定位更新或删除一定会成功。 将行读入游标时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将锁定这些行，以确保随后可对它们进行修改。 如果还指定了 `FAST_FORWARD` 或 `STATIC`，则无法指定 `SCROLL_LOCKS`。  
  
OPTIMISTIC  
指定如果行自读入游标以来已得到更新，则通过游标进行的定位更新或定位删除不成功。 当将行读入游标时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不锁定行。 相反，它使用 timestamp 列值的比较，或者如果表没有 timestamp 列则使用校验和值，以确定将行读入游标后是否已修改该行。 如果已修改该行，尝试进行的定位更新或定位删除将失败。 如果还指定了 `FAST_FORWARD`，则无法指定 `OPTIMISTIC`。  
  
 TYPE_WARNING  
 指定如果游标从所请求的类型隐式转换为另一种类型，则向客户端发送警告消息。  
  
 select_statement  
 定义游标结果集的标准 SELECT 语句。 在游标声明的 select_statement 中不允许使用关键字 `COMPUTE`、`COMPUTE BY`、`FOR BROWSE` 和 `INTO`。  
  
> [!NOTE]  
> 可以在游标声明中使用查询提示；但如果还使用 `FOR UPDATE OF` 子句，请在 `FOR UPDATE OF` 之后指定 `OPTION (<query_hint>)`。  
  
如果 select_statement 中的子句与所请求的游标类型的功能有冲突，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将游标隐式转换为其他类型。 有关详细信息，请参阅“隐式游标转换”。  
  
FOR UPDATE [OF column_name [,...n]]  
定义游标中可更新的列。 如果提供了 `OF <column_name> [, <... n>]`，则只允许修改所列出的列。 如果指定了 `UPDATE`，但未指定列的列表，则除非指定了 `READ_ONLY` 并发选项，否则可以更新所有的列。  
  
## <a name="remarks"></a>注解  
`DECLARE CURSOR` 定义了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 服务器游标的属性，例如游标的滚动行为和用于生成游标所操作的结果集的查询。 `OPEN` 语句填充结果集，`FETCH` 返回结果集中的行。 `CLOSE` 语句释放与游标关联的当前结果集。 `DEALLOCATE` 语句释放游标所使用的资源。  
  
`DECLARE CURSOR` 语句的第一种格式采用 ISO 语法来声明游标行为。 `DECLARE CURSOR` 的第二种格式使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 扩展插件，这些扩展插件允许使用在 ODBC 或 ADO 的数据库 API 游标函数中所使用的相同游标类型来定义游标。  
  
不能混淆这两种格式。 如果在 `CURSOR` 关键字前指定 `SCROLL` 或 `INSENSITIVE` 关键字，则不能在 `CURSOR` 和 `FOR <select_statement>` 关键字之间使用任何关键字。 如果在 `CURSOR` 和 `FOR <select_statement>` 关键字之间指定了任何关键字，则无法在 `CURSOR` 关键字前指定 `SCROLL` 或 `INSENSITIVE`。  
  
如果使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法的 `DECLARE CURSOR` 未指定 `READ_ONLY`、`OPTIMISTIC` 或 `SCROLL_LOCKS`，则默认值如下所示：  
  
-   如果 `SELECT` 语句不支持更新（由于权限不够、访问的远程表不支持更新等等），则游标为 `READ_ONLY`。  
  
-   `STATIC` 和 `FAST_FORWARD` 游标默认为 `READ_ONLY`。  
  
-   `DYNAMIC` 和 `KEYSET` 游标默认为 `OPTIMISTIC`。  
  
游标名称只能被其他 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句引用。 它们不能被数据库 API 函数引用。 例如，声明游标之后，不能通过 OLE DB、ODBC 或 ADO 函数或方法引用游标名称。 不能使用 API 的提取函数或方法来提取游标行；只能通过 [!INCLUDE[tsql](../../includes/tsql-md.md)] FETCH 语句提取这些行。  
  
在声明游标后，可使用下列系统存储过程确定游标的特性。  
  
|系统存储过程|说明|  
|------------------------------|-----------------|  
|**sp_cursor_list**|返回当前在连接上可视的游标列表及其特性。|  
|**sp_describe_cursor**|说明游标属性，例如是只前推的游标还是滚动游标。|  
|**sp_describe_cursor_columns**|说明游标结果集中的列的属性。|  
|**sp_describe_cursor_tables**|说明游标所访问的基表。|  
  
 在声明游标的 select_statement 中可以使用变量。 游标变量值在声明游标后不发生更改。  
  
## <a name="permissions"></a>权限  
 `DECLARE CURSOR` 的权限默认授予对游标中使用的视图、表和列具有 `SELECT` 权限的任何用户。
 
## <a name="limitations-and-restrictions"></a>限制和局限

不能在具有聚集列存储索引的表中使用游标或触发器。 此限制不适用于非聚集列存储索引；可以在具有非聚集列存储索引的表中使用游标和触发器。 
  
## <a name="examples"></a>示例  
  
### <a name="a-using-simple-cursor-and-syntax"></a>A. 使用简单的游标和语法  

在打开该游标时所生成的结果集包括表中的所有行和所有列。 可以更新该游标，并且所有的更新和删除都会在对该游标所做的提取操作中表现出来。 `FETCH NEXT` 是唯一可用的提取选项，因为未指定 `SCROLL` 选项。  
 
```sql  
DECLARE vend_cursor CURSOR  
    FOR SELECT * FROM Purchasing.Vendor  
OPEN vend_cursor  
FETCH NEXT FROM vend_cursor;  
```  
  
### <a name="b-using-nested-cursors-to-produce-report-output"></a>B. 使用嵌套游标生成报表输出  
 下例说明如何通过嵌套游标生成复杂的报表。 为每个供应商声明内部游标。  
  
```sql  
SET NOCOUNT ON;  
  
DECLARE @vendor_id INT, @vendor_name NVARCHAR(50),  
    @message VARCHAR(80), @product NVARCHAR(50);  
  
PRINT '-------- Vendor Products Report --------';  
  
DECLARE vendor_cursor CURSOR FOR   
SELECT VendorID, Name  
FROM Purchasing.Vendor  
WHERE PreferredVendorStatus = 1  
ORDER BY VendorID;  
  
OPEN vendor_cursor  
  
FETCH NEXT FROM vendor_cursor   
INTO @vendor_id, @vendor_name  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    PRINT ' '  
    SELECT @message = '----- Products From Vendor: ' +   
        @vendor_name  
  
    PRINT @message  
  
    -- Declare an inner cursor based     
    -- on vendor_id from the outer cursor.  
  
    DECLARE product_cursor CURSOR FOR   
    SELECT v.Name  
    FROM Purchasing.ProductVendor pv, Production.Product v  
    WHERE pv.ProductID = v.ProductID AND  
    pv.VendorID = @vendor_id  -- Variable value from the outer cursor  
  
    OPEN product_cursor  
    FETCH NEXT FROM product_cursor INTO @product  
  
    IF @@FETCH_STATUS <> 0   
        PRINT '         <<None>>'       
  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
  
        SELECT @message = '         ' + @product  
        PRINT @message  
        FETCH NEXT FROM product_cursor INTO @product  
        END  
  
    CLOSE product_cursor  
    DEALLOCATE product_cursor  
        -- Get the next vendor.  
    FETCH NEXT FROM vendor_cursor   
    INTO @vendor_id, @vendor_name  
END   
CLOSE vendor_cursor;  
DEALLOCATE vendor_cursor;  
```  
  
## <a name="see-also"></a>另请参阅  
 [@@FETCH_STATUS (Transact-SQL)](../../t-sql/functions/fetch-status-transact-sql.md)   
 [CLOSE (Transact-SQL)](../../t-sql/language-elements/close-transact-sql.md)   
 [游标 (Transact-SQL)](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DEALLOCATE (Transact-SQL)](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [FETCH (Transact-SQL)](../../t-sql/language-elements/fetch-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
