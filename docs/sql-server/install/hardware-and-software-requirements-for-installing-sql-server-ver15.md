---
title: SQL Server 2019：硬件和软件要求
description: 安装和运行 SQL Server 2019 的硬件、软件和操作系统要求的列表。
ms.custom: sqlfreshmay19
ms.date: 09/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Setup [SQL Server], software
- software [SQL Server]
- installing SQL Server, software
- operating systems [SQL Server], SQL Server requirements
- Setup [SQL Server], cross-language support
- operating systems [SQL Server], cross-language support
- network connections [SQL Server], requirements
- disk space [SQL Server], SQL Server installations
- WOW [SQL Server]
- Setup [SQL Server], hardware
- dependencies [SQL Server], SQL Server installations
- cluster hardware requirements [SQL Server]
- endpoints [SQL Server], SQL Server installations
- Internet [SQL Server], SQL Server installations
- hardware [SQL Server]
- Windows on Windows [SQL Server]
- installing SQL Server, hardware
- Setup Configuration Checker
- SCC [SQL Server]
- operating systems [SQL Server]
- space [SQL Server], SQL Server installations
- system configuration checker
- installing SQL Server, cross-language support
- Internet [SQL Server]
- space [SQL Server]
- extended system support [SQL Server]
- 64-bit edition [SQL Server]
- failover clustering [SQL Server]
- failover clustering [SQL Server], hardware requirements
- 32-bit edition [SQL Server]
- locales [SQL Server], SQL Server installations
- cross-language support
- disk space [SQL Server]
- localized SQL Server versions
ms.assetid: 09bcf20b-0a40-4131-907f-b61479d5e4d8
ms.author: chadam
author: cawrites
ms.openlocfilehash: 52c29a7de29a324d7bda2fba4cb52073cf62d752
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352432"
---
# <a name="sql-server-2019-hardware-and-software-requirements"></a>SQL Server 2019：硬件和软件要求
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

本文列出了在 Windows 操作系统上安装和运行 SQL Server 2019 至少需要满足的硬件和软件要求。

有关其他版本的 SQL Server 的硬件和软件要求，请参阅：
- [SQL Server 2016 和 2017](hardware-and-software-requirements-for-installing-sql-server.md)
- [Linux 上的 SQL Server](../../linux/sql-server-linux-setup.md#system)
- [大数据群集](../../big-data-cluster/deployment-guidance.md)

##  <a name="hardware-requirements"></a><a name="pmosr"></a> 硬件要求  
 以下内存和处理器要求适用于所有版本的 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]：  
  
