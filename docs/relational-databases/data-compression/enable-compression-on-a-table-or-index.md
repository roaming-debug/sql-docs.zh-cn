---
title: 对表或索引启用压缩功能
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中对表或索引启用压缩功能。
ms.custom: ''
ms.date: 01/22/2021
ms.prod: sql
ms.reviewer: ''
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.compwiz.compressiontype.f1
- sql13.swb.compwiz.outputoptions.f1
- sql13.swb.compwiz.summary.f1
- sql13.swb.compwiz.scriptfileoption.f1
- sql13.swb.compwiz.progress.f1
- sql13.swb.compwiz.welcome.f1
- sql13.swb.compwiz.createjob.f1
- sql13.swb.compwiz.selectaction.f1
helpviewer_keywords:
- data compression wizard
- compression [SQL Server], enable
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 59d15d0f056a757fea1545025832990254a7e4ae
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766353"
---
# <a name="enable-compression-on-a-table-or-index"></a>对表或索引启用压缩功能

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本文说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中对表或索引启用[数据压缩](../../relational-databases/data-compression/data-compression.md)功能。  
  
 **本文内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要对表或索引启用压缩功能，可使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   不能为系统表启用压缩功能。  
  
-   如果表是堆，ONLINE 模式的重新生成操作将在单个线程内完成。 请为多线程堆重新生成操作使用 OFFLINE 模式。 除非指定了 ONLINE 选项，否则重新生成操作处于 OFFLINE 状态。 有关执行 ONLINE 重新生成的完整信息，请参阅[联机执行索引操作](../indexes/perform-index-operations-online.md)。
  
-   如果表具有非对齐索引，则无法更改单个分区的压缩设置。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求对表或索引具有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-enable-compression-on-a-table-or-index"></a>对表或索引启用压缩功能  
  
1.  在对象资源管理器中，展开包含要压缩的表的数据库，然后展开 **“表”** 文件夹。  
  
2.  若要压缩索引，请展开包含要压缩的索引的表，然后展开 **“索引”** 文件夹。  
  
3.  右键单击要压缩的表或索引，指向“存储”并选择“管理压缩…” 。  
  
4.  在数据压缩向导中的“欢迎使用数据压缩向导”页上，选择“下一步” 。  
  
5.  在 **“选择压缩类型”** 页上，选择要应用于要压缩的表或索引中的每个分区的压缩类型。 完成后，选择“下一步”。  
  
     **“选择压缩类型”** 页上提供了以下选项：  
  
     “对所有分区使用相同压缩类型”复选框  
     选择此选项将为所有分区配置相同的压缩设置。 此操作将启用选择框并禁用网格中的 **“压缩类型”** 列。 选中此复选框后，相邻列表中的选项为 **“无”** 、 **“Row”** 和 **“Page”** 。  
  
     **分区号**  
     列出表或索引中的每个分区。 此列为只读。  
  
     **“压缩类型”**  
     为每个分区选择压缩选项。 在选中 **“对所有分区使用相同压缩类型”** 的情况下此选项不可用。 列表选项为 **“无”** 、 **“Row”** 和 **“Page”** 。  
  
     **边界**  
     显示分区的边界。 此列为只读。  
  
     **行计数**  
     显示此分区中的行数。 此列为只读。  
  
     **当前空间**  
     显示此分区当前所占的空间 (MB)。 此列为只读。  
  
     **请求的压缩空间**  
     选择“计算”之后，此列将显示在使用“压缩类型”列中指定的设置进行压缩后每个分区的估计大小 。 此列为只读。  
  
     **计算**  
     选择此项可估算每个分区在使用“压缩类型”列中指定的设置进行压缩之后的大小。  
  
