---
title: 来自 DTA 的输出
description: 了解如何使用数据库引擎优化顾问在 SQL Server 安装中创建的摘要、建议、报表和优化日志。
ms.custom: seo-dt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.dta.sessionmonitor.f1
- sql13.dta.reports.f1
- sql13.dta.recommendations.f1
- sql13.dta.applyrecommendations.f1
helpviewer_keywords:
- viewing logs
- recommendations [Database Engine Tuning Advisor]
- summary tuning information [SQL Server]
- Database Engine Tuning Advisor [SQL Server], recommendations
- Database Engine Tuning Advisor [SQL Server], logs
- Database Engine Tuning Advisor [SQL Server], viewing output
- Database Engine Tuning Advisor [SQL Server], reports
- logs [SQL Server], tuning
- reports [SQL Server], tuning
- viewing tuning output
ms.assetid: 47f9d9a7-80b0-416d-9d9a-9e265bc190dc
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: e81bbff1dd2eedace6ac7a5e535db9e9689157e8
ms.sourcegitcommit: b09f069c6bef0655b47e9953a4385f1b52bada2b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92734669"
---
# <a name="view-and-work-with-the-output-from-the-database-engine-tuning-advisor-dta"></a>查看和使用来自数据库引擎优化顾问 (DTA) 的输出

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  数据库引擎优化顾问在优化数据库时，会创建摘要、建议、报告和优化日志。 可以使用优化日志输出来对数据库引擎优化顾问优化会话进行故障排除。 可以使用摘要、建议和报告来确定是要实施优化建议，还是继续优化直到查询性能可提高到安装 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所需的程度为止。 有关如何使用数据库优化引擎来创建工作负荷和优化数据库的信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。  
  
