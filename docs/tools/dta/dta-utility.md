---
title: dta 实用工具
description: dta 实用工具是数据库引擎优化顾问的命令提示符版本，借助它，可以在应用程序和脚本中使用功能。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- physical design structures [SQL Server]
- command prompt utilities [SQL Server], dta
- dta utility
- tuning databases [SQL Server], Database Engine Tuning Advisor
- workloads [SQL Server], analyzing
- dta utility, about dta utility
- performance [SQL Server], Database Engine Tuning Advisor
- Database Engine Tuning Advisor [SQL Server], command prompt
- optimizing databases [SQL Server]
ms.assetid: a0b210ce-9b58-4709-80cb-9363b68a1f5a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/09/2017
ms.openlocfilehash: a80c3fc49962ad4a67430163f81d00b37c05329b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169215"
---
# <a name="dta-utility"></a>dta 实用工具

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

**dta** 实用工具是数据库引擎优化顾问的命令提示符版。 通过 **dta** 实用工具，您可以在应用程序和脚本中使用数据库引擎优化顾问功能。  

与数据库引擎优化顾问一样， **dta** 实用工具可以分析工作负荷，并可为该工作负荷推荐可改进服务器性能的物理设计结构。 工作负荷可以是计划缓存、 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 跟踪文件或跟踪表，也可以是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。 物理设计结构包括索引、索引视图和分区。 分析了工作负荷后， **dta** 实用工具将生成数据库物理设计结构建议，并可生成实现该建议所需的脚本。 可以在命令提示符处，使用 **-if** 或 **-it** 参数指定工作负荷。 也可以在命令提示符处，使用 **-ix** 参数指定 XML 输入文件。 在这种情况下，在 XML 输入文件中指定工作负荷。  
  
## <a name="syntax"></a>语法  
  
```  
  
dta  
[ -? ] |  
[  
      [ -S server_name[ \instance ] ]  
      { { -U login_id [-P password ] } | -E  }  
      { -D database_name [ ,...n ] }  
      [ -d database_name ]   
      [ -Tl table_list | -Tf table_list_file ]  
      { -if workload_file | -it workload_trace_table_name  |   
        -ip | -iq }  
      { -ssession_name | -IDsession_ID }  
      [ -F ]  
      [ -of output_script_file_name ]  
      [ -or output_xml_report_file_name ]  
      [ -ox output_XML_file_name ]  
      [ -rl analysis_report_list [ ,...n ] ]  
      [ -ix input_XML_file_name ]  
      [ -A time_for_tuning_in_minutes ]  
      [ -n number_of_events ]
      [ -I time_window_in_hours ]  
      [ -m minimum_improvement ]  
      [ -fa physical_design_structures_to_add ]  
      [ -fi filtered_indexes]  
      [ -fc columnstore_indexes]  
      [ -fp partitioning_strategy ]  
      [ -fk keep_existing_option ]  
      [ -fx drop_only_mode ]  
      [ -B storage_size ]  
      [ -c max_key_columns_in_index ]  
      [ -C max_columns_in_index ]  
      [ -e | -e tuning_log_name ]  
      [ -N online_option]  
      [ -q ]  
      [ -u ]  
      [ -x ]  
      [ -a ]  
]  
```  
  
## <a name="arguments"></a>参数  
 **-?**  
 显示用法信息。  
  
 **-A** _time_for_tuning_in_minutes_  
 指定优化时间限制（分钟）。 **dta** 使用指定的时间优化工作负荷，并生成一个包含建议的物理设计更改的脚本。 默认情况下， **dta** 采用的优化时间为 8 小时。 指定 0 允许不限制优化时间。 **dta** 可能在限制时间之前完成整个工作负荷的优化。 但是，为确保整个工作负荷都得到优化，我们建议您指定不受限制的优化时间 (-A 0)。  
  
 **-a**  
 优化工作负荷和应用建议时不作提示。  
  
 **-B** _storage_size_  
 指定推荐的索引和分区可以使用的最大空间 (MB)。 优化多个数据库时，建议对所有数据库都进行空间计算。 默认情况下， **dta** 采用下列存储大小中较小的一个：  
  
-   当前原始数据大小的三倍，包括数据库中表的堆和聚集索引的总大小。  
  