6.  在 **“选择输出选项”** 页上，指定要如何完成压缩。 选择 **“创建脚本”** 可以基于向导中的前一页创建 SQL 脚本。 选择 **“立即运行”** 可以在完成向导中的其余页后创建新的已分区表。 选择 **“计划”** 可以在将来的预定时间创建新的已分区表。  
  
     如果选择 **“创建脚本”** ， **“脚本选项”** 下的以下选项将可用：  
  
     **将脚本保存到文件**  
     将脚本生成为 .sql 文件。 在“文件名”框中输入文件名和位置，或选择“浏览”以打开“脚本文件位置”对话框  。 从 **“另存为”** 选择 **“Unicode 文本”** 或 **“ANSI 文本”** 。  
  
     **将脚本保存到剪贴板**  
     将脚本保存到剪贴板。  
  
     **将脚本保存到“新建查询”窗口**  
     将脚本生成到新的查询编辑器窗口。 这是默认选项。  
  
     如果选择“计划”，则选择“更改计划” 。  
  
    1.  在“新建作业计划”对话框的“名称”框中，输入作业计划的名称 。  
  
    2.  在 **“计划类型”** 列表中选择计划类型：  
  
        -   **SQL Server 代理启动时自动启动**  
  
        -   **CPU 空闲时启动**  
  
        -   **重复执行**。 如果新的已分区表定期使用新信息更新，请选择此选项。  
  
        -   **执行一次**。 此选项是默认选择。  
  
    3.  选择或清除 **“已启用”** 复选框以启用或禁用计划。  
  
    4.  如果选择 **“重复执行”** ：  
  
        1.  在 **“频率”** 下的 **“执行”** 列表中，指定执行的频率：  
  
            -   如果选择 **“每天”** ，请在 **“执行间隔”** 框中输入重复作业计划的频率（天）。  
  
            -   如果选择 **“每周”** ，请在 **“执行间隔”** 框中输入重复作业计划的频率（周）。 选择运行作业计划的一周中的某天或某些天。  
  
            -   如果选择 **“每月”** ，可以选择 **“天”** 或 **“特定日期”** 。  
  
                -   如果选择 **“天”** ，请输入要运行作业计划的当月日期和作业计划的重复频率（月）。 例如，如果要每隔一个月在当月的 15 日运行计划作业，请选择“天”，在第一个框中输入“15”，在第二个框中输入“2”。 请注意，第二个框中允许的最大数是“99”。  
  
                -   如果选择 **“特定日期”** ，请选择要运行作业计划的当月内一周的特定一天和作业计划的重复频率（月）。 例如，如果要每隔一个月在当月的最后一个工作日运行作业计划，请选择“天”，从第一个列表中选择“最后一周”，从第二个列表中选择“工作日”，然后在最后一个框中输入“2”  。 还可以从前两个列表中选择“第一周”、“第二周”、“第三周”或“第四周”以及特定工作日（例如：星期日或星期三）。 请注意，最后一个框中允许的最大数是“99”。  
  
        2.  在 **“每天频率”** 下，指定作业计划运行的当天作业计划的重复频率。  
  
            -   如果选择 **“执行一次，时间为:”** ，请在 **“执行一次，时间为:”** 框中输入运行作业计划的当天的特定时间。 输入当天的小时、分钟和秒以及 AM 或 PM。  
  
            -   如果选择 **“执行间隔”** ，请在 **“频率”** 下指定所选日运行作业计划的频率。 例如，如果要在运行作业计划的当天每隔 2 小时重复一次，请选择“执行间隔”，在第一个框中输入“2”，然后从列表中选择“小时” 。 从此列表中还可以选择“分钟”和“秒”。 请注意，第一个框中允许的最大数是“100”。  
  
                 在 **“开始时间”** 框中，输入开始运行作业计划的时间。 在 **“结束时间”** 框中，输入停止重复作业计划的时间。 输入当天的小时、分钟和秒以及 AM 或 PM。  
  
        3.  在 **“持续时间”** 下的 **“开始日期”** 中，输入希望作业计划开始运行的日期。 选择 **“结束日期”** 或 **“无结束日期”** 以指示作业计划应在何时停止运行。 如果选择 **“结束日期”** ，输入希望作业计划停止运行的日期。  
  
    5.  如果选择“执行一次”，请在“执行一次”下的“日期”框中输入将运行作业计划的日期。 在 **“时间”** 框中，输入将运行作业计划的时间。 输入当天的小时、分钟和秒以及 AM 或 PM。  
  
    6.  在 **“摘要”** 下的 **“说明”** 中，验证所有作业计划设置均正确。  
  
    7.  选择“确定”。  
  
     完成此页后，选择“下一步”。  
  
7.  在 **“检查摘要”** 页的 **“检查所做选择”** 下，展开所有可用选项以确认所有压缩设置正确。 如果一切正常，请选择“完成”。  
  
