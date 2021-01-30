---
description: sp_trace_create (Transact-SQL)
title: sp_trace_create (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_create_TSQL
- sp_trace_create
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_create
ms.assetid: f3a43597-4c5a-4520-bcab-becdbbf81d2e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c63a724ad0b82a1a11145cc559ac52fc2d6f9cc3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200617"
---
# <a name="sp_trace_create-transact-sql"></a>sp_trace_create (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建跟踪定义。 新的跟踪将处于停止状态。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用扩展事件。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_trace_create [ @traceid = ] trace_id OUTPUT   
          , [ @options = ] option_value   
          , [ @tracefile = ] 'trace_file'   
     [ , [ @maxfilesize = ] max_file_size ]  
     [ , [ @stoptime = ] 'stop_time' ]  
     [ , [ @filecount = ] 'max_rollover_files' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @traceid = ] trace_id` 为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 新的跟踪分配的数字。 用户提供的任何输入都被忽略。 *trace_id* 的值为 **int**，默认值为 NULL。 用户使用 *trace_id* 值标识、修改和控制此存储过程定义的跟踪。  
  
`[ @options = ] option_value` 指定为跟踪设置的选项。 *option_value* 为 **int**，没有默认值。 用户可以通过指定所选出选项之和来选择这些选项的组合。 例如，若要打开 TRACE_FILE_ROLLOVER 和 SHUTDOWN_ON_ERROR 的选项，请为 *option_value* 指定 **6** 。  
  
 下表列出了选项、说明和选项值。  
  
|选项名|选项值|说明|  
|-----------------|------------------|-----------------|  
|TRACE_FILE_ROLLOVER|**2**|指定在达到 *max_file_size* 时，将关闭当前跟踪文件并创建新文件。 所有新记录都将写入新文件。 新文件将与前一个文件同名，但是在文件名后将附加一个整数以指示其序列。 例如，如果命名原始跟踪文件为 filename.trc，则命名下个跟踪文件为 filename_1.trc，命名再下一个跟踪文件为 filename_2.trc，等等。<br /><br /> 随着更多滚动更新跟踪文件的创建，附加到文件名的整数值继续增加。<br /><br /> 如果在指定此选项时未指定 *max_file_size* 的值，SQL Server 使用 *MAX_FILE_SIZE* (5 MB) 的默认值。|  
|SHUTDOWN_ON_ERROR|**4**|指定无论任何原因，如果不能将跟踪写入文件，则 SQL Server 将关闭。 执行安全审核跟踪时，该选项很有用。|  
|TRACE_PRODUCE_BLACKBOX|**8**|指定服务器产生的最后 5 MB 跟踪信息记录将由服务器保存。 TRACE_PRODUCE_BLACKBOX 与所有其他选项不兼容。|  
  
`[ @tracefile = ] 'trace_file'` 指定跟踪将写入的位置和文件名。 *trace_file* 为 **nvarchar (245)** ，无默认值。 *trace_file* 可以是本地目录 (例如 n ' C:\MSSQL\Trace\trace.trc ' ) ，也可以是共享或路径 (n ' \\ \\ *Servername* \\ *共享* \\ *目录*\trace.trc ' ) 的 UNC。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将 **trace.dat.trc** 扩展名追加到所有跟踪文件名。 如果指定了 TRACE_FILE_ROLLOVER 选项和 *max_file_size* ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 则当原始跟踪文件增长到其最大大小时，将创建一个新的跟踪文件。 新文件具有与原始文件相同的名称，但追加了 _ *n* 来指示其序列（从 **1** 开始）。 例如，如果第一个跟踪文件的名称为 **trace.dat.trc**，则第二个跟踪文件将命名为 **filename_1. trace.dat.trc**。  
  
 如果您使用 TRACE_FILE_ROLLOVER 选项，我们建议您在原始跟踪文件名中不要使用下划线字符。 如果您使用了下划线，则会发生以下行为：  
  
-   [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 如果) 中配置了两个文件滚动更新选项，则不会自动加载或提示您加载滚动更新文件 (。  
  
-   使用 *number_files* 参数时，fn_trace_gettable 函数不会加载滚动更新文件 () 其中，原始文件名以下划线和数值结尾。 （这不适用于在文件滚动更新时自动追加的下划线和数字。）  
  
> [!NOTE]  
>  作为对这两个行为的解决方法，您可以重命名这些文件以便删除原始文件名中的下划线。 例如，如果原始文件的名称为 **my_trace trace.dat.trc**，并且滚动更新文件的 my_trace_1 名称为 **trace.dat.trc**，则在中打开文件之前，您可以将文件重命名为 **mytrace** **mytrace_1 和 trace.dat.trc** 。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]  
  
 使用 TRACE_PRODUCE_BLACKBOX 选项时，不能指定 *trace_file* 。  
  
`[ @maxfilesize = ] max_file_size` 指定跟踪文件可以增长)  (MB 的最大大小（MB）。 *max_file_size* 为 **bigint**，默认值为 **5**。  
  
 如果在没有 TRACE_FILE_ROLLOVER 选项的情况下指定了此参数，则当使用的磁盘空间超过 *max_file_size* 指定的数量时，跟踪将停止记录到文件中。  
  
`[ @stoptime = ] 'stop_time'` 指定停止跟踪的日期和时间。 *stop_time* 为 **datetime**，默认值为 NULL。 如果为 NULL，该跟踪将一直运行，直到它被手动停止或服务器关闭。  
  
 如果同时指定了 *stop_time* 和 *max_file_size* ，并且未指定 TRACE_FILE_ROLLOVER，则在达到指定的停止时间或最大文件大小时，跟踪将会超出。 如果指定 *stop_time*、 *max_file_size* 和 TRACE_FILE_ROLLOVER，则跟踪会在指定的停止时间停止，前提是跟踪未填满驱动器。  
  
`[ @filecount = ] 'max_rollover_files'` 指定要用相同的基准文件名维护的最大数目或跟踪文件。 *max_rollover_files* 为 **int**，大于1。 此参数仅在指定了 TRACE_FILE_ROLLOVER 选项时有效。 指定 *max_rollover_files* 时，将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 打开新的跟踪文件前删除最旧的跟踪文件，以保持不超过 *max_rollover_files* 的跟踪文件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过向基准文件名追加数字来跟踪跟踪文件的新旧程度。  
  
 例如，将 *trace_file* 参数指定为 "c:\mytrace" 时，名为 "mytrace_123 c：\ trace.dat.trc" 的文件早于名为 "c：\ mytrace_124. trace.dat.trc" 的文件。 如果 *max_rollover_files* 设置为2，则 SQL Server 在创建跟踪文件 "mytrace_125 c：\ trace.dat.trc" 之前删除文件 "c：\ mytrace_123"。  
  
 请注意，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 一次仅尝试删除一个文件，且不能删除另一进程正在使用的文件。 因此，如果在跟踪运行时另一应用程序正在使用跟踪文件，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将这些跟踪文件保留在文件系统中。  
  
## <a name="return-code-values"></a>返回代码值  
 下表说明在存储过程完成后用户可能获得的代码值。  
  
|返回代码|描述|  
|-----------------|-----------------|  
|0|没有错误。|  
|1|未知错误。|  
|10|无效选项。 指定的选项不兼容时返回此代码。|  
|12|文件未创建。|  
|13|内存不足。 在没有足够内存执行指定的操作时返回此代码。|  
|14|无效停止时间。 在指定的停止时间已发生时返回此代码。|  
|15|参数无效。 在用户已提供不兼容的参数时返回此代码。|  
  
## <a name="remarks"></a>备注  
 **sp_trace_create** 是一种 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程，它执行以前由 **xp_trace_ \* *_ SQL Server 早期版本中提供的扩展存储过程执行的许多操作。使用 _* sp_trace_create** 而不是：  
  
-   **xp_trace_addnewqueue**  
  
-   **xp_trace_setqueuecreateinfo**  
  
-   **xp_trace_setqueuedestination**  
  
 **sp_trace_create** 仅创建跟踪定义。 该存储过程不能用于启动或更改跟踪。  
  
 严格类型化)  (**sp_trace_xx** 的所有 SQL 跟踪存储过程的参数。 如果没有用正确的输入参数数据类型（参数说明中指定的类型）来调用这些参数，则存储过程将返回错误。  
  
 对于 **sp_trace_create**， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户必须对跟踪文件文件夹具有写入权限。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户不是跟踪文件所在计算机上的管理员，则必须将写入权限显式授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户。  
  
> [!NOTE]  
>  可以通过使用 **fn_trace_gettable** 系统函数，将使用 **sp_trace_create** 创建的跟踪文件自动加载到表中。 有关如何使用此系统函数的信息，请参阅 [&#40;transact-sql&#41;sys.fn_trace_gettable ](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md)。  
  
 有关使用跟踪存储过程的示例，请参阅[创建跟踪 (Transact-SQL)](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)。  
  
 **TRACE_PRODUCE_BLACKBOX** 具有以下特征：  
  
-   它属于滚动更新跟踪。 默认 *file_count* 为2，但用户可以使用 *filecount* 选项重写此默认值。  
  
-   与其他跟踪一样，默认 *file_size* 是 5 MB，可以更改。  
  
-   不能指定文件名。 文件将保存为： **N '%SQLDIR%\MSSQL\DATA\blackbox.trc '**  
  
-   跟踪中仅包含以下事件和它们的列：  
  
    -   **RPC starting**  
  
    -   **Batch starting**  
  
    -   **Exception**  
  
    -   **注重**  
  
-   无法在此跟踪中添加或删除事件或列。  
  
-   不能为此跟踪指定筛选器。  
  
## <a name="permissions"></a>权限  
 用户必须拥有 ALTER TRACE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_generateevent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [SQL 跟踪](../../relational-databases/sql-trace/sql-trace.md)  
  
  
