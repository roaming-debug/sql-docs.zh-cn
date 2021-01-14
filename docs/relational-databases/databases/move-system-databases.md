---
description: 移动系统数据库
title: 移动系统数据库 | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- moving system databases
- disaster recovery [SQL Server], moving database files
- database files [SQL Server], moving
- data files [SQL Server], moving
- tempdb database [SQL Server], moving
- system databases [SQL Server], moving
- scheduled disk maintenace [SQL Server]
- moving databases
- msdb database [SQL Server], moving
- moving database files
- relocating database files
- planned database relocations [SQL Server]
- master database [SQL Server], moving
- model database [SQL Server], moving
- Resource database [SQL Server]
- databases [SQL Server], moving
ms.assetid: 72bb62ee-9602-4f71-be51-c466c1670878
author: stevestein
ms.author: sstein
ms.openlocfilehash: c24a98684e87eb94a3cd9e100f203509789b7a0f
ms.sourcegitcommit: 4a813a0741502c56c0cd5ecaafafad2e857a9d7f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98031101"
---
# <a name="move-system-databases"></a>移动系统数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中移动系统数据库。 移动系统数据库在下列情况下可能很有用：  
  
-   故障恢复。 例如，数据库处于可疑模式下或因硬件故障而关闭。  
  
-   预先安排的重定位。  
  
-   为预定的磁盘维护操作而进行的重定位。  
  
 下列过程适用于在同一 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例内移动数据库文件。 若要将数据库移动另一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中或另一台服务器上，请使用 [备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md) 操作。  

 本主题中的过程需要数据库文件的逻辑名称。 若要获取该名称，请在 [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) 目录视图中查询名称列。  
  
> [!IMPORTANT]  
>  如果移动系统数据库并且随后重新生成 master 数据库，则必须再次移动系统数据库，因为重新生成操作会在默认位置安装所有系统数据库。  

> [!IMPORTANT]  
>  移动文件之后，[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 服务帐户必须有权访问新文件的文件夹位置中的文件。 有关更多信息，请参阅[配置数据库引擎访问的文件系统权限](../../database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access.md)。
    
  
##  <a name="planned-relocation-and-scheduled-disk-maintenance-procedure"></a><a name="Planned"></a> 预先安排的重定位与预定的磁盘维护过程  
 若要将移动系统数据库数据或日志文件的操作作为预先安排的重定位或预定的维护操作的一部分，请按照下列步骤操作。 此过程适用于除 master 和 Resource 数据库以外的所有系统数据库。  
  
1.  对于要移动的每个文件，请运行以下语句。  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name , FILENAME = 'new_path\os_file_name' )  
    ```  
  
2.  停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例或关闭系统以执行维护。 有关详细信息，请参阅 [启动、停止、暂停、继续、重启 SQL Server 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)。  
  
3.  将文件移动到新位置，并验证 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 服务帐户是否仍具有访问这些文件的权限。

4.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例或服务器。 有关详细信息，请参阅 [启动、停止、暂停、继续、重启 SQL Server 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)。  
  
5.  通过运行以下查询来验证文件更改。  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
 如果移动了 msdb 数据库并为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库邮件 [配置了](../../relational-databases/database-mail/database-mail.md)实例，则请完成下列附加步骤。  
  
1.  通过运行以下查询，验证是否已为 msdb 数据库启用 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 。  
  
    ```  
    SELECT is_broker_enabled   
    FROM sys.databases  
    WHERE name = N'msdb';  
    ```  
  
     有关启用 [!INCLUDE[ssSB](../../includes/sssb-md.md)]的详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)中移动系统数据库。  
  
2.  通过发送测试邮件来验证数据库邮件是否正常运行。  
  
##  <a name="failure-recovery-procedure"></a><a name="Failure"></a> 故障恢复过程  
 如果由于硬件故障而必须移动文件，则请按照下列步骤将文件重新定位到一个新位置。 此过程适用于除 master 和 Resource 数据库以外的所有系统数据库。  
  
> [!IMPORTANT]  
>  如果数据库无法启动，即处于可疑模式下或处于未恢复状态，则只有 sysadmin 固定角色的成员才可以移动该文件。  
  
1.  如果启动了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，则将其停止。  
  
2.  通过在命令提示符下输入下列命令之一，在仅 master 恢复模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 在这些命令中指定的参数区分大小写。 如果未按所示方式指定参数，则命令会失败。  
  
    -   对于默认的 (MSSQLSERVER) 实例，请运行以下命令：  
  
        ```  
        NET START MSSQLSERVER /f /T3608
        ```  
  
    -   对于命名实例，请运行以下命令：  
  
        ```  
        NET START MSSQL$instancename /f /T3608
        ```  
  
     有关详细信息，请参阅 [启动、停止、暂停、继续、重启 SQL Server 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)。  
  
3.  对于要移动的每个文件，请使用 **sqlcmd** 命令或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 运行以下语句。  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE( NAME = logical_name , FILENAME = 'new_path\os_file_name' )  
    ```  
  
     有关使用 **sqlcmd** 实用工具的详细信息，请参阅 [使用 sqlcmd 实用工具](../../ssms/scripting/sqlcmd-use-the-utility.md)。  
  