8.  在 **“压缩向导进度”** 页上，监视有关创建分区向导的操作的状态信息。 根据在向导中选择的选项，“进度”页可能会包含一个操作或多个操作。 最上面的方框显示向导的总体状态和向导已接收到的状态、错误和警告消息数。  
  
     **“压缩向导进度”** 页上提供以下选项：  
  
     **详细信息**  
     提供向导执行的操作所返回的操作、状态和所有消息。  
  
     **Action**  
     指定每个操作的类型和名称。  
  
     **Status**  
     指示向导操作作为一个整体返回的值是“成功”还是“失败”。  
  
     **消息**  
     提供从该进程中返回的任何错误或警告消息。  
  
     **Report**  
     创建包含创建分区向导结果的报告。 这些选项是 **“查看报告”** 、 **“将报告保存到文件”** 、 **“将报告复制到剪贴板”** 和 **“将报告作为电子邮件发送”** 。  
  
     **查看报告**  
     打开“查看报告”对话框，其中包含关于创建分区向导进度的文本报告。  
  
     **将报告保存到文件**  
     打开“将报告另存为”对话框。  
  
     **将报告复制到剪贴板**  
     将向导的进度报告结果复制到剪贴板。  
  
     **“将报告作为电子邮件发送”**  
     将向导的进度报告结果复制到电子邮件。  
  
     完成后，选择“关闭”。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  

### <a name="sql-server"></a>SQL Server

在 SQL Server 中，运行 `sp_estimate_data_compression_savings`，然后对表或索引启用压缩。 请参阅以下部分。 

#### <a name="to-enable-compression-on-a-table"></a>对表启用压缩功能  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准栏上，选择“新建查询”  。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后选择“执行”。 此示例先执行存储过程 `sp_estimate_data_compression_savings` 以返回对象的估计大小（如果此对象使用的是 ROW 压缩设置）。 之后，此示例会对指定表中的所有分区启用 ROW 压缩。  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    EXEC sp_estimate_data_compression_savings 'Production', 'TransactionHistory', NULL, NULL, 'ROW' ;  
  
    ALTER TABLE Production.TransactionHistory REBUILD PARTITION = ALL  
    WITH (DATA_COMPRESSION = ROW);   
    GO  
    ```  
  
#### <a name="to-enable-compression-on-an-index"></a>对索引启用压缩功能  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准栏上，选择“新建查询”  。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后选择“执行”。 此示例先查询 `sys.indexes` 目录视图以返回 `index_id` 表上每个索引的名称和 `Production.TransactionHistory` 。 之后，此示例将执行存储过程 `sp_estimate_data_compression_savings` 以返回指定索引 ID 的估计大小（如果要使用 PAGE 压缩设置）。 最后，此示例将重新生成索引 ID 2 (`IX_TransactionHistory_ProductID`)，并指定 PAGE 压缩。  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    SELECT name, index_id  
    FROM sys.indexes  
    WHERE OBJECT_NAME (object_id) = N'TransactionHistory';  
  
    EXEC sp_estimate_data_compression_savings   
        @schema_name = 'Production',   
        @object_name = 'TransactionHistory',  
        @index_id = 2,   
        @partition_number = NULL,   
        @data_compression = 'PAGE' ;   
  
    ALTER INDEX IX_TransactionHistory_ProductID ON Production.TransactionHistory REBUILD PARTITION = ALL WITH (DATA_COMPRESSION = PAGE);  
    GO  
    ``` 
    
### <a name="on-azure-sql-database"></a>Azure SQL 数据库

Azure SQL 数据库不支持 `sp_estimate_data_compression_savings` 存储过程。 以下脚本启用压缩，且不必估计压缩量。 

#### <a name="to-enable-compression-on-a-table"></a>对表启用压缩功能  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准栏上，选择“新建查询”  。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后选择“执行”。 此示例会对指定表中的所有分区启用 ROW 压缩。  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  

    ALTER TABLE Production.TransactionHistory REBUILD PARTITION = ALL  
    WITH (DATA_COMPRESSION = ROW);   
    GO  
    ```  
  
#### <a name="to-enable-compression-on-an-index"></a>对索引启用压缩功能  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准栏上，选择“新建查询”  。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后选择“执行”。 此示例先查询 `sys.indexes` 目录视图以返回 `index_id` 表上每个索引的名称和 `Production.TransactionHistory` 。 最后，此示例将重新生成索引 ID 2 (`IX_TransactionHistory_ProductID`)，并指定 PAGE 压缩。  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    SELECT name, index_id  
    FROM sys.indexes  
    WHERE OBJECT_NAME (object_id) = N'TransactionHistory';  
    
    ALTER INDEX IX_TransactionHistory_ProductID ON Production.TransactionHistory REBUILD PARTITION = ALL WITH (DATA_COMPRESSION = PAGE);  
    GO  
    ``` 
  
 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL) ](../../t-sql/statements/alter-table-transact-sql.md) 和 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [数据压缩](../../relational-databases/data-compression/data-compression.md)   
 [sp_estimate_data_compression_savings (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md)  
  
  
