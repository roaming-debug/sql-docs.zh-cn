---
title: 为 Linux 上的 SQL Server 创建和配置可用性组
description: 本教程演示如何为 Linux 上的 SQL Server 创建和配置可用性组，以及如何创建可用性组终结点和证书。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/28/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: ecb634a770b4990bfbbc094b15498b03bdea4a57
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171999"
---
# <a name="create-and-configure-an-availability-group-for-sql-server-on-linux"></a>为 Linux 上的 SQL Server 创建和配置可用性组

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本教程介绍如何为 Linux 上的 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 创建和配置可用性组 (AG)。 与 Windows 上的 [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] 及更早版本不同，你可以在创建或不创建基础 Pacemaker 群集的情况下启用 AG。 如果需要，稍后才会与群集进行集成。

本教程包括以下任务：
 
> [!div class="checklist"]
> * 启用可用性组。
> * 创建可用性组终结点和证书。
> * 使用 [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) 或 Transact-SQL 创建可用性组。
> * 为 Pacemaker 创建 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 登录和权限。
> * 在 Pacemaker 群集中创建可用性组资源（仅限外部类型）。

## <a name="prerequisite"></a>先决条件
- 按照[为 Linux 上的 SQL Server 部署 Pacemaker 群集](sql-server-linux-deploy-pacemaker-cluster.md)中所述部署 Pacemaker 高可用性群集。


## <a name="enable-the-availability-groups-feature"></a>启用可用性组功能

不同于在 Windows 中，你无法使用 PowerShell 或 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Configuration Manager 启用可用性组 (AG) 功能。 在 Linux 下，必须使用 `mssql-conf` 启用该功能。 有两种方法可以启用可用性组功能：使用 `mssql-conf` 实用程序，或手动编辑 `mssql.conf` 文件。

> [!IMPORTANT]
> 必须为仅配置副本启用 AG 功能，即使在 [!INCLUDE[ssexpress-md](../includes/ssexpress-md.md)] 上也是如此。

### <a name="use-the-mssql-conf-utility"></a>使用 mssql-conf 实用程序

出现提示时，请发出以下内容：

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
```

### <a name="edit-the-mssqlconf-file"></a>编辑 mssql.conf 文件

还可以修改位于 `/var/opt/mssql` 文件夹下的 `mssql.conf` 文件，以添加以下行：

```
[hadr]

hadr.hadrenabled = 1
```

### <a name="restart-ssnoversion-md"></a>重启 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]
启用可用性组后，与 Windows 一样，必须重启 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]。 可以通过以下方式完成：

```bash
sudo systemctl restart mssql-server
```

## <a name="create-the-availability-group-endpoints-and-certificates"></a>创建可用性组终结点和证书

可用性组使用 TCP 终结点进行通信。 在 Linux 下，仅当证书用于身份验证时，才支持 AG 的终结点。 这意味着必须在所有其他实例上还原来自一个实例的证书，这些实例将是参与相同 AG 的副本。 即使对于仅配置副本，也需要证书过程。 

只能通过 Transact-SQL 完成创建终结点和还原证书。 也可以使用非 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 生成的证书。 还需要一个进程来管理和替换任何过期的证书。

> [!IMPORTANT]
> 如果计划使用 [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] 向导创建 AG，则仍需要使用 Linux 上的 Transact-SQL 创建和还原证书。

有关各种命令可用选项的完整语法（例如附加安全性），请参阅：

-   [BACKUP CERTIFICATE](../t-sql/statements/backup-certificate-transact-sql.md)
-   [CREATE CERTIFICATE](../t-sql/statements/create-certificate-transact-sql.md)
-   [CREATE ENDPOINT](../t-sql/statements/create-endpoint-transact-sql.md)

> [!NOTE]
> 虽然你将创建可用性组，但终结点类型使用 FOR DATABASE_MIRRORING，因为某些基础特性曾与现已弃用的功能共享。

此示例将为三节点配置创建证书。 实例名称为 LinAGN1、LinAGN2 和 LinAGN3。

1.  在 LinAGN1 上执行以下操作以创建主密钥、证书和终结点，以及备份证书。 对于本例，终结点使用典型的 TCP 端口 5022。
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN1_Cert
    WITH SUBJECT = 'LinAGN1 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN1_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN1_Cert,
        ROLE = ALL);
    
    GO
    ```
    
