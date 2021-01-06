---
title: 为可用性组创建群集 DTC 资源
description: 本主题介绍如何为 SQL Server AlwaysOn 可用性组完整配置群集 DTC 资源。
ms.custom: seodec18
ms.date: 08/30/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 0e332aa4-2c48-4bc4-a404-b65735a02cea
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c976a81277f18f89486343af32c96509f8383fda
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643262"
---
# <a name="create-clustered-dtc-resource-for-an-always-on-availability-group"></a>为 AlwaysOn 可用性组创建群集 DTC 资源

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

本主题介绍如何为 SQL Server AlwaysOn 可用性组完整配置群集 DTC 资源。 完整配置过程可能要长达一小时才能完成。 

本演练将按照[为 SQL Server 可用性组群集化 DTC](../../../database-engine/availability-groups/windows/cluster-dtc-for-sql-server-2016-availability-groups.md) 中的要求，创建群集 DTC 资源和 SQL Server 可用性组。

本演练使用 PowerShell 和 Transact-SQL (T-SQL) 脚本。  许多 T-SQL 脚本要求启用 **SQLCMD 模式** 。  有关 **SQLCMD 模式** 的详细信息，请参阅 [在查询编辑器中启用 SQLCMD 脚本撰写](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md)。  必须导入 PowerShell 模块 **FailoverClusters** 。  有关导入 PowerShell 模块的详细信息，请参阅 [导入 PowerShell 模块](/powershell/scripting/developer/module/importing-a-powershell-module)。  本演练基于以下条件：
- 已满足[针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md) 中的所有要求。  
- 域为 `contoso.lab`。
- 用户在将要创建 DTC 网络名称资源的 OU 中具有“创建计算机对象”权限。
- 用户是对群集中的所有节点具有管理员权限的域用户。
- 为备份创建了名为 `sqlbackups` 的文件共享。
- 默认 SQL Server 实例命名为 `SQLNODE1` 和 `SQLNODE2`。
- 所有 SQL Server 实例使用相同的服务帐户。
- 用户是所有 SQL Server 实例上固定 SQL Server 角色 sysadmin 的成员。
- DTC 无法解析的事务的默认结果将设置为 `presume commit`。
- 镜像端点将使用端口 `5022`。
- 不存在其他可用性组或群集 DTC 资源。
- 群集详细信息（现有）：
  - 名称：`Cluster`
  - 网络名称：`Cluster Network 1`
  - 多个节点：`SQLNODE1, SQLNODE2`
  - 共享存储：`Cluster Disk 3`（归 `SQLNODE1` 所有）
- 群集详细信息（待创建）：
  - 网络名称资源：`DTCnet1`
  - DTC 网络名称资源：`DTC1`
  - DTC 物理磁盘资源：`DTCDisk1`
  - DTC IP 和子网资源：`192.168.2.54`、`255.255.255.0`
  - DTC IP 资源：`DTCIP1`

