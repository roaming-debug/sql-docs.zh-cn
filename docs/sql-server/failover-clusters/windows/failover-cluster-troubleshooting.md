---
title: 故障转移群集故障排除 | Microsoft Docs
description: 了解故障转移群集的疑难解答，包括从故障中恢复、解决常见问题以及使用扩展存储过程/COM 对象。
ms.custom: ''
ms.date: 10/21/2015
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- troublshooting, failover clustering
- failover clustering, troubleshooting
- cluster troubleshooting
ms.assetid: 84012320-5a7b-45b0-8feb-325bf0e21324
author: cawrites
ms.author: chadam
ms.openlocfilehash: 75090ce180ff6e71796c9363e39768f09ec3f91b
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96121097"
---
# <a name="failover-cluster-troubleshooting"></a>故障转移群集疑难解答
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题提供有关以下问题的信息：  
  
-   故障排除的基本步骤。  
  
-   从故障转移群集故障中恢复。  
  
-   解决最常见的故障转移群集问题。  
  
-   使用扩展存储过程和 COM 对象。  
  
## <a name="basic-troubleshooting-steps"></a>故障排除的基本步骤  
 第一个诊断步骤是运行最新的群集验证检查。 有关验证的详细信息，请参阅[故障转移群集分步指南：针对故障转移群集验证硬件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10))。  这可以在不中断任何服务的情况下完成，因为它不影响任何联机的群集资源。 一旦故障转移群集功能安装完成，就可以在任何时候，包括部署群集前、创建群集期间以及群集运行时进行验证。 实际上，一旦群集处于使用状态就会执行其他测试，检查高度可用的工作负载是否遵循了最佳做法。 在这几十项测试中，只有几项会影响正在运行的群集工作负载，而这些工作负载都处于存储类别中，因此跳过这整个类别是避免破坏性测试的简便方法。  
故障转移群集附带了内置的保护措施，以防止在验证过程中运行存储测试时出现意外故障时间。 如果在启动验证时该群集有任何联机组，并且存储测试保持选定状态，则会提示用户确认是否要要运行所有测试（会导致故障时间），或跳过所有联机组的磁盘测试以避免故障时间。 如果已从测试中排除整个存储类别，则不会显示此提示。 这可以使群集验证在没有故障时间的情况下进行。  
  
#### <a name="how-to-revalidate-your-cluster"></a>如何重新验证群集  
  
1.  在故障转移群集管理单元中，控制台树内，确保选定“故障转移群集管理”，然后在“管理”下面，单击“验证配置”。  
  
2.  按照向导中的说明指定服务器和测试，然后运行这些测试。 在运行测试之后会显示 **“摘要”** 页。  
  
3.  仍位于“摘要”  页面时，单击“查看报告”  以查看测试结果。  
  
     若要在关闭向导之后查看测试结果，请参阅 **%SystemRoot%\Cluster\Reports\Validation Report date and time.html** ，其中 **%SystemRoot%** 是安装操作系统的文件夹（例如， **C:\Windows**）。  
  
4.  若要查看可帮助你解释测试结果的帮助主题，请单击“关于群集验证测试的详细信息” 。  
  
 若要在关闭向导后查看关于群集验证的帮助主题，在故障转移群集管理单元中，依次单击“帮助”、“帮助主题”和“内容”选项卡，展开故障转移群集帮助的内容并单击“验证故障转移群集配置”。  验证向导运行完毕后，“摘要报告”  会显示结果。 所有带绿色复选标记或者在某些情况下带黄色三角形标记（警告）的测试表示通过。 查看问题区域（红色的 X 或黄色的问号）时，在汇总测试结果的报表部分中，单击单个测试以查看详细信息。 任何红色的 X 标示的问题必须在疑难解答 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 问题之前解决。  
  
 **安装更新**  
  
 安装更新是避免系统出现问题的重要部分。 有用的链接：  
  