2.  在 LinAGN2 上执行相同操作：
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    WITH SUBJECT = 'LinAGN2 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN2_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN2_Cert,
        ROLE = ALL);
    
    GO
    ```
    
3.  最后，在 LinAGN3 上执行相同的序列：
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    WITH SUBJECT = 'LinAGN3 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN3_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN3_Cert,
        ROLE = ALL);
    
    GO
    ```
    
4.  使用 `scp` 或其他实用程序，将证书的备份复制到将成为 AG 一部分的每个节点。
    
    对于本示例：
    
    - 将 LinAGN1_Cert 复制到 LinAGN2 和 LinAGN3
    - 将 LinAGN2_Cert 复制到 LinAGN1 和 LinAGN3。
    - 将 LinAGN3_Cert 复制到 LinAGN1 和 LinAGN2。
    
5.  将所有权和与复制的证书文件相关联的组更改为 `mssql`。
    
    ```bash
    sudo chown mssql:mssql <CertFileName>
    ```
    
6.  在 LinAGN1 上创建与 LinAGN2 和 LinAGN3 关联的实例级登录名和用户。
    
    ```SQL
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
7.  在 LinAGN1 上还原 LinAGN2_Cert 和 LinAGN3_Cert。 具有其他副本的证书是 AG 通信和安全的一个重要方面。
    
    ```SQL
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
8.  Grant the logins associated with LinAG2 and LinAGN3 permission to connect to the endpoint on LinAGN1.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
9.  在 LinAGN2 上创建与 LinAGN1 和 LinAGN3 关联的实例级登录名和用户。
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
10. 在 LinAGN2 上还原 LinAGN1_Cert 和 LinAGN3_Cert。
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    ```
    
11. 授予与 LinAG1 和 LinAGN3 关联的登录权限以连接到 LinAGN2 上的终结点。
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
12. 在 LinAGN3 上创建与 LinAGN1 和 LinAGN2 关联的实例级登录名和用户。
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    ```
    
13. 在 LinAGN3 上还原 LinAGN1_Cert 和 LinAGN2_Cert。 
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    ```
    
14. 授予与 LinAG1 和 LinAGN2 关联的登录权限以连接到 LinAGN3 上的终结点。
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    ```

## <a name="create-the-availability-group"></a>创建可用性组

本文介绍如何使用 [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] 或 Transact-SQL 以创建 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 的可用性组。

### <a name="use-ssmanstudiofull-md"></a>使用 [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]

本部分介绍如何使用 SSMS 和新可用性组向导创建群集类型为“外部”的 AG。

1.  在 SSMS 中展开“AlwaysOn 可用性组”，右键单击“可用性组”并选择“新建可用性组向导”  。

2.  在“简介”对话框上单击“下一步”。

3.  在“指定可用性组选项”对话框中，输入可用性组的名称，并在下拉列表中选择“外部”或“无”群集类型。 在部署 Pacemaker 时应使用“外部”。 “无”适用于专用方案，如读取横向扩展。选择数据库级别运行状况检测选项是可选的。 有关此选项的详细信息，请参阅[可用性组数据库级别运行状况检测故障转移选项](../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md)。 单击“下一步”。

    ![创建可用性组 03](./media/sql-server-linux-create-availability-group/image3.png)

4.  在“选择数据库”对话框中，选择将参与 AG 的数据库。 每个数据库在添加到 AG 之前必须具有完整备份。 单击“下一步”。

5.  在“指定副本”对话框中，单击“添加副本”。

6.  在“连接到服务器”对话框中，输入将作为次要副本的 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 的 Linux 实例的名称以及要连接的凭据。 单击“连接”。

7.  对包含仅配置副本或其他次要副本的实例重复前两个步骤。

8.  现在应该在指定副本对话框中列出所有三个实例。 如果使用“外部”群集类型，对于真正的次要副本，请确保可用性模式与主副本的可用性模式匹配，并将故障转移模式设置为“外部”。 对于仅配置副本，请选择“仅配置”可用性模式。

    下面的示例显示具有两个副本的 AG，一个“外部”群集类型和一个“仅配置”副本。

    ![创建可用性组 04](./media/sql-server-linux-create-availability-group/image4.png)

    下面的示例显示具有两个副本的 AG，一个“无”群集类型和一个“仅配置”副本。

    ![创建可用性组 05](./media/sql-server-linux-create-availability-group/image5.png)

