---
title: 获取 & 配置备份服务器
description: 本文介绍如何将非设备 Windows 系统配置为备份服务器，以与分析平台系统中的备份和还原功能 (AP) 和并行数据仓库 (PDW) 一起使用。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: b6e0bbfd3acbc7616fca0f180d6de3238a724231
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489717"
---
# <a name="acquire-and-configure-a-backup-server-for-parallel-data-warehouse"></a>为并行数据仓库获取和配置备份服务器
本文介绍如何将非设备 Windows 系统配置为备份服务器，以与分析平台系统中的备份和还原功能 (AP) 和并行数据仓库 (PDW) 一起使用。  
  
  
## <a name="backup-server-basics"></a><a name="Basics"></a>备份服务器基础知识  
备份服务器：  
  
-   由你自己的 IT 团队提供和管理。  
  
-   不需要任何 PDW 特定的软件或工具。 PDW 不会将任何软件安装到备份服务器上。  
  
-   位于自己的非设备机架中，不能放置在 AP 设备中。  
  
-   可以连接到设备的 "无线网络"。 可以通过不受控制的或以太网执行备份;出于性能原因，建议使用 "不限"。  
  
-   位于你自己的客户域中，而不是设备域。 你的客户域与设备域之间没有信任关系。  
  
-   承载备份文件共享，这是使用服务器消息块 (SMB) 应用程序级网络协议的 Windows 文件共享。 备份文件共享权限为 Windows 域用户 (通常这是一个专用备份用户) 在该共享上执行备份和还原操作的能力。 Windows 域用户的用户名和密码凭据存储在 PDW 中，以便 PDW 可以对备份文件共享执行备份和还原操作。  
  
## <a name="step-1-determine-capacity-requirements"></a><a name="Step1"></a>步骤1：确定容量要求  
备份服务器的系统要求几乎完全取决于您自己的工作负载。 在购买或预配备份服务器之前，需要确定容量需求。 备份服务器不必专用于备份，前提是它将处理工作负荷的性能和存储要求。 你还可以有多个备份服务器，以将每个数据库备份并还原到多个服务器中的一个。  
  
使用 [备份服务器容量规划工作表](backup-capacity-planning-worksheet.md) 来帮助确定容量需求。  
  
## <a name="step-2-acquire-the-backup-server"></a><a name="Step2"></a>步骤2：获取备份服务器  
既然您更好地了解了容量要求，您可以规划需要购买或预配的服务器和网络组件。 将以下要求列表纳入购买计划，然后购买服务器或预配现有服务器。  
  
### <a name="software-requirements"></a>软件要求  
使用 Windows 文件共享 (SMB) 协议的任何文件服务器。  
  
建议 Windows Server 2012 或更高版本，以便：  
  
-   获取通过 SMB 进行的文件预分配的性能优势。  
  
