---
description: 'cdc。 &lt;&gt; (transact-sql 的 _CT capture_instance) '
title: cdc。 &lt;capture_instance &gt; _CT (transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- cdc
- cdc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.<capture_instance>_CT
ms.assetid: 979c8110-3c54-4e76-953c-777194bc9751
author: cawrites
ms.author: chadam
ms.openlocfilehash: db855911d000c9d39c738f39aec0fa8486912957
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183370"
---
# <a name="cdcltcapture_instancegt_ct-transact-sql"></a>cdc。 &lt;&gt; (transact-sql 的 _CT capture_instance) 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对源表启用变更数据捕获时创建的更改表。 该表为对源表执行的每个插入和删除操作返回一行，为对源表执行的每个更新操作返回两行。 如果在启用源表时未指定更改表的名称，则会使用一个派生的名称。 名称的格式为 cdc。*capture_instance* _CT 其中 *capture_instance* 是源表的架构名称和格式 *schema_table* 的源表名称。 例如，如果对 **AdventureWorks** 示例数据库中的表 **Person** 启用了变更数据捕获，则派生的更改表名称将 **cdc.Person_Address_CT**。  
  
 建议不要 **直接查询系统表**。 请改为执行 [cdc.fn_cdc_get_all_changes_<capture_instance>](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) 和 [cdc.fn_cdc_get_net_changes_ ](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)<capture_instance 函数。  
  

  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**__$start_lsn**|**binary(10)**|与相应更改的提交事务关联的日志序列号 (LSN)。<br /><br /> 在同一事务中提交的所有更改将共享同一个提交 LSN。 例如，如果对源表的 delete 操作删除两行，则更改表将包含两行，每行都具有相同的 **__ $ start_lsn** 值。|  
|**__ $ end_lsn**|**binary(10)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中，此列始终为 NULL。|  
|**__$seqval**|**binary(10)**|用于对事务内的行更改进行排序的序列值。|  
|**__$operation**|**int**|标识与相应更改关联的数据操作语言 (DML) 操作。 可以是以下值之一：<br /><br /> 1 = 删除<br /><br /> 2 = 插入<br /><br /> 3 = 更新（旧值）<br /><br /> 列数据中具有执行更新语句之前的行值。<br /><br /> 4 = 更新（新值）<br /><br /> 列数据中具有执行更新语句之后的行值。|  
|**__$update_mask**|**varbinary(128)**|基于更改表的列序号的位掩码，用于标识那些发生更改的列。|  
|*\<captured source table columns>*|多种多样|更改表中的其余列是在创建捕获实例时源表中标识为已捕获列的那些列。 如果已捕获列的列表中未指定任何列，则源表中的所有列将包括在此表中。|  
|**__ $ command_id** |**int** |跟踪事务中的操作顺序。 |  
  
## <a name="remarks"></a>备注  

`__$command_id`列是列在版本2012到2016的累积更新中引入。 有关版本和下载信息，请参阅知识库文章 3030352 [修复：为 Microsoft SQL Server 数据库启用变更数据捕获后，更改表的排序顺序不正确](https://support.microsoft.com/help/3030352/fix-the-change-table-is-ordered-incorrectly-for-updated-rows-after-you)。  有关详细信息，请参阅 [升级到最新 CU 后，CDC 功能可能会在升级为 SQL Server 2012、2014和2016后中断](/archive/blogs/sql_server_team/cdc-functionality-may-break-after-upgrading-to-the-latest-cu-for-sql-server-2012-2014-and-2016)。

## <a name="captured-column-data-types"></a>已捕获列的数据类型  
 此表中包含的已捕获列具有与其对应源列相同的数据类型和值，但下述情况例外：  
  
-   **时间戳** 列定义为 **二进制 (8)**。  
  
-   **标识** 列定义为 **int** 或 **bigint**。  
  
 不过，这些列中的值与源列的值相同。  
  
### <a name="large-object-data-types"></a>大型对象数据类型  
 当 __ $ operation = 1 或   \_ \_ $operation = 3 时，将始终为数据类型为 image、text 和 ntext 的列分配 NULL 值。 如果在更新过程中更改了列 ，则在 (= 3 时，将为数据类型 **varbinary (max)**、 **varchar (max)** 或 **nvarchar) max $operation** 分配 NULL 值 \_ \_ 。 当 \_ \_ $operation = 1 时，将在删除时为这些列分配其值。 捕获实例中包含的计算列的值始终为 **NULL**。  
  
 默认情况下，在一个 INSERT、UPDATE、WRITETEXT 或 UPDATETEXT 语句中可添加到已捕获列的最大大小为 65,536 字节或 64 KB。 若要增加此大小以支持较大的 LOB 数据，请使用 " [配置最大文本复制大小" 服务器配置选项](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md) 来指定更大的最大大小。 有关详细信息，请参阅 [配置 max text repl size 服务器配置选项](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md)。  
  
## <a name="data-definition-language-modifications"></a>数据定义语言修改  
 对源表所做的 DDL 修改（如添加或删除列）记录在 [cdc.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) 表中。 这些更改不会应用于更改表。 也就是说，更改表的定义保持不变。 当向更改表中插入行时，捕获进程将忽略那些未显示在与源表关联的已捕获列的列表中的列。 如果某列出现在已捕获列的列表中，但已不再位于源表中，则会为该列指定一个 null 值。  
  
 更改源表中列的数据类型也会记录在 " [cdc.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) " 表中。 但是，此更改不会改变更改表的定义。 当捕获进程遇到对源表所做的 DDL 更改的此日志记录时，更改表中的此已捕获列的数据类型会进行相应更改。  
  
 如果需要修改源表中已捕获列的数据类型以减小该数据类型的大小，请使用以下过程以确保可以成功修改更改表中的对应列。  
  
1.  在源表中，更新要修改的列中的值以适合所计划的数据类型大小。 例如，如果将数据类型从 **int** 更改为 **smallint**，请将值更新为适合于 **smallint** 范围的大小-32768 到32767。  
  
2.  在更改表中，对对应列执行相同的更新操作。  
  
3.  通过指定新数据类型来更改源表。 该数据类型更改成功传播到更改表。  

## <a name="data-manipulation-language-modifications"></a>数据操作语言修改  
 对启用了变更数据捕获的源表执行插入、更新和删除操作时，这些 DML 操作的记录将显示在数据库事务日志中。 变更数据捕获进程从事务日志中检索有关这些更改的信息，并向更改表中添加一行或两行来记录更改。 条目添加到更改表中的顺序与它们提交到源表的顺序是相同的，不过更改表条目的提交通常必须对一组更改执行，而不是对单个条目执行。  
  
 在 "更改表" 项中， **__ $ start_lsn** 列用于记录与对源表所做的更改关联的提交 lsn， **__ $ seqval 列** 用于对其事务中的更改进行排序。 这些元数据列可共同用于确保保留源更改的提交顺序。 因为捕获进程从事务日志获取其更改信息，所以必须注意更改表条目不会与其对应的源表更改同步显示。 在捕获进程处理了事务日志中的相关更改条目后，对应的更改将异步显示。  
  
 对于插入和删除操作，会设置更新掩码中的所有位。 对于更新操作，会修改更新（旧值）行和更新（新值）行中的更新掩码以指出在更新过程中有所更改的列。  
  
## <a name="see-also"></a>另请参阅  
 [sys.sp_cdc_enable_table &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [sys.sp_cdc_get_ddl_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md)  
  