9.  如果要更改备份首选项，请单击“备份首选项”选项卡。有关 AG 备份首选项的详细信息，请参阅[配置可用性副本备份](../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)。

10. 如果使用可读辅助数据库或创建群集类型为“无”的 AG 以进行读取扩展，则可以通过选择“侦听器”选项卡来创建侦听器。稍后还可以添加侦听器。 要创建侦听器，请选择选项“创建可用性组侦听器”并输入名称、TCP/IP 端口以及使用静态或自动分配的 DHCP IP 地址。 请记住，对于群集类型为“无”的 AG，IP 应为静态，并设置为主 IP 地址。

    ![创建可用性组 06](./media/sql-server-linux-create-availability-group/image6.png)

11. 如果为可读方案创建了侦听器，则 SSMS 17.3 或更高版本允许在向导中创建只读路由。 也可以稍后通过 SSMS 或 Transact-SQL 添加它。 立即添加只读路由：

    a.  选择“只读路由”选项卡。

    b.  输入只读副本的 URL。 这些 URL 类似于终结点，只是它们使用的是实例的端口，而不是终结点。

    c.  选择每个 URL，并从底部选择可读副本。 要进行多选，请按住 Shift 或单击并拖动。

12. 单击“下一步”。

13. 选择次要副本的初始化方法。 默认情况下是使用[自动种子设定](../database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group.md)，这需要在所有参与 AG 的服务器上使用相同的路径。 也可以让向导进行备份、复制和还原（第二个选项）；如果已在副本上手动备份、复制和还原数据库，请将其连接（第三个选项）；或稍后添加数据库（最后一个选项）。 与证书一样，如果手动备份和复制证书，则需要在其他副本上设置备份文件的权限。 单击“下一步”。

14. 在“验证”对话框中，如果所以内容都没有成功返回，请进行调查。 某些警告是可接受的而不是致命的，例如，不创建侦听器。 单击“下一步”。

15. 在“摘要”对话框中，单击“完成”。 现在将开始创建 AG。

16. AG 创建完成后，单击“结果”上的“关闭”。 现在可以在动态管理视图中以及 SSMS 中的 AlwaysOn 可用性组文件夹下查看副本上的 AG。

### <a name="use-transact-sql"></a>使用 Transact-SQL

本节介绍使用 Transact-SQL 创建 AG 的示例。 创建 AG 后，可以配置侦听器和只读路由。 可以使用 `ALTER AVAILABILITY GROUP` 修改 AG 本身，但无法在 [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] 中更改群集类型。 如果不打算创建群集类型为“外部”的 AG，则必须将其删除并使用群集类型“无”重新创建它。 可在以下链接中找到详细信息和其他选项：

-   [CREATE AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/create-availability-group-transact-sql.md)（创建可用性组 (Transact-SQL)）
-   [更改可用性组 (Transact-SQL)](../t-sql/statements/alter-availability-group-transact-sql.md)
-   [为可用性组配置只读路由 (SQL Server)](../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)
-   [创建或配置可用性组侦听器 (SQL Server)](../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)

#### <a name="example-one---two-replicas-with-a-configuration-only-replica-external-cluster-type"></a>示例一 - 具有仅配置副本的两个副本（外部群集类型）

此示例显示如何创建使用仅配置副本的双副本 AG。

1.  在将成为主副本的节点上执行，主副本包含数据库的完整读/写副本。 此示例使用自动种子设定。

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1' WITH (
       ENDPOINT_URL = N' TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       SEEDING_MODE = AUTOMATIC),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       AVAILABILITY_MODE = CONFIGURATION_ONLY);
       
    GO
    ```
    
2.  在连接到其他副本的查询窗口中，执行以下操作将副本联接到 AG，并启动从主副本到次要副本的种子设定过程。
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3. 在连接到仅配置副本的查询窗口中，将其联接到 AG。
    
   ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
   ```

#### <a name="example-two---three-replicas-with-read-only-routing-external-cluster-type"></a>示例二 - 具有只读路由的三个副本（外部群集类型）

此示例显示了三个完整副本以及如何将只读路由配置为 AG 初始创建的一部分。