-   所有已附连磁盘驱动器的可用空间加上原始数据的大小。  
  
 默认存储大小不包括非聚集索引和索引视图。  
  
 **-C** _max_columns_in_index_  
 指定 **dta** 推荐的索引中的最大列数。 最大值为 1024。 默认情况下，此参数设置为 16。  
  
 **-c** _max_key_columns_in_index_  
 指定 **dta** 推荐的索引中的最大键列数。 默认值为 16，允许该最大值。 **dta** 也会考虑创建带有包含列的索引。 建议的包含列的索引可能会超出此参数中指定的列数。  
  
 **-D** _database_name_  
 指定要优化的每个数据库的名称。 第一个数据库是默认数据库。 可以指定多个数据库，各数据库名称用逗号进行分隔。例如：  
  
```  
dta -D database_name1, database_name2...  
```  
  
 此外，也可以对各个数据库名称使用 -D  参数，从而指定多个数据库。例如：  
  
```  
dta -D database_name1 -D database_name2... n  
```  
  
 **-D** 参数是必需的。 如果尚未指定 **-d** 参数，则 **dta** 将默认连接到工作负荷中第一个 `USE database_name` 子句指定的数据库。 如果工作负荷中没有显式的 `USE database_name` 子句，则必须使用 **-d** 参数。  
  
 例如，如果有一个工作负荷未包含显式的 `USE database_name` 子句，则在使用以下 **dta** 命令时将不会生成建议：  
  
```  
dta -D db_name1, db_name2...  
```  
  
 但是，如果使用同一工作负荷时使用了以下带 **-d** 参数的 **dta** 命令，则将生成建议：  
  
```  
dta -D db_name1, db_name2 -d db_name1  
```  
  
 **-d** _database_name_  
 指定优化工作负荷时 **dta** 连接到的第一个数据库。 只能为此参数指定一个数据库。 例如：  
  
```  
dta -d AdventureWorks2012 ...  
```  
  
 如果指定多个数据库名称， **dta** 将返回错误。 **-d** 参数是可选的。  
  
 如果使用 XML 输入文件，则可使用 **TuningOptions** 元素下的 **DatabaseToConnect** 元素来指定 **dta** 连接的第一个数据库。 有关详细信息，请参阅 [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)。  
  
 如果只优化一个数据库，则 **-d** 参数的功能将类似于 **sqlcmd** 实用工具中的 **-d** 参数的功能，但不执行 USE *database_name* 语句。 有关详细信息，请参阅 [sqlcmd Utility](../../tools/sqlcmd-utility.md)。  
  
 **-E**  
 使用可信连接而不请求密码。 必须使用指定登录 ID 的 **-E** 参数或 **-U** 参数。  
  
 **-e** _tuning_log_name_  
 指定 **dta** 记录其无法优化的事件的表或文件的名称。 表的创建位置是执行优化的服务器。  
  
 如果使用表，则用以下格式指定其名称： *[database_name].[owner_name].table_name*。 下表显示了每个参数的默认值：  
  
|参数|默认值|详细信息|  
|---------------|-------------------|-------------|  
|*database_name*|使用 -D  选项指定的 database_name ||  
|*owner_name*|**dbo**|*owner_name* 必须为 **dbo**。 如果指定了其他值，则 **dta** 执行将失败并返回错误。|  
|*table_name*|无||  
  
 如果使用文件，则指定 .xml 作为其扩展名。 例如，TuningLog.xml。  
  
> [!NOTE]  
>  如果删除该会话，则 **dta** 实用工具不会删除用户指定的优化日志表的内容。 优化超大型工作负荷时，建议为优化日志指定一个表。 由于优化大型工作负荷可导致优化日志增大，因此使用表时删除会话的速度可快得多。  
  
 **-F**  
 允许 **dta** 覆盖现有的输出文件。 如果已经存在同名输出文件，并且没有指定 **-F** ，则 **dta** 将返回错误。 你可以使用具有 **-of** 、 **-or** 或 **-ox** 的 **-F**。  
  
 **-fa** _physical_design_structures_to_add_  
 指定 **dta** 应在建议中包括的物理设计结构的类型。 下表列出并说明了可为此参数指定的值。 未指定任何值时，dta  将使用默认值 -fa IDX  。  
  
|值|说明|  
|-----------|-----------------|  
|IDX_IV|索引和索引视图。|  
|IDX|仅限索引。|  
|IV|仅限索引视图。|  
|NCL_IDX|仅限非聚集索引。|  
  
 **-fi**  
 指定将在新建议中考虑筛选的索引。 有关详细信息，请参阅 [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md)。  
  
**-fc**  
 指定将在新建议中考虑的列存储索引。 DTA 将考虑聚集列存储索引和非聚集列存储索引。 有关详细信息，请参阅    
[数据引擎优化顾问 (DTA) 中的列存储索引建议](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md)。
 ||  
|-|  
|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本。|  

  
 **-fk** _keep_existing_option_  
 指定 **dta** 在生成其建议时必须保留的现有物理设计结构。 下表列出并介绍了可以为此参数指定的值：  
  