4.  退出 **sqlcmd** 实用工具或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
5.  停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。 例如，运行 `NET STOP MSSQLSERVER`。  
  
6.  将文件移动到新位置。  
  
7.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。 例如，运行 `NET START MSSQLSERVER`。  
  
8.  通过运行以下查询来验证文件更改。  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
##  <a name="moving-the-master-database"></a><a name="master"></a> 移动 master 数据库  
 若要移动 master 数据库，请按下列步骤进行操作。  
  
1.  在 **“开始”** 菜单中，依次指向 **“所有程序”** 、 **“Microsoft SQL Server”** 和 **“配置工具”** ，然后单击 **“SQL Server 配置管理器”** 。  
  
2.  在“SQL Server 服务”节点中，右键单击 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例（如 **SQL Server (MSSQLSERVER)** ），并选择“属性”。  
  
3.  在“SQL Server (_instance_name_) 属性” 对话框中，单击“启动参数”选项卡。  
  
4.  在“现有参数”框中，选择 -d 参数。 在“指定启动参数”框中，将该参数更改为主数据文件的新路径。 单击 **“更新”** 以保存更改。
  
5.  在“现有参数”框中，选择 -l 参数。 在“指定启动参数”框中，将该参数更改为主日志文件的新路径。 单击 **“更新”** 以保存更改。

     数据文件的参数值必须跟在 `-d` 参数的后面，日志文件的参数值必须跟在 `-l` 参数的后面。 下面的示例显示用于 master 数据文件默认位置的参数值。  
  
     `-dC:\Program Files\Microsoft SQL Server\MSSQL<version>.MSSQLSERVER\MSSQL\DATA\master.mdf`  
  
     `-lC:\Program Files\Microsoft SQL Server\MSSQL<version>.MSSQLSERVER\MSSQL\DATA\mastlog.ldf`  
  
     如果 master 数据文件预先安排的重定位是 `E:\SQLData`，则参数值将做如下更改：  
  
     `-dE:\SQLData\master.mdf`  
  
     `-lE:\SQLData\mastlog.ldf`  

6.  单击“确定”将永久保存所作更改并关闭“SQL Server (instance_name)属性”对话框 。

7.  通过右键单击实例名称并选择“停止”来停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
8.  将 master.mdf 和 mastlog.ldf 文件移动到新位置。  
  
9.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。  
  
10. 通过运行以下查询，验证 master 数据库的文件更改。  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID('master');  
    GO  
    ```  

11. 此时 SQL Server 应正常运行。 但是 Microsoft 建议还调整 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\instance_ID\Setup` 处的注册表项，其中 instance_ID 类似于 `MSSQL13.MSSQLSERVER`。 在该配置单元中，将 `SQLDataRoot` 值更改为新路径。 未能更新注册表可能会导致修补和升级失败。

  
##  <a name="moving-the-resource-database"></a><a name="Resource"></a> 移动 Resource 数据库  
 Resource 数据库的位置是 \<*drive*>:\Program Files\Microsoft SQL Server\MSSQL\<version>.\<*instance_name*>\MSSQL\Binn\\。 无法移动该数据库。  
  
