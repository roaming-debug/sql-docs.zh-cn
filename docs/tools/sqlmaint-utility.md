---
title: sqlmaint 实用工具
description: 在 SQL Server 中，使用 sqlmaint 可以运行 DBCC 检查、备份数据库及其事务日志、更新统计信息以及重新生成索引。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- database maintenance plans [SQL Server]
- sqlmaint utility
- maintaining databases [SQL Server]
- backups [SQL Server], sqlmaint utility
- command prompt utilities [SQL Server], sqlmaint
- maintenance plans [SQL Server], command prompt
- backing up [SQL Server], sqlmaint utility
ms.assetid: 937a9932-4aed-464b-b97a-a5acfe6a50de
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c822bd2acbd92bcfd9b3008c78c3ccef244f3c88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338552"
---
# <a name="sqlmaint-utility"></a>sqlmaint 实用工具
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  如果成功运行，则 **sqlmaint** 实用工具可以对一个或多个数据库执行一组指定的维护操作。 使用 **sqlmaint** 可以运行 DBCC 检查、备份数据库及其事务日志、更新统计信息以及重新生成索引。 所有数据库维护活动都会生成报表，可以将此报表发送到指定的文本文件、HTML 文件或电子邮件帐户。 **sqlmaint** 可以执行使用早期版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]创建的数据库维护计划。 若要从命令提示符运行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 维护计划，请使用 [dtexec 实用工具](../integration-services/packages/dtexec-utility.md)。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../includes/ssnotedepnextavoid-md.md)] 将使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 维护计划功能代替此实用工具。 有关维护计划的详细信息，请参阅 [维护计划](../relational-databases/maintenance-plans/maintenance-plans.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqlmaint   
[-?] |  
[  
     [-S server_name[\instance_name]]  
     [-U login_ID [-P password]]  
     {  
          [-D database_name | -PlanName name | -PlanID guid ]  
          [-Rpt text_file]  
          [-To operator_name]  
          [-HtmlRpt html_file [-DelHtmlRpt <time_period>] ]  
          [-RmUnusedSpace threshold_percentfree_percent]  
          [-CkDB | -CkDBNoIdx]  
          [-CkAl | -CkAlNoIdx]  
          [-CkCat]  
          [-UpdOptiStats sample_percent]  
          [-RebldIdx free_space]  
          [-SupportComputedColumn]  
          [-WriteHistory]  
          [  
               {-BkUpDB [backup_path] | -BkUpLog [backup_path] }  
               {-BkUpMedia  
                    {DISK [  
                           [-DelBkUps <time_period>]   
                           [-CrBkSubDir ]   
                           [-UseDefDir ]   
                          ]  
                     | TAPE   
                    }  
               }  
               [-BkUpOnlyIfClean]  
               [-VrfyBackup]  
          ]  
     }  
]  
<time_period> ::=  
number[minutes | hours | days | weeks | months]  
```  
  
## <a name="arguments"></a>参数  
 参数与其值之间必须用一个空格分隔。 例如，在 **-S** 和 *server_name* 之间必须留一个空格。  
  
 **-?**  
 指定返回 **sqlmaint** 的语法关系图。 此参数必须单独使用。  
  
 **-S** _server_name_[ **\\** _instance\_name_]  
 指定 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的目标实例。 指定要连接到该服务器上 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 的默认实例的 _server\_name_。 指定要连接到该服务器上 [!INCLUDE[ssDE](../includes/ssde-md.md)] 的命名实例的 _server\_name_\\_instance\_name_。 如果未指定服务器， **sqlmaint** 将连接到本地计算机上的 [!INCLUDE[ssDE](../includes/ssde-md.md)] 默认实例。  
  
 **-U** _login_ID_  
 指定连接服务器时使用的登录 ID。 如果未提供， **sqlmaint** 将尝试使用 [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows 身份验证。 如果 *login_ID* 包含特殊字符，则必须用双引号 (") 引起来；否则，双引号为可选。  
  
> [!IMPORTANT]  
>  请尽可能使用 Windows 身份验证。  
  
 **-P** _password_  
 指定登录 ID 的密码。 仅在同时提供 **-U** 参数时有效。 如果 *password* 包含特殊字符，则必须用双引号引起来；否则，双引号为可选。  
  
> [!IMPORTANT]  
>  密码不屏蔽。 请尽可能使用 Windows 身份验证。  
  
 **-D** _database_name_  
 指定要在其中执行维护操作的数据库的名称。 如果 *database_name* 包含特殊字符，则必须用双引号引起来；否则，双引号为可选。  
  
 **-PlanName** _name_  
 指定使用数据库维护计划向导定义的数据库维护计划的名称。 **sqlmaint** 仅使用该计划中的数据库列表信息。 在其他 **sqlmaint** 参数中指定的任何维护活动都会应用到此数据库列表。  
  
 **-PlanID** _guid_  
 指定使用数据库维护计划向导定义的数据库维护计划的全局唯一标识符 (GUID)。 **sqlmaint** 仅使用该计划中的数据库列表信息。 在其他 **sqlmaint** 参数中指定的任何维护活动都会应用到此数据库列表。 这必须与 msdb.dbo.sysdbmaintplans 中的 plan_id 值匹配。  
  
 **-Rpt** _text_file_  
 指定包含要生成的报表的文件的完整路径和名称。 报表也可在屏幕上生成。 报表通过在文件名中添加日期来维护版本信息。 日期是按以下格式生成的：在文件名末尾句点之前使用 _ *yyyyMMddhhmm* 格式。 *yyyy* = 年， *MM* = 月， *dd* = 日， *hh* = 小时， *mm* = 分钟。  
  
 如果您在 1996 年 12 月 1 日上午 10:23 运行过该实用工具， 在 1996 年 12 月 1 日， *text_file* 的值为：  
  
```  
c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint.rpt  
```  
  
 则生成的文件名为：  
  
```  
c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint_199612011023.rpt  
```  
  
 *sqlmaint* 访问远程服务器时， **text_file** 需要完整的通用命名约定 (UNC) 文件名。  
  
 **-To**  _operator_name_  
 指定通过 SQL 邮件接收生成的报表的操作员。  
  
 **-HtmlRpt** _html_file_  
 指定 HTML 报表将生成到其中的文件的完整路径和名称。 **sqlmaint** 生成文件名的方式是在文件名中追加格式为 _ *yyyyMMddhhmm* 的字符串，这与针对 **-Rpt** 参数的文件命名方式相同。  
  
 *sqlmaint* 访问远程服务器时， **html_file** 需要完整的 UNC 文件名。  
  
 -DelHtmlRpt \<*time_period*>  
 指定报表文件创建后的时间间隔超出 \<*time_period*> 时，删除报表目录中的所有 HTML 报表。 **-DelHtmlRpt** 将查找名称符合由 *html_file* 参数生成的模式的文件。 如果 html_file 为 c:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint.htm，则 -DelHtmlRpt 将导致 sqlmaint 删除任何名称与 C:\Program Files\Microsoft SQL Server\Mssql\Backup\AdventureWorks2012_maint\*.htm 模式匹配的文件，以及早于指定 \<*time_period*> 的文件。  
  
 **-RmUnusedSpace** _threshold_percent free_percent_  
 指定从 **-D**. 指定的数据库中删除未使用的空间。 该选项仅适用于定义为自动增长的数据库。 *Threshold_percent* 指定在 **sqlmaint** 可以尝试删除未使用数据空间之前数据库必须达到的大小 (MB)。 如果数据库小于 *threshold_percent*，则不采取任何操作。 *Free_percent* 指定数据库中必须保留的未使用空间的大小，以数据库最终大小的百分比表示。 例如，如果一个 200 MB 的数据库包含 100 MB 数据，则将 *free_percent* 指定为 10 将使数据库最终大小变为 110 MB。 请注意，如果数据库小于 *free_percent* 加上数据库中数据量的大小，则数据库不会扩展。 例如，如果 108 MB 的数据库有 100 MB 数据，则将 *free_percent* 指定为 10 不会将数据库扩展为 110 MB，而是仍保持为 108 MB。  
  
 **-CkDB** |  **-CkDBNoIdx**  
 指定在 **-D** 指定的数据库中运行 DBCC CHECKDB 语句或带 NOINDEX 选项的 DBCC CHECKDB 语句。 有关详细信息，请参阅 DBCC CHECKDB。  
  
 如果 *sqlmaint* 运行时数据库正在使用，则将在 **text_file** 中写入一个警告。  
  
 **-CkAl** |  **-CkAlNoIdx**  
 指定在 **-D** 指定的数据库中运行带 NOINDEX 选项的 DBCC CHECKALLOC 语句。 有关详细信息，请参阅 [DBCC CHECKALLOC (Transact-SQL)](../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md)。  
  
 **-CkCat**  
 指定在 **-D** 指定的数据库中运行 DBCC CHECKCATALOG (Transact-SQL) 语句。 有关详细信息，请参阅 [DBCC CHECKCATALOG (Transact-SQL)](../t-sql/database-console-commands/dbcc-checkcatalog-transact-sql.md)。  
  
 **-UpdOptiStats** _sample_percent_  
 指定对数据库中的每个表运行下列语句：  
  
```  
UPDATE STATISTICS table WITH SAMPLE sample_percent PERCENT;  
```  
  
 如果表包含计算列，则在使用 **-UpdOptiStats** 时，还必须指定 **-SupportedComputedColumn** 参数。  
  
 有关详细信息，请参阅 [更新统计信息 (Transact-SQL)](../t-sql/statements/update-statistics-transact-sql.md)创建的数据库维护计划。  
  
 **-RebldIdx** _free_space_  
 指定应使用 *free_space* 百分比值作为填充因子的反数，重新生成目标数据库的表索引。 例如，如果 *free_space* 百分比是 30，则使用的填充因子为 70。 如果指定 *free_space* 百分比值为 100，则使用原始填充因子值重新生成索引。  
  
 如果索引位于计算列，则在使用 **-RebldIdx** 时，还必须指定 **-SupportComputedColumn** 参数。  
  
 **-RebldIdx**  
 对计算列使用 **sqlmaint** 运行 DBCC 维护命令时，必须指定此参数。  
  
 **-WriteHistory**  
 指定在 msdb.dbo.sysdbmaintplan_history 中为 **sqlmaint** 所执行的每个维护操作建立一个条目。 如果指定 **-PlanName** 或 **-PlanID** ，则 sysdbmaintplan_history 中的条目使用指定计划的 ID。 如果指定 **-D** ，则通过给计划 ID 赋予零值来生成 sysdbmaintplan_history 中的条目。  
  
 **-BkUpDB** [ *backup_path*] |  **-BkUpLog** [ *backup_path* ]  
 指定备份操作。 **-BkUpDb** 备份整个数据库。 **-BkUpLog** 只备份事务日志。  
  
 *backup_path* 指定备份的目录。 如果还指定了 *backup_path* ，则不需要 **backup_path** 。如果同时指定了二者，则 **backup_path** 会覆盖后者。 备份的存放地址可以是目录或磁带设备地址（例如， \\\\.\TAPE0）。 数据库备份的文件名按如下格式自动生成：  
  
```  
dbname_db_yyyyMMddhhmm.BAK  
  
```  
  
 其中  
  
-   *dbname* 是被备份的数据库的名称。  
  
-   *yyyyMMddhhmm* 为备份操作的时间，其中 *yyyy* = 年， *MM* = 月， *dd* = 日， *hh* = 小时， *mm* = 分钟。  
  
 事务备份的文件名自动使用类似的格式生成：  
  
```  
dbname_log_yyyymmddhhmm.BAK  
  
```  
  
 如果使用 **-BkUpDB** 参数，则必须同时使用 **-BkUpMedia** 参数指定介质。  
  
 **-BkUpMedia**  
 指定备份的介质类型，DISK 或 TAPE。  
  
 **DISK**  
 指定备份介质为磁盘。  
  
 **-DelBkUps**< *time_period* >  
 对于磁盘备份，指定如果创建备份后的时间间隔超出了 \<*time_period*>，则删除备份目录中的所有备份文件。  
  
 **-CrBkSubDir**  
 对于磁盘备份，指定在 [*backup_path*] 目录中创建子目录。如果同时指定了 **-UseDefDir** ，则在默认备份目录中创建子目录。 子目录的名称根据 **-D** 中指定的数据库名称生成。 **-CrBkSubDir** 提供一种简单的方法将不同数据库的所有备份放置到单独的子目录中，而无需更改 *backup_path* 参数。  
  
 **backup_path**  
 对于磁盘备份，指定在默认的备份目录中创建备份文件。 如果同时指定两者，则 **UseDefDir** 将覆盖 *backup_path* 。 在默认 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 安装中，默认备份目录为 C:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\Backup。  
  
 **TAPE**  
 指定备份介质为磁带。  
  
 **-BkUpOnlyIfClean**  
 指定仅当指定的 **-Ck** 检查未发现数据问题时才进行备份。 维护操作的运行顺序与其在命令提示中出现的顺序相同。 如果还要指定 **-BkUpOnlyIfClean** 或指定无论检查报告问题与否都进行备份，则需在 **-BkUpDB**/ **-BkUpLog** 参数之前指定 **-CkDB**、 **-CkDBNoIdx**、 **-CkAl**、 **-CkAlNoIdx** **-CkTxtAl** 或 **-CkCat** 参数。  
  
 **-VrfyBackup**  
 指定备份完成时，对备份运行 RESTORE VERIFYONLY。  
  
 *number*[**minutes**\| **hours**\| **day**\| **weeks**\| **months**]  
 指定时间间隔，用于确定报表或备份文件是否旧到需要将其删除。 *number* 是一个整数，后跟时间单位（没有空格）。 有效示例：  
  
-   **12weeks**  
  
-   **3months**  
  
-   **15days**  
  
 如果仅指定 *number* ，则默认日期部分为 **weeks**。  
  
## <a name="remarks"></a>备注  
 **sqlmaint** 实用工具可对一个或多个数据库执行维护操作。 如果指定 **-D** ，则在剩余的命令开关中指定的操作仅对指定数据库执行。 如果指定 **-PlanName** 或 **-PlanID** ，则 **sqlmaint** 只从指定的维护计划中检索数据库列表信息。 在其余的 **sqlmaint** 参数中指定的所有操作都会应用于从计划获取的列表中的每个数据库。 **sqlmaint** 实用工具不会应用在计划本身中定义的任何维护活动。  
  
 如果成功运行，则 **sqlmaint** 实用工具将返回 0，如果失败则返回 1。 在下列情况下将报告失败：  
  
-   任何维护操作失败。  
  
-   **-CkDB**、 **-CkDBNoIdx**、 **-CkAl**、 **-CkAlNoIdx**、 **-CkTxtAl** 或 **-CkCat** 检查发现数据问题。  
  
-   发生常规错误。  
  
## <a name="permissions"></a>权限  
 对 **具有** 读取和执行 **权限的任何 Windows 用户都可以执行** sqlmaint `sqlmaint.exe`实用工具，默认情况下，该工具存储在 `x:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER1\MSSQL\Binn` 文件夹中。 此外，使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -login_ID **指定的** 登录名必须具有执行指定的操作所需的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 权限。 如果使用 Windows 身份验证连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ，则映射到经过身份验证的 Windows 用户的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 登录名必须具有执行指定操作所需的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 权限。  
  
 例如，使用 **-BkUpDB** 需要执行 BACKUP 语句的权限。 使用 **-UpdOptiStats** 参数需要执行 UPDATE STATISTICS 语句的权限。 有关详细信息，请参阅联机丛书中相应主题的“权限”部分。  
  
## <a name="examples"></a>示例  
  
### <a name="a-performing-dbcc-checks-on-a-database"></a>A. 对数据库执行 DBCC 检查  
  
```  
sqlmaint -S MyServer -D AdventureWorks2012 -CkDB -CkAl -CkCat -Rpt C:\MyReports\AdvWks_chk.rpt  
```  
  
### <a name="b-updating-statistics-using-a-15-sample-in-all-databases-in-a-plan-also-shrink-any-of-the-database-that-have-reached-110-mb-to-having-only-10-free-space"></a>B. 使用计划中所有数据库的 15% 样本更新统计信息。 同时，压缩任何已达到 110 MB 的数据库，以便仅使用 10% 可用空间  
  
```  
sqlmaint -S MyServer -PlanName MyUserDBPlan -UpdOptiStats 15 -RmUnusedSpace 110 10  
```  
  
### <a name="c-backing-up-all-the-databases-in-a-plan-to-their-individual-subdirectories-in-the-default-xprogram-filesmicrosoft-sql-servermssql13mssqlservermssqlbackup-directory-also-delete-any-backups-older-than-2-weeks"></a>C. 将计划中的所有数据库备份到默认 x:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup 目录下各自的子目录中。 同时，删除所有超过两个星期的备份  
  
```  
sqlmaint -S MyServer -PlanName MyUserDBPlan -BkUpDB -BkUpMedia DISK -UseDefDir -CrBkSubDir -DelBkUps 2weeks  
```  
  
### <a name="d-backing-up-a-database-to-the-default-xprogram-filesmicrosoft-sql-servermssql13mssqlservermssqlbackup-directory"></a>D. 将数据库备份到默认 x:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup 目录中。  
  
```  
sqlmaint -S MyServer -BkUpDB -BkUpMedia DISK -UseDefDir  
```  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../t-sql/statements/backup-transact-sql.md)   
 [更新统计信息 (Transact-SQL)](../t-sql/statements/update-statistics-transact-sql.md)  
  
  
