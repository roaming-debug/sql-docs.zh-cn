---
description: Stretch Database 扩展事件
title: Stretch Database 扩展事件
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 70485e74-2e25-4e7e-be6c-9dd1780a42e3
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: f965b68279398720ff7b1b9bcb76b5ec73410466
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080016"
---
# <a name="extended-events-for-stretch-database"></a>Stretch Database 扩展事件
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


Stretch Database 提供了一系列扩展事件以进行故障排除。  
  
有关详细信息，请参阅 [扩展事件](../../relational-databases/extended-events/extended-events.md)。 有关如何启动扩展事件会话进行故障排除的详细信息，请参阅 [创建扩展事件会话](/previous-versions/sql/sql-server-2016/hh213147(v=sql.130))  
  
## <a name="list-of-extended-events-for-stretch-database"></a>Stretch Database 扩展事件列表  
  
事件名称|事件说明   
---------|---------  
remote_data_archive_db_ddl|当处理用于延伸数据的数据库 T-SQL ddl 时发生。  
remote_data_archive_provision_operation|当设置操作开始或结束时发生。  
remote_data_archive_query_rewrite|当 RelOp_Get 在查询重写进行延伸期间被替换时发生。  
remote_data_archive_table_ddl|当处理用于延伸数据的表 T-SQL ddl 时发生。  
remote_data_archive_telemetry|每当本地系统将遥测事件传输给 Azure DB 时发生。  
remote_data_archive_telemetry_rejected|每当 AzureDB Stretch 遥测事件被拒绝时发生  
repopulate_stretch_schema_task_queue_complete|报告重新填充延伸架构任务队列完成。  
repopulate_stretch_schema_task_queue_start|报告重新填充延伸架构任务队列启动。  
stretch_codegen_errorlog|报告从代码生成器的输出  
stretch_codegen_start|报告延伸代码生成开始  
stretch_create_remote_table_start|报告远程表创建开始  
stretch_database_disable_completed|报告 ALTER DATABASE SET REMOTE_DATA_ARCHIVE OFF 命令完成  
stretch_database_enable_completed|报告 ALTER DATABASE SET REMOTE_DATA_ARCHIVE ON 命令完成  
stretch_database_reauthorize_completed|报告 sp_rda_reauthorize_db spec proc 完成  
stretch_index_reconciliation_codegen_completed|报告延伸远程索引操作的代码生成完成  
stretch_index_update_step_completed|报告已延伸索引更新操作的持续时间  
stretch_migration_debug_trace|延伸迁移操作的调试跟踪。  
stretch_migration_dequeue_migration|在为数据库取消排队延伸迁移任务时引发事件。  
stretch_migration_queue_migration|将数据包排队，以便开始迁移数据库和对象。  
stretch_migration_requeue_migration|在重新排队延伸迁移任务包时引发事件。  
stretch_migration_start_migration|开始迁移数据库和对象。  
stretch_migration_start_unmigration|开始取消数据库和对象迁移。  
stretch_remote_column_execution_completed|报告延伸列生成代码远程执行的完成  
stretch_remote_column_reconciliation_codegen_completed|报告已完成延伸远程列对帐的代码生成  
stretch_remote_index_execution_completed|报告延伸索引生成代码远程执行的完成  
stretch_schema_queue_task|报告何时对数据包进行排队以处理数据库和对象的架构任务。  
stretch_schema_script_execution_completed|报告延伸架构任务处理期间延伸脚本执行完成。  
stretch_schema_script_execution_skipped|报告延伸架构任务处理期间跳过延伸脚本执行。  
stretch_schema_script_execution_start|报告延伸架构任务处理期间开始执行延伸脚本。  
stretch_schema_task_failed|报告延伸架构任务执行期间延伸架构函数失败。  
stretch_schema_task_skipped|报告在延伸架构函数期间跳过了延伸架构任务。  
stretch_schema_task_start|报告在延伸架构任务期间延伸架构函数的开始信息。  
stretch_schema_task_succeeded|报告延伸架构任务执行期间延伸架构函数成功完成。  
stretch_sp_migration_get_batch_id|调用 sp_stretch_get_batch_id  
stretch_sync_metadata_start|报告在迁移任务期间元数据检查的开始。  
stretch_table_codegen_completed|报告已延伸表的代码生成完成  
stretch_table_complete_data_reconciliation|完成数据库和对象的数据对帐。  
stretch_table_data_reconciliation_event|报告已完成一系列行的数据对帐  
stretch_table_data_reconciliation_results_event|报告错误或已成功完成好几批行的数据对帐  
stretch_table_hinted_admin_delete_event|报告使用管理提示的延伸删除 DML 操作的执行情况  
stretch_table_hinted_admin_update_event|报告使用管理提示的延伸更新 DML 操作的执行情况  
stretch_table_provisioning_step_completed|报告已延伸表设置操作的持续时间  
stretch_table_query_error|报告在重写延伸查询时引发的错误  
stretch_table_remote_creation_completed|报告为已延伸表远程执行已生成代码完成  
stretch_table_row_migration_event|报告一批行迁移完成  
stretch_table_row_migration_results_event|报告错误或成功完成若干批行的迁移  
stretch_table_row_unmigration_event|报告一批行取消迁移完成  
stretch_table_row_unmigration_results_event|报告错误或成功完成若干批行的取消迁移  
stretch_table_start_data_reconciliation|启动数据库和对象的数据对帐。  
stretch_table_unprovision_completed|报告为未延伸表删除本地资源完成  
stretch_table_validation_error|报告用户启用延伸时表验证完成  
stretch_unprovision_table_start|报告延伸表取消设置开始  
  
## <a name="see-also"></a>另请参阅  
[延伸数据库的管理和故障排除](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)