1.  在将成为主副本的节点上执行，主副本包含数据库的完整读/写副本。 此示例使用自动种子设定。

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN2.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:1433')),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN2.FullyQualified.Name:1433')),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN3.FullyQualified.Name:1433'))
    LISTENER '<ListenerName>' (WITH IP = ('<IPAddress>', '<SubnetMask>'), Port = 1433);
    
    GO
    ```
    
    有关此配置的一些注意事项：
    
    - AGName 是可用性组的名称。
    - DBName 是将与可用性组一起使用的数据库的名称。 它也可以是以逗号分隔的名称列表。
    - ListenerName 是一个与任何基础服务器/节点不同的名称。 它将与 IPAddress 一起在 DNS 中注册。
    - IPAddress 是与 ListenerName 关联的 IP 地址 。 它也是唯一的，与任何服务器/节点都不一样。 应用程序和最终用户将使用 ListenerName 或 IPAddress 连接到 AG 。
    - SubnetMask 是 IPAddress 的子网掩码；例如，255.255.255.0 。

2.  在连接到其他副本的查询窗口中，执行以下操作将副本联接到 AG，并启动从主副本到次要副本的种子设定过程。
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3.  对第三个副本重复步骤 2。

#### <a name="example-three---two-replicas-with-read-only-routing-none-cluster-type"></a>示例三 - 具有只读路由的两个副本（“无”群集类型）

此示例显示使用群集类型“无”创建双副本配置。 它用于不需要故障转移的读取扩展场景。 这将使用循环功能创建实际上是主副本的侦听器以及只读路由。

1.  在将成为主副本的节点上执行，主副本包含数据库的完整读/写副本。 此示例使用自动种子设定。

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = NONE)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name: <PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name'.'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:<PortOfInstance>'));
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:<PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL =N'TCP://LinAGN2.FullyQualified.Name:<PortOfInstance>'));
    LISTENER '<ListenerName>' (WITH IP = ('<PrimaryReplicaIPAddress>', '<SubnetMask>'), Port = <PortOfListener>);
    
    GO
    ```
    
    其中
    - AGName 是可用性组的名称。
    - DBName 是将与可用性组一起使用的数据库的名称。 它也可以是以逗号分隔的名称列表。
    - PortOfEndpoint 是所创建终结点使用的端口号。
    - PortOfInstance 是 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 实例使用的端口号。
    - ListenerName 是一个与任何基础副本不同的名称，但实际不会使用该名称。
    - PrimaryReplicaIPAddress 是主要副本的 IP 地址。
    - SubnetMask 是 IPAddress 的子网掩码 。 例如，255.255.255.0。
    
2.  将辅助副本联接到 AG 并启动自动种子设定。
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = NONE);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```

## <a name="create-the-ssnoversion-md-login-and-permissions-for-pacemaker"></a>为 Pacemaker 创建 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 登录和权限

Linux 上基于 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 的 Pacemaker 高可用性群集需要访问 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 实例，以及可用性组本身的权限。 这些步骤用于创建登录和关联权限，以及指示 Pacemaker 如何登录 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 的文件。

1.  在连接到第一个副本的查询窗口中，执行以下命令：

    ```SQL
    CREATE LOGIN PMLogin WITH PASSWORD ='<StrongPassword>';
    
    GO
    
    GRANT VIEW SERVER STATE TO PMLogin;
    
    GO
    
    GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::<AGThatWasCreated> TO PMLogin;
    
    GO
    ```
    
2.  在节点 1 上输入命令 
    ```bash
    sudo emacs /var/opt/mssql/secrets/passwd
    ```
    
    该操作将打开 Emacs 编辑器。
    
3.  在编辑器中输入以下两行：

    ```
    PMLogin
    <StrongPassword>
    ```
    
4.  按住 Ctrl，再按 X，然后按 C 退出并保存文件。

5.  执行 
    ```bash
    sudo chmod 400 /var/opt/mssql/secrets/passwd
    ```
    
    锁定文件。

6.  在充当副本的其他服务器上重复步骤 1-5。

## <a name="create-the-availability-group-resources-in-the-pacemaker-cluster-external-only"></a>在 Pacemaker 群集中创建可用性组资源（仅限外部类型）

在 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 中创建可用性组后，如果指定了“外部”群集类型，则必须在 Pacemaker 中创建相应的资源。 与 AG 关联的资源有两个：AG 本身和 IP 地址。 如果不使用侦听器功能，则可以选择配置 IP 地址资源，但建议这样做。

创建的 AG 资源是一种特殊的资源，称为克隆。 AG 资源基本上在每个节点上都有副本，并且有一个控制资源称为“master”。 master 与承载主副本的服务器相关联。 其他资源承载次要副本（常规副本或仅配置副本），并且可以在故障转移时提升为主副本。

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

1.  用以下语法创建 AG 资源：

    **Red Hat Enterprise Linux (RHEL) 和 Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failure-timeout=30s --master meta notify=true
    ```

    >[!NOTE]
    >在 RHEL 7.4 中，使用 master 可能会遇到警告。 要避免此情况，请使用 `sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failover-timeout=30s master notify=true`
   
    **SUSE Linux Enterprise Server (SLES)**
    
    ```bash
    primitive <NameForAGResource> \
    ocf:mssql:ag \
    params ag_name="<AGName>" \
    meta failure-timeout=60s \
    op start timeout=60s \
    op stop timeout=60s \
    op promote timeout=60s \
    op demote timeout=10s \
    op monitor timeout=60s interval=10s \
    op monitor timeout=60s interval=11s role="Master" \
    op monitor timeout=60s interval=12s role="Slave" \
    op notify timeout=60s
    ms ms-ag_cluster <NameForAGResource> \
    meta master-max="1" master-node-max="1" clone-max="3" \
    clone-node-max="1" notify="true" \
    commit
    ```
    
    其中 NameForAGResource 是针对 AG 的群集资源指定的唯一名称，AGName 是已创建的 AG 的名称 。
 
