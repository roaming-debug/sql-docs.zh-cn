---
title: 创建域独立可用性组
description: 创建使用工作组群集的可用性组的步骤。 这使 SQL Server 2016（及更高版本）能够在不需要 Active Directory 域服务的 WSFC 基础上部署 AlwaysOn 可用性组，因此每个服务器不需要属于相同域。
ms.custom: seodec18
ms.date: 09/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], domain independent
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6c809abd5c333013b197dd48ca2fb4832bf6b615
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643603"
---
# <a name="create-a-domain-independent-availability-group"></a>创建域独立可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

AlwaysOn 可用性组 (AG) 需要一个基础 Windows Server 故障转移群集 (WSFC)。 通过 Windows Server 2012 R2 部署 WSFC 始终要求加入 WSFC（也称为节点）的服务器都联接到相同域。 有关 Active Directory 域服务 (AD DS) 的详细信息，请参阅[此处](/previous-versions/windows/it-pro/windows-server-2003/cc759073(v=ws.10))。

与之前通过数据库镜像 (DBM) 配置部署的内容相比，AD DS 和 WSFC 依赖项更加复杂，因为可使用证书跨多个数据中心部署 DBM，而无需任何此类依赖项。  跨越多个数据中心的传统可用性组要求所有服务器必须加入相同 Active Directory 域 - 加入不同的域，甚至加入受信任的域均不起作用。 所有服务器必须都是相同 WSFC 的节点。 下图显示了这一配置。 SQL Server 2016 也具有分布式可用性组，它们也可以不同方式实现此目标。


![跨越两个数据中心的 WSFC 连接到相同域][1]

Windows Server 2012 R2 引入了[与 Active Directory 分离的群集](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970(v=ws.11))，它是 Windows Server 故障转移群集的一种特殊形式，可与可用性组一起使用。 这种类型的 WSFC 仍要求节点以域加入到相同的 Active Directory 域，但在这种情况下，WSFC 使用 DNS，而不使用域。 由于仍涉及域，因此与 Active Directory 分离的群集仍无法提供与域完全无关的体验。

Windows Server 2016 引入了一种基于与 Active Directory 分离的群集的新的 Windows Server 故障转移群集 - 工作组集群。 工作组群集允许 SQL Server 2016 基于无需 AD DS 的 WSFC 部署可用性组。 SQL Server 要求使用证书以获得终结点安全性，就像数据库镜像方案需要证书一样。  此类型的可用性组称为域独立可用性组。 使用基础工作组群集部署可用性组可以支持构成 WSFC 的节点的以下组合：
- 没有任何节点加入到域。
- 所有节点都加入到不同的域。
- 节点混合，包括已加入域的节点和未加入域的节点。

下图显示了域独立可用性组的一个示例，其中数据中心 1 中的节点已加入域，但数据中心 2 中的节点则仅使用 DNS。 在这种情况下，在将为 WSFC 的节点的所有服务器上设置 DNS 后缀。 访问可用性组的每个应用程序和服务器必须看到相同的 DNS 信息。


![具有两个加入域的节点的工作组群集][2]

域独立可用性组不仅仅用于多站点或灾难恢复方案。 它还可在单个数据中心部署，甚至可与[基本可用性组](basic-availability-groups-always-on-availability-groups.md)（也称为标准版可用性组）配合使用，提供类似于以前使用数据库镜像和所示证书实现的体系结构。


![标准版 AG 的高级视图][3]

部署域独立可用性组具有一些已知的注意事项：
- 可与仲裁配合使用的唯一见证服务器类型是磁盘和[云](/windows-server/failover-clustering/deploy-cloud-witness)，这是 Windows Server 2016 中的新增功能。 磁盘可能会出现问题，因为可用性组很有可能不会使用共享磁盘。
- 虽然只能使用 PowerShell 创建 WSFC 的基础工作组群集变体，但之后可以使用故障转移群集管理器进行管理。
- 如果需要 Kerberos，则必须部署附加到 Active Directory 域的标准 WSFC，可能无需域独立可用性组。
- 在可以配置侦听器时，必须先在 DNS 中注册侦听器然后才能使用。 如上所述，没有任何 Kerberos 支持侦听器。
- 连接到 SQL Server 的应用程序应主要使用 SQL Server 身份验证，因为域可能不存在，或可能未配置为协同工作。 
- 在配置可用性组时将使用证书。

## <a name="set-and-verify-the-dns-suffix-on-all-replica-servers"></a>在所有副本服务器上设置并验证 DNS 后缀

域独立可用性组的工作组群集需要常见 DNS 后缀。 若要在将托管可用性组的副本的每个 Windows Server 上设置和验证 DNS 后缀，请遵循以下说明：

1. 使用 Windows 键 + X 快捷方式，选择“系统”。
2. 如果计算机名和完整计算机名相同，则尚未设置 DNS 后缀。 例如，如果计算机名为 ALLAN，则完整计算机名的值不应为 ALLAN。 它应该类似于 ALLAN.SQLHA.LAB。 SQLHA.LAB 是 DNS 后缀。 工作组的值应显示为 WORKGROUP。 如果需要设置 DNS 后缀，选择“更改设置”。
3. 在“系统属性”对话框中，在“计算机名”选项卡上单击“更改”。
4. 在“计算机名/域更改”对话框中，单击“更多”。
5. 在“DNS 后缀和 NetBIOS 计算机名”对话框中，输入常见 DNS 后缀作为主 DNS 后缀。 
6. 单击“确定”关闭“DNS 后缀和 NetBIOS 计算机名”对话框。
7. 单击“确定”关闭“计算机名/域更改”对话框。
8. 系统将提示重启服务器，使更改生效。 单击“确定”关闭“计算机名/域更改”对话框。
9. 单击“关闭”关闭“系统属性”对话框。
10. 系统将提示重启计算机。 如果不想立即重启，单击“稍后重启”，否则单击“立即重启”。
11. 重新启动服务器后，再次查看系统，验证已配置常见 DNS 后缀。

