---
title: SQLdiag 实用工具
description: 使用 SQLdiag 实用工具从 SQL Server 和其他类型的服务器中收集日志和数据文件，以及一直监视服务器或对问题进行故障排除。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], SQLdiag
- stopping diagnostic collection
- storing diagnostic information
- performance [SQL Server], diagnostic collection
- diagnostic records [SQL Server]
- scripts [SQL Server], diagnostic collection
- logs [SQL Server], diagnostic collection
- starting diagnostic collection
- clustered instance of SQL Server
- monitoring performance [SQL Server], diagnostic collection
- security [SQL Server], diagnostic collection
- SQLDIAG service
- space [SQL Server], diagnostic collection
- SQLdiag utility
- disk space [SQL Server], diagnostic collection
- configuration files [SQL Server]
- automatic diagnostic collection
- clusters [SQL Server], diagnostic collection
ms.assetid: 45ba1307-33d1-431e-872c-a6e4556f5ff2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c9f3528ac4f377e379afd26e6583df3fe63fb2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353334"
---
# <a name="sqldiag-utility"></a>SQLdiag 实用工具
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  **SQLdiag** 实用工具是一般用途的诊断信息收集实用工具，可作为控制台应用程序或服务运行。 可以使用 **SQLdiag** 从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 和其他类型的服务器中收集日志和数据文件，同时还可将其用于一直监视服务器或对服务器的特定问题进行故障排除。 **SQLdiag** 旨在加快和简化为 [!INCLUDE[msCoName](../includes/msconame-md.md)] 客户支持服务部门收集诊断信息的过程。  
  
> [!NOTE]  
>  该实用工具可以进行更改，因此依赖于其命令行参数或行为的应用程序或脚本可能无法在未来版本中正常运行。  
  
 **SQLdiag** 可以收集下列类型的诊断信息：  
  
-   Windows 性能日志  
  
-   Windows 事件日志  
  
-   [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] 跟踪  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 阻塞信息  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置信息  
  
 通过编辑配置文件 SQLDiag.xml，可以指定希望 **SQLdiag** 收集的信息类型，详细介绍见下文。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqldiag   
     { [/?] }  
     |  
     { [/I configuration_file]  
       [/O output_folder_path]  
       [/P support_folder_path]  
       [/N output_folder_management_option]  
       [/M machine1 [ machine2 machineN]| @machinelistfile]  
       [/C file_compression_type]  
       [/B [+]start_time]  
       [/E [+]stop_time]  
       [/A SQLdiag_application_name]  
       [/T { tcp [ ,port ] | np | lpc } ]  
       [/Q] [/G] [/R] [/U] [/L] [/X] }  
     |  
     { [START | STOP | STOP_ABORT] }  
     |  
     { [START | STOP | STOP_ABORT] /A SQLdiag_application_name }  
