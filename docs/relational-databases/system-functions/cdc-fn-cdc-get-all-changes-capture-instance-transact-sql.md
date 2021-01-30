---
description: '&lt; &gt; (transact-sql 的 capture_instance cdc.fn_cdc_get_all_changes_) '
title: cdc.fn_cdc_get_all_changes_ &lt; capture_instance &gt;  (transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_all_changes_<capture_instance>
- change data capture [SQL Server], querying metadata
- cdc.fn_cdc_get_all_changes_<capture_instance>
ms.assetid: c6bad147-1449-4e20-a42e-b51aed76963c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5250335e5266212c49acc2b789479129786dccd0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196219"
---
# <a name="cdcfn_cdc_get_all_changes_ltcapture_instancegt--transact-sql"></a>&lt; &gt; (transact-sql 的 capture_instance cdc.fn_cdc_get_all_changes_) 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为在指定日志序列号 (LSN) 范围内应用到源表的每项更改返回一行。 如果源行在该间隔内有多项更改，则每项更改都会表示在返回的结果集中。 除了返回更改数据外，四个元数据列还提供了将更改应用到另一个数据源所需的信息。 行筛选选项可控制元数据列的内容以及结果集中返回的行。 当指定“all”行筛选选项时，针对每项更改将只有一行来标识该更改。 当指定“all update old”选项时，更新操作会表示为两行：一行包含更新之前已捕获列的值，另一行包含更新之后已捕获列的值。  
  
 此枚举函数是在对源表启用变更数据捕获时创建的。 函数名称是派生的，并使用 **cdc.fn_cdc_get_all_changes_**_capture_instance_ 格式，其中 *capture_instance* 是在源表启用变更数据捕获时为捕获实例指定的值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
cdc.fn_cdc_get_all_changes_capture_instance ( from_lsn , to_lsn , '<row_filter_option>' )  
  
<row_filter_option> ::=  
{ all  
 | all update old  
}  
```  
  
## <a name="arguments"></a>参数  
 *from_lsn*  
 LSN 值，它表示要包含在结果集中的 LSN 范围的低端点。 *from_lsn* 是 **(10) 的二进制**。  
  
 结果集中仅包含 [cdc. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) 更改表中值为 **__ $ start_lsn** 大于或等于 *from_lsn* 的行。  
  
 *to_lsn*  
 LSN 值，它表示要包含在结果集中的 LSN 范围的高端点。 *to_lsn* 是 **(10) 的二进制**。  
  
 结果集中仅包含 [cdc. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) 更改表中值为 **__ $ start_lsn** 大于或等于 *from_lsn* 且小于 *等于 to_lsn 的* 行。  
  
 <row_filter_option>：： = {all | 所有更新旧版本}  
 控制元数据列的内容和结果集中所返回的行的选项。  
  
 可以是下列选项之一：  
  
 all  
 返回指定 LSN 范围内的所有更改。 对于由更新操作导致的更改，此选项只返回在应用更新之后包含新值的行。  
  
 all update old  
 返回指定 LSN 范围内的所有更改。 对于由更新操作导致的更改，此选项将返回在更新之前包含列值的行和更新之后包含列值的行。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**__$start_lsn**|**binary(10)**|与更改关联的提交 LSN，用于保留更改的提交顺序。 在同一事务中提交的更改将共享同一个提交 LSN 值。|  
|**__$seqval**|**binary(10)**|用于对某事务内的行更改进行排序的序列值。|  
|**__$operation**|**int**|标识将更改数据行应用到目标数据源所需的数据操作语言 (DML) 操作。 可以是以下值之一：<br /><br /> 1 = 删除<br /><br /> 2 = 插入<br /><br /> 3 = 更新（捕获的列值是执行更新操作前的值）。 仅当指定了行筛选选项“all update old”时才应用此值。<br /><br /> 4 = 更新（捕获的列值是执行更新操作后的值）。|  
|**__$update_mask**|**varbinary(128)**|位掩码，为捕获实例标识的每个已捕获列均对应于一个位。 当 **__ $ operation** = 1 或2时，该值将所有已定义的位设置为1。 当 **__ $ operation** = 3 或4时，只有与更改的列相对应的位设置为1。|  
|**\<captured source table columns>**|多种多样|函数返回的其余列是在创建捕获实例时标识的已捕获列。 如果已捕获列的列表中未指定任何列，则将返回源表中的所有列。|  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份或 **db_owner** 固定数据库角色。 对于所有其他用户，要求对源表中的所有已捕获列具有 SELECT 权限；如果已定义捕获实例的访问控制角色，则还要求具有该数据库角色的成员身份。 如果调用方没有查看源数据的权限，则该函数将返回错误229， ( "拒绝对对象 ' fn_cdc_get_all_changes_ ... '、数据库 ' \<DatabaseName> '、架构为 cdc ' 的 SELECT 权限。) 。  
  
## <a name="remarks"></a>备注  
 如果指定的 LSN 范围不在捕获实例的更改跟踪时间线范围之内，则函数将返回错误 208（“为过程或函数 cdc.fn_cdc_get_all_changes 提供的参数数目不足。”）。  
  
 当 **__ $ operation** = 1 或 **__ $ operation** = 3 时，将始终为数据类型为 **image**、 **text** 和 **ntext** 的列分配 NULL 值。 如果在更新过程中更改了列，则在 **__ $ operation** = 3 时，将为数据类型 **varbinary (max)**、 **varchar (max)** 或 **nvarchar (max)** 分配 NULL 值。 当 **__ $ operation** = 1 时，将在删除时为这些列分配其值。 捕获实例中包含的计算列的值始终为 NULL。  
  
## <a name="examples"></a>示例  
 有几个 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 模板可用于演示如何使用变更数据捕获查询函数。 这些模板在的 " **视图** " 菜单中可用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 。 有关详细信息，请参阅 [模板资源管理器](../../ssms/template/template-explorer.md)。  
  
 本示例显示`Enumerate All Changes for Valid Range Template`。 它使用函数 `cdc.fn_cdc_get_all_changes_HR_Department` 报告捕获实例的所有当前可用更改，该实例 `HR_Department` 是针对数据库中的源表 HumanResources 定义的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 。  
  
```  
-- ========  
-- Enumerate All Changes for Valid Range Template  
-- ========  
USE AdventureWorks2012;  
GO  
  
DECLARE @from_lsn binary(10), @to_lsn binary(10);  
SET @from_lsn = sys.fn_cdc_get_min_lsn('HR_Department');  
SET @to_lsn   = sys.fn_cdc_get_max_lsn();  
SELECT * FROM cdc.fn_cdc_get_all_changes_HR_Department  
  (@from_lsn, @to_lsn, N'all');  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#62; &#40;Transact-sql&#41;cdc.fn_cdc_get_net_changes_&#60;capture_instance ](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_map_time_to_lsn &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md)   
 [sys.sp_cdc_get_ddl_history &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md)   
 [sys.sp_cdc_get_captured_columns &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-captured-columns-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [关于变更数据捕获 (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
