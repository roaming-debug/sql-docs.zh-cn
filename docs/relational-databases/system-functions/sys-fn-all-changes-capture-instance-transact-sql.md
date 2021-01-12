---
description: '&lt; &gt; (transact-sql 的 capture_instance sys.fn_all_changes_) '
title: sys.fn_all_changes_ &lt; capture_instance &gt; (transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_all_changes
- sys.fn_all_changes
- fn_all_changes_TSQL
- sys.fn_all_changes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_all_changes_<capture_instance>
- sys.fn_all_changes_<capture_instance>
ms.assetid: 564fae96-b88c-4f22-9338-26ec168ba6f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c1b411daa6ccdfdb34da1a9560520416e26e1191
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096467"
---
# <a name="sysfn_all_changes_ltcapture_instancegt-transact-sql"></a>&lt; &gt; (transact-sql 的 capture_instance sys.fn_all_changes_) 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **所有更改** 查询函数的包装。 创建这些函数所必需的脚本由 sys.sp_cdc_generate_wrapper_function 存储过程生成。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
fn_all_changes_<capture_instance> ('start_time' ,'end_time', '<row_filter_option>' )  
  
<capture_instance> ::= The name of the capture instance.  
<row_filter_option> ::=  
{ all  
  | all update old  
}  
```  
  
## <a name="arguments"></a>参数  
 *start_time*  
 **Datetime** 值，表示要包含在结果集中的更改表条目范围的低端点。  
  
 结果集中仅包含 cdc. <capture_instance>_CT 更改表中具有大于 *start_time* 的关联提交时间的行。  
  
 如果为此参数提供 NULL 值，则查询范围的低端点对应于捕获实例的有效范围的低端点。  
  
 *end_time*  
 **Datetime** 值，表示要包含在结果集中的更改表条目范围的高端点。  
  
 此参数可以采用以下两种可能的含义之一：具体取决于在 @closed_high_end_point 调用 sys.sp_cdc_generate_wrapper_function 时为包装函数生成 create 脚本时选择的值：  
  
-   @closed_high_end_point = 1  
  
     结果集中仅包含 cdc. <capture_instance>_CT 更改表中的关联提交时间小于或等于 end_time 的行。  
  
-   @closed_high_end_point = 0  
  
     只有 cdc.capture_instance_CT 更改表中具有严格小于 end_time 的关联提交时间的行才会包括在结果集中。  
  
 如果为此参数提供 NULL 值，则查询范围的高端点对应于捕获实例的有效范围的高端点。  
  
 <row_filter_option>：： = {all | 所有更新旧版本}  
 控制结果集中返回的元数据列和行的内容的选项。  
  
 可以是下列选项之一：  
  
 all  
 返回指定 LSN 范围内的所有更改。 对于由于更新操作而导致的更改，此选项仅返回在应用更新后包含新值的行。  
  
 all update old  
 返回指定 LSN 范围内的所有更改。 对于由于更新操作而导致的更改，此选项返回两行，它们分别包含更新前和更新后的列值。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|列类型|说明|  
|-----------------|-----------------|-----------------|  
|__CDC_STARTLSN|**binary(10)**|与更改关联的事务的提交 LSN。 在同一事务中提交的所有更改将共享同一个提交 LSN。|  
|__CDC_SEQVAL|**binary(10)**|用于对事务中的行更改进行排序的序列值。|  
|\<columns from @column_list>|**随着**|在调用以生成用于创建包装函数的脚本时 sp_cdc_generate_wrapper_function 在 *column_list* 参数中标识的列。|  
|__CDC_OPERATION|**nvarchar (2)**|操作代码，用于指示将行应用到目标环境时所必需的操作。 它将根据调用中提供的参数 *row_filter_option* 值而有所不同：<br /><br /> *row_filter_option* = "all"<br /><br /> 'D' - 删除操作<br /><br /> 'I' - 插入操作<br /><br /> 'UN' - 更新操作的新值<br /><br /> *row_filter_option* = "all update old"<br /><br /> 'D' - 删除操作<br /><br /> 'I' - 插入操作<br /><br /> 'UN' - 更新操作的新值<br /><br /> 'UO' - 更新操作的旧值|  
|\<columns from @update_flag_list>|**bit**|通过将 _uflag 追加到列名称的末尾所命名的位标记。 当 \_ "UO" 的值为 "" _CDC_OPERATION 时，标记始终设置为 NULL。 当 \_ _CDC_OPERATION 为 "UN" 时，如果更新生成对相应列的更改，则将其设置为1。 否则为 0。|  
  
## <a name="remarks"></a>备注  
 Fn_all_changes_<capture_instance> 函数作为 cdc.fn_cdc_get_all_changes_<capture_instance 查询函数的包装。 sys.sp_cdc_generate_wrapper 存储过程用于生成用于创建包装的脚本。  
  
 不会自动创建包装函数。 必须做两件事，才能创建包装函数：  
  
1.  运行该存储过程以生成用于创建包装的脚本。  
  
2.  执行该脚本以实际创建包装函数。  

 包装函数使用户能够系统地查询在按 **日期时间** 值（而不是按 LSN 值）界定的间隔内发生的更改。 包装函数执行所提供的 **日期时间** 值与作为查询函数的参数内部所需的 LSN 值之间的所有必需转换。 当包装函数以串行方式用于处理更改数据流时，它们可确保不会丢失或重复数据，前提是遵循以下约定： @end_time 与一个调用关联的间隔值作为 @start_time 与随后的调用关联的间隔值提供。  
  
 通过在创建脚本时使用 @closed_high_end_point 参数，您可以生成包装以支持指定查询窗口中的闭合上限或开放上限。 就是说，您可以决定其提交时间等于提取间隔的上限的条目是否要包括在间隔中。 默认情况下，包括上限。  
  
 **所有更改** 包装函数返回的结果集将更改表的 __ $ start_lsn 和 \_ \_ $seqval 列 \_ 分别作为 _CDC_STARTLSN 和 _CDC_SEQVAL 的列返回 \_ 。 但在生成包装时，只会在 *\@ column_list* 参数中显示的那些跟踪列之后执行这些操作。 如果 *\@ COLUMN_LIST* 为 NULL，则返回所有跟踪的源列。 源列后跟操作列， \_ _CDC_OPERATION 是标识操作的一个或两个字符的列。  
  
 然后，将位标志追加到在 @update_flag_list 参数中标识的每个列的结果集的末尾。 对于 " **所有更改** " 包装，如果 __CDC_OPERATION 是 ""、"I" 或 "UO"，则位标志将始终为 NULL。 如果 \_ _CDC_OPERATION 为 "UN"，则该标志将设置为1或0，具体取决于更新操作是否导致对列的更改。  
  
 变更数据捕获配置模板 "实例化架构的 CDC 包装 Tvf" 演示了如何使用 sp_cdc_generate_wrapper_function 存储过程获取架构的已定义查询函数的所有包装函数的 CREATE 脚本。 然后，此模板创建这些脚本。 有关模板的详细信息，请参阅 [模板资源管理器](../../ssms/template/template-explorer.md)。  
  
## <a name="see-also"></a>另请参阅  
 [sys.sp_cdc_generate_wrapper_function &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  