|组件|要求|  
|---------------|-----------------|  
|硬盘|[!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 要求最少 6 GB 的可用硬盘空间。<br/><br/> 磁盘空间要求将随所安装的 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 组件不同而发生变化。 有关详细信息，请参阅本文后面部分的[硬盘空间要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#HardDiskSpace) 。 有关支持的数据文件存储类型的信息，请参阅 [Storage Types for Data Files](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes)。|  
|监视|[!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 要求有 Super-VGA (800x600) 或更高分辨率的显示器。|  
|Internet|使用 Internet 功能需要连接 Internet（可能需要付费）。|  
|内存\*|**最低要求：**<br/><br/> Express Edition：512 MB<br/><br/> 所有其他版本：1 GB<br/><br/> **推荐：**<br/><br/> Express Edition：1 GB<br/><br/> 所有其他版本：至少 4 GB，并且应随着数据库大小的增加而增加来确保最佳性能。|  
|处理器速度|最低要求：x64 处理器：  1.4 GHz<br/><br/> **推荐：** 2.0 GHz 或更快|  
|处理器类型|x64 处理器：AMD Opteron、AMD Athlon 64、支持 Intel EM64T 的 Intel Xeon，以及支持 EM64T 的 Intel Pentium IV|  
  
> [!NOTE]  
> 仅 x64 处理器支持 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 的安装。 x86 处理器不再支持此安装。  
  
 \*内存至少必须有 2GB RAM，才能在[!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) 中安装[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]组件。此要求不同于 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 的最低内存要求。 有关安装 DQS 的信息，请参阅 [Install Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)。  


##  <a name="software-requirements"></a><a name="hwswr"></a> 软件要求  

以下要求适用于所有安装：  
  
|组件|要求|  
|---------------|-----------------|  
|操作系统|Windows 10 TH1 1507 或更高版本<br/><br>Windows Server 2016 或更高版本<br/><br/>
|.NET Framework|最低版本操作系统包括最低版本 .NET 框架。|  
|网络软件|[!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 支持的操作系统具有内置网络软件。 独立安装项的命名实例和默认实例支持以下网络协议：共享内存、命名管道和 TCP/IP。<br/><br/> |  

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序安装该产品所需的以下软件组件：  
  
   - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client    
   - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序支持文件  


> [!IMPORTANT]
> 对于 PolyBase 功能还有其他硬件和软件要求。 有关详细信息，请参阅 [PolyBase 入门](../../relational-databases/polybase/polybase-guide.md)。  
  

##  <a name="operating-system-support"></a><a name="TOP_Principal"></a> 操作系统支持 

下表显示了与各版本的 Windows 兼容的 SQL Server 2019 版本：  
  

| SQL Server 版本：               | Enterprise | 开发人员 | Standard | Web | Express |  
| :------------------------         | :--------- | :-------- | :------- | :-- | :------ | 
| Windows Server 2019 Datacenter    |    是     |    是    |    是   | 是 |   是   |
| Windows Server 2019 Standard      |    是     |    是    |    是   | 是 |   是   |
| Windows Server 2019 Essentials    |    是     |    是    |    是   | 是 |   是   |
| Windows Server 2016 Datacenter    |    是     |    是    |    是   | 是 |   是   |
| Windows Server 2016 Standard      |    是     |    是    |    是   | 是 |   是   |
| Windows Server 2016 Essentials    |    是     |    是    |    是   | 是 |   是   |
| Windows 10 IoT 企业版         |    否      |    是    |    是   | 否  |   是   |
| Windows 10 企业版             |    否      |    是    |    是   | 否  |   是   |
| Windows 10 专业版           |    否      |    是    |    是   | 否  |   是   |
| Windows 10 家庭版                   |    否      |    是    |    是   | 否  |   是   |
| &nbsp; | &nbsp; |

### <a name="server-core-support"></a>Server Core 支持

以下 Windows Server 版本支持在 Server Core 模式上安装 SQL Server 2019：

:::row:::
    :::column:::
        Windows Server 2019 Core
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Windows Server 2016 Core
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
:::row-end:::

有关如何在 Server Core 上安装 SQL Server 的详细信息，请参阅[在 Server Core 上安装 SQL Server](../../database-engine/install-windows/install-sql-server-on-server-core.md)。 


##  <a name="cross-language-support"></a><a name="CrossLanguageSupport"></a> 跨语言支持  
 有关跨语言支持和以本地化语言安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的注意事项的详细信息，请参阅 [SQL Server 中的本地语言版本](../../sql-server/install/local-language-versions-in-sql-server.md)。  
  
##  <a name="disk-space-requirements"></a><a name="HardDiskSpace"></a> 磁盘空间要求  
 在安装 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]的过程中，Windows Installer 会在系统驱动器中创建临时文件。 在运行安装程序以安装或升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]之前，请检查系统驱动器中是否有至少 6.0 GB 的可用磁盘空间用来存储这些文件。 即使在将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件安装到非默认驱动器中时，此项要求也适用。  
  
 实际硬盘空间需求取决于系统配置和您决定安装的功能。 下表提供了 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 各组件对磁盘空间的要求。  
  
|**功能**|**磁盘空间要求**|  
|-----------------|--------------------------------|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] 和数据文件、复制、全文搜索以及 Data Quality Services|1480 MB|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] （如上所示）带有 R Services（数据库内）|2744 MB|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] （如上所示）带有针对外部数据的 PolyBase 查询服务|4194 MB|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 和数据文件|698 MB|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|967 MB|  
|[!INCLUDE[rsql_platform](../../includes/rsql-platform-md.md)] （独立）|280 MB|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] - SharePoint|1203 MB|  
|用于 SharePoint 产品的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序|325 MB|  
|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]|121 MB|  
|客户端工具连接|328 MB|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|306 MB|  
|客户端组件（除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书组件和 Integration Services 工具之外）|445 MB|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]|280 MB|  
|用于查看和管理帮助内容的[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书组件*|27 MB|  
|所有功能|8030 MB|  
  
 *下载的联机丛书内容需要 200 MB 的磁盘空间。  
  
##  <a name="storage-types-for-data-files"></a><a name="StorageTypes"></a> 数据文件的存储类型  
 支持的数据文件存储类型包括：  
  
- 本地磁盘 
    - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目前支持标准本机扇区大小为 512 字节和 4 KB 的磁盘驱动器。  扇区大小大于 4 KB 的硬盘在尝试存储 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件时可能会导致错误。  要详细了解 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的硬盘扇区大小支持，请参阅 [SQL Server 中的硬盘驱动器扇区大小支持边界](https://support.microsoft.com/kb/926930) 
    - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集安装只支持使用本地磁盘安装 tempdb 文件。 确保为 tempdb 数据和日志文件指定的路径在所有群集节点上均有效。 在故障转移期间，如果 tempdb 目录对故障转移目标节点不可用，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源将无法联机。
- 共享存储  
- [存储空间直通 \(S2D\)](/windows-server/storage/storage-spaces/storage-spaces-direct-overview)  
- SMB 文件共享  
    - 无论是独立安装还是群集安装， [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据文件均不支持 SMB 存储。 请改用直接连接的存储、存储区域网络或 S2D。 
    - SMB 存储可由 Windows 文件服务器或第三方 SMB 存储设备承载。 如果使用 Windows 文件服务器，该 Windows 文件服务器版本应为 2008 或更高。 有关将 SMB 文件共享作为存储选项安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的详细信息，请参阅 [SMB 文件共享用作存储选项时安装 SQL Server](../../database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option.md)。  
  
  
  
##  <a name="installing-ssnoversion-on-a-domain-controller"></a><a name="DC_support"></a> 在域控制器上安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 出于安全方面的考虑，我们建议您不要将 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] 安装在域控制器上。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序不会阻止在作为域控制器的计算机上进行安装，但存在以下限制：  
  
- 在域控制器上，无法在本地服务帐户下运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。    
- 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装到计算机上之后，无法将此计算机从域成员更改为域控制器。 必须先卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，然后才能将主机计算机更改为域控制器。    
- 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装到计算机上之后，无法将此计算机从域控制器更改为域成员。 必须先卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，然后才能将主机计算机更改为域成员。   
- 在群集节点用作域控制器的情况下，不支持[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例。   
- 只读域控制器不支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序不能在只读域控制器上创建安全组或设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户。 在这种情况下，安装将失败。 
- 在仅可以访问只读域控制器的环境中不支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例。 
  
## <a name="installation-media"></a>安装媒体

可以从以下位置获取相关安装介质： 
  
- [SQL Server 评估中心](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019)
- [最新累积更新](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

你也可以创建一个[已运行 SQL Server 的 Azure 虚拟机](/azure/virtual-machines/windows/sql/quickstart-sql-vm-create-portal)，不过由于虚拟化的开销，虚拟机上的 SQL Server 比本地运行的速度要慢。


## <a name="next-steps"></a>后续步骤

查看安装 SQL Server 的硬件和软件要求后，即可开始[规划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)或查看 [SQL Server 的安全注意事项](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)。