2.  为将与侦听器功能关联的 AG 创建 IP 地址资源。

    **RHEL 和 Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForIPResource> ocf:heartbeat:IPaddr2 ip=<IPAddress> cidr_netmask=<Netmask>
    ```

    **SLES**
    
    ```bash
    crm configure \
    primitive <NameForIPResource> \
       ocf:heartbeat:IPaddr2 \
       params ip=<IPAddress> \
          cidr_netmask=<Netmask>
    ```
    
    其中 NameForIPResource 是 IP 资源的唯一名称，IPAddress 是分配给资源的静态 IP 地址 。 在 SLES 上还需要提供网络掩码。 例如，对于子网掩码，255.255.255.0 的值为 24。
    
3.  要确保 IP 地址和 AG 资源在同一节点上运行，必须配置并置约束。

    **RHEL 和 Ubuntu**
    
    ```bash
    sudo pcs constraint colocation add <NameForIPResource> <NameForAGResource>-master INFINITY with-rsc-role=Master
    ```

    **SLES**
    
    ```bash
    crm configure <NameForConstraint> inf: \
    <NameForIPResource> <NameForAGResource>:Master 
    commit
    ```
    
    其中 NameForIPResource 是 IP 资源的名称，NameForAGResource 是 AG 资源的名称，在 SLES 上，NameForConstraint 是约束的名称  。

4.  创建排序约束以确保 AG 资源在 IP 地址之前启动并运行。 虽然并置约束意味着排序约束，但这将强制执行它。

    **RHEL 和 Ubuntu**
    
    ```bash
    sudo pcs constraint order promote <NameForAGResource>-master then start <NameForIPResource>
    ```
    
    **SLES**
    
    ```bash
    crm configure \
    order <NameForConstraint> inf: <NameForAGResource>:promote <NameForIPResource>:start
    commit
    ```
    
    其中 NameForIPResource 是 IP 资源的名称，NameForAGResource 是 AG 资源的名称，在 SLES 上，NameForConstraint 是约束的名称  。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何为 Linux 上的 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 创建和配置可用性组。 你已了解如何执行以下操作：
> [!div class="checklist"]
> * 启用可用性组。
> * 创建 AG 终结点和证书。
> * 使用 [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) 或 Transact-SQL 创建 AG。
> * 为 Pacemaker 创建 [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] 登录和权限。
> * 在 Pacemaker 群集中创建 AG 资源。

对于大多数 AG 管理任务，包括升级和故障转移，请参阅：

> [!div class="nextstepaction"]
> [对 Linux 上的 SQL Server 运行 HA 可用性组](sql-server-linux-availability-group-failover-ha.md)

