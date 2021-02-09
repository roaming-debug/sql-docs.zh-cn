---
title: 使用 SMB 文件共享存储安装 SQL Server | Microsoft Docs
description: 在 SQL Server 中，安装系统数据库和数据库引擎用户数据库时可以选择服务器消息块 (SMB) 文件服务器作为存储。
ms.custom: ''
ms.date: 09/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 8b7810b2-637e-46a3-9fe1-d055898ba639
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 9c0ed977deed5b4eb031406368f3d010062da7df
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536352"
---
# <a name="install-sql-server-with-smb-fileshare-storage"></a>使用 SMB 文件共享存储安装 SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]开始，在安装系统数据库（Master、Model、MSDB 和 TempDB）和 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 用户数据库时可以选择服务器消息块 (SMB) 文件服务器作为存储。 这同时适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立安装和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集安装 (FCI)。  
  
> [!NOTE]  
>  当前 SMB 文件共享上不支持 Filestream。  
  
## <a name="installation-considerations"></a>安装注意事项  
  
### <a name="smb-fileshare-formats"></a>SMB 文件共享格式：  
 在指定 SMB 文件共享时，下面是针对独立和 FCI 数据库的支持的通用命名约定 (UNC) 路径格式：  
  
-   \\\ServerName\ShareName\  
  
-   \\\ServerName\ShareName  
  
 有关通用命名约定的详细信息，请参阅 [UNC](/openspecs/windows_protocols/ms-dtyp/62e862f4-2a51-452e-8eeb-dc4ff5ee33cc)。  
  
 不支持环回 UNC 路径（其服务器名称为 localhost、127.0.0.1 或本地计算机名称的 UNC 路径）。 作为一种特殊情况，如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用的文件服务器群集承载在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所运行的同一节点上，则也不受支持。 若要防止出现这种情况，建议在单独的 Windows 群集中创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与文件服务器群集。  
  
 不支持以下 UNC 路径格式：  
  
-   环回路径，例如 \\\localhost\\..\ 或 \\\127.0.0.1\\...\  
  
-   管理共享，例如 \\\servername\x$  
  
-   其他 UNC 路径格式，例如 \\\\?\x:\  
  
-   映射的网络驱动器。  
  
### <a name="supported-data-definition-language-ddl-statements"></a>支持的数据定义语言 (DDL) 语句  
 以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] DDL 语句和数据库引擎存储过程支持 SMB 文件共享：  
  
1.  [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)  
  
2.  [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
  
3.  [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
4.  [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)  
  
### <a name="installation-options"></a>安装选项  
  
-   在安装程序 UI“数据库引擎配置”页的“数据目录”选项卡上，将参数“数据根目录”设置为“\\\fileserver1\share1\"”。  
  
-   在命令提示安装中，将“/INSTALLSQLDATADIR”指定为“\\\fileserver1\share1\"”。  
  
     下面是使用 SMB 文件共享选项在独立服务器上安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的语法示例：  
  
    ```  
    Setup.exe /q /ACTION=Install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="<DomainName\UserName>" /AGTSVCPASSWORD="<StrongPassword>" /INSTALLSQLDATADIR="\\FileServer\Share1\" /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     安装具有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的单节点 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]故障转移群集实例（默认实例）：  
  
    ```  
    setup.exe /q /ACTION=InstallFailoverCluster /InstanceName=MSSQLSERVER /INDICATEPROGRESS /ASSYSADMINACCOUNTS="<DomainName\UserName>" /ASDATADIR=<Drive>:\OLAP\Data /ASLOGDIR=<Drive>:\OLAP\Log /ASBACKUPDIR=<Drive>:\OLAP\Backup /ASCONFIGDIR=<Drive>:\OLAP\Config /ASTEMPDIR=<Drive>:\OLAP\Temp /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'" /FAILOVERCLUSTERNETWORKNAME="<Insert Network Name>" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;Cluster Network;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="MSSQLSERVER" /Features=AS,SQL /ASSVCACCOUNT="<DomainName\UserName>" /ASSVCPASSWORD="xxxxxxxxxxx" /AGTSVCACCOUNT="<DomainName\UserName>" /AGTSVCPASSWORD="xxxxxxxxxxx" /INSTALLSQLDATADIR="\\FileServer\Share1\" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx" /SQLSYSADMINACCOUNTS="<DomainName\UserName> /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     有关 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中不同命令行参数选项的用法的详细信息，请参阅 [从命令提示符安装 SQL Server 2016](./install-sql-server-from-the-command-prompt.md)。  
  
## <a name="operating-system-considerations-smb-protocol-vs-ssnoversion"></a>操作系统注意事项（SMB 协议与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）  
 不同的 Windows 操作系统具有不同的 SMB 协议版本，并且 SMB 协议版本对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]而言是透明的。 您可以就 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]而言发现不同 SMB 协议版本的好处。  
  