```  
  
## <a name="arguments"></a>参数  
 **/?**  
 显示用法信息。  
  
 **/I** _configuration_file_  
 设置 **SQLdiag** 要使用的配置文件。 默认情况下， **/I** 设置为 SQLDiag.Xml。  
  
 **/O** _output_folder_path_  
 将 **SQLdiag** 输出重定向到指定文件夹。 如果未指定 **/O** 选项，则 **SQLdiag** 输出结果将会写入 **SQLdiag** 启动文件夹下名为 SQLDIAG 的子文件夹中。 如果 SQLDIAG 文件夹不存在，则 **SQLdiag** 将会尝试创建该文件夹。  
  
> [!NOTE]  
>  输出文件夹位置相对于可使用 **/P** 指定的支持文件夹的位置。 若要为输出文件夹设置一个完全不同的位置，请为 **/O** 指定完整的目录路径。  
  
 **/P** _support_folder_path_  
 设置支持文件夹路径。 默认情况下，将 **/P** 设置为存放 **SQLdiag** 可执行文件的文件夹。 支持文件夹包含 **SQLdiag** 支持文件，如 XML 配置文件、Transact-SQL 脚本以及该实用工具在收集诊断信息过程中所使用的其他文件。 如果使用该选项指定一个备用的支持文件路径，则 **SQLdiag** 会自动将其所需的支持文件复制到指定的文件夹（如果这些文件尚未存在）。  
  
> [!NOTE]  
>  若要将当前文件夹设置为支持路径，请在命令行中指定 **%cd%** ，如下所示：  
>   
>  **SQLDIAG /P %cd%**  
  
 **/N** _output_folder_management_option_  
 设置 **SQLdiag** 在其启动时，是覆盖还是重命名输出文件夹。 可用选项：  
  
 1 = 覆盖输出文件夹（默认）  
  
 2 = 当 **SQLdiag** 启动时，将输出文件夹重命名为 SQLDIAG_00001、SQLDIAG_00002 等等。 重命名当前输出文件夹之后， **SQLdiag** 将输出写入默认输出文件夹 SQLDIAG。  
  
> [!NOTE]  
>  **SQLdiag** 在启动时不会将输出追加到当前输出文件夹。 它只能覆盖默认的输出文件夹（选项 1）或重命名该文件夹（选项 2），然后将输出写入名为 SQLDIAG 的新默认输出文件夹。  
  
 **/M** _machine1_ [ *machine2* *machineN*] | *\@machinelistfile*  
 覆盖在配置文件中指定的计算机。 默认情况下，配置文件为 SQLDiag.Xml，也可以使用 **/I** 参数进行设置。 如果指定多台计算机，请用空格分隔各个计算机名称。  
  
 使用“\@machinelistfile”可指定要存储在配置文件中的计算机列表文件名。  
  
 **/C** _file_compression_type_  
 设置 **SQLdiag** 输出文件夹文件所用的文件压缩类型。 可用选项：  
  
 0 = 无（默认）  
  
 1 = 使用 NTFS 压缩  
  
 **/B** [ **+** ]*start_time*  
 按照以下格式指定开始收集诊断数据的日期和时间：  
  
 YYYYMMDD_HH:MM:SS  
  
 时间使用二十四小时制指定。 例如，下午 2:00 应指定为 **14:00:00**。  
  
 使用 **+** 并且不带日期（只使用 HH:MM:SS），以指定与当前日期和时间相对的时间。 例如，如果指定 **/B +02:00:00**，则 **SQLdiag** 将会在 2 小时后开始收集信息。  
  
 不要在 **+** 和指定的 *start_time* 之间插入空格。  
  
 如果指定的开始时间是过去的某一时间，则 **SQLdiag** 将会强行更改开始日期，以使开始日期和时间为将来的日期和时间。 例如，如果指定时间为 **/B 01:00:00** ，而当前时间为 08:00:00，则 **SQLdiag** 将会强行将开始日期更改为下一天。  
  
 请注意， **SQLdiag** 使用运行实用工具的计算机上的本地时间。  
  
 **/E** [ **+** ]*stop_time*  
 按照以下格式指定停止收集诊断数据的日期和时间：  
  
 YYYYMMDD_HH:MM:SS  
  
 时间使用二十四小时制指定。 例如，下午 2:00 应指定为 **14:00:00**。  
  
 使用 **+** 并且不带日期（只使用 HH:MM:SS），以指定与当前日期和时间相对的时间。 例如，如果使用 **/B +02:00:00 /E +03:00:00** 指定开始时间和结束时间，则 **SQLdiag** 将会在 2 小时后开始收集信息，经过 3 小时的信息收集后便停止收集并退出。 如果不指定 **/B** ，则 **SQLdiag** 将会立即开始收集诊断信息，并按 **/E** 指定的日期和时间结束收集操作。  
  
 不要在 **+** 和指定的 *start_time* 或 *end_time* 之间插入空格。  
  
 请注意， **SQLdiag** 使用运行实用工具的计算机上的本地时间。  
  
 **/A**  _SQLdiag_application_name_  
 使你可针对同一个 **实例运行多个** SQLdiag [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实用工具的实例。  
  
 每个 *SQLdiag_application_name* 标识不同的 **SQLdiag** 的实例。 *SQLdiag_application_name* 实例和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例名称之间没有任何关系。  
  
 *SQLdiag_application_name* 可用于启动或停止特定的 **SQLdiag** 服务实例。  
  
 例如：  
  
 **SQLDIAG START /A**  _SQLdiag_application_name_  
  
 它也可与 **/R** 选项一起使用，以将特定的 **SQLdiag** 实例注册为服务。 例如：  
  
 **SQLDIAG /R /A** _SQLdiag_application_name_  
  
> [!NOTE]  
>  **SQLdiag** 会自动在为 *SQLdiag_application_name* 指定的实例名称之前添加前缀 DIAG$。 如果你将 **SQLdiag** 注册为服务，则上述操作会提供有意义的服务名称。  
  
 /T { tcp [ ,*port* ] | np | lpc }  
 使用指定协议连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的实例。  
  
 tcp [,*port*]  
 传输控制协议/Internet 协议 (TCP/IP)。 您可以选择为连接指定一个端口号。  
  
 np  
 命名管道。 默认情况下， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的默认实例侦听命名管道 `\\.\pipe\sql\query` 和 `\\.\pipe\MSSQL$<instancename>\sql\query` 以获取命名实例。 您不能使用备用管道名称连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例。  
  
 lpc  
 本地过程调用。 如果客户端要连接到同一计算机上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例，则可以使用此 Shared Memory 协议。  
  
 **/Q**  
 在静默模式下运行 **SQLdiag** 。 **/Q** 可取消所有提示，如密码提示。  
  
 **/G**  
 在常规模式下运行 **SQLdiag** 。 指定 **/G** 后， **SQLdiag** 在启动时不会强制执行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 连接检查或验证用户是否为 **sysadmin** 固定服务器角色的成员。 **SQLdiag** 会让 Windows 来确定用户是否具有收集各个请求的诊断信息的相应权限。  
  
 如果未指定 **/G** ，则 **SQLdiag** 将进行检查，以确定用户是否为 Windows **Administrators** 组的成员，如果该用户不是 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Administrators **组成员，则不收集** 诊断信息。  
  
 **/R**  
 将 **SQLdiag** 注册为服务。 你将 **SQLdiag** 注册为服务时指定的所有命令行参数，都将留到以后用来运行该服务。  
  
 将 **SQLdiag** 注册为服务时，默认服务名称为 SQLDIAG。 可以使用 **/A** 参数更改服务名称。  
  
 使用 **START** 命令行参数可以启动该服务：  
  
 **SQLDIAG START**  
  
 也可使用 **net start** 命令启动该服务：  
  
 **net  start SQLDIAG**  
  
 **/U**  
 将 **SQLdiag** 撤消注册为服务。  
  
 如果撤消注册命名的 **SQLdiag** 实例，则也要使用 **/A** 参数。  
  
 **/L**  
 如果还分别使用 **/B** 或 **/E** 参数指定了开始时间或结束时间，则以连续模式运行 **SQLdiag** 。 在诊断信息收集工作因预定关闭而停止后，**SQLdiag** 会自动重新启动。 例如，使用 **/E** 或 **/X** 参数。  
  
> [!NOTE]  
>  如果未使用 **SQLdiag** 和 **/L** 命令行参数指定开始时间或结束时间，则 **SQLdiag** 将忽略 **/L** comm将忽略 line arguments.  
  
 使用 **/L** 并不表示是服务模式。 若要在 **SQLdiag** 作为服务运行时使用 **/L** ，请在注册该服务时的命令行中指定它。  
  
 **/X**  
 在快照模式下运行 **SQLdiag** 。 **SQLdiag** 会拍摄所有配置的诊断信息的快照，然后自动关闭。  
  
 **START** | **STOP** | **STOP_ABORT**  
 启动或停止 **SQLdiag** 服务。 **STOP_ABORT** 强制在服务未完成当前正在进行的诊断信息收集的情况下尽快地关闭该服务。  
  
 使用这些服务控制参数时，它们必须为命令行中使用的第一个参数。 例如：  
  
 **SQLDIAG START**  
  
 只有指定 **SQLdiag** 命名实例的 **/A** 参数才可以与 **START**、 **STOP** 或 **STOP_ABORT** 一起使用，以控制特定的 **SQLdiag** 服务实例。 例如：  
  
 **SQLDIAG START /A** _SQLdiag_application_name_  
  
## <a name="security-requirements"></a>安全要求  
 除非以通用模式（通过指定 /G 命令行参数）运行 SQLdiag，否则运行 SQLdiag 的用户必须为 Windows“管理员”组的成员和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sysadmin 固定服务器角色的成员。 默认情况下， **SQLdiag** 使用 Windows 身份验证连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ，但是它也支持 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证。  
  
## <a name="performance-considerations"></a>性能注意事项  
 运行 **SQLdiag** 的性能效果取决于其配置收集的诊断数据的类型。 例如，如果已配置 **SQLdiag** 来收集 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] 跟踪信息，则选择跟踪的事件类越多，服务器性能所受的影响就越大。  
  
 运行 **SQLdiag** 对服务器性能的影响大体相当于分别收集配置的诊断信息的开销之和。 例如，使用 **SQLdiag** 收集某个跟踪和使用 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]收集该跟踪所产生的性能开销相同。 使用 **SQLdiag** 对性能的影响可以忽略不计。  
  
## <a name="required-disk-space"></a>所需磁盘空间  
 因为 **SQLdiag** 可收集不同类型的诊断信息，所以，运行 **SQLdiag** 所需的可用磁盘空间也有所不同。 诊断信息的收集量取决于服务器正在处理的工作负荷的性质和数量，范围可以从数 MB 到数 GB。  
  
## <a name="configuration-files"></a>配置文件  
 **SQLdiag** 启动时会读取指定的配置文件和命令行参数。 可以在配置文件中指定 **SQLdiag** 收集的诊断信息的类型。 默认情况下， **SQLdiag** 使用 SQLDiag.Xml 配置文件，每次运行工具时都会提取该文件，它位于 **SQLdiag** 实用工具启动文件夹中。 配置文件使用 XML 架构 SQLDiag_schema.xsd，每次运行 **SQLdiag** 时，也会将该架构从可执行文件提取到实用工具启动目录中。  
  
### <a name="editing-the-configuration-files"></a>编辑配置文件  
 可以复制和编辑 SQLDiag.Xml，以更改 **SQLdiag** 收集的诊断数据的类型。 编辑配置文件时，请始终使用可将配置文件对照其 XML 架构进行验证的 XML 编辑器，如 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]。 不应直接编辑 SQLDiag.Xml。 而应制作一个 SQLDiag.Xml 副本，为副本指定一个新名称并将其存储在同一文件夹中。 然后，编辑此新文件，并使用 **/I** 参数将该文件传递给 **SQLdiag**。  
  
#### <a name="editing-the-configuration-file-when-sqldiag-runs-as-a-service"></a>SQLdiag 作为服务运行时编辑配置文件  
 如果已将 **SQLdiag** 作为服务运行并需要编辑配置文件，请指定 **/U** 命令行参数以撤消注册 SQLDIAG 服务，然后使用 **/R** 命令行参数重新注册服务。 撤消服务注册和重新注册服务会删除 Windows 注册表中缓存的旧配置信息。  
  
## <a name="output-folder"></a>输出文件夹  
 如果未使用 **/O** 参数指定输出文件夹， **SQLdiag** 将在 **SQLdiag** 启动文件夹下创建一个名为 SQLDIAG 的子文件夹。 对于涉及大量跟踪的诊断信息收集（例如使用 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] 时），请确保输出文件夹位于本地驱动器，并有足够的空间存储请求的诊断信息输出。  
  
 重新启动 **SQLdiag** 后，它将覆盖输出文件夹的内容。 若要避免出现这种情况，请在命令行中指定 **/N 2** 。  
  
## <a name="data-collection-process"></a>数据收集过程  
 在 **SQLdiag** 启动时，它会执行收集 SQLDiag.Xml 中指定的诊断数据所必需的初始化检查。 该过程需要持续数秒。 如果 **SQLdiag** 作为控制台应用程序运行，则在它开始收集诊断数据时会显示一条消息，通知你 **SQLdiag** 已开始收集，你可以按 CTRL+C 停止收集。 如果 **SQLdiag** 作为服务运行，类似消息将被写入 Windows 事件日志。  
  
 如果要使用 **SQLdiag** 诊断可以再现的问题，则请等到收到此消息后再在服务器中再现问题。  
  
 **SQLdiag** 以并行方式收集大多数诊断数据。 除了通过 Windows 性能日志和事件日志收集的信息之外，所有诊断信息都是通过连接到工具（如 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sqlcmd 实用工具或 Windows 命令处理器）来收集的。 **SQLdiag** 在每台计算机上使用一个工作线程来监视其他工具的诊断数据收集，经常会同时等待几个工具完成操作。 在收集过程中， **SQLdiag** 会将每个诊断信息的输出内容传送到输出文件夹。  
  
## <a name="stopping-data-collection"></a>停止数据收集  
 **SQLdiag** 在开始收集诊断数据后会持续进行收集，直至收集被停止或在配置好的指定时间停止。 可以使用 **/E** 参数或 **/X** 参数将 **SQLdiag** 配置为在指定时间停止，第一个参数允许指定一个停止时间，第二个参数则使 **SQLdiag** 以快照模式运行。  
  
 **SQLdiag** 停止时将停止它启动的所有诊断。 例如，它将停止正在收集的 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] 跟踪，停止执行其正在运行的 [!INCLUDE[tsql](../includes/tsql-md.md)] 脚本，同时还停止它在数据收集期间生成的任何子进程。 诊断数据收集完成后，便会退出 **SQLdiag** 。  
  
> [!NOTE]  
>  不支持暂停 **SQLdiag** 服务。 如果尝试暂停 **SQLdiag** 服务，则该服务会在完成收集你暂停时它正在收集的诊断信息后停止。 如果在停止 **SQLdiag** 后重新启动，则应用程序会重新启动并覆盖输出文件夹。 若要避免覆盖输出文件夹，请在命令行中指定 **/N 2** 。  
  
 **停止作为控制台应用程序运行的 SQLdiag**  
  
 如果正将 **SQLdiag** 作为控制台应用程序运行，则在运行 **SQLdiag** 的控制台窗口中按 CTRL+C 将其停止。 按下 Ctrl+C 之后，控制台窗口中将显示一条消息，通知你正在终止 **SQLDiag** 数据收集，此时请等待过程结束。该过程可能需要几分钟的时间。  
  
 按 Ctrl+C 两次可终止所有子诊断过程并立即终止应用程序。  
  
 **停止作为服务运行的 SQLdiag**  
  
 如果将 **SQLdiag** 作为服务运行，则在 **SQLdiag** 启动文件夹中运行 **SQLDiag STOP** 可将其停止。  
  
 如果在同一台计算机中运行多个 **SQLdiag** 实例，则停止该服务时，也可以将 **SQLdiag** 实例名传递到命令行中。 例如，若要停止名为 Instance1 的 **SQLdiag** 实例，请使用以下语法：  
  
```  
SQLDIAG STOP /A Instance1  
```  
  
> [!NOTE]  
>  **/A** 是唯一可以与 **START**、 **STOP** 或 **STOP_ABORT** 一起使用的命令行参数。 如果需要使用服务控制谓词之一指定 **SQLdiag** 命名实例，则在命令行中控制谓词的后面指定 **/A** ，如上述语法示例中所示。 使用控制谓词时，它们必须为命令行中的第一个参数。  
  
 若要尽快停止服务，请在实用工具启动文件夹中运行 **SQLDIAG STOP_ABORT** 。 该命令会在不等待当前正在执行的任何诊断信息收集完成的情况下将其中止。  
  
> [!NOTE]  
>  使用 **SQLDiag STOP** 或 **SQLDIAG STOP_ABORT** 以停止 **SQLdiag** 服务。 请不要使用 Windows 服务控制台停止 **SQLdiag** 或其他 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 服务。  
  
## <a name="automatically-starting-and-stopping-sqldiag"></a>自动启动和停止 SQLdiag  
 若要在指定时间自动启动和停止诊断数据收集，请使用 /Bstart\_time 和 /Estop\_time 参数（使用 24 小时制）。 例如，如果要解决一个总是在 02:00:00 左右出现的问题，可以配置 **SQLdiag** ，使其自动在 01:00 开始收集诊断数据，并自动在 03:00:00 停止收集。 可以使用 **/B** 和 **/E** 参数指定开始和停止时间。 请使用 24 小时制来指定确切的开始和停止日期和时间，格式为 YYYYMMDD_HH:MM:SS。 若要指定一个相对的开始或停止时间，请在开始和停止时间之前添加前缀 **+** ，并省略日期部分 (YYYYMMDD_)，如下例所示。在下例中，可使 **SQLdiag** 在等待 1 小时后开始收集信息，然后经过 3 小时的信息收集后便停止并退出：  
  
```  
sqldiag /B +01:00:00 /E +03:00:00  
```  
  
 如果指定了相对的 *start_time* ，则 **SQLdiag** 将会在与当前日期和时间相对的某个时间启动。 如果指定了相对的 *end_time* ，则 **SQLdiag** 将会在与指定的 *start_time* 相对的某个时间结束。 如果指定的开始或结束日期和时间均为过去的时间，则 **SQLdiag** 将会强行更改开始日期，以使开始日期和时间为将来的时间。  
  
 这对所选的开始和结束日期具有重要的意义。 请考虑以下示例：  
  
```  
sqldiag /B +01:00:00 /E 08:30:00  
```  
  
 如果当前时间为 08:00，则在诊断收集操作实际开始之前，结束时间已经过去。 因为当开始和结束日期为过去的时间时， **SQLDiag** 会自动将开始和结束日期调整为下一天，所以，该示例中的诊断收集将在当天的 09:00（已使用 **+** 指定的一个相对开始时间）开始，一直到第二天早上的 08:30 才结束。  
  
### <a name="stopping-and-restarting-sqldiag-to-collect-daily-diagnostics"></a>停止并重新启动 SQLdiag 以收集每天的诊断信息  
 若要 **SQLdiag** 每天收集一组指定的诊断信息而无需手动启动和停止，可使用 **/L** 参数。 **/L** 参数可以使 **SQLdiag** 在预定时间关闭后自动重新启动，从而实现连续运行。 指定了 **/L** 之后，如果 **SQLdiag** 因到达 **/E** 参数指定的结束时间而停止，或者因使用 **/X** 参数在快照模式下运行而停止，则 **SQLdiag** 将会重新启动，而不是退出。  
  
 在以下示例中，指定了 **SQLdiag** 以连续模式运行，在 03:00:00 和 05:00:00 之间收集诊断数据后自动重新启动。  
  
```  
sqldiag /B 03:00:00 /E 05:00:00 /L  
```  
  
 在以下示例中，指定了 **SQLdiag** 以连续模式运行，在 03:00:00 拍摄诊断数据快照后自动重新启动。  
  
```  
sqldiag /B 03:00:00 /X /L  
```  
  
## <a name="running-sqldiag-as-a-service"></a>将 SQLdiag 作为服务运行  
 如果希望使用 **SQLdiag** 长时间收集诊断数据，而在该时间段内可能需要注销运行 **SQLdiag** 的计算机，则可将其作为服务运行。  
  
 **将 SQLDiag 注册为作为服务运行**  
  
 通过在命令行指定 **/R** 参数，可以将 **SQLdiag** 注册为作为服务运行。 这会将 **SQLdiag** 注册为作为服务运行。 **SQLdiag** 服务名为 SQLDIAG。 将 **SQLDiag** 注册为服务时在命令行上指定的任何其他参数将被保留，以供启动服务时重新使用。  
  
 若要更改默认的 SQLDIAG 服务名称，请使用 **/A** 命令行参数来指定其他名称。 **SQLdiag** 会自动为使用 **/A** 指定的任意 **SQLdiag** 实例名称添加前缀 DIAG$，以创建有合理的服务名称。  
  
 **撤消 SQLDIAG 服务注册**  
  
 若要撤消服务注册，请指定 **/U** 参数。 撤消 **SQLdiag** 的服务注册将同时删除该服务的 Windows 注册表项。  
  
 **启动或重新启动 SQLDIAG 服务**  
  
 若要启动或重新启动 SQLDIAG 服务，请从命令行运行 **SQLDiag START** 。  
  
 如果使用 **/A** 参数运行多个 **SQLdiag** 实例，则启动服务时也可以在命令行中传递 **SQLdiag** 实例名。 例如，若要启动名为 Instance1 的 **SQLdiag** 实例，请使用以下语法：  
  
```  
SQLDIAG START /A Instance1  
```  
  
 也可使用 **net start** 命令启动 SQLDIAG 服务。  
  
 重新启动 **SQLdiag** 后，它将覆盖当前输出文件夹中的内容。 若要避免出现这种情况，请在命令行中指定 **/N 2** ，以在实用工具启动时重命名输出文件夹。  
  
 不支持暂停 **SQLdiag** 服务。  
  
## <a name="running-multiple-instances-of-sqldiag"></a>运行多个 SQLdiag 实例  
 通过在命令行中指定 /ASQLdiag\_application\_name，可以在同一台计算机上运行多个 SQLdiag 实例 。 这对于同时从同一个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例收集不同诊断信息集的操作会很有用。 例如，可以将 **SQLdiag** 命名实例配置为连续执行轻型数据收集。 然后，如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]中出现特定问题，则可以运行默认 **SQLdiag** 实例以收集该问题的诊断信息，也可以收集 [!INCLUDE[msCoName](../includes/msconame-md.md)] 客户支持服务部门要求你收集用以诊断问题的诊断信息集。  
  
## <a name="collecting-diagnostic-data-from-clustered-sql-server-instances"></a>从群集 SQL Server 实例中收集诊断数据  
 **SQLdiag** 支持从群集 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例中收集诊断数据。 要从群集 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例中收集诊断信息，请确保已在配置文件 SQLDiag.Xml 中为 \<Machine> 元素的 name 属性指定了 "."，并且命令行中未指定 /G 参数。 默认情况下，将在配置文件中为 **name** 属性指定 **"."** ，并禁用 **/G** 参数。 通常，在从群集 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例中收集诊断信息时，无需编辑配置文件或更改命令行参数。  
  
 如果将 **"."** 指定为计算机名称，则 **SQLdiag** 会检测到它正在群集中运行，同时从该群集中安装的所有虚拟 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例中检索诊断信息。 如果只希望从计算机上运行的其中一个虚拟 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例中收集诊断信息，请在 SQLDiag.Xml 中为 \<Machine> 元素的 name 属性指定该虚拟 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。  
  
> [!NOTE]  
>  若要从群集 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] 实例中收集 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 跟踪信息，必须在群集中启用管理共享 (ADMIN$)。  
  
## <a name="see-also"></a>另请参阅  
 [命令提示实用工具参考（数据库引擎）](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