|值|说明|  
|-----------|-----------------|  
|无|无现有结构|  
|ALL|所有现有结构|  
|ALIGNED|所有分区对齐结构。|  
|CL_IDX|表中的所有聚集索引|  
|IDX|表中的所有聚集索引和非聚集索引|  
  
 **-fp** _partitioning_strategy_  
 指定 **dta** 建议的新物理设计结构（索引和索引视图）是否应进行分区以及如何进行分区。 下表列出并介绍了可以为此参数指定的值：  
  
|值|说明|  
|-----------|-----------------|  
|无|不分区|  
|FULL|完全分区（选择该值可增强性能）|  
|ALIGNED|仅限对齐分区（选择该值可增强可管理性）|  
  
 ALIGNED 表示在 **dta** 生成的建议中，每个建议的索引都完全按定义该索引的基础表所用的方式进行分区。 索引视图中的非聚集索引与索引视图对齐。 只能为此参数指定一个值。 默认值为 -fp NONE  。  
  
 **-fx** _drop_only_mode_  
 指定 **dta** 仅考虑删除现有物理设计结构。 没有考虑任何新的物理设计结构。 如果指定此选项， **dta** 将评估现有物理设计结构的使用情况，并建议删除很少使用的结构。 此参数不带任何值， 它不能与 **-fa**、 **-fp** 或 **-fk ALL** 参数一起使用。  
  
 **-ID** _session_ID_  
 为优化会话指定一个数字标识符。 如果未指定，则 **dta** 将生成一个 ID 号。 可以使用此标识符查看现有优化会话的信息。 如果不指定 **-ID** 值，则必须用 **-s** 指定会话名。  
  
 **-ip**  
 指定计划高速缓存可用作工作负荷。 分析显式选择的数据库的前 1000 个计划缓存事件。 可使用 -n  选项更改此值。  

**-iq**  
 指定查询存储可用作工作负载。 对于显式选择的数据库，将分析查询存储中的前 1,000 个事件。 可使用 -n  选项更改此值。  有关详细信息，请参阅[查询存储](../../relational-databases/performance/how-query-store-collects-data.md)和[使用查询存储中的工作负荷优化数据库](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md)。
 ||  