![成功配置了 DNS 后缀][4]

  > [!NOTE]
  > 若要使用多个子网且有静态 DNS，必须先制定用于更新与侦听器关联的 DNS 记录的流程，再执行故障转移，否则网络名称将无法联机。

## <a name="create-a-domain-independent-availability-group"></a>创建域独立可用性组

当前，完全使用 SQL Server Management Studio 无法实现创建域独立可用性组。 尽管创建域独立可用性组与创建常规可用性组基本相同，但某些方面（如创建证书）只有使用 Transact-SQL 才能实现。 以下示例假定具有两个副本的可用性组配置：一个主要副本和一个次要副本。 

1. [使用此链接中的说明](https://techcommunity.microsoft.com/t5/Failover-Clustering/Workgroup-and-Multi-domain-clusters-in-Windows-Server-2016/ba-p/372059)部署工作组群集，该群集由将加入可用性组的所有服务器组成。 确保在配置工作组群集之前已配置常见 DNS 后缀。
2. 在将加入可用性组的每个实例上[启用 AlwaysOn 可用性组功能](./enable-and-disable-always-on-availability-groups-sql-server.md)。 这需要重新启动每个 SQL Server 实例。
3. 将托管主要副本的每个实例都需要数据库主密钥。 如果主密钥不存在，运行以下命令：

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Strong Password';
   GO
   ```

4. 在将为主要副本的实例上，创建将用于次要副本上的入站连接和用于保护主要副本上的终结点的证书。

   ```sql
   CREATE CERTIFICATE InstanceA_Cert 
   WITH SUBJECT = 'InstanceA Certificate';
   GO
   ``` 

5. 备份该证书。 如有必要，还可以使用私钥提供进一步保护。 此示例不使用私钥。

   ```sql
   BACKUP CERTIFICATE InstanceA_Cert 
   TO FILE = 'Backup_path\InstanceA_Cert.cer';
   GO
   ```

6. 重复步骤 4 和 5 以创建和备份每个次要副本的证书，并对证书采用适当的名称，如 InstanceB_Cert。
7. 在主要副本上，必须为可用性组的每个次要副本创建登录名。 此登录名将被授予权限，以连接到域独立可用性组使用的终结点。 例如，对于名为 InstanceB 的副本：

   ```sql
   CREATE LOGIN InstanceB_Login WITH PASSWORD = 'Strong Password';
   GO
   ```

8. 在每个次要副本上，为主要副本创建登录名。 此登录名将被授予权限，以连接到终结点。 例如，在名为 InstanceB 的副本上：

   ```sql
   CREATE LOGIN InstanceA_Login WITH PASSWORD = 'Strong Password';
   GO
   ```

9. 在所有实例上，为创建的每个登录名创建一个用户。 在还原证书时会使用此用户。 例如，若要为主要副本创建用户：

   ```sql
   CREATE USER InstanceA_User FOR LOGIN InstanceA_Login;
   GO
   ```

10. 对于可能是主要副本的任何副本，在所有相关次要副本上创建登录名和用户。
11. 在每个实例上，为创建了登录名和用户的其他实例还原证书。 在主要副本上，还原所有次要副本证书。 在每个次要副本上，还原主要副本的证书，另外在任何其他副本上，也可能是还原主要副本的证书。 例如：

   ```sql
   CREATE CERTIFICATE [InstanceB_Cert]
   AUTHORIZATION InstanceB_User
   FROM FILE = 'Restore_path\InstanceB_Cert.cer'
   ```

12. 创建可用性组可在将为副本的实例上使用的终结点。 对于可用性组，终结点必须具有类型 DATABASE_MIRRORING。 终结点使用在步骤 4 中为该实例创建的证书进行身份验证。 使用证书创建终结点的示例语法如下所示。 使用合适的加密方法和其他与环境相关的选项。 有关可用选项的详细信息，请参阅 [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md)（创建终结点 (Transact-SQL)）。

   ```sql
   CREATE ENDPOINT DIAG_EP
   STATE = STARTED
   AS TCP (   
    LISTENER_PORT = 5022,
    LISTENER_IP = ALL
         )
   FOR DATABASE_MIRRORING (
    AUTHENTICATION = CERTIFICATE InstanceX_Cert,
    ROLE = ALL
         )
   ```

13. 向在步骤 9 中在该实例上创建的每个用户分配权限，使其能够连接到终结点。 

   ```sql
   GRANT CONNECT ON ENDPOINT::DIAG_EP TO [InstanceX_User];
   GO
   ```

14. 配置基础证书和终结点安全性后，使用你喜欢的方法创建可用性组。 建议手动备份、复制和还原用于初始化次要副本的备份，或使用[自动种子设定](automatically-initialize-always-on-availability-group.md)。 使用向导初始化次要副本涉及使用服务器消息块 (SMB) 文件，在使用未加入域的工作组群集时该文件可能不起作用。
15. 创建侦听器时，请确保在 DNS 中注册其名称和 IP 地址。

### <a name="next-steps"></a>后续步骤 

- [使用可用性组向导 (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

- [使用“新建可用性组”对话框 (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
- [使用 Transact-SQL 创建可用性组](create-an-availability-group-transact-sql.md)

<!--Image references-->
[1]: ./media/diag-wsfc-two-data-centers-same-domain.png
[2]: ./media/diag-workgroup-cluster-two-nodes-joined.png
[3]: ./media/diag-high-level-view-ag-standard-edition.png
[4]: ./media/diag-successful-dns-suffix.png