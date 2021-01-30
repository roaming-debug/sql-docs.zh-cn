---
description: '&lt; &gt; (transact-sql 的 capture_instance sys.fn_net_changes_) '
title: sys.fn_net_changes_ &lt; capture_instance &gt; (transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_net_changes_TSQL
- fn_net_changes_TSQL
- fn_net_changes
- sys.fn_net_changes
dev_langs:
- TSQL
helpviewer_keywords:
- fn_net_changes_<capture_instance>
- sys.fn_net_changes_<capture_instance>
ms.assetid: 342fa030-9fd9-4b74-ae4d-49f6038a5073
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fdaa7d3a2f741ba480443a997fe561c4aa5c3a50
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201908"
---
# <a name="sysfn_net_changes_ltcapture_instancegt-transact-sql"></a>&lt; &gt; (transact-sql 的 capture_instance sys.fn_net_changes_) 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **净更改** 查询函数的包装器。 创建这些函数所必需的脚本由 sys.sp_cdc_generate_wrapper_function 存储过程生成。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
fn_net_changes_<capture_instance> ('start_time', 'end_time', '<row_filter_option>' )  
  
<capture_instance> ::= The name of the capture instance.  
<row_filter_option> ::=  
{ all  
  | all with mask  
  | all with merge  
}  
```  
  
## <a name="arguments"></a>参数  
 *start_time*  
 **Datetime** 值，表示要包含在结果集中的更改表条目范围的低端点。  
  
 结果集中仅包含 cdc. <capture_instance>_CT 更改表中的关联提交时间严格大于 *start_time* 的行。  
  
 如果为此参数提供 NULL 值，则查询范围的低端点对应于捕获实例的有效范围的低端点。  
  
 *end_time*  
 **Datetime** 值，表示要包含在结果集中的更改表条目范围的高端点。  
  
 此参数可以采用以下两种含义之一：具体取决于在调用 sys.sp_cdc_generate_wrapper_function 时选择的值， @closed_high_end_point 以生成用于创建包装函数的脚本：  
  
-   @closed_high_end_point = 1  
  
     结果集中仅包含 cdc. <capture_instance>_CT 更改表中值为 \_ \_ $start _lsn 的值更改表中的行。  
  
-   @closed_high_end_point = 0  
  
     在结果集中只包含 cdc. <capture_instance>_CT 更改表中值的值为 \_ \_ $start _lsn 的更改表中的值。  
  
 如果为此参数提供 NULL 值，则查询范围的高端点对应于捕获实例的有效范围的高端点。  
  
 *<row_filter_option>* ：： = {all | all with mask | all with merge}  
 控制元数据列的内容和结果集中所返回的行的选项。 可以是下列选项之一：  
  
 all  
 返回更改了内容列的某一行的最终内容，以及在元数据列 __CDC_OPERATION 中应用该行所需的操作。  
  
 all with mask  
 返回所有更改了内容列的行的最终内容，以及在元数据列 __CDC_OPERATION 中应用每一行所需的操作。 如果在生成用于创建包装函数的脚本时指定了更新标志列表，则需要使用此选项来填充更新掩码。  
  
 all with merge  
 返回内容列中所有已更改的行的最终内容。  
  
 列 __CDC_OPERATION 将是以下两个值之一：  
  
-   D，如果必须删除该行。  
  
-   M，如果必须插入或更新该行。  
  
 确定是否需要插入或更新以便对目标应用更改的逻辑会增加查询的复杂性。 如果不需要区分插入和更新操作，请使用此选项以提高性能。 此方式在合并操作直接可用的目标环境中效果最好，例如 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 环境。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|列类型|说明|  
|-----------------|-----------------|-----------------|  
|\<columns from @column_list>|**随着**|在调用以生成用于创建包装的脚本时，将在 sp_cdc_generate_wrapper_function 的 **column_list** 参数中标识的列。 如果 *column_list* 为空，则所有跟踪的源列将出现在结果集中。|  
|__CDC_OPERATION|**nvarchar (2)**|一个操作代码，它指示将行应用于目标环境所需的操作。 操作将因以下调用中提供的参数 *row_filter_option* 的值而异：<br /><br /> *row_filter_option* = "all"、"all with mask"<br /><br /> 'D' - 删除操作<br /><br /> 'I' - 插入操作<br /><br /> 'UN' - 更新操作<br /><br /> *row_filter_option* = "all with merge"<br /><br /> 'D' - 删除操作<br /><br /> 'M' - 插入操作或更新操作|  
|\<columns from @update_flag_list>|**bit**|通过将 _uflag 追加到列名称末尾所命名的位标记。 仅当 *row_filter_option* **= "all with mask"** 且 \_ _CDC_OPERATION **= ' UN '** 时，此标志才采用非 null 值。 如果在查询窗口中修改相应的列，则会将其设置为 1； 否则为 0。|  
  
## <a name="remarks"></a>备注  
 Fn_net_changes_<capture_instance> 函数作为 cdc.fn_cdc_get_net_changes_<capture_instance 查询函数的包装。 sys.sp_cdc_generate_wrapper 存储过程用于创建包装的脚本。  
  
 不会自动创建包装函数。 必须做两件事，才能创建包装函数：  
  
1.  运行该存储过程以生成用于创建包装的脚本。  
  
2.  执行该脚本以实际创建包装函数。  
  
 包装函数使用户能够系统地查询在按 **日期时间** 值（而不是按 LSN 值）界定的间隔内发生的更改。 包装函数执行所提供的 **日期时间** 值与作为查询函数的参数内部所需的 LSN 值之间的所有必需转换。 当包装函数以串行方式用于处理更改数据流时，它们可确保不会丢失或重复数据，前提是遵循以下约定： @end_time 与一个调用关联的间隔值作为 @start_time 与随后的调用关联的间隔值提供。  
  
 通过在创建脚本时使用 @closed_high_end_point 参数，您可以生成包装以支持指定查询窗口中的闭合上限或开放上限。 就是说，您可以决定其提交时间等于提取间隔的上限的条目是否要包括在间隔中。 默认情况下，包括上限。  
  
 **净更改** 包装函数返回的结果集仅返回生成包装时位于中的那些被跟踪列 @column_list 。 如果 @column_list 为 NULL，则返回所有被跟踪的源列。 源列后面是一个操作列 __CDC_OPERATION，该列包含一个或两个用于标识操作的字符。  
  
 然后，将位标志追加到在参数中标识的每个列的结果集 @update_flag_list 。 对于 **净更改** 包装程序，如果 @row_filter_option 在包装函数的调用中使用的是 "all" 或 "all with merge"，则位标志将始终为 NULL。 如果 @row_filter_option 设置为 "all with mask" 并且 __CDC_OPERATION 为 "d" 或 "I"，则标志的值也将为 NULL。 如果 \_ _CDC_OPERATION 为 "UN"，则该标志将设置为1或0，具体取决于 **net** update 操作是否导致对列的更改。  
  
 变更数据捕获配置模板 "实例化架构的 CDC 包装 Tvf" 演示了如何使用 sp_cdc_generate_wrapper_function 存储过程获取架构的已定义查询函数的所有包装函数的 CREATE 脚本。 然后，此模板创建这些脚本。 有关模板的详细信息，请参阅 [模板资源管理器](../../ssms/template/template-explorer.md)。  
  
## <a name="see-also"></a>另请参阅  
 [sys.sp_cdc_generate_wrapper_function &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)  
  
  
