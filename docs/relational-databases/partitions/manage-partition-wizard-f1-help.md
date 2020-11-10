---
description: 管理分区向导
title: 管理分区向导
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
f1_keywords:
- sql13.swb.managepartition.createjob.f1
- sql13.swb.managepartition.progress.f1
- sql13.swb.managepartition.getstart.f1
- sql13.swb.managepartition.selectswitchtables.f1
- sql13.swb.managepartition.stagingtable.f1
- sql13.swb.managepartition.switchin.f1
- sql13.swb.managepartition.switchout.f1
- sql13.swb.managepartition.partitionaction.f1
- sql13.swb.managepartition.summary.f1
- sql13.swb.managepartition.selectoutput.f1
helpviewer_keywords:
- wizards [SQL Server Management Studio] See Manage Partition Wizard
ms.assetid: e2478d26-dea4-428d-98c5-aad2d2a30da8
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 756dd29442fe224f5e0518066cbbfdcbf2ba5ea6
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364669"
---
# <a name="manage-partition-wizard"></a>管理分区向导 
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用 **管理分区向导** 可以通过分区切换或实现可调窗口应用场景来管理和修改现有已分区表。 使用此向导，可以轻松管理分区，并可简化表中数据的定期迁入和迁出。  
  
### <a name="to-start-the-manage-partition-wizard"></a>启动管理分区向导  
  