|-|  
|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本。|  
     
 **-if** _workload_file_  
 指定用作优化输入的工作负荷文件的路径和文件名。 该文件必须采用下列格式之一：.trc（SQL Server Profiler 跟踪文件）、.sql（SQL 文件）或 .log（[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 跟踪文件）。 必须指定一个工作负荷文件或一个工作负荷表。  
  
 **-it** _workload_trace_table_name_  
 指定包含用于优化的工作负荷跟踪的表名。 如果使用表，则用以下格式指定其名称：[*database_name*] **.** [*owner_name*] **.** _table_name_。  
  
 下表显示了每个参数的默认值：  
  
|参数|默认值|  
|---------------|-------------------|  
|*database_name*|使用 -D  选项指定的 database_name |  
|*owner_name*|**dbo**。|  
|*table_name*|无。|  
  
> [!NOTE]  
>  *owner_name* 必须为 **dbo**。 如果指定了其他任何值，则 **dta** 执行将失败并返回错误。 另请注意，必须指定一个工作负荷表或一个工作负荷文件。  
  
 **-ix** _input_XML_file_name_  
 指定包含 **dta** 输入信息的 XML 文件的名称。 该文件必须是符合 DTASchema.xsd 的有效 XML 文档。 在命令提示符中指定的优化选项参数将覆盖与其冲突的此 XML 文件中的相应值。 唯一的例外情况是：在计算模式下，在 XML 输入文件中输入用户指定的配置。 例如，如果在 XML 输入文件的 **Configuration** 元素中输入了配置，并将 **EvaluateConfiguration** 元素指定为优化选项之一，则 XML 输入文件中指定的优化选项将覆盖命令提示符中输入的任何优化选项。  

 **-k** _maxtotalindexes_  
 建议中的索引数上限。  

 **-k** _maxtotalindexes_  
 每表的索引数上限。

 **-m** _minimum_improvement_  
 指定推荐的配置必须满足的最小改进百分比。  
  
 **-N** _online_option_  
 指定是否联机创建物理设计结构。 下表列出并说明了可为此参数指定的值：  
  
|值|说明|  
|-----------|-----------------|  
|OFF|建议的物理设计结构都无法联机创建。|  
|ON|所有建议的物理设计结构都可以联机创建。|  
|MIXED|数据库引擎优化顾问会尝试建议可以联机创建的物理设计结构。|  
  
 如果索引是联机创建的，则在其对象定义中追加 ONLINE = ON。  
  
 **-n** _number_of_events_  
 指定 **dta** 应在工作负荷中优化的事件数。 如果指定此参数，并且工作负荷是包含持续时间信息的跟踪文件，则 **dta** 将按持续时间的降序优化事件。 此参数可用于比较物理设计结构的两个配置。 若要比较两个配置，请为这两个配置指定相同的要优化的事件数，再为这两个配置指定不受限制的优化时间。如下所示：  
  
```  
dta -n number_of_events -A 0  
```  
  
 在此示例中，必须指定不受限制的优化时间 (`-A 0`)。 否则，数据库引擎优化顾问将采用默认的 8 小时优化时间。
 
 **-I** _time_window_in_hours_   
   指定在使用 -iq  选项（查询存储中的工作负载）时，必须执行某个查询，以便 DTA 考虑优化它的时间窗口（以小时为单位）。 
```  
dta -iq -I 48  
```  
在此示例中，DTA 将查询存储用作工作负载源，只考虑在过去 48 小时内执行的查询。  
  ||  
|-|  
|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本。|  


  
 **-of** _output_script_file_name_  
 指定 **dta** 使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本将建议写入指定的文件名和目标。  
  
 可以将 **-F** 与此选项一起使用。 请确保文件名是唯一的，尤其在使用了 **-or** 和 **-ox** 时更应注意。  
  
 **-or** _output_xml_report_file_name_  
 指定 **dta** 使用 XML 将建议写入输出报告。 如果提供了文件名，建议就会被写入该文件名。 否则， **dta** 将使用会话名生成文件名，并将该文件名写入当前目录。  
  
 可以将 **-F** 与此选项一起使用。 请确保文件名是唯一的，尤其在使用了 **-of** 和 **-ox** 时更应注意。  
  
 **-ox** _output_XML_file_name_  
 指定 **dta** 使用 XML 文件将建议写入指定的文件名和目标。 请确保数据库引擎优化顾问有写入目标目录的权限。  
  
 可以将 **-F** 与此选项一起使用。 请确保文件名是唯一的，尤其在使用了 **-of** 和 **-or** 时更应注意。  
  
 **-P** _password_  
 指定登录 ID 的密码。 如果未使用此选项， **dta** 将提示输入密码。  
  
 **-q**  
 设置静默模式。 不向控制台写入信息，其中包括进度和标题信息。  
  
 **-rl** _analysis_report_list_  
 指定要生成的分析报告列表。 下表列出并说明了可为此参数指定的值：  
  
|值|报表|  
|-----------|------------|  
|ALL|所有分析报告|  
|STMT_COST|语句开销报告|  
|EVT_FREQ|事件频率报告|  
|STMT_DET|语句详细报告|  
|CUR_STMT_IDX|语句-索引关系报告（当前配置）|  
|REC_STMT_IDX|语句-索引关系报告（建议配置）|  
|STMT_COSTRANGE|语句开销范围报告|  
|CUR_IDX_USAGE|索引使用报告（当前配置）|  
|REC_IDX_USAGE|索引使用报告（建议配置）|  
|CUR_IDX_DET|索引详细报告（当前配置）|  
|REC_IDX_DET|索引详细报告（建议配置）|  
|VIW_TAB|视图-表关系报告|  
|WKLD_ANL|工作负荷分析报告|  
|DB_ACCESS|数据库访问报告|  
|TAB_ACCESS|表访问报告|  
|COL_ACCESS|列访问报告|  
  
 指定多个报告时，用逗号分隔各个值。例如：  
  
```  
... -rl EVT_FREQ, VIW_TAB, WKLD_ANL ...  
```  
  
 **-S** _server_name_[ *\instance*]  
 指定要连接到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机和实例的名称。 如果未指定 *server_name* ，则 **dta** 将连接到本地计算机上的默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 如果连接到命名实例或从网络中的远程计算机执行 **dta** ，则必须使用此选项。  
  
 **-s** _session_name_  
 指定优化会话的名称。 如果未指定 **-ID** ，则必须使用此选项。  
  
 **-Tf** _table_list_file_  
 指定包含要优化的一组表的文件名。 文件中列出的每个表应另起一行。 表名的命名应限定为三个部分，例如， **AdventureWorks2012.HumanResources.Department**。 为了调用表的扩展功能，可以选择在现有表名后加上一个数字，指示表中的预定行数。 数据库引擎优化顾问在优化或评估引用这些表的工作负荷中的语句时，将考虑这些预定行数。 请注意， *number_of_rows* 计数和 *table_name* 之间可以有一个或多个空格。  
  
 以下是 *table_list_file* 的文件格式：  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 此参数是在命令提示符中输入表列表 ( **-Tl**) 的替代方式。 如果使用了 **-Tl**，请不要使用表列表文件 ( **-Tf**)。 如果同时使用这两个参数， **dta** 将失败并返回错误。  
  
 如果省略 **-Tf** 和 **-Tl** 参数，则将考虑对指定数据库中的所有用户表进行优化。  
  
 **-Tl** _table_list_  
 在命令提示符中指定要优化的一组表。 各表名间用逗号分隔。 如果使用 **-D** 参数只指定一个数据库，则无需使用数据库名限定表名。 在其他情况下，使用以下格式的完全限定名： *database_name.schema_name.table_name* ，每个表都必须使用此格式。  
  
 此参数是使用表列表文件 ( **-Tf**) 的替代方法。 如果同时使用 **-Tl** 和 **-Tf** ，则 **dta** 将失败并返回错误。  
  
 **-U** _login_id_  
 指定用于连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的登录 ID。  
  
 **-u**  
 启动数据库引擎优化顾问 GUI。 所有参数均被视为用户界面的初始设置。  
  
 **-x**  
 启动优化会话，然后退出。  
  
## <a name="remarks"></a>备注  
 按 Ctrl+C 一次可停止优化会话，并可根据 **dta** 此时已完成的分析生成建议。 系统将提示您确定是否要生成建议。 再次按 Ctrl+C 停止优化会话，而不生成建议。  
  
## <a name="examples"></a>示例  
 **A.优化在其建议中包含索引和索引视图的工作负荷**  
  
 此示例使用安全连接 (`-E`) 连接到 MyServer 中的 **tpcd1G** 数据库，以分析工作负荷并创建建议。 此示例将输出写入到名为 script.sql 的脚本文件。 如果 script.sql 已存在，由于指定了 **参数，** dta `-F` 将覆盖该文件。 优化会话的运行时间没有限制，以确保完成工作负荷分析 (`-A 0`)。 建议必须至少提供 5% 的改进 (`-m 5`)。 **dta** 应在其最终建议中包含索引和索引视图 (`-fa IDX_IV`)。  
  
```  
dta -S MyServer -E -D tpcd1G -if tpcd_22.sql -F -of script.sql -A 0 -m 5 -fa IDX_IV  
```  
  
 **B.限制磁盘使用**  
  
 此示例将数据库总大小（包括原始数据和其他索引）限定为 3 GB (`-B 3000`)，并将输出定向到 d:\result_dir\script1.sql。 该示例的运行时间不会超过 1 小时 (`-A 60`)。  
  
```  
dta -D tpcd1G -if tpcd_22.sql -B 3000 -of "d:\result_dir\script1.sql" -A 60  
```  
  
 **C.限制优化查询的数量**  
  
 此示例将从 orders_wkld.sql 文件读取的最大查询数限定为 10 (`-n 10`)，并将运行时间设为 15 分钟 (`-A 15`)，以先完成的为准。 要确保所有 10 个查询都得到优化，请用 `-A 0` 指定不受限制的优化时间。 如果时间紧迫，则通过用 `-A` 参数指定用于优化的分钟数来指定适当的时间限制，如本例所示。  
  
```  
dta -D orders -if orders_wkld.sql -of script.sql -A 15 -n 10  
```  
  
 **D.优化文件中列出的特定表**  

 该示例演示了 *table_list_file* （ **-Tf** 参数）的用法。 table_list.txt 文件的内容如下所示：  

```
AdventureWorks2012.Sales.Customer  100000  
AdventureWorks2012.Sales.Store  
AdventureWorks2012.Production.Product  2000000  
```

table_list.txt 的内容指定：  

- 应当仅优化数据库中的 **Customer**、 **Store** 和 **Product** 表。  
  
- **Customer** 和 **Product** 表中的行数分别假定为 100,000 和 2,000,000。  
  
- **Store** 中的行数假定为表中的当前行数。  

    请注意， *table_list_file* 中的行数计数和前面的表名间可以有一个或多个空格。  
    
    优化时间为 2 小时 (`-A 120`)，输出将写入 XML 文件 (`-ox XMLTune.xml`)。  

``` 
dta -D pubs -if pubs_wkld.sql -ox XMLTune.xml -A 120 -Tf table_list.txt  
``` 

## <a name="see-also"></a>另请参阅

- [命令提示实用工具参考（数据库引擎）](../../tools/command-prompt-utility-reference-database-engine.md)
- [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)