##  <a name="view-tuning-output"></a><a name="View"></a> 查看优化输出  
 下列过程描述了如何使用数据库引擎优化顾问 GUI 来查看优化建议、摘要、报告和优化日志。 有关用户界面选项的信息，请参阅本主题后面的 [用户界面说明](#UI) 。  
  
 还可以使用 GUI 查看由 **dta** 命令行实用工具生成的优化输出。  
  
> [!NOTE]  
>  如果使用 **dta** 命令行实用工具并使用 **-ox** 参数来指定将输出写入 XML 文件，则可以通过单击 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 的“文件”  菜单上的“打开文件”  ，打开和查看 XML 输出文件。 有关详细信息，请参阅 [Use SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md)。 有关 **dta** 命令行实用工具的详细信息，请参阅 [dta 实用工具](../../tools/dta/dta-utility.md)。  
  
#### <a name="to-view-tuning-recommendations-with-the-database-engine-tuning-advisor-gui"></a>使用数据库引擎优化顾问 GUI 查看优化建议  
  
1.  使用数据库引擎优化顾问 GUI 或 **dta** 命令行实用工具优化数据库。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。 如果希望使用现有优化会话，请跳过此步骤，转到步骤 2。  
  
2.  启动数据库引擎优化顾问 GUI。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。 若要查看现有优化会话的优化建议，请双击“会话监视器”  窗口中的会话名称将会话打开。  
  
     在新的优化会话完成之后或工具加载了现有会话之后，将显示 **“建议”** 页。  
  
3.  在 **“建议”** 页上，单击 **“分区建议”** 和 **“索引建议”** 以查看显示优化会话结果的窗格。 如果在设置此会话的优化选项时未指定分区，则 **“分区建议”** 窗格为空。  
  
4.  在 **“分区建议”** 或 **“索引建议”** 窗格中，使用滚动条来查看网格中显示的所有信息。  
  
5.  取消选中 **“建议”** 选项卡式页面底部的 **“显示现有对象”** 。 这将导致网格仅显示建议中引用的那些数据库对象。 使用底部滚动条来查看建议网格中最右侧的列，再单击“定义”  列中的某一项来查看或复制在数据库中创建该对象的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。  
  
6.  若要将此建议中创建或删除所有数据库对象的所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本另存为一个脚本文件，请单击 **“操作”** 菜单上的 **“保存建议”** 。  

#### <a name="to-view-the-tuning-summary-and-reports-with-the-database-engine-tuning-advisor-gui"></a>使用数据库引擎优化顾问 GUI 查看优化摘要和报告  
  
1.  使用数据库引擎优化顾问 GUI 或 **dta** 命令行实用工具优化数据库。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。 如果希望使用现有优化会话，请跳过此步骤，转到步骤 2。  
  
2.  启动数据库引擎优化顾问 GUI。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。 若要查看现有优化会话的优化摘要和报告，可以双击“会话监视器”  窗口中的会话名称将会话打开。  
  
3.  在新的优化会话完成之后或者工具加载了现有会话之后，单击 **“报告”** 选项卡。  
  
4.  **“优化摘要”** 窗格包含有关优化会话的信息。 由 **“预期的提高百分比”** 和 **“建议使用的空间”** 项提供的信息对决定是否要实施建议特别有用。  
  
5.  在 **“优化报告”** 窗格中，单击 **“选择报告”** 来选择要查看的优化报告。  
  
#### <a name="to-view-tuning-logs-with-the-database-engine-tuning-advisor-gui"></a>用数据库引擎优化顾问 GUI 查看优化日志  
  
1.  使用数据库引擎优化顾问 GUI 或 **dta** 命令行实用工具优化数据库。 请确保在优化工作负荷时选中 **“常规”** 选项卡上的 **“保存优化日志”** 。 如果希望使用现有优化会话，请跳过此步骤，转到步骤 2。  
  
2.  启动数据库引擎优化顾问 GUI。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。 若要查看现有优化会话的优化摘要和报告，可以双击“会话监视器”  窗口中的会话名称将会话打开。  
  
3.  在新的优化会话完成之后或者工具加载了现有会话之后，单击 **“进度”** 选项卡。 **“优化日志”** 窗格将显示日志的内容。 日志包含有关数据库引擎优化顾问无法分析的工作负荷事件的信息。  
  
     如果优化会话中的所有事件均被数据库引擎优化顾问分析，将显示消息指示此会话的优化日志为空。 如果在最初运行优化会话时未选中 **“常规”** 选项卡上的 **“保存优化日志”** ，将显示一条消息来指出这一点。  
  
##  <a name="implement-tuning-recommendations"></a><a name="Implement"></a> 实施优化建议  
 可以手动实施数据库引擎优化顾问的建议，也可以在优化会话中自动实施。 如果需要在实施建议之前检查优化结果，请使用数据库引擎优化顾问 GUI。 然后，您可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 手动运行数据库引擎优化顾问在分析完工作负荷之后生成的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本，以便实施建议。 如果不需要在实施建议之前检查结果，则可以将 **-a** 选项与 **dta** 命令提示符实用工具结合使用。 这可以使实用工具在分析完工作负荷之后自动实施优化建议。 下列过程介绍了如何使用这两个数据库引擎优化顾问界面来实施优化建议。  
  
#### <a name="to-manually-implement-tuning-recommendations-with-the-database-engine-tuning-advisor-gui"></a>使用数据库引擎优化顾问 GUI 来手动实施优化建议  
  
1.  使用数据库引擎优化顾问 GUI 或 **dta** 命令提示符实用工具来优化数据库。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。 如果希望使用现有优化会话，请跳过此步骤，转到步骤 2。  
  
2.  启动数据库引擎优化顾问 GUI。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。 若要在现有的优化会话中实施优化建议，请在“会话监视器”  中双击会话名称将会话打开。  
  
3.  在新的优化会话完成之后，或者在工具加载了现有会话之后，在 **“操作”** 菜单上，单击 **“应用建议”** 。  
  
4.  在 **“应用建议”** 对话框中，选择 **“立即应用”** 或 **“安排以后执行”** 。 如果选择 **“安排以后执行”** ，请选择相应的日期和时间。  
  
5.  单击 **“确定”** 应用建议。  
  
#### <a name="to-automatically-implement-tuning-recommendations-using-the-dta-command-prompt-utility"></a>使用 dta 命令提示实用工具来自动实施优化建议  
  
1.  确定您希望数据库引擎优化顾问在分析过程中考虑添加、删除或保留的数据库功能（索引、索引视图、分区）。  
  
     在开始优化之前，请注意下列事项：  
  
    -   使用跟踪表作为工作负荷时，该表必须位于数据库引擎优化顾问正在优化的那台服务器上。 如果所创建的跟踪表位于其他服务器上，请将它移到数据库引擎优化顾问正在优化的服务器上。  
  
    -   如果优化会话运行的时间超出了预期的运行时间，可以按 CTRL+C 来结束优化会话。 在这种情况下，按 CTRL+C 会强行使 **dta** 根据它所处理的工作负荷生成尽可能好的建议，而不会浪费该工具在优化工作负荷上已经使用的时间。  
  
2.  在命令提示符下，输入以下内容：  
  
    ```  
    dta -E -D DatabaseName -if WorkloadFile -s SessionName -a  
    ```  
  
     其中 **-E** 指定优化会话使用可信连接（而不是登录 ID 和密码）， **-D** 指定要优化的数据库的名称或工作负荷所使用的多个数据库的逗号分隔列表， **-if** 指定工作负荷文件的名称和路径， **-s** 指定优化会话的名称， **-a** 指定让 **dta** 命令提示符实用工具在分析完工作负荷之后自动应用优化建议，而不显示提示。 有关使用 **dta** 命令提示符实用工具来优化数据库的详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。  
  
3.  按 Enter。  
  
##  <a name="perform-exploratory-analysis"></a><a name="Analysis"></a> 执行探索性分析  
 数据库管理员可以通过用户指定的数据库引擎优化顾问配置功能执行探索性分析。 使用此功能，数据库管理员将所需物理数据库设计指定给数据库引擎优化顾问，然后就可以评估该设计的性能效果，而无需实施该设计。 数据库引擎优化顾问图形用户界面 (GUI) 和命令行实用工具都支持用户指定的配置。 但是，命令行实用工具提供的灵活性最大。  
  
 如果您使用的是数据库引擎优化顾问 GUI，则可以评估实施部分数据库引擎优化顾问优化建议的效果，但无法向数据库引擎优化顾问添加假设物理设计结构来进行评估。  
  
 以下过程说明如何在两个工具界面上使用用户指定的配置功能。  
  
### <a name="using-database-engine-tuning-advisor-gui-to-evaluate-tuning-recommendations"></a>使用数据库引擎优化顾问 GUI 评估优化建议  
 以下过程说明如何评估数据库引擎优化顾问生成的建议，但是无法通过 GUI 指定新的物理设计结构以进行评估。  
  
##### <a name="to-evaluate-tuning-recommendations-with-the-database-engine-tuning-advisor-gui"></a>使用数据库引擎优化顾问 GUI 评估优化建议  
  
1.  使用数据库引擎优化顾问 GUI 优化数据库。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。 若要评估现有优化会话，请在“会话监视器”  中双击该会话。  
  
2.  在 **“建议”** 选项卡上，清除那些已列出但您不想使用的物理设计结构。  
  
3.  在 **“操作”** 菜单上，单击 **“评估建议”** 。 将为您创建一个新的优化会话。  
  
4.  键入新的 **“会话名称”** 。 若要查看正在评估的物理数据库设计结构配置，请选择数据库引擎优化顾问应用程序窗口底部的 **“说明”** 区域中的 **“单击此处可查看配置部分”** 。  
  
5.  在工具栏中，单击 **“开始分析”** 按钮。 数据库引擎优化顾问完成后，您可以在 **“建议”** 选项卡上查看结果。  
  
### <a name="using-database-engine-tuning-advisor-gui-to-export-tuning-session-results-for-what-if-tuning-analysis"></a>使用数据库引擎优化顾问 GUI 导出假设优化分析的优化会话结果  
 以下过程说明如何将数据库引擎优化顾问优化会话结果导出到 XML 文件，然后就可以使用 **dta** 命令行实用工具编辑并优化该文件。 这使您可以在确定新的假设物理设计结构是否能够实现所需性能改善之前，对这些结构执行优化分析，而不会在数据库中实施这些结构时生成开销。 使用数据库引擎优化顾问 GUI 先优化数据库，然后将优化结果导出到 **.xml** 文件，对于使用灵活的数据库引擎优化顾问 XML 架构执行模拟分析的 XML 新用户而言是一种很好的方法。  
  
##### <a name="to-export-tuning-session-results-from-the-database-engine-tuning-advisor-gui-for-what-if-analysis-with-the-dta-command-line-utility"></a>使用 dta 命令行实用工具从数据库引擎优化顾问 GUI 导出假设分析的优化会话结果  
  
1.  使用数据库引擎优化顾问 GUI 优化数据库。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。 若要评估现有优化会话，请在“会话监视器”  中双击该会话。  
  
2.  在 **“文件”** 菜单上，单击 **“导出会话结果”** ，然后将其保存为 XML 文件。  
  
3.  在您最喜爱的 XML 编辑器、文本编辑器或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中打开在步骤 2 中创建的 XML 文件。 向下滚动到 **Configuration** 元素。 复制 **Configuration** 元素部分并将其粘贴到 XML 输入文件模板中的 **TuningOptions** 元素之后。 保存此 XML 输入文件。  
  
4.  在步骤 3 中创建的新 XML 输入文件中，指定 **TuningOptions** 元素中所需的任何优化选项，编辑“配置”  元素部分（根据分析需要，添加或删除物理设计结构），保存文件并根据数据库引擎优化顾问 XML 架构验证该文件。 有关编辑此 XML 文件的详细信息，请参阅 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)。  
  