-   使用即时文件初始化 (IFI) 执行备份操作。 你的 IT 团队在备份服务器上管理此设置。 PDW Configuration Manager ( # A0) 未设置或控制您的备份服务器上的 IFI。 以前版本的 Windows 没有 IFI，但仍可用作备份服务器。  
  
### <a name="networking-requirements"></a>网络要求  
尽管不是必需的，但建议使用备份服务器的连接类型。 准备将加载服务器连接到设备的 "不工作网络"：  
  
1.  计划将服务器的机架接近于设备，以便可以将其连接到设备未被交换机。 若要深入了解有关不受阻止的 Mellanox 技术的详细信息，请参阅白皮书 [简介](https://www.mellanox.com/pdf/whitepapers/IB_Intro_WP_190.pdf)。  
  
2.  购买 Mellanox ConnectX-3 FDR 无限单或双端口网络适配器。 建议购买包含两个端口的网络适配器，以在数据传输期间获得容错。 需要两个端口网络适配器才能实现高可用性。  
  
3.  为双端口卡购买2个 FDR 的，或者为单端口卡购买1个 FDR 的电源线。 FDR 的无线电缆会将加载服务器连接到设备的 "无线网络"。 缆线长度取决于负载服务器与设备不会的交换机之间的距离，取决于您的环境。  
  
## <a name="step-3-connect-the-server-to-the-infiniband-networks"></a><a name="Step3"></a>步骤3：将服务器连接到不工作网络  
使用以下步骤将加载服务器连接到 "未占用网络"。 如果服务器未使用 "未使用" 网络，请跳过此步骤。  
  
1.  使服务器接近于设备，以便可以将其连接到设备的 "无线网络"。  
  
2.  将不限的 Mellanox ConnectX-3 FDR 的网络适配器安装到加载服务器。  
  
3.  使用 FDR 电缆将无线网络适配器连接到第一个设备机架中的两个未被使用的交换机之一。  
  
4.  为未被配置的网络适配器安装和配置适当的 Windows 驱动程序。  
  
    -   适用于 Windows 的不受使用的驱动程序由 OpenFabrics 联盟（一种行业协会，这是一家不受人  正确的驱动程序可能已随您的 "未使用" 网络适配器一起分发。 否则，可以从 www.openfabrics.org 下载该驱动程序。  
  
5.  为网络适配器配置 "未设置" 和 "DNS" 设置。 有关配置说明，请参阅 [配置无线网络适配器](configure-infiniband-network-adapters.md)。  
  
## <a name="step-4-configure-the-backup-file-share"></a><a name="Step4"></a>步骤4：配置备份文件共享  
PDW 通过 UNC 文件共享来访问备份服务器。 若要设置文件共享：  
  
1.  在备份服务器上创建一个用于存储备份的文件夹。  
  
2.  为备份文件夹创建名为备份共享的文件共享。  
  
3.  指定或创建要用于执行备份和还原的客户域中的 Windows 域帐户。 出于安全原因，最好使用专用帐户作为备份用户。  
  
4.  添加对备份共享的权限，以便只有受信任的帐户和域备份帐户才能访问、读取和写入共享位置。  
  
5.  向 PDW 添加备份域帐户凭据。  
  
    例如：  
  
    ```sql  
    EXEC sp_pdw_add_network_credentials '10.192.147.63', 'seattle\david', '********';  
    ```  
  
    有关详细信息，请参阅以下存储过程：  
  
    -   [sp_pdw_add_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md)  
  
    -   [sp_pdw_remove_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md)  
  
## <a name="step-5-start-backing-up-your-data"></a><a name="Step5"></a>步骤5：开始备份数据  
你现在已准备好开始将数据备份到备份服务器。  
  
若要备份数据，请使用查询客户端连接到 SQL Server PDW，然后提交备份数据库或还原数据库命令。 使用 DISK = 子句来指定备份服务器和备份位置。  
  
> [!IMPORTANT]  
> 请记住使用备份服务器的未使用的 IP 地址。 否则，数据将通过以太网复制，而不是不受控制。  
  
例如：  
  
```sql  
BACKUP DATABASE Invoices TO DISK = '\\10.172.14.255\backups\yearly\Invoices2013Full';  
  
RESTORE DATABASE Invoices2013Full  
FROM DISK = '\\10.172.14.255\backups\yearly\Invoices2013Full'  
```  
  
有关详细信息，请参阅： 
  
-   [BACKUP DATABASE](../t-sql/statements/backup-transact-sql.md?view=aps-pdw-2016&preserve-view=true)   
  
-   [还原数据库](../t-sql/statements/restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)  
  
## <a name="security-notices"></a><a name="Security"></a>安全通知  
备份服务器未加入设备的专用域。 它位于你自己的网络中，你自己的域和专用设备域之间没有信任关系。  
  
由于 PDW 备份并未存储在设备上，因此 IT 团队负责管理备份安全性的所有方面。 例如，这包括管理备份数据的安全性、用于存储备份的服务器的安全性以及将备份服务器连接到 AP 设备的网络基础结构的安全性。  
  
### <a name="manage-network-credentials"></a>管理网络凭据  
  
对备份目录的网络访问权限基于标准 Windows 文件共享安全。 在执行备份之前，需要创建或指定将用于向备份目录验证 PDW 的 Windows 帐户。 此 Windows 帐户必须有权访问、创建和写入备份目录。  
  
> [!IMPORTANT]  
> 若要降低数据的安全风险，建议指定一个 Windows 帐户专门用于执行备份和还原操作。 仅允许此帐户访问备份位置，不要授予对其他位置的访问权限。  
  
若要将用户名和密码存储在 PDW 中，请使用 [sp_pdw_add_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md) 存储过程。 PDW 使用 Windows 凭据管理器来存储和加密控件节点和计算节点上的用户名和密码。 不使用 BACKUP DATABASE 命令备份凭据。  
  
若要从 PDW 删除网络凭据，请使用 [sp_pdw_remove_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md) 存储过程。  
  
若要列出 SQL Server PDW 中存储的所有网络凭据，请使用 [sys.dm_pdw_network_credentials](../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md) 动态管理视图。  
  
### <a name="secure-communications"></a>安全通信  
  
加载服务器上的操作可以使用 UNC 路径从受信任的内部网络外部拉取数据。 网络上或具有影响名称解析能力的攻击者可以截获或修改发送到 PDW 的数据。 这会带来篡改和信息泄露的风险。 有助于降低篡改风险：

- 需要对连接进行签名。 
- 在加载服务器上，在 "安全设置 \ 本地策略 \ 安全选项： Microsoft 网络客户端：对通信进行数字签名 (始终) ：已启用" 中设置以下组策略选项。  
  
## <a name="see-also"></a>另请参阅  
[备份和还原](backup-and-restore-overview.md)  
