---
description: 调试数据流
title: 调试数据流 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- progress reporting [Integration Services]
- data viewers [Integration Services]
- data flow [Integration Services], debugging
- debugging [Integration Services], data flow
- counting rows
ms.assetid: 1c574f1b-54f7-4c05-8e42-8620e2c1df0f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c69bed5fbec76fab380f91515827630fa60938a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342160"
---
# <a name="debugging-data-flow"></a>调试数据流

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器包含可用于解决 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中数据流问题的功能和工具。  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器提供数据查看器。  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器和 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 转换提供行计数。  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器在运行时提供进度报告。  
  
## <a name="data-viewers"></a>数据查看器  
 数据查看器显示数据流中两个组件间的数据。 当数据从数据源提取出来并首次进入数据流时、转换对数据进行更新前后以及数据加载到其目标之前，数据查看器均可显示数据。  
  
 若要查看数据，请将数据查看器附加到连接两个数据流组件的路径。 在数据流组件间查看数据的能力，使得确定异常数据值、查看转换更改列值的方式以及发现转换失败的原因变得更加简单。 例如，您可能发现引用表中的查找失败，而为更正此问题，可以添加一个为空列提供默认数据的转换。  
  
 数据查看器可以在网格中显示数据。 若使用网格方式，则选择要显示的列。 选定列的值以表格的格式显示。  
  
 您还可以在一个路径上包含多个数据查看器。 可以用不同的格式显示相同的数据（例如，为数据创建一个图表视图和一个网格视图），也可以为不同的数据列创建不同的数据查看器。  
  
 在将数据查看器添加到路径时， [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器会将数据查看器图标添加到 **“数据流”** 选项卡的设计图面中，紧临该路径。 可具有多个输出的转换（如有条件拆分转换）可以在每个路径上包含一个数据查看器。  
  
 在运行时， **“数据查看器”** 窗口会打开，并显示由数据查看器格式所指定的信息。 例如，使用网格格式的数据查看器会显示选定列的数据、传递到数据流组件的输出行的行数以及所显示的行数。 信息按缓冲显示，根据数据流中的行的宽度，缓冲可以包含较多或较少的行。  
  
 在 **“数据查看器”** 对话框中，可以将数据复制到剪贴板、从表中清除所有数据、重新配置数据查看器、恢复数据流，以及分离或附加数据查看器。  
  
#### <a name="to-add-a-data-viewer"></a>添加数据查看器  
  
-   [将数据查看器添加到数据流](#add_viewer)  
  
## <a name="row-counts"></a>行计数  
 通过路径已传递的行的数量在 **设计器中的** “数据流” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 选项卡的设计图面上紧临该路径显示。 随着数据在路径中的移动，该数量会定期更新。  
  
 您还可以将行计数转换添加到数据流中，以捕获变量中的最终行计数。 有关详细信息，请参阅 [Row Count Transformation](../../integration-services/data-flow/transformations/row-count-transformation.md)。  
  
## <a name="progress-reporting"></a>进度报告  
 在运行包时， [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器用指示状态的颜色显示每个数据流组件，以此在 **“数据流”** 选项卡设计图面上描绘进度。 当每个组件开始执行其工作时，颜色由无色变为黄色，而在成功完成后，则变为绿色。 红色指示组件失败。  
  
 下表介绍颜色编码。  
  
|Color|说明|  
|-----------|-----------------|  
|无色|等待被数据流引擎调用。|  
|Yellow|正在执行转换、提取数据或加载数据。|  
|绿色|成功运行。|  
|红色|运行中出现错误。|  

## <a name="analysis-of-data-flow"></a>数据流分析
  你可以使用 [catalog.execution_data_statistics](../../integration-services/system-views/catalog-execution-data-statistics.md) SSISDB 数据库视图分析包的数据流  。 此视图在每当数据流组件将数据发送到下游组件时显示一行。 这些信息可用来进一步了解发送到每个组件的行。  
  
> [!NOTE]  
>  日志记录级别必须设置为“详细”  ，才能通过 catalog.execution_data_statistics 视图捕获信息。  
  
 以下示例显示在包的组件之间发送的行数。  
  
```sql
use SSISDB  
select package_name, task_name, source_component_name, destination_component_name, rows_sent  
from catalog.execution_data_statistics  
where execution_id = 132  
order by source_component_name, destination_component_name   
```  
  
 以下示例计算每个组件针对特定的执行每毫秒所发送的行数。 计算的值为：  
  
-   **total_rows** - 组件发送的所有行的总和  
  
-   **wall_clock_time_ms** - 每个组件已使用的执行总时间（以毫秒为单位）  
  
-   **num_rows_per_millisecond** - 每个组件每毫秒发送的行数  
  
 **HAVING** 子句用于防止在计算中出现被零除错误。  
  
```sql  
use SSISDB  
select source_component_name, destination_component_name,  
    sum(rows_sent) as total_rows,  
    DATEDIFF(ms,min(created_time),max(created_time)) as wall_clock_time_ms,  
    ((0.0+sum(rows_sent)) / (datediff(ms,min(created_time),max(created_time)))) as [num_rows_per_millisecond]  
from [catalog].[execution_data_statistics]  
where execution_id = 132  
group by source_component_name, destination_component_name  
having (datediff(ms,min(created_time),max(created_time))) > 0  
order by source_component_name desc  
```  

## <a name="configure-an-error-output-in-a-data-flow-component"></a>在数据流组件中配置错误输出
  很多数据流组件支持错误输出，根据组件的不同， [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器提供几种不同的错误输出配置方法。 除了配置错误输出外，您还可以配置错误输出的列。 其中包括配置由该组件添加的 **ErrorCode** 和 **ErrorColumn** 列。  
  
### <a name="configuring-an-error-output"></a>配置错误输出  
 若要配置错误输出，您有两种选择：  
  
-   使用 **“配置错误输出”** 对话框。 可以使用此对话框，在支持错误输出的任意数据流组件中配置错误输出。  
  
-   使用组件的编辑器对话框。 某些组件允许您直接从其编辑器对话框配置错误输出。 但是，您不能从以下组件的编辑器对话框配置错误输出：ADO NET 源、导入列转换、OLE DB 命令转换或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 目标。  
  
 下面的过程介绍如何使用这些对话框来配置错误输出。  
  
#### <a name="to-configure-an-error-output-using-the-configure-error-output-dialog-box"></a>使用“配置错误输出”对话框配置错误输出  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，单击 **“数据流”** 选项卡。  
  
4.  将红色箭头表示的错误输出从错误源组件拖到数据流中的另一个组件。  
  
5.  在 **“配置错误输出”** 对话框中，为组件输入中的每列在 **“错误”** 列和 **“截断”** 列中选择一个操作。  
  
6.  若要保存已更新的包，请在 **“文件”** 菜单中单击 **“保存选定项”** 。  
  
#### <a name="to-add-an-error-output-using-the-editor-dialog-box-for-the-component"></a>使用组件的编辑器对话框添加错误输出  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，单击 **“数据流”** 选项卡。  
  
4.  双击要在其中配置错误输出的数据流组件，然后根据不同组件，执行下列步骤之一：  
  
    -   单击 **“配置错误输出”** 。  
  
    -   单击 **“错误输出”** 。  
  
5.  为每列设置 **“错误”** 选项。  
  
6.  为每列设置 **“截断”** 选项。  
  
7.  单击“确定”。  
  
8.  若要保存已更新的包，请在 **“文件”** 菜单中单击 **“保存选定项”** 。  
  
### <a name="configuring-error-output-columns"></a>配置错误输出列  
 若要配置错误输出列，必须使用 **“高级编辑器”** 对话框中的 **“输入属性和输出属性”** 选项卡。  
  
#### <a name="to-configure-error-output-columns"></a>配置错误输出列  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，单击 **“数据流”** 选项卡。  
  
4.  右键单击要配置其错误输出列的组件，再单击“显示高级编辑器”  。  
  
5.  单击“输入和输出属性”选项卡并展开“\<component name> 错误输出”，然后展开“输出列”  。  
  
6.  单击某列，然后更新其属性。  
  
    > [!NOTE]  
    >  列的列表中包括组件输入中的列、由以前的错误输出添加的 **ErrorCode** 和 **ErrorColumn** 列，以及由此组件添加的 **ErrorCode** 和 **ErrorColumn** 列。  
  
7.  单击“确定” **。**  
  
8.  若要保存已更新的包，请在 **“文件”** 菜单中单击 **“保存选定项”** 。  

## <a name="add-a-data-viewer-to-a-data-flow"></a><a name="add_viewer"></a> 将数据查看器添加到数据流
  本主题介绍如何在数据流中添加和配置数据查看器。 数据查看器可以显示在两个数据流组件之间移动的数据。 例如，数据查看器可以显示在数据流中的转换修改数据之前从数据源中提取的数据。  
  
 路径将一个数据流组件的输出连接到另一个组件的输入，以此连接数据流中的组件。  
  
 在可以将数据查看器添加到包之前，包必须包括一个数据流任务和至少两个已连接在一起的数据流组件。  
  
 将数据查看器添加到错误输出即可查看对错误的描述以及发生了错误的列的名称。 默认情况下，错误输出只包括针对错误和列的数字标识符。  
  
### <a name="to-add-a-data-viewer-to-a-data-flow"></a>将数据查看器添加到数据流  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  如果 **“控制流”** 选项卡尚未处于活动状态，请单击该选项卡。  
  
4.  单击要将数据查看器附加到其数据流的数据流任务，然后单击 **“数据流”** 选项卡。  
  
5.  右键单击两个数据流组件之间的路径，然后单击“编辑”。  
  
6.  在 **“常规”** 页上，可以查看和编辑路径属性。 例如，从“路径批注”下拉列表中，你可以选择要在路径旁边显示的批注。  
  
7.  在 **“元数据”** 页上，您可以查看列元数据并将元数据复制到剪贴板。  
  
8.  在 **“数据查看器”** 页上，单击 **“启用数据查看器”** 。  
  
9. 在“要显示的列”区域中，选择要在数据查看器中显示的列。 默认情况下，所有可用列都处于选定状态并在 **“已显示的列”** 列表中列出。 选择不希望使用的列，然后单击向左键，将它们移到 **“未使用的列”** 列表中。  
  
    > [!NOTE]  
    >  在网格中，表示 DT_DATE、DT_DBTIME2、DT_FILETIME、DT_DBTIMESTAMP、DT_DBTIMESTAMP2 和 DT_DBTIMESTAMPOFFSET 数据类型的值显示为 ISO 8601 格式字符串，空间分隔符将替代 **T** 分隔符。 表示 DT_DATE 和 DT_FILETIME 数据类型的值包括七位秒小数。 因为 DT_FILETIME 数据类型仅存储三位秒小数，网格会将其余四位显示为零。 表示 DT_DBTIMESTAMP 数据类型的值包含三位秒小数。 对于表示 DT_DBTIME2、DT_DBTIMESTAMP2 和 DT_DBTIMESTAMPOFFSET 数据类型的值，秒小数的数字位数与为列数据类型指定的小数位数对应。 有关 ISO 8601 格式的详细信息，请参阅 [Date and Time Formats](../data-flow/parsing-data.md)。 有关数据类型的详细信息，请参阅 [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md)。  
  
10. 单击“确定”。  

## <a name="data-flow-taps"></a>数据分流
 运行时，可在包的数据流路径上添加数据分流点，并将数据分流点的输出定向到外部文件。 若要使用此功能，您必须使用项目部署工具将 SSIS 项目部署到 SSIS 服务器。 将包部署到服务器之后，需要对 SSISDB 数据库执行 T-SQL 脚本，以便在执行该包之前添加数据分流点。 下面是一个示例方案：  
  
1.  通过使用 [catalog.create_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md)存储过程，创建包的执行实例。  
  
2.  通过使用 [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md) 或 [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md) 存储过程，添加数据分流点。  
  
3.  使用 [catalog.start_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)，启动包的执行实例。  
  
 下面是执行上述方案中所述步骤的示例 SQL 脚本：  
  
```sql
Declare @execid bigint  
EXEC [SSISDB].[catalog].[create_execution] @folder_name=N'ETL Folder', @project_name=N'ETL Project', @package_name=N'Package.dtsx', @execution_id=@execid OUTPUT  
EXEC [SSISDB].[catalog].add_data_tap @execution_id = @execid, @task_package_path = '\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[Flat File Source.Flat File Source Output]', @data_filename = 'output.txt'  
EXEC [SSISDB].[catalog].[start_execution] @execid  
```  
  
 该 create_execution 存储过程的文件夹名称、项目名称和包名称参数对应于 Integration Services 目录中的文件夹、项目和包名称。 如下图所示，您可以从 SQL Server Management Studio 获取在 create_execution 调用中使用的文件夹、项目和包名称。 如果您在这里没有看到该 SSIS 项目，则可能还未将该项目部署到 SSIS 服务器。 在 Visual Studio 中右键单击 SSIS 项目，然后单击“部署”以将该项目部署到目标 SSIS 服务器。  
  
 您可以不键入 SQL 语句，而是通过执行以下步骤来生成执行包脚本：  
  
1.  右键单击“Package.dtsx”  ，然后单击“执行”  。  
  
2.  单击 **“脚本”** 工具栏按钮以生成脚本。  
  
3.  现在，在 start_execution 调用的前面添加 add_data_tap 语句。  
  
 add_data_tap 存储过程的 task_package_path 参数对应于 Visual Studio 中数据流任务的 PackagePath 属性。 在 Visual Studio 中，右键单击“数据流任务”  ，然后单击“属性”  启动“属性”窗口。  请记下 **PackagePath** 属性的值，以将其用作 add_data_tap 存储过程调用的 task_package_path 参数值。  
  
 add_data_tap 存储过程的 dataflow_path_id_string 参数对应于您要添加数据分流点的数据流路径的 IdentificationString 属性。 若要获取 dataflow_path_id_string，请单击数据流路径（数据流中任务间的箭头），并记下“属性”窗口中 **IdentificationString** 属性的值。  
  
 执行脚本时，输出文件存储在 \<Program Files>\Microsoft SQL Server\110\DTS\DataDumps 中。 如果已存在同名文件，则将创建带有后缀的新文件（例如：output[1].txt）。  
  
 如前所述，也可使用 [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md)存储过程代替使用 add_data_tap 存储过程。 此存储过程将数据流任务的 ID 用作参数，而不使用 task_package_path。 您可以从 Visual Studio 中的属性窗口获取数据流任务的 ID。  
  
### <a name="removing-a-data-tap"></a>删除数据分流点  
 可使用 [catalog.remove_data_tap](../../integration-services/system-stored-procedures/catalog-remove-data-tap.md) 存储过程删除数据分流点，然后再启动执行。 此存储过程将数据分流点 ID 用作参数，此 ID 可作为 add_data_tap 存储过程的输出来获取。  
  
```sql
DECLARE @tap_id bigint  
EXEC [SSISDB].[catalog].add_data_tap @execution_id = @execid, @task_package_path = '\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[Flat File Source.Flat File Source Output]', @data_filename = 'output.txt' @data_tap_id=@tap_id OUTPUT  
EXEC [SSISDB].[catalog].remove_data_tap @tap_id  
```  
  
### <a name="listing-all-data-taps"></a>列出所有数据分流点  
 您也可以使用 catalog.execution_data_taps 视图列出所有数据分流点。 下面的示例提取一个规范执行实例（ID：54）的数据分流点。  
  
```sql 
select * from [SSISDB].[catalog].execution_data_taps where execution_id=@execid  
```  
  
### <a name="performance-consideration"></a>性能注意事项  
 启用详细日志记录级别并添加数据分流点会增加数据集成解决方案所执行的 I/O 操作数。 因此，建议仅出于故障排除目的添加数据分流点。  
  
### <a name="video"></a>视频  
 此 [video on TechNet](../../sql-server/index.yml) 演示了如何在 SQL Server 2012 SSISDB 目录中添加/使用数据分流点，从而有助于以编程方式对包进行调试并在运行时捕获部分结果。 它还讨论了如何列出/删除这些数据分流点，并讨论了在 SSIS 包中使用数据分流点的最佳实践。  
 
## <a name="see-also"></a>另请参阅  
 [数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)  
  