5.  将在步骤 4 中创建的 XML 文件用作 **dta** 命令行实用工具的输入。 有关通过此工具使用 XML 输入文件的信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)中的“使用 dta 实用工具优化数据库”部分。  
  
### <a name="using-the-user-specified-configuration-feature-with-the-dta-command-line-utility"></a>通过 dta 命令行实用工具使用用户指定的配置功能  
 如果您是一个经验丰富的 XML 开发人员，您可以创建一个用于指定物理数据库设计结构的工作负荷和假设配置（例如索引、索引视图或分区）的数据库引擎优化顾问 XML 输入文件。 然后，你可以使用 **dta** 命令行实用工具来分析此假设配置对你的数据库查询性能的影响。 以下分步解释这个过程：  
  
##### <a name="to-use-the-user-specified-configuration-feature-with-the-dta-command-line-utility"></a>通过 dta 命令行实用工具使用用户指定的配置功能  
  
1.  创建优化工作负荷。 有关执行此任务的信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。  
  
2.  将 [XML 输入文件示例用户指定配置 (DTA)](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md) 复制并粘贴到你的 XML 编辑器或文本编辑器。 使用此示例为您的优化会话创建 XML 输入文件。 有关执行此任务的信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)中的“创建 XML 输入文件”部分。  
  