##  <a name="follow-up-after-moving-all-system-databases"></a><a name="Follow"></a> 后续任务：移动所有系统数据库后  
 如果已将所有系统数据库都移到新的驱动器/卷或移到使用不同驱动器盘符的另一个服务器，请进行下列更新。  
  
-   更改 SQL Server 代理日志路径。 如果不更新此路径，SQL Server 代理将无法启动。  
  
-   更改数据库默认位置。 如果指定为默认位置的驱动器盘符和路径不存在，则可能无法创建新的数据库。  
  
#### <a name="change-the-sql-server-agent-log-path"></a>更改 SQL Server 代理日志路径  
  
1.  从 SQL Server Management Studio 的对象资源管理器中，展开 **“SQL Server 代理”** 。  
  
2.  右键单击 **“错误日志”** ，然后单击 **“配置”** 。  
  
3.  在 **“配置 SQL Server 代理错误日志”** 对话框中，指定 SQLAGENT.OUT 文件的新位置。 默认位置为：C:\Program Files\Microsoft SQL Server\MSSQL\<version>.<instance_name>\MSSQL\Log\\。  
  
#### <a name="change-the-database-default-location"></a>更改数据库默认位置  
  
1.  从 SQL Server Management Studio 的对象资源管理器中，右键单击 SQL Server 所在服务器，然后单击 **“属性”** 。  
  
2.  在 **“服务器属性”** 对话框中，选择 **“数据库设置”** 。  
  
3.  在 **“数据库默认位置”** 下，找到数据文件和日志文件的新位置。  
  
4.  先停止然后启动 SQL Server 服务以完成更改。  
  
##  <a name="examples"></a><a name="Examples"></a> 示例  
  
### <a name="a-moving-the-tempdb-database"></a>A. 移动 tempdb 数据库  
 作为预先安排的重定位的一部分，下面的示例将 `tempdb` 数据和日志文件移动到一个新位置。  
  
> [!NOTE]  
>  由于每次启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时都将重新创建 tempdb，所以不必实际移动数据和日志文件。 在步骤 3 中重新启动服务时，将在新位置中创建这些文件。 在重新启动服务之前，tempdb 将继续使用现有位置中的数据和日志文件。  
  
1.  确定 `tempdb` 数据库的逻辑文件名称以及在磁盘上的当前位置。  
  
    ```  
    SELECT name, physical_name AS CurrentLocation  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'tempdb');  
    GO  
    ```  
  
2.  使用 `ALTER DATABASE`更改每个文件的位置。  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE tempdb   
    MODIFY FILE (NAME = tempdev, FILENAME = 'E:\SQLData\tempdb.mdf');  
    GO  
    ALTER DATABASE tempdb   
    MODIFY FILE (NAME = templog, FILENAME = 'F:\SQLLog\templog.ldf');  
    GO  
    ```  
  
3.  停止再重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例。  
  
4.  验证文件更改。  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'tempdb');  
    ```  
  
5.  将 `tempdb.mdf` 和 `templog.ldf` 文件从其原始位置删除。  
  
## <a name="see-also"></a>另请参阅  
 [Resource 数据库](../../relational-databases/databases/resource-database.md)   
 [tempdb 数据库](../../relational-databases/databases/tempdb-database.md)   
 [master 数据库](../../relational-databases/databases/master-database.md)   
 [msdb 数据库](../../relational-databases/databases/msdb-database.md)   
 [model 数据库](../../relational-databases/databases/model-database.md)   
 [移动用户数据库](../../relational-databases/databases/move-user-databases.md)   
 [移动数据库文件](../../relational-databases/databases/move-database-files.md)   
 [启动、停止、暂停、继续、重新启动数据库引擎、SQL Server 代理或 SQL Server Browser 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [重新生成系统数据库](../../relational-databases/databases/rebuild-system-databases.md)  
  