|操作系统|SMB2 协议版本|优势 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|----------------------|---------------------------|-------------------------------------------|  
|[!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] SP 2|2.0|与以前的 SMB 版本相比改进的性能。<br /><br /> 持续性，帮助从临时网络问题恢复。|  
|[!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)] SP 1，包含 Server Core|2.1|支持大型 MTU，这有利于大型数据传输，例如 SQL 备份和还原。 此功能必须由用户启用。 有关如何启用此功能的详细信息，请参阅 [SMB 中的新增功能](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff625695(v=ws.10)) (https://go.microsoft.com/fwlink/?LinkID=237319) 。<br /><br /> 针对 SQL OLTP 样式工作负荷的显著的性能改进。 这些性能改进要求应用修补程序。 有关控件的详细信息，请参阅[此处](https://mskb.pkisolutions.com/kb/2536493) (https://mskb.pkisolutions.com/kb/2536493) 。|  
|[!INCLUDE[win8srv](../../includes/win8srv-md.md)]，包含 Server Core|3.0|支持文件共享的透明故障转移，提供零停机时间，并且在文件服务器群集配置中，无需 SQL DBA 或文件服务器管理员参与。<br /><br /> 支持同时使用多个网络接口的 IO，并且可承受网络接口故障。<br /><br /> 支持具有 RDMA 功能的网络接口。<br /><br /> 有关这些功能和服务器消息块的详细信息，请参阅 [服务器消息块概述](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11)) (https://go.microsoft.com/fwlink/?LinkId=253174) 。<br /><br /> 通过持续可用性支持向外扩展文件服务器 (SoFS)。|  
|[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2，包含 Server Core|3.2|支持文件共享的透明故障转移，提供零停机时间，并且在文件服务器群集配置中，无需 SQL DBA 或文件服务器管理员参与。<br /><br /> 支持同时使用多个网络接口的 IO，并且使用 SMB 多通道可承受网络接口故障。<br /><br /> 使用 SMB Direct 支持具有 RDMA 功能的网络接口。<br /><br /> 有关这些功能和服务器消息块的详细信息，请参阅 [服务器消息块概述](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11)) (https://go.microsoft.com/fwlink/?LinkId=253174) 。<br /><br /> 通过持续可用性支持向外扩展文件服务器 (SoFS)。<br /><br /> 针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] OLTP 所共有的小型随机读/写 I/O 进行优化。<br /><br /> 默认情况下启用最大传输单位 (MTU)，这可以显著增强大型连续传输（例如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据仓库和数据库备份或还原）的性能。|  
  
## <a name="security-considerations"></a>安全注意事项  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户应具有对 SMB 共享文件夹的 FULL CONTROL 共享权限和 NTFS 权限。 使用 SMB 文件服务器时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户可以是域帐户或系统帐户。 有关共享和 NTFS 权限的详细信息，请参阅 [文件服务器上的共享和 NTFS 权限](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754178(v=ws.11)) (https://go.microsoft.com/fwlink/?LinkId=245535) 。  
  
    > [!NOTE]  
    >  对 SMB 共享文件夹的 FULL CONTROL 共享权限和 NTFS 权限应被限制为： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户和具有管理服务器角色的 Windows 用户。  
  
     建议将域帐户用作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户。 如果系统帐户用作服务帐户，则按以下格式为计算机帐户授予权限：\<*domain_name*>\\<computer_name>\*$*。  
  
    > [!NOTE]  
    >  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中，如果 SMB 文件共享指定为存储选项，则需要将域帐户指定为服务帐户。 对于 SMB 文件共享，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装后系统帐户只能指定为服务帐户。  
    >   
    >  虚拟帐户无法通过身份验证，因而无法访问远程位置。 所有虚拟帐户均使用计算机帐户的权限。 以 \<*domain_name*>\\<computer_name>\*$* 格式预配计算机帐户。  
  
-   用于安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的帐户应该对用作数据目录的 SMB 文件共享文件夹或群集安装过程中的任何其他数据文件夹（用户数据库目录、用户数据库日志目录、TempDB 目录、TempDB 日志目录、备份目录）具有 FULL CONTROL 权限。  
  
-   用于安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的帐户应具有对 SMB 文件服务器的 SeSecurityPrivilege 特权。 若要授予此特权，请使用文件服务器上的“本地安全策略”控制台将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装帐户添加到“管理审核和安全日志”策略中。 在“本地安全策略”控制台中“本地策略”下的“用户权限分配”部分可以找到此设置。  
  
## <a name="known-issues"></a>已知问题  
  
-   在您分离网络连接的存储设备上的某一 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 数据库后，如果尝试重新连接该 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库，则可能会遇到数据库权限问题。 有关详细信息，请参阅[错误 5120](../../relational-databases/errors-events/mssqlserver-5120-database-engine-error.md)。
  
-   如果将 SMB 文件共享作为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]群集实例的存储选项，默认情况下无法将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集诊断日志写入该文件共享，因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源 DLL 缺乏对该文件共享的读/写权限。 若要解决此问题，请尝试使用以下方法之一：  
  
    1.  将对该文件共享的读/写权限授予群集中的所有计算机对象。  
  
    2.  将诊断日志的位置设置为本地文件路径。 请参阅以下示例：  
  
        ```sql  
        ALTER SERVER CONFIGURATION  
        SET DIAGNOSTICS LOG PATH = 'C:\logs';  
        ```  
  
## <a name="see-also"></a>另请参阅  
 [计划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)   
 [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
  