-   在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，选择数据库，右键单击要在其中创建分区的表，指向“存储”，然后单击“管理分区”。  
  
     **Note** 如果 **“管理分区”** 不可用，则所选的表可能不包含分区。 在 **“存储”** 子菜单上单击 **“创建分区”** ，然后使用 **创建分区向导** 在表中创建分区。  
  
 有关分区和索引的一般信息，请参阅 [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。  
  
 本节提供使用 **“管理分区向导”** 管理、修改和实现分区时所需的信息。  
  
##  <a name="in-this-section"></a><a name="Top"></a> 本节内容  
 以下章节提供关于 **“管理分区向导”** 页的帮助。  
  
 [管理分区向导（“选择分区操作”页）](#SelectPartitionAction)  
  
 [管理分区向导（切入页）](#SwitchIn)  
  
 [管理分区向导（切出页）](#SwitchOut)  
  
 [管理分区向导（“选择临时表选项”页）](#StagingTableOptions)  
  
 [管理分区向导（“选择输出选项”页）](#OutputOption)  
  
 [管理分区向导（“新建作业计划”页）](#NewJob)  
  
 [管理分区向导（“摘要”页）](#Summary)  
  
 [管理分区向导（“进度”页）](#Progress)  
  
##  <a name="select-partition-action-page"></a><a name="SelectPartitionAction"></a> 选择分区操作页面  
 使用 **“选择分区操作”** 页可以选择要对分区执行的操作。  
  
### <a name="create-a-staging-table"></a>创建临时表  
 如果您有一个定期迁入迁出数据的已分区表，则分区切换是常见分区任务；例如，您有一个存储当前季度数据的已分区表，必须在每季度末移入新数据并归档旧数据。  
  
 该向导使用相同的分区列、表、列结构和索引来设计临时表，并将新表存储在源分区所在的文件组中。  
  
 若要创建临时表以切入或切出分区数据，请选择 **“为分区切换创建临时表”** 。  
  
### <a name="sliding-window-scenario"></a>滑动窗口应用场景  
 若要管理滑动窗口应用场景中的分区，请选择“管理滑动窗口应用场景中的分区数据”。  
  
### <a name="ui-element-list"></a>UI 元素列表  
 **“为分区切换创建临时表”**  
 为切入和切出现有已分区表的数据创建临时表。  
  
 **切出分区**  
 在从表中删除分区时提供选项。  
  
 **切入分区**  
 在向表中添加分区时提供选项。  
  
 **管理滑动窗口应用场景中的分区数据**  
 将空分区追加到可用于切入数据的现有表中。 向导当前支持切入上一个分区和切出第一个分区。  
  
 ![用于返回页首链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [本部分](#Top)  
  
##  <a name="select-partition-switching-in-options-page"></a><a name="SwitchIn"></a> 选择分区切入选项页  
 使用“选择分区切入选项”页可以选择要切入到已分区表中的临时表。  
  
### <a name="ui-element-list"></a>UI 元素列表  
 **显示所有分区**  
 选择此选项可以显示所有分区，包括当前位于已分区表的分区。  
  
 **分区网格**  
 显示所选分区的分区名称、“左边界”、“右边界”、“文件组”和“行计数”。  
  
 **切入表**  
 选择包含您要添加到您的已分区表的分区的临时表。 必须先创建此临时表，然后才能使用“管理分区向导”来切入分区。  
  
 ![用于返回页首链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [本部分](#Top)  
  
##  <a name="select-partition-switching-out-options-page"></a><a name="SwitchOut"></a> 选择分区切出选项页  
 使用“选择分区切出选项”页可以选择用于保存要从已分区表中切出的分区数据的分区和临时表。  
  
### <a name="ui-element-list"></a>UI 元素列表  
 **分区网格**  
 显示所选分区的分区名称、“左边界”、“右边界”、“文件组”和“行计数”。  
  
 **切出表**  
 选择要将您的数据切出到的新表或现有表。  
  
 **新建**  
 输入要用于从当前源表切出的分区的临时表的新名称。  
  
 **现有**  
 选择要用于从当前源表切出的分区的现有临时表。 如果现有表包含数据，这些数据将被您切出的数据覆盖。  
  
 ![用于返回页首链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [本部分](#Top)  
  
##  <a name="select-the-staging-table-options-page"></a><a name="StagingTableOptions"></a> 选择临时表选项页  
 使用 **“选择临时表选项”** 页可以创建用于切换已分区数据的临时表。  
  
 临时表必须与源表所在的选定分区位于同一文件组中。 临时表必须镜像源表与目标表的设计。  
  
 您可以在位于源分区的临时表中创建相同的索引。 临时表会自动基于源分区的元素包含约束。 此约束通常根据源分区的边界值生成。  
  
### <a name="ui-element-list"></a>UI 元素列表  
 **临时表名**  
 为临时表创建名称，或接受编辑框中显示的默认名称。  
  
 **切换分区**  
 选择您要从当前表中切换出的源分区。  
  
 **新建边界值**  
 为临时表中的分区选择或输入所需边界值。  
  
 **文件组**  
 为新表选择文件组。  
  
 ![用于返回页首链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [本部分](#Top)  
  
##  <a name="select-output-option-page"></a><a name="OutputOption"></a> 选择输出选项页  
 使用 **“选择输出选项”** 页，以指定您想要如何完成对分区的修改。  
  
### <a name="create-script"></a>创建脚本  
 当向导完成时，它会在查询编辑器中创建脚本以修改表中的分区。 如果要查看脚本，请选择 **“创建脚本”** ，然后手动执行脚本。  
  
 **将脚本保存到文件**  
 将脚本生成到 .sql 文件中。 指定 **Unicode** 或 **“ANSI 文本”** 。 若要指定文件的名称和位置，请单击 **“浏览”** 。  
  
 **将脚本保存到剪贴板**  
 将脚本保存到剪贴板。  
  
 **将脚本保存到“新建查询”窗口**  
 将脚本生成到查询编辑器窗口。 如果没有打开编辑器窗口，则将打开一个新的编辑器窗口作为脚本的目标。  
  
### <a name="run-immediately"></a>立即运行  
 **Run immediately**  
 当单击“下一步”或“完成”时，令向导完成对分区的修改。  
  
### <a name="schedule"></a>计划  
 选中此项可以在计划的日期和时间修改表分区。  
  
 **更改计划**  
 打开“新建作业计划”对话框，你可以在此对话框中选择、更改或查看计划作业的属性。  
  
 ![用于返回页首链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [本部分](#Top)  
  
##  <a name="new-job-schedule-page"></a><a name="NewJob"></a> 新建作业计划页  
 使用 **“新建作业计划”** 页可以查看和更改计划的属性。  
  
### <a name="options"></a>选项  
 为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业选择所需的计划类型。  
  
 **名称**  
 为计划键入一个新名称。  
  
 **计划中的作业**  
 查看使用此计划的现有作业。  
  
 **计划类型**  
 选择计划的类型。  
  
 **已启用**  
 启用或禁用计划。  
  
### <a name="recurring-schedule-types-options"></a>重复执行计划类型选项  
 选择已计划作业的频率。  
  
 **执行**  
 选择重复执行计划的间隔。  
  
 **执行间隔**  
 选择重复执行计划的间隔天数或星期数。 此选项对于每月计划不可用。  
  
 **星期一**  
 设置作业在特定的星期一发生。 只用于每周计划。  
  
 **星期二**  
 设置作业在特定的星期二发生。 只用于每周计划。  
  
 **星期三**  
 设置作业在特定的星期三发生。 只用于每周计划。  
  
 **星期四**  
 设置作业在特定的星期四发生。 只用于每周计划。  
  
 **星期五**  
 设置作业在特定的星期五发生。 只用于每周计划。  
  
 **星期六**  
 设置作业在特定的星期六发生。 只用于每周计划。  
  
 **星期日**  
 设置作业在特定的星期日发生。 只用于每周计划。  
  
 **Day**  
 选择每月中执行此计划的日期。 只用于每月计划。  
  
 **每**  
 选择两次计划间隔的月数。 只用于每月计划。  
  
 **“应用程序池:”**  
 为某月内特定一周的特定一天指定计划。 只用于每月计划。  
  
 **执行一次，时间为**  
 设置每天执行作业的时间。  
  
 **执行间隔**  
 设置作业两次发生之间的时间间隔（小时或分钟）。  
  
 **开始日期**  
 设置此计划开始生效的日期。  
  
 **结束日期**  
 设置计划的失效日期。  
  
 **无结束日期**  
 指定计划将无限期地保持有效。  
  
### <a name="one-time-schedule-types-options"></a>一次性计划类型选项  
 如果计划某个作业运行一次，您必须选择一个将来的日期和时间。  
  
 **Date**  
 选择作业的计划运行日期。  
  
 **时间**  
 选择作业的计划运行时间。  
  
 ![用于返回页首链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [本部分](#Top)  
  
##  <a name="summary-page"></a><a name="Summary"></a> 摘要页  
 使用 **“摘要”** 可检查您在前面的页中选择的选项。  
  
### <a name="ui-element-list"></a>UI 元素列表  
 **检查所做选择**  
 显示您在向导的每一页中所做的选择。 单击节点可展开和查看以前选择的选项。  
  
 ![用于返回页首链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [本部分](#Top)  
  
##  <a name="progress-page"></a><a name="Progress"></a> “进度”页  
 使用 **“进度”** 页可以监视有关 **“管理分区向导”** 操作的状态信息。 根据在向导中选择的选项， **“进度”** 页可能会包含一个操作或多个操作。 最上面的方框显示向导的总体状态和向导已接收到的状态、错误和警告消息数。  
  
### <a name="options"></a>选项  
 **详细信息**  
 提供向导执行的操作所返回的操作、状态和所有消息。  
  
 **Action**  
 指定每个操作的类型和名称。  
  
 **Status**  
 指示向导操作作为一个整体返回的值是“成功”还是“失败”。  
  
 **消息**  
 提供从该进程中返回的任何错误或警告消息。  
  
 **Stop**  
 停止向导的操作。  
  
 **Report**  
 创建一个包含“管理分区向导”的结果的报告。 选项包括：  
  
-   **查看报告**  
  
-   **将报告保存到文件**  
  
-   **将报告复制到剪贴板**  
  
-   **“将报告作为电子邮件发送”**  
  
 **查看报告**  
 打开“查看报告”对话框。 此对话框包含 **管理分区向导** 进度的文本报告。  
  
 **关闭**  
 关闭向导。  
  
 ![用于返回页首链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [本部分](#Top)  
  
## <a name="see-also"></a>另请参阅  
 [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md)  
  