3.  编辑示例 XML 输入文件中的 **TuningOptions** 和 **Configuration** 元素。 在 **TuningOptions** 元素中，指定希望数据库引擎优化顾问在优化会话期间考虑的物理设计结构。 在 **Configuration** 元素中，指定与希望数据库引擎优化顾问分析的物理数据库设计结构的假设配置相匹配的物理设计结构。 有关哪些属性和子元素可与 **TuningOptions** 和“配置”  父元素一起使用的信息，请参阅 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)。  
  
4.  保存该输入文件，以 **.xml** 为扩展名。  
  
5.  根据数据库引擎优化顾问 XML 架构验证在步骤 4 中保存的 XML 输入文件。 此架构在安装 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时安装在以下位置：  
  
    ```  
    C:\Program Files\Microsoft SQL Server\100\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd  
    ```  
  
     数据库引擎优化顾问 XML 架构也可通过 [https://schemas.microsoft.com/sqlserver/2004/07/dta/](https://schemas.microsoft.com/sqlserver/2004/07/dta/) 在线获得。  
  
6.  创建工作负荷和 XML 输入文件后，你就可以将该输入文件提交到 **dta** 命令行实用工具进行分析。 请确保为 **-ox** 实用工具参数指定 XML 输出文件名。 这样将创建一个具有 **Configuration** 元素中指定的建议配置的 XML 输出文件。 如果要再次运行数据库引擎优化顾问以检查基于该输出的另一个假设配置，您可以从输出文件中复制 **Configuration** 元素内容，然后将其粘贴到一个新的或原始 XML 输入文件中。 有关将 XML 输入文件与 **dta** 实用工具结合使用的信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)中的“使用 dta 实用工具优化数据库”部分。  
  
     优化完成后，可以使用数据库引擎优化顾问 GUI 来查看优化报告，也可以打开该 XML 输出文件查看 **TuningSummary** 和 **Configuration** 元素，以便查看数据库引擎优化顾问建议。 有关查看优化会话结果的信息，请参阅本主题中前面的 [查看优化输出](#View) 。 此外，注意，XML 输出文件可能包含数据库引擎优化顾问分析报告。  
  
7.  重复步骤 6 和步骤 7，直到您所创建的假设配置能使查询性能达到您的要求为止。 然后，可以实施该新配置。 有关详细信息，请参阅本主题前面的 [实施优化建议](#Implement) 。  
  
##  <a name="review-evaluate-and-clone-tuning-sessions"></a><a name="ReviewEvaluateClone"></a> 查看、评估和克隆优化会话  
 每当用户开始分析工作负荷对数据库的影响时，数据库引擎优化顾问都会创建新的优化会话。 可以使用数据库引擎优化顾问 GUI 中的“会话监视器”查看或重新加载在指定的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上运行的所有优化会话  。 能够查看所有现有优化会话后，就可以轻松实现以下操作：根据现有会话克隆会话，编辑现有优化建议、然后使用数据库引擎优化顾问来评估编辑的会话，或定期执行优化以监视数据库的物理设计。 例如，您可以按月优化数据库。  
  
 必须先通过使用数据库引擎优化顾问优化工作负荷，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上创建优化会话，然后才能查看服务器实例的优化会话。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。  
  
### <a name="review-existing-tuning-sessions"></a>查看现有优化会话  
 请按照以下步骤浏览指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上的现有优化会话。  
  
##### <a name="to-review-existing-tuning-sessions"></a>查看现有优化会话  
  
1.  启动数据库引擎优化顾问 GUI。 有关详细信息，请参阅 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)。  
  
2.  所有现有优化会话都显示在 **“会话监视器”** 窗口的上半部分。 显示的会话数取决于在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上优化数据库的次数。 可以使用滚动条查看所有优化会话。  
  
3.  单击优化会话名称后，其详细信息便会显示在 **“会话监视器”** 窗口的下半部分。  
  
4.  双击优化会话名称，其信息将加载到数据库引擎优化顾问中。 加载会话信息后，可以选择任意选项卡查看此优化会话的有关信息。  
  
### <a name="evaluate-existing-tuning-sessions-as-hypothetical-configurations"></a>采用假设配置评估现有优化会话  
 请按照以下步骤评估现有优化会话。 评估现有优化会话涉及查看和编辑其建议，然后重新优化。 例如，您决定仅对 **table1** 创建索引，因此需要从现有优化建议中去掉创建索引视图和分区的步骤。 然后，数据库引擎优化顾问创建新的优化会话，将编辑过的建议作为假设配置，并优化数据库的工作负荷。 这意味着数据库引擎优化顾问优化数据库的工作负荷时，就像已实施了编辑过的建议，这样，用户就可以执行有限的假设分析。 由于使用数据库引擎优化顾问 GUI 时只能选择部分现有建议，因此，只能执行有限的假设分析。 若要执行完整的模拟分析，必须将数据库引擎优化顾问 XML 输入文件与 **dta** 命令行实用工具结合使用，指定一种全新的假设配置，而不是采用任何现有优化会话的一部分。  
  
##### <a name="to-evaluate-an-existing-tuning-session"></a>评估现有优化会话  
  
1.  启动数据库引擎优化顾问后，双击“会话监视器”  上半部分中的优化会话，这会将会话信息加载到数据库引擎优化顾问中。  
  
2.  单击 **“进度”** 选项卡查看优化日志，该日志包含有关数据库引擎优化顾问不能优化的工作负荷中的所有事件的错误信息。 这些信息可以帮助您评估工作负荷的影响。  
  
3.  如果想进一步查看此会话的优化结果，请单击 **“报表”** 选项卡。您可以在其中查看优化摘要或从 **“选择报表”** 列表中选择优化报表。  
  
4.  单击 **“建议”** 选项卡查看优化建议。  
  
5.  如果不能确定是否实施某些建议，请取消选中这些建议。  
  
6.  在 **“操作”** 菜单上，单击 **“评估建议”** 。 数据库引擎优化顾问将编辑过的建议作为假设配置来创建新的优化会话。 若要查看 XML 格式的假设配置，请选择 **“单击此处可查看配置部分”** 。  
  
7.  在 **“常规”** 选项卡的 **“会话名称”** 中键入会话名称，并确保已指定正确的 **“工作负荷”** 。  
  
8.  在 **“优化选项”** 选项卡上，可以指定优化时间或任何 **“高级选项”** 。  
  
9. 在工具栏中，单击 **“开始分析”** 按钮。 数据库引擎优化顾问将使用假设配置开始优化数据库。 数据库引擎优化顾问完成后，您可以像平时查看其他会话一样查看此会话的结果。  
  
### <a name="clone-existing-tuning-sessions"></a>克隆现有优化会话  
 可以通过选择数据库引擎优化顾问中的克隆选项来根据现有会话创建新的优化会话。 使用克隆选项时，新的优化会话会基于现有的会话。 然后，可以根据需要更改新会话的优化选项。 按照前面的过程来评估现有会话时，数据库引擎优化顾问也会创建新的优化会话，但您不能更改优化选项。  
  
##### <a name="to-create-new-tuning-sessions-by-cloning-existing-sessions"></a>通过克隆现有会话创建新的优化会话  
  
1.  启动数据库引擎优化顾问后，双击“会话监视器”  上半部分中的优化会话，这会将会话信息加载到数据库引擎优化顾问中。  
  
2.  在 **“操作”** 菜单上，单击 **“克隆会话”** 。  
  
3.  在 **“常规”** 选项卡的 **“会话名称”** 中键入会话名称，并确保已指定正确的 **“工作负荷”** 。  
  
4.  在 **“优化选项”** 选项卡上，可以指定优化时间、数据库引擎优化顾问应考虑创建的物理设计结构以及应考虑在其建议中删除的内容。  
  
5.  如果要设置建议的空间限制、每个索引的最大列数以及是否希望数据库引擎优化顾问生成在 **联机时可以实施的建议，请单击** “高级选项” [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
6.  像其他优化会话一样，单击工具栏上的 **“开始分析”** 按钮分析工作负荷的影响。 数据库引擎优化顾问完成后，您可以像平时查看其他会话一样查看此会话的结果。  
  
##  <a name="user-interface-descriptions"></a><a name="UI"></a> 用户界面说明  
  
### <a name="sessions-monitor"></a>会话监视器  
 **“会话监视器”** 显示有关在数据库引擎优化顾问中打开的会话的信息。 若要在属性窗口中显示有关会话的信息，请在 **“会话监视器”** 中选择会话名称。  
  
### <a name="recommendations-tab"></a>“建议”选项卡  
 在数据库引擎优化顾问完成工作负荷的分析后，将显示 **“建议”** 选项卡。 此网格包含对涉及的每个对象的建议。 上方网格将显示分区建议（如果有的话），下方网格将显示索引建议。 如果没有建议，则不显示网格。  
  
 **“定义”** 列以超链接形式包含所建议分区或索引的定义。 此列通常很窄，无法看到整个定义。 单击超链接可以显示一个对话框，其中包含完整定义及 **“复制到剪贴板”** 按钮。  
  
#### <a name="partition-recommendations"></a>“分区建议”  
 **Database Name**  
 包含建议修改的对象的数据库。  
  
 **建议**  
 为提高性能而建议执行的操作。 可能的值为 Create 和 Drop。  
  
 **建议目标**  
 受建议影响的分区函数或分区方案。 此列中的图标反映对删除或添加 **“建议目标”** 的建议，以及该建议是分区函数还是分区方案。  
  
 **详细信息**  
 “建议目标”  的说明。 其值可能是一个范围（对于分区函数），或为空（对于分区方案）。  
  
 **分区数**  
 由建议的分区函数定义的分区数。 当此函数与方案一起使用并应用于表时，该值就是表中的数据被划分的分区数。  
  
 **定义**  
 “建议目标”  的定义。 单击此列以打开“SQL 脚本预览”对话框，该对话框中包含建议操作的脚本。  
  
##### <a name="index-recommendations"></a>“索引建议”  
 **Database Name**  
 包含建议修改的对象的数据库。  
  
 **Object Name**  
 与建议相关的表。  
  
 **建议**  
 为提高性能而建议执行的操作。 可能的值为 Create 和 Drop。  
  
 **建议目标**  
 受建议影响的索引或视图。 此列中的图标反映对删除或添加 **“建议目标”** 的建议。  
  
 **详细信息**  
 “建议目标”  的说明。 其值可能为聚集索引视图或者为空（表示非聚集索引）。 还指示索引是否是唯一的。  
  
 **分区方案**  
 如果建议了分区，则在此列中提供分区方案。  
  
 **大小(KB)**  
 所建议的新对象的预期大小。 如果此列空白，请单击 **“有关现有对象的大小，请参阅‘报告’”** 。  
  
 **定义**  
 “建议目标”  的定义。 单击此列以打开“SQL 脚本预览”对话框，该对话框中包含建议操作的脚本。  
  
 **“建议”**  
 选择此项可以在网格中显示所有现有对象，即使数据库引擎优化顾问没有提供与对象相关的建议。  
  
 **“有关现有对象的大小，请参阅‘报告’”**  
 选择此项可以查看针对建议网格中现有对象提供大小信息的报告。  
  
### <a name="actions-menuapply-recommendations-options"></a>“操作”菜单/“应用建议”选项  
 在分析工作负荷并提出建议后，请在 **“操作”** 菜单上单击 **“应用建议”** 以打开 **“应用建议”** 对话框。  
  
 **“立即应用”**  
 为该建议生成脚本，并运行脚本以实施建议。  
  
 **“安排以后执行”**  
 为该建议生成脚本，并将操作另存为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。  
  
 **Date**  
 指定要运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业以应用建议的日期。  
  
 **时间**  
 指定要运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业以应用建议的时间。  
  
### <a name="reports-tab-options"></a>“报告”选项卡选项  
 在数据库引擎优化顾问完成工作负荷的分析后，将显示 **“报告”** 选项卡。  
  
 **“优化摘要”**  
 显示数据库引擎优化顾问建议的摘要。  
  
 **Date**  
 数据库引擎优化顾问创建报告的日期。  
  
 **时间**  
 数据库引擎优化顾问创建报告的时间。  
  
 **Server**  
 作为数据库引擎优化顾问工作负荷的目标的服务器。  
  
 **要优化的数据库**  
 受数据库引擎优化顾问建议影响的数据库。  
  
 **工作负荷文件**  
 当工作负荷为文件时显示。  
  
 **工作负荷表**  
 当工作负荷为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表时显示。  
  
 **“工作负荷”**  
 在工作负荷已从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的查询编辑器中导入后显示。  
  
 **最长优化时间**  
 配置为可用于数据库引擎优化顾问分析的最长时间。  
  
 **优化所用的时间**  
 数据库引擎优化顾问分析工作负荷实际使用的时间。  
  
 **“预期的提高百分比”**  
 接受数据库引擎优化顾问的所有建议后，目标工作负荷预期的提高百分比。  
  
 **建议的最大空间(MB)**  
 建议使用的最大空间。 此值是在进行分析之前使用 **“优化选项”** 选项卡上的 **“高级选项”** 按钮进行配置的。  
  
 **当前所用空间(MB)**  
 由被分析的数据库中的索引当前使用的空间。  
  
 **建议使用的空间(MB)**  
 接受数据库引擎优化顾问的所有建议后，由索引使用的预期近似空间量。  
  
 **工作负荷中的事件数**  
 工作负荷中包含的事件数。  
  
 **已优化的事件数**  
 工作负荷中已优化的事件数。 如果无法优化某个事件，该事件将列在优化日志中。可以在 **“进度”** 选项卡上访问优化日志。  
  
 **已优化的语句数**  
 工作负荷中已优化的语句数。 如果无法优化某个语句，该语句将列在优化日志中。可以在 **“进度”** 选项卡上访问优化日志。  
  
 **优化集内的 SELECT 语句百分比**  
 已优化语句中 SELECT 语句所占的百分比。 仅在存在已优化的 SELECT 语句时显示。  
  
 **优化集内的 UPDATE 语句百分比**  
 已优化语句中 UPDATE 语句所占的百分比。 仅在存在已优化的 UPDATE 语句时显示。  
  
 **建议创建的索引数/建议删除的索引数**  
 建议在已优化数据库中创建或删除的索引数。 仅在建议中包含索引时显示。  
  
 **建议创建的视图索引数/建议删除的视图索引数**  
 建议在已优化数据库中创建或删除的视图索引数。 仅在建议中包含视图索引时显示。  
  
 **建议创建的统计信息数**  
 建议对已优化的数据库创建的统计信息数。 仅在统计信息有建议值时显示。  
  
 **Select Report**  
 查看所选报告的详细信息。 对于不同的报告，网格中的列可能会有所不同。  
  
## <a name="see-also"></a>另请参阅  
 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [dta 实用工具](../../tools/dta/dta-utility.md)  
  