-   [推荐用于基于 Windows Server 2012 R2 的故障转移群集的修补程序和更新](https://support.microsoft.com/kb/2920151)  
  
-   [推荐用于基于 Windows Server 2012 的故障转移群集的修补程序和更新](https://support.microsoft.com/kb/2784261)  
  
-   [推荐用于基于 Windows Server 2008 R2 的故障转移群集的修补程序和更新](https://support.microsoft.com/kb/980054)  
  
-   [推荐用于基于 Windows Server 2008 的故障转移群集的修补程序和更新](https://support.microsoft.com/kb/957311)  
  
## <a name="recovering-from-failover-cluster-failure"></a>从故障转移群集故障中恢复  
 通常，故障转移群集故障有以下两个原因：  
  
-   双节点群集的一个节点出现硬件故障。 此硬件故障可能是由 SCSI 卡或操作系统中的故障造成的。  
  
     若要从此故障中恢复，请使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序将故障节点从故障转移群集中删除，在计算机脱机的情况下解决硬件故障，重新将计算机联机，然后将修复后的节点添加回故障转移群集实例。  
  
     有关详细信息，请参阅[创建新的 SQL Server 故障转移群集（安装程序）](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)和[从故障转移群集实例失败中恢复](../../../sql-server/failover-clusters/windows/recover-from-failover-cluster-instance-failure.md)。  
  
-   操作系统故障。 在这种情况下，节点处于脱机状态，但并非无法解决该故障。  
  
     若要从操作系统故障中恢复，请恢复节点并测试故障转移。 如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例未能正确地进行故障转移，必须使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序从故障转移群集中删除 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，进行必要的修复，重新将计算机联机，然后将修复后的节点添加回故障转移群集实例。  
  
     使用此方法从操作系统故障中恢复可能要花些时间。 如果很容易从操作系统故障中恢复，请不要使用此方法。  
  
     有关详细信息，请参阅[创建新的 SQL Server 故障转移群集（安装程序）](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)和[如何：从故障转移群集失败中恢复（案例 2）](recover-from-failover-cluster-instance-failure.md)。  
  
## <a name="resolving-common-problems"></a>解决常见问题  
 以下列表介绍了常见的使用问题并说明如何解决这些问题。  
  
### <a name="problem-incorrect-use-of-command-prompt-syntax-to-install-sql-server"></a>问题：不正确使用命令提示语法安装 SQL Server  
 **问题 1：** 在从命令提示符使用 **/qn** 开关时，很难诊断安装程序问题，因为 **/qn** 开关取消了所有安装程序对话框和错误消息。 如果指定了 **/qn** 开关，则所有安装程序消息（包括错误消息）都将写入安装程序日志文件。 有关日志文件的详细信息，请参阅 [查看和阅读 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  
  
 **解决方法 1**：使用 /qb 开关替代 /qn 开关 。 如果使用 **/qb** 开关，将显示每个步骤中的基本 UI（包括错误消息）。  
  
### <a name="problem-sql-server-cannot-log-on-to-the-network-after-it-migrates-to-another-node"></a>问题：在迁移到另一个节点之后，SQL Server 无法登录到网络  
 **问题 1：** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户无法与域控制器取得联系。  
  
 **解决方法 1**：检查事件日志以查看是否存在网络连接问题，例如适配器故障或 DNS 问题。 验证是否能成功对域控制器运行 ping 命令。  
  
 **问题 2：** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户密码在所有群集节点上并非全都一致，或者节点没有重启从失败的节点迁移过来的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务。  
  
 **解决方法 2：** 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器更改 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户密码。 如果不这样做，并且更改了其中一个节点上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户密码，也必须更改所有其他节点上的密码。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器会自动执行此操作。  
  
### <a name="problem-sql-server-cannot-access-the-cluster-disks"></a>问题：SQL Server 无法访问群集磁盘  
 **问题 1：** 并未在所有节点上都更新了固件或驱动程序。  
  
 **解决方法 1：** 确保所有节点都使用正确的固件版本和相同的驱动程序版本。  
  
 **问题 2：** 如果失败的节点位于具有不同驱动器号的共享群集磁盘上，则其他节点无法恢复从该节点迁移过来的群集磁盘。  
  
 **解决方法 2：** 两台服务器上的群集磁盘的磁盘驱动器号必须相同。 如果不相同，请检查操作系统和 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 群集服务 (MSCS) 的原始安装。  
  
### <a name="problem-failure-of-a-sql-server-service-causes-failover"></a>问题：SQL Server 服务故障导致故障转移  
 **解决方法：** 若要防止特定服务的故障导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 组进行故障转移，请使用 Windows 中的群集管理器配置这些服务，如下所示：  
  
-   在 **“全文属性”** 对话框的 **“高级”** 选项卡中，清除 **“影响组”** 复选框。 但是，如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 导致故障转移，则全文搜索服务将重新启动。  
  
### <a name="problem-sql-server-does-not-start-automatically"></a>问题：SQL Server 不自动启动  
 **解决方法：** 使用 MSCS 中的群集管理器自动启动某个故障转移群集。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务应该设置为手动启动，应该在 MSCS 中配置群集管理器，使其启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务。 有关详细信息，请参阅 [管理服务](https://msdn.microsoft.com/library/ms178096\(v=sql.105\).aspx)。  
  
### <a name="problem-the-network-name-is-offline-and-you-cannot-connect-to-sql-server-using-tcpip"></a>问题：网络名称离线，无法使用 TCP/IP 连接到 SQL Server  
 **问题 1：** DNS 失败，而群集资源设置为需要 DNS。  
  
 **解决方法 1：** 更正 DNS 问题。  
  
 **问题 2：** 网络上存在重复的名称。  
  
 **解决方法 2：** 使用 NBTSTAT 查找重复的名称，然后更正问题。  
  
 **问题 3：** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不是使用命名管道进行连接。  
  
 **解决方法 3：** 若要使用命名管道进行连接，请使用 SQL Server 配置管理器创建一个别名，以连接到适当的计算机。 例如，如果有一个群集包含两个节点（**节点 A** 和 **节点 B**）和一个具有默认实例的故障转移群集实例 (**Virtsql**)，则可以执行下列步骤连接到网络名称资源已离线的服务器：  
  
1.  使用群集管理器确定包含 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的组在哪个节点上运行。 对于本例，此节点是 **节点 A**。  
  
2.  使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] net start **在该计算机上启动** 服务。 有关使用 **net start** 的详细信息，请参阅 [手动启动 SQL Server](https://msdn.microsoft.com/library/ms191193\(v=sql.105\).aspx)。  
  
3.  在 **节点 A** 上启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] SQL Server 配置管理器。查看服务器正在侦听的管道名称。 它应类似于 \\\\.\\$$\VIRTSQL\pipe\sql\query。  
  
4.  在客户端计算机上，启动 SQL Server 配置管理器。  
  
5.  创建别名 SQLTEST1 以通过命名管道连接到此管道名称。 为此，请输入 **节点 A** 作为服务器名称并将管道名称编辑为 \\\\.\pipe\\$$\VIRTSQL\sql\query。  
  
6.  使用别名 SQLTEST1 作为服务器名称连接到此实例。  
  
### <a name="problem-sql-server-setup-fails-on-a-cluster-with-error-11001"></a>问题：群集上的 SQL Server 安装程序因发生错误 11001 而失败  
 **问题：** [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL.X\Cluster] 中存在孤立的注册表项  
  
 **解决方法：** 确保当前未使用 MSSQL.X 注册表配置单元，然后删除该群集项。  
  
### <a name="problem-cluster-setup-error-the-installer-has-insufficient-privileges-to-access-this-directory-drivemicrosoft-sql-server-the-installation-cannot-continue-log-on-as-an-administrator-or-contact-your-system-administrator"></a>问题：群集安装错误：“安装程序没有足够的特权，无法访问此目录: \<drive>\Microsoft SQL Server。 安装无法继续。 请以管理员身份登录，或与您的系统管理员联系”  
 **问题：** 此错误是由于 SCSI 共享驱动器未正确分区而引起的。  
  
 **解决方法：** 执行下列步骤以在该共享磁盘上重新创建一个分区：  
  
1.  从群集中删除磁盘资源。  
  
2.  删除磁盘上的所有分区。  
  
3.  在磁盘属性中验证磁盘是否为基本磁盘。  
  
4.  在共享磁盘上创建一个分区、格式化该磁盘并为磁盘分配一个驱动器号。  
  
5.  使用群集管理器 (cluadmin) 将磁盘添加到群集中。  
  
6.  运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序。  
  
### <a name="problem-applications-fail-to-enlist-sql-server-resources-in-a-distributed-transaction"></a>问题：应用程序无法在分布式事务中登记 SQL Server 资源  
 **问题：** 由于在 Windows 中未完全配置 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC)，因此应用程序可能无法在分布式事务中登记 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源。 此问题可能会影响使用分布式事务的链接服务器、分布式查询和远程存储过程。 有关如何配置 MS DTC 的详细信息，请参阅 [Before Installing Failover Clustering](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)。  
  
 **解决方法：** 若要避免此类问题，必须在安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和配置 MS DTC 的服务器上完全启用 MS DTC 服务。  
  
 若要完全启用 MS DTC，请执行下列步骤：  
  
1.  在控制面板中打开 **“管理工具”** ，然后打开 **“计算机管理”** 。  
  
2.  在“计算机管理”的左侧面板中，展开 **“服务和应用程序”** ，然后单击 **“服务”** 。  
  
3.  在“计算机管理”的右窗格中，右键单击“分布式事务处理协调器”，并选择“属性”。  
  
4.  在 **“Distributed Transaction Coordinator 的属性”** 窗口中，单击 **“常规”** 选项卡，再单击 **“停止”** 来停止此服务。  
  
5.  在“分布式事务处理协调器”窗口中，单击“登录”选项卡，将登录帐户设置为 NT AUTHORITY\NetworkService。  
  
6.  单击 **“应用”** 和 **“确定”** 以关闭 **“分布式事务处理协调器”** 窗口。 关闭 **“计算机管理”** 窗口。 关闭 **“管理工具”** 窗口。  
  
## <a name="using-extended-stored-procedures-and-com-objects"></a>使用扩展存储过程和 COM 对象  
 如果在故障转移群集配置中使用扩展存储过程，所有扩展存储过程都必须安装在与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]相关的群集磁盘中。 这样做可确保当某节点发生故障转移时，仍能继续使用扩展存储过程。  
  
 如果扩展存储过程使用 COM 组件，管理员必须在群集的所有节点中都注册 COM 组件。 活动节点的注册表中必须包含加载和执行 COM 组件所需的信息，才能创建这些组件。 否则，这些信息将保留在最先注册 COM 组件的计算机的注册表中。  
  
## <a name="see-also"></a>另请参阅  
 [查看和读取 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [扩展存储过程的工作方式](../../../relational-databases/extended-stored-procedures-programming/how-extended-stored-procedures-work.md)   
 [扩展存储过程的执行特征](../../../relational-databases/extended-stored-procedures-programming/execution-characteristics-of-extended-stored-procedures.md)  
  