## <a name="1-check-operating-system"></a>1.检查操作系统
对于受支持的分布式事务，[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 必须在 Windows Server 2016 或 Windows Server 2012 R2 上运行。  对于 Windows Server 2012 R2，必须安装 KB3090973 中的更新，网址：[https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973)。  此脚本将检查操作系统版本以及是否需要安装修补程序 3090973。  在 `SQLNODE1` 上运行以下 PowerShell 脚本。

```powershell  
# A few OS checks

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {

    # At least 2012 R2
    $os = (Get-WmiObject -class Win32_OperatingSystem -ComputerName $node).caption;
    IF($os -like "*2012 R2*" -or $os -like "*2016*")
    {
        Write-Host "$os is supported on $node.";
    }
    ELSE
    {
        Write-Host "STOP!  $os is not supported on $node.";
    }
    
    # KB 3090973
    IF($os -like "*2012 R2*")
    {
        $kb = Get-Hotfix -ComputerName $node | Where {$_.HotFixID -eq 'KB3090973'};
        IF($kb)
        {
            Write-Host "KB3090973 is installed on $node."
        }
        ELSE
        {
            Write-Host "HotFixID KB3090973 must be applied on $node.  See https://support.microsoft.com/kb/3090973 for additional information and to download the hotfix.";
        }
    }
    ELSE
    {
        Write-Host "KB3090973 is not applicable to $os on $node."
    }
}
```  
## <a name="2---configure-firewall-rules"></a>2. 配置防火墙规则
此脚本会在托管可用性组副本的每个 SQL Server 上以及其他所有参与分布式事务的服务器上，将防火墙配置为允许 DTC 流量通过。  此外，还会将防火墙配置为允许数据库镜像端点的连接通过。  在 `SQLNODE1` 上运行以下 PowerShell 脚本。

```powershell  
# Configure Firewall

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {
    Get-NetFirewallRule -CimSession $node -DisplayGroup 'Distributed Transaction Coordinator' -Enabled False -ErrorAction SilentlyContinue | Enable-NetFirewallRule;
    New-NetFirewallRule -CimSession $node -DisplayName 'SQL Server Mirroring' -Description 'Port 5022 for SQL Server Mirroring' -Action Allow -Direction Inbound -Protocol TCP -LocalPort 5022 -RemotePort Any -LocalAddress Any -RemoteAddress Any;
    };
```  
## <a name="3--configure-in-doubt-xact-resolution"></a>3.配置“未决事务解析” 
此脚本针对未决事务将“未决事务解析”服务器配置选项配置为“假设提交”  。  在 SQL Server Management Studio (SSMS) 中以 **SQLCMD 模式** 对 `SQLNODE1` 运行以下 T-SQL 脚本。

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Configure in-doubt xact resolution on all SQL Server instances to presume commit
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------
```

## <a name="4-create-test-databases"></a>4.创建测试数据库
该脚本将在 `SQLNODE1` 上创建一个名为 `AG1` 的数据库，在 `SQLNODE2` 上创建一个名为 `dtcDemoAG1` 的数据库。  在 SSMS 中以 **SQLCMD 模式** 对 `SQLNODE1` 运行以下 T-SQL 脚本。

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- On SQLNODE1 
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'AG1')
BEGIN
    DROP DATABASE AG1;
END
GO

CREATE DATABASE AG1;
ALTER DATABASE AG1 SET RECOVERY FULL WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::AG1 to sa;
GO

USE AG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );

INSERT Names
VALUES ('AG1', GETDATE());
GO


-- Against SQNODE2
:connect SQLNODE2
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'dtcDemoAG1')
BEGIN
    DROP DATABASE dtcDemoAG1;
END
GO

CREATE DATABASE dtcDemoAG1;
ALTER DATABASE dtcDemoAG1 SET RECOVERY SIMPLE WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::dtcDemoAG1 to sa;
GO

USE dtcDemoAG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );
GO    
----------------------------------------------------------------
```
## <a name="5---create-endpoints"></a>5. 创建端点
此脚本将创建一个名为 `AG1_endpoint` 的端点，以侦听 TCP 端口 `5022`。  在 SSMS 中以 **SQLCMD 模式** 对 `SQLNODE1` 运行以下 T-SQL 脚本。

```sql  
/**********************************************
Execute on SQLNODE1 in SQLCMD mode
**********************************************/

-- Create endpoint on server instance that hosts the primary replica:
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------
```

## <a name="6---prepare-databases-for-availability-group"></a>6. 为可用性组准备数据库
该脚本将备份 `SQLNODE1` 上的 `AG1` 并将其还原到 `SQLNODE2`。  在 SSMS 中以 **SQLCMD 模式** 对 `SQLNODE1` 运行以下 T-SQL 脚本。

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Backup database
BACKUP DATABASE AG1 
TO DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH FORMAT, STATS = 10;

-- Backup transaction log
BACKUP LOG AG1
TO DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH FORMAT, STATS = 10;
GO


-- Restore database and logs on secondary WITH NORECOVERY
:connect SQLNODE2
USE [master]
RESTORE DATABASE AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH NORECOVERY, STATS = 10;

RESTORE LOG AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH NORECOVERY, STATS = 10;
GO
```

## <a name="7---create-availability-group"></a>7. 创建可用性组
[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 必须使用 **CREATE AVAILABILITY GROUP** 命令和 **WITH DTC_SUPPORT = PER_DB** 子句创建。  当前不可更改现有可用性组。  新建可用性组向导不允许为新的可用性组启用 DTC 支持。  以下脚本将创建新的可用性组并加入辅助数据库。  在 SSMS 中以 **SQLCMD 模式** 对 `SQLNODE1` 运行以下 T-SQL 脚本。

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

--  Create Availability Group on SQLNODE1 
USE master;
CREATE AVAILABILITY GROUP DTCAG1
WITH (DTC_SUPPORT = PER_DB) 
FOR DATABASE AG1
REPLICA ON 
  'SQLNODE1' WITH
     (
     ENDPOINT_URL = 'TCP://SQLNODE1.contoso.lab:5022', 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ),
  'SQLNODE2' WITH 
     (
     ENDPOINT_URL = 'TCP://SQLNODE2.contoso.lab:5022',
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ); 
GO


-- SQLNODE2
-- Join secondary replica to the Availability Group 
:connect SQLNODE2
ALTER AVAILABILITY GROUP DTCag1 JOIN;

-- Join database to the Availability Group
ALTER DATABASE AG1 SET HADR AVAILABILITY GROUP = DTCAG1;
GO
```

> [!IMPORTANT]
> 无法在现有 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 上启用 DTC。  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将接受用于现有可用性组的以下语法：  
> 
> USE master;    
> ALTER AVAILABILITY GROUP \<availability_group\>  
> SET (DTC_Support = Per_DB)  
> 
> 但实际上不会进行任何配置更改。  可以使用以下 T-SQL 查询确认 **dtc_support** 配置：  
> 
> SELECT name, dtc_support FROM sys.availability_groups  
> 
> 为可用性组启用 DTC 支持的唯一方法是使用 Transact-SQL 创建可用性组。
 
## <a name="8--prepare-cluster-resources"></a><a name="ClusterDTC"></a>8.准备群集资源

此脚本将准备 DTC 从属资源：磁盘和 IP。  将向 Windows 群集添加共享存储。  将创建网络资源，然后创建 DTC 并使其成为可用性组的资源。  在 `SQLNODE1` 上运行以下 PowerShell 脚本。 感谢 [Allan Hirt](https://sqlha.com/2013/03/12/how-to-properly-configure-dtc-for-clustered-instances-of-sql-server-with-windows-server-2008-r2/) 提供脚本！

```powershell  
# Create a clustered Microsoft Distributed Transaction Coordinator properly in the resource group with SQL Server

\<#----------------------------------- Begin User Input -----------------------------------#>
$AGgrp = "DTCag1";                          # Name of the WSFC resource group that will contain the DTC resource

$WSFC = (Get-Cluster).Name;                 # Windows Failover Cluster name
$DTCnetwk = "Cluster Network 1"             # WSFC Network to use for the DTC IP address

$ClusterAvailableDisk = "Cluster Disk 3";   # Designated disk that can support failover clustering and is visible to all nodes, but not yet part of the set of clustered disks
$DTCdisk = "DTCDisk1";                      # Name of the disk to be used with DTC

$DTCipresnm = "DTCIP1";                     # WSFC Friendly Name of the DTC's IP resource 
$DTCipaddr = "192.168.2.54";                # IP address of the DTC resource 
$DTCsubnet = "255.255.255.0";               # Subnet for the DTC IP address 
$DTCnetnm = "DTCNet1";                      # WSFC Friendly Name of the Network Name resource
$DTCresnm = "DTC1";                         # Name of the WSFC DTC Network Name resource; Name must be unique in AD
\<#------------------------------------ End User Input ------------------------------------#>


# Make a new disk available for use in a failover cluster.
Get-ClusterAvailableDisk | Where {$_.Name -eq $ClusterAvailableDisk} | Add-ClusterDisk;

# Rename disk
$resource = Get-ClusterResource $ClusterAvailableDisk; $resource.Name = $DTCdisk;

# Create the IP resource
Add-ClusterResource -Name $DTCipresnm -ResourceType "IP Address" -Group $AGgrp;

# Set the network to use, IP address, and subnet
# All three have to be configured at the same time
$DTCIPres = Get-ClusterResource $DTCipresnm;
$ntwk = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Network,$DTCnetwk;
$ipaddr = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Address,$DTCipaddr;
$subnet = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,SubnetMask,$dtcsubnet;

$setdtcipparams = $ntwk,$ipaddr,$subnet;
$setdtcipparams | Set-ClusterParameter;

# Create the Network Name resource
Add-ClusterResource $DTCnetnm -ResourceType "Network Name" -Group $AGgrp;

# Set the value for the Network Name resource
Get-ClusterResource $DTCnetnm | Set-ClusterParameter DnsName $DTCresnm;

# Add the IP address as a depenency of the Network Name resource
Add-ClusterResourceDependency $DTCnetnm $DTCipresnm;

# Create the Distributed Transaction Coordinator resource
Add-ClusterResource $DTCresnm -ResourceType "Distributed Transaction Coordinator" -Group $AGgrp;

# Add the Network Name as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCnetnm;

# Move the disk into the resource group with SQL Server
Move-ClusterResource -Name $DTCdisk -Group $AGgrp;

# Add the disk as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCdisk;

# Bring the IP resource online
Start-ClusterResource $DTCipresnm;

# Bring the Network Name resource online
Start-ClusterResource $DTCnetnm;

# Bring the DTC resource online
Start-ClusterResource $DTCresnm;
```  

## <a name="9---enable-network-dtc"></a>9. 启用网络 DTC 

以下脚本将为群集 DTC 服务启用网络 DTC 访问，以允许远程计算机通过网络加入分布式事务。  在 `SQLNODE1` 上运行以下 PowerShell 脚本。

```powershell  
# Enable Network DTC access for the clustered DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

# Enter Name of DTC resource

$DtcName = "DTC1";
\<# ------- End of User Input ------- #>

[bool]$restart = 0;
$node = (Get-ClusterResource -Name $DtcName).OwnerNode.Name;
$DtcSettings = Get-DtcNetworkSetting -DtcName $DtcName;

IF ($DtcSettings.InboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -InboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($DtcSettings.OutboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -OutboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($restart -eq 1)
{
    Stop-Dtc -CimSession $node -DtcName $DTCname -Confirm:$false;
    Start-Dtc -CimSession $node -DtcName $DTCname;
}
```  

## <a name="10--disable-and-stop-the-local-dtc-service-on-each-node"></a>10.禁用并停止各节点上的本地 DTC 服务

若要确保分布式事务使用群集 DTC 资源，请禁用两个节点上的本地 DTC。  以下脚本将禁用并停止各节点上的本地 DTC 服务。  在 `SQLNODE1` 上运行以下 PowerShell 脚本。
```powershell  
# Disable local DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$DTCname = 'Local';
$nodes = (Get-ClusterNode).Name;

 foreach ($node in $nodes) {

    $service = Get-WmiObject -class Win32_Service -computername $node -Filter "Name='MSDTC'";
    IF ($service.StartMode -ne 'Disabled')
    {
        $service.ChangeStartMode('Disabled');
    }
    
    IF ($service.State -ne 'Stopped')
    {
        $service.StopService();
    }
}
```  

## <a name="11--cycle-the-ssnoversion-service-for-each-instance"></a>11.对每个实例循环运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务

完整配置群集 DTC 服务后，需要停止然后重启可用性组中的每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，确保 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通过注册使用此 DTC 服务。

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务第一次需要分布式事务时，需向 DTC 服务注册。 之后，SQL Server 服务会一直使用该 DTC 服务，直到它重新启动。 如果群集 DTC 服务可用，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将向群集 DTC 服务注册。 如果群集 DTC 服务不可用，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将向本地 DTC 服务注册。 为了确保 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 向群集 DTC 服务注册，请停止然后重启每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 

请按照以下 T-SQL 脚本中包含的步骤操作：
```sql  
/*
Gracefully cycle the SQL Server service and failover the Availability Group
    a.  On SQLNODE2, cycle the SQL Server service from SQL Server Configuration Manger

    b.  On SQLNODE2 failover the Availability Group to SQLNODE2
        Execute T-SQL script, below, on SQLNODE2 (Use Results to Text)

    c.  On SQLNODE1, cycle the SQL Server service from SQL Server Configuration Manger

    d.  On SQLNODE1 failover the Availability Group to SQLNODE1 once the databases are back in sync.
        Execute T-SQL script, below, on SQLNODE1 (Use Results to Text)
*/

SET NOCOUNT ON;

-- Ensure replica is secondary
IF (
SELECT rs.is_primary_replica 
    FROM sys.availability_groups ag
    JOIN sys.dm_hadr_database_replica_states rs
    ON  ag.group_id = rs.group_id
    WHERE ag.name = N'DTCag1'
    AND rs.is_local = 1) = 0
BEGIN
    -- Wait for SYNCHRONIZED state
    DECLARE @ctr tinyint = 0;
    declare @msg varchar(128);
    WHILE (SELECT synchronization_state 
        FROM sys.availability_groups ag
        JOIN sys.dm_hadr_database_replica_states rs
        ON  ag.group_id = rs.group_id
        WHERE ag.name = N'DTCag1'
        AND rs.is_primary_replica = 0
        AND rs.is_local = 1) <> 2
    BEGIN
        WAITFOR DELAY '00:00:01'
        SET @ctr += 1
        SET @msg = 'Waiting for databases to become synchronized. Duration in seconds: ' + cast(@ctr AS varchar(3))
        RAISERROR (@msg, 0, 1) WITH NOWAIT
    END

    ALTER AVAILABILITY GROUP DTCAG1 FAILOVER;
    SELECT 'Failover complete' AS [Sucess]
END
ELSE BEGIN
    SELECT 'This instance is the primary replica.  Connect to the secondary replica and try again.' AS [Error]
END

```

## <a name="12--test-configuration"></a>12.测试配置

此测试使用一个从 `SQLNODE1` 链接到 `SQLNODE2` 的服务器创建分布式事务。  确保可用性组的主要副本位于 `SQLNODE1`上。 若要测试配置，请执行以下操作：

- 创建链接服务器
- 执行分布式事务

### <a name="create-linked-servers"></a>创建链接服务器  
以下脚本将在 `SQLNODE1`上创建两个链接服务器。  在 SSMS 中对 `SQLNODE1`运行以下 T-SQL 脚本。

```sql  
-- SQLNODE1
IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE1')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE1';   
END

IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE2')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE2';   
END
 ```

### <a name="execute-a-distributed-transaction"></a>执行分布式事务
此脚本先返回当前 DTC 事务统计信息，  然后利用 `SQLNODE1` 和 `SQLNODE2`上的数据库执行分布式事务。  接着，该脚本再次返回 DTC 事务统计信息，此时其计数应已增加。  以物理方式连接到 `SQLNODE1` ，并在 SSMS 中以 `SQLNODE1` SQLCMD 模式 **对** 运行以下 T-SQL 脚本。

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
    Must be physically connected to SQLNODE1
*******************************************************************/

USE AG1;
SET NOCOUNT ON;

-- Get Baseline
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;

SET XACT_ABORT ON
BEGIN DISTRIBUTED TRANSACTION
    INSERT INTO SQLNODE1.[AG1].[dbo].[Names] VALUES ('TestValue1', GETDATE());
    INSERT INTO SQLNODE2.[dtcDemoAG1].[dbo].[Names] VALUES ('TestValue2', GETDATE());
COMMIT TRAN
GO

-- Review DTC Transaction Statistics
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;
```

> [!IMPORTANT]
> 必须执行 `USE AG1` 语句，确保数据库上下文设置为 `AG1`。  否则会收到以下错误消息：“其他会话正在使用事务的上下文。”