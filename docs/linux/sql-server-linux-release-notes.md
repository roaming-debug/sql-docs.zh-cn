---
title: Linux 上的 SQL Server 2017 的发行说明
description: 本文包含 Linux 上运行的 SQL Server 2017 的发行说明和支持功能。 发行说明适用于最新版本和几个以前的版本。
author: VanMSFT
ms.author: vanto
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1314744f-fcaf-46db-800e-2918fa7e1b6c
ms.openlocfilehash: fe088ed697e4150b2272f3a390b4002714ab4dc4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338280"
---
# <a name="release-notes-for-sql-server-2017-on-linux"></a>Linux 上的 SQL Server 2017 的发行说明

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

以下发行说明适用于 Linux 上运行的 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]。 本文按各版本分为不同的部分。 GA 版本提供了详细的支持，并列出了已知的问题。 每个累积更新 (CU) 或常规分发版本 (GDR) 都有指向介绍 CU 更改的支持文章的链接，还有指向 Linux 包下载的链接。

> [!TIP]
> 这些发行说明专门用于 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 版本。 有关新 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 的详细信息，请参阅 [Linux 上的 SQL Server 2019 预览版的发行说明](sql-server-linux-release-notes-2019.md?view=sql-server-ver15&preserve-view=true)。

## <a name="supported-platforms"></a>支持的平台

| 平台 | 文件系统 | 安装指南 |
|-----|-----|-----|
| Red Hat Enterprise Linux 7.3 - 7.8 或 8.0 - 8.3 服务器 | XFS 或 EXT4 | [安装指南](quickstart-install-connect-red-hat.md) | 
| SUSE Enterprise Linux Server v12 SP2 - SP5 | XFS 或 EXT4 | [安装指南](quickstart-install-connect-suse.md) |
| Ubuntu 16.04 LTS、18.04 LTS | XFS 或 EXT4 | [安装指南](quickstart-install-connect-ubuntu.md) | 
| 适用于 Windows、Mac 或 Linux 的 Docker 引擎 1.8 及更高版本 | 空值 | [安装指南](quickstart-install-connect-docker.md) | 

> [!TIP]
> 有关详细信息，请查看 Linux 上 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的[系统要求](sql-server-linux-setup.md#system)。 有关 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的最新支持策略，请参阅 [Microsoft SQL Server 的技术支持策略](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server)。

## <a name="tools"></a>工具

以 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 为目标的大多数现有客户端工具都可以无缝地以 Linux 上运行的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 为目标。 某些工具与 Linux 配合使用时可能有特定的版本要求。 有关 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 工具的完整列表，请参阅[适用于 SQL Server 的 SQL 工具和实用程序](../tools/overview-sql-tools.md)。

## <a name="release-history"></a>版本历史记录

下表列出了 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的版本历史记录。

| 发布               | 版本       | 发布日期 |
|-----------------------|---------------|--------------|
| [CU22-GDR](#CU22)         | 14.0.3370.1  | 2021-01-12 |
| [CU22](#CU22)         | 14.0.3356.20  | 2020-09-10   |
| [CU21](#CU21)         | 14.0.3335.7   | 2020-07-01   |
| [CU20](#CU20)         | 14.0.3294.2   | 2020-04-10   |
| [CU19](#CU19)         | 14.0.3281.6   | 2020-02-05   |
| [CU18](#CU18)         | 14.0.3257.3   | 2019-12-09   |
| [CU17](#CU17)         | 14.0.3238.1   | 2019-10-08   |
| [CU16](#CU16)         | 14.0.3223.3   | 2019-08-01   |
| [CU15](#CU15)         | 14.0.3162.1   | 2019-05-23   |
| [CU14](#CU14)         | 14.0.3076.1   | 2019-03-25   |
| [CU13](#CU13)         | 14.0.3048.4   | 2018-12-18   |
| [CU12](#CU12)         | 14.0.3045.24  | 2018-10-24   |
| [CU11](#CU11)         | 14.0.3038.14  | 2018-09-20   |
| [CU10](#CU10)         | 14.0.3037.1   | 2018-08-27   |
| [CU9-GDR2](#CU9-GDR2) | 14.0.3035.2   | 2018-08-18   |
| [GDR2](#GDR2)         | 14.0.2002.14  | 2018-08-18   |
| [CU9](#CU9)           | 14.0.3030.27  | 2018-07-18   |
| [CU8](#CU8)           | 14.0.3029.16  | 2018-06-21   |
| [CU7](#CU7)           | 14.0.3026.27  | 2018-05-24   |
| [CU6](#CU6)           | 14.0.3025.34  | 2018-04-19   |
| [CU5](#CU5)           | 14.0.3023.8   | 2018-03-20   |
| [CU4](#CU4)           | 14.0.3022.28  | 2018-02-20   |
| [CU3](#CU3)           | 14.0.3015.40  | 2018-01-03   |
| [GDR1](#GDR1)         | 14.0.2000.63  | 2018-01-03   |
| [CU2](#CU2)           | 14.0.3008.27  | 2017-11-28   |
| [CU1](#CU1)           | 14.0.3006.16  | 2017-10-24   |
| [GA](#GA)             | 14.0.1000.169 | 2017-10-02   |

## <a name="how-to-install-updates"></a><a id="cuinstall"></a> 如何安装更新

如果已配置 CU 存储库 (mssql-server-2017)，则在执行新安装时将获得 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 包的最新 CU。 CU 存储库是 Linux 上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的所有包安装文章的默认库。 如果已配置 GDR 存储库 (mssql-server-2017-gdr)，将仅获得自 GA 以来发布的关键安全更新。 如果需要 Docker 容器 CU 或 GDR 更新，请参阅 [适用于 Docker 引擎的 Linux 上的 Microsoft SQL Server](https://hub.docker.com/r/microsoft/mssql-server)。 有关存储库配置的详细信息，请参阅[为 Linux 上的 SQL Server 配置存储库](sql-server-linux-change-repo.md)。

如果要更新现有的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 包，请为每个包运行相应的更新命令以获取最新的 CU。 有关每个包的特定更新说明，请参阅以下安装指南：

- [安装 SQL Server 包](sql-server-linux-setup.md#upgrade)
- [安装全文搜索包](sql-server-linux-setup-full-text-search.md)
- [安装 SQL Server Integration Services](sql-server-linux-setup-ssis.md)
- [启用 SQL Server 代理](sql-server-linux-setup-sql-agent.md)

## <a name="cu22-gdr-january-2021"></a><a id="CU22-GDR"></a> CU22-GDR（2021 年 1 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 22-GDR (CU22-GDR) 版本。 此发行版的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3370.1。 若要了解此发行版中的修补程序和改进，请参阅 <https://support.microsoft.com/help/4577467>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 自 CU20 起，SQL Server 2017 现已开始支持 Ubuntu 18.04 和 RHEL 8。
>
> Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包，不适用于 Ubuntu 18.04 的 SSIS 包除外。 若要查找 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>。
>
> Red Hat 的脱机包安装链接指向 RHEL 8 包，不适用于 RHEL 8 的 SSIS 包除外。 若要查找 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2017/>。

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3370.1-23-18 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3370.1-18.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3370.1-18.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3370.1-18.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) |
| SLES RPM 包 | 14.0.3370.1-18 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3370.1-18.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3370.1-18.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3370.1-18.x86_64.rpm) | 
| Ubuntu 18.04 Debian 包 | 14.0.3370.1-18 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3370.1-18_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3370.1-18_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3370.1-18_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu22-september-2020"></a><a id="CU22"></a> CU22（2020 年 9 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 22 (CU22) 版本。 此发行版的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3356.20。 若要了解此发行版中的修补程序和改进，请参阅 <https://support.microsoft.com/help/4577467>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 自 CU20 起，SQL Server 2017 现已开始支持 Ubuntu 18.04 和 RHEL 8。
>
> Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包，不适用于 Ubuntu 18.04 的 SSIS 包除外。 若要查找 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>。
>
> Red Hat 的脱机包安装链接指向 RHEL 8 包，不适用于 RHEL 8 的 SSIS 包除外。 若要查找 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2017/>。

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3356.20-23 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3356.20-23.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3356.20-23.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3356.20-23.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3356.20-23 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3356.20-23.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3356.20-23.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3356.20-23.x86_64.rpm) | 
| Ubuntu 18.04 Debian 包 | 14.0.3356.20-23 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3356.20-23_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3356.20-23_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3356.20-23_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu21-july-2020"></a><a id="CU21"></a> CU21（2020 年 7 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 21 (CU21) 发行版。 此发行版的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3335.7。 若要了解此发行版中的修补程序和改进，请参阅 <https://support.microsoft.com/help/4557397>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 自 CU20 起，SQL Server 2017 现已开始支持 Ubuntu 18.04 和 RHEL 8。
>
> Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包，不适用于 Ubuntu 18.04 的 SSIS 包除外。 若要查找 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>。
>
> Red Hat 的脱机包安装链接指向 RHEL 8 包，不适用于 RHEL 8 的 SSIS 包除外。 若要查找 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2017/>。

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3335.7-17 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3335.7-17.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3335.7-17.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3335.7-17.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3335.7-17 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3335.7-17.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3335.7-17.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3335.7-17.x86_64.rpm) | 
| Ubuntu 18.04 Debian 包 | 14.0.3335.7-17 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3335.7-17_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3335.7-17_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3335.7-17_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu20-april-2020"></a><a id="CU20"></a> CU20（2020 年 4 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 20 (CU20) 发行版。 此发行版的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3294.2。 若要了解此发行版中的修补程序和改进，请参阅 <https://support.microsoft.com/help/4541283>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 自 CU20 起，SQL Server 2017 现已开始支持 Ubuntu 18.04 和 RHEL 8。
>
> Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包，不适用于 Ubuntu 18.04 的 SSIS 包除外。 若要查找 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/>。
>
> Red Hat 的脱机包安装链接指向 RHEL 8 包，不适用于 RHEL 8 的 SSIS 包除外。 若要查找 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2017/>。

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3294.2-27 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-14.0.3294.2-27.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-ha-14.0.3294.2-27.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2017/mssql-server-fts-14.0.3294.2-27.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3294.2-27 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3294.2-27.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3294.2-27.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3294.2-27.x86_64.rpm) | 
| Ubuntu 18.04 Debian 包 | 14.0.3294.2-27 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3294.2-27_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3294.2-27_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3294.2-27_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu19-february-2020"></a><a id="CU19"></a> CU19（2020 年 2 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 19 (CU19) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]版本为 14.0.3281.6。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4535007](https://support.microsoft.com/help/4535007)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3281.6-2 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3281.6-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3281.6-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3281.6-2.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3281.6-2 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3281.6-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3281.6-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3281.6-2.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3281.6-2 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3281.6-2_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3281.6-2_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3281.6-2_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu18-december-2019"></a><a id="CU18"></a> CU18（2019 年 12 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 18 (CU18) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3257.3。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4527377](https://support.microsoft.com/help/4527377)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3257.3-13 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3257.3-13.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3257.3-13.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3257.3-13.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3257.3-13 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3257.3-13.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3257.3-13.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3257.3-13.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3257.3-13 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3257.3-13_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3257.3-13_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3257.3-13_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

### <a name="added-support"></a>添加的支持

- 从 CU18 开始，Linux 上的 SQL Server 2017 支持变更数据捕获 (CDC)。
- 从 CU18 开始，Linux 上的 SQL Server 2017 支持事务复制。

### <a name="remarks"></a>备注

SQL Server 2017 容器现在具有新的标记模式，如下面的示例所述。

- `mcr.microsoft.com/mssql/server:<SQL Server Version>-<update>-<Linux Distribution>-<Linux Distribution Version>`

  此模式将使用标记中所述的组合提取容器映像。

- `mcr.microsoft.com/mssql/server:<SQL Server Version>-latest`

    此模式将根据最新的受支持 Ubuntu 版本提取最新的 SQL Server 版本。

**示例：**

`mcr.microsoft.com/mssql/server:2017-CU18-ubuntu-16.04`

此模式将根据 Ubuntu 16.04 容器提取 SQL Server 2017 CU18。

`mcr.microsoft.com/mssql/server:2017-latest`

此模式将根据 Ubuntu 16.04 容器提取最新的 SQL Server 2017 版本（在撰写本文时为 CU18）。

> [!NOTE]
> 以后，我们将不再为 SQL Server 2017 容器发布采用其他标记模式的容器。


## <a name="cu17-october-2019"></a><a id="CU17"></a> CU17（2019 年 10 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 17 (CU17) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3238.1。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4515579](https://support.microsoft.com/help/4515579)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3238.1-19 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3238.1-19.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3238.1-19.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3238.1-19.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3238.1-19 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3238.1-19.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3238.1-19.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3238.1-19.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3238.1-19 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3238.1-19_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3238.1-19_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3238.1-19_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu16-august-2019"></a><a id="CU16"></a> CU16（2019 年 8 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 16 (CU16) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3223.3。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4508218](https://support.microsoft.com/help/4508218)。

### <a name="whats-new"></a>新增功能

|新增功能或更新 | 详细信息 |
|:---|:---|
| MSDTC 支持 | 支持适用于 SQL Server 2017 的 Microsoft 分布式事务处理协调器 (MSDTC)。 有关详细信息，请参阅[如何在 Linux 上配置 Microsoft 分布式事务处理协调器 (MSDTC)](sql-server-linux-configure-msdtc.md)。 |

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3223.3-15 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3223.3-15.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3223.3-15.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3223.3-15.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3223.3-15 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3223.3-15.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3223.3-15.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3223.3-15.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3223.3-15 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3223.3-15_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3223.3-15_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3223.3-15_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu15-may-2019"></a><a id="CU15"></a> CU15（2019 年 5 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 15 (CU15) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3162.1。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4498951](https://support.microsoft.com/help/4498951)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3162.1-1 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3162.1-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3162.1-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3162.1-1.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3162.1-1 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3162.1-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3162.1-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3162.1-1.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3162.1-1 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3162.1-1_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3162.1-1_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3162.1-1_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu14-mar-2019"></a><a id="CU14"></a> CU14（2019 年 3 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 14 (CU14) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3076.1。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4484710](https://support.microsoft.com/help/4484710)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3076.1-2 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3076.1-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3076.1-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3076.1-2.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3076.1-2 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3076.1-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3076.1-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3076.1-2.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3076.1-2 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3076.1-2_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3076.1-2_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3076.1-2_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu13-dec-2018"></a><a id="CU13"></a> CU13（2018 年 12 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 13 (CU13) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3048.4。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4466404](https://support.microsoft.com/help/4466404)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3048.4-1 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3048.4-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3048.4-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3048.4-1.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3048.4-1 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3048.4-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3048.4-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3048.4-1.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3048.4-1 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3048.4-1_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3048.4-1_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3048.4-1_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu12-oct-2018"></a><a id="CU12"></a> CU12（2018 年 10 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 12 (CU12) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3045.24。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4464082](https://support.microsoft.com/help/4464082)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3045.24-1 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3045.24-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3045.24-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3045.24-1.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3045.24-1 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3045.24-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3045.24-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3045.24-1.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3045.24-1 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3045.24-1_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3045.24-1_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3045.24-1_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu11-sept-2018"></a><a id="CU11"></a> CU11（2018 年 9 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 11 (CU11) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3038.14。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4462262](https://support.microsoft.com/help/4462262)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3038.14-2 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3038.14-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3038.14-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3038.14-2.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3038.14-2 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3038.14-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3038.14-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3038.14-2.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3038.14-2 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3038.14-2_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3038.14-2_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3038.14-2_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu10-aug-2018"></a><a id="CU10"></a> CU10（2018 年 8 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 10 (CU10) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3037.1。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4342123](https://support.microsoft.com/help/4342123)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3037.1-2 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3037.1-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3037.1-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3037.1-2.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3037.1-2 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3037.1-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3037.1-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3037.1-2.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3037.1-2 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3037.1-2_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3037.1-2_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3037.1-2_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu9-gdr2-aug-2018"></a><a id="CU9-GDR2"></a> CU9-GDR2（2018 年 8 月）

这是一个安全更新，其中还包括以前发布的 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 (CU9)。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3035.2。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4293805](https://support.microsoft.com/help/4293805)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3035.2-1 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3035.2-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3035.2-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3035.2-1.x86_64.rpm)| 
| SLES RPM 包 | 14.0.3035.2-1 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3035.2-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3035.2-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3035.2-1.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3035.2-1 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3035.2-1_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3035.2-1_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3035.2-1_amd64.deb)<br/> |

## <a name="gdr2-aug-2018"></a><a id="GDR2"></a> GDR2（2018 年 8 月）

这是一个安全更新，只包含 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的 GDR2（和 GDR1）安全修补程序。  此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.2002.14。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4293803](https://support.microsoft.com/help/4293803)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.2002.14-1 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-14.0.2002.14-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-ha-14.0.2002.14-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-fts-14.0.2002.14-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.2002.14-1 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-14.0.2002.14-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-ha-14.0.2002.14-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-fts-14.0.2002.14-1.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.2002.14-1 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server/mssql-server_14.0.2002.14-1_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.2002.14-1_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.2002.14-1_amd64.deb) |

## <a name="cu9-jul-2018"></a><a id="CU9"></a> CU9（2018 年 7 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 9 (CU9) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3030.27。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4341265](https://support.microsoft.com/help/4341265)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3030.27-1 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3030.27-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3030.27-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3030.27-1.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3030.27-1 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3030.27-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3030.27-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3030.27-1.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3030.27-1 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3030.27-1_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3030.27-1_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3030.27-1_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu8-jun-2018"></a><a id="CU8"></a> CU8（2018 年 6 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 8 (CU8) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3029.16。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4338363](https://support.microsoft.com/help/4338363)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3029.16-1 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3029.16-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3029.16-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3029.16-1.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3029.16-1 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3029.16-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3029.16-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3029.16-1.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3029.16-1 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3029.16-1_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3029.16-1_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3029.16-1_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu7-may-2018"></a><a id="CU7"></a> CU7（2018 年 5 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 7 (CU7) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3026.27。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4229789](https://support.microsoft.com/help/4229789)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3026.27-2 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3026.27-2 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3026.27-2 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3026.27-2_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3026.27-2_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3026.27-2_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu6-apr-2018"></a><a id="CU6"></a> CU6（2018 年 4 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 6 (CU6) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3025.34。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4101464](https://support.microsoft.com/help/4101464)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3025.34-3 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3025.34-3 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3025.34-3 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3025.34-3_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3025.34-3_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3025.34-3_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu5-mar-2018"></a><a id="CU5"></a> CU5（2018 年 3 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 5 (CU5) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3023.8。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4092643](https://support.microsoft.com/help/4092643)。

### <a name="known-upgrade-issue"></a>已知升级问题

从先前版本升级到 CU5 时，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可能无法启动，并出现以下错误：

```
Error: 4860, Severity: 16, State: 1.
Cannot bulk load. The file "C:\Install\SqlTraceCollect.dtsx" does not exist or you don't have file access rights.
Error: 912, Severity: 21, State: 2.
Script level upgrade for database 'master' failed because upgrade step 'msdb110_upgrade.sql' encountered error 200, state
```

要解决此错误，请启用 SQL Server 代理并使用以下命令重启 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]：

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
sudo systemctl start mssql-server
```

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3023.8-5 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3023.8-5 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3023.8-5 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3023.8-5_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3023.8-5_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3023.8-5_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu4-feb-2018"></a><a id="CU4"></a> CU4（2018 年 2 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 4 (CU4) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3022.28。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4056498](https://support.microsoft.com/help/4056498)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 从 CU4 开始，SQL Server 代理不再作为单独的包进行安装。 它与引擎包一起安装，必须启用才能使用。

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3022.28-2 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3022.28-2 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3022.28-2 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3022.28-2_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3022.28-2_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3022.28-2_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="gdr1-jan-2018"></a><a id="GDR1"></a> GDR1（2018 年 1 月）

这是一个安全更新，只包含 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的 GDR1 安全修补程序。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.2000.63。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4057122](https://support.microsoft.com/help/4057122)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.2000.63-3 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-14.0.2000.63-3.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-ha-14.0.2000.63-3.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017-gdr/mssql-server-fts-14.0.2000.63-3.x86_64.rpm) | 
| SLES RPM 包 | 14.0.2000.63-3 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-14.0.2000.63-3.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-ha-14.0.2000.63-3.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017-gdr/mssql-server-fts-14.0.2000.63-3.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.2000.63-3 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server/mssql-server_14.0.2000.63-3_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.2000.63-3_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.2000.63-3_amd64.deb) |

## <a name="cu3-jan-2018"></a><a id="CU3"></a> CU3（2018 年 1 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 3 (CU3) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3015.40。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4052987](https://support.microsoft.com/help/4052987)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3015.40-1 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[SQL Server 代理 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3015.40-1 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[SQL Server 代理 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3015.40-1 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3015.40-1_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3015.40-1_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3015.40-1_amd64.deb)</br>[SQL Server 代理 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3015.40-1_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu2-nov-2017"></a><a id="CU2"></a> CU2（2017 年 11 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 2 (CU2) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3008.27。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/4052574](https://support.microsoft.com/help/4052574)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3008.27-1 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[SQL Server 代理 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3008.27-1 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[SQL Server 代理 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3008.27-1 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3008.27-1_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3008.27-1_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3008.27-1_amd64.deb)</br>[SQL Server 代理 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3008.27-1_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="cu1-oct-2017"></a><a id="CU1"></a> CU1（2017 年 10 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的累积更新 1 (CU1) 版本。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.3006.16。 有关此版本中的修补程序和改进的信息，请参阅 [https://support.microsoft.com/help/KB4053439](https://support.microsoft.com/help/4038634)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.3006.16-3 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[SQL Server 代理 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.3006.16-3 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[SQL Server 代理 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.3006.16-3 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3006.16-3_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3006.16-3_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3006.16-3_amd64.deb)</br>[SQL Server 代理 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3006.16-3_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="ga-oct-2017"></a><a id="GA"></a> GA（2017 年 10 月）

这是 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的正式发布版 (GA)。 此版本的 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 版本为 14.0.1000.169。

### <a name="package-details"></a>包详细信息

下表列出了包的详细信息以及 RPM 和 Debian 包的下载位置。 如果使用以下安装指南中的步骤，则无需直接下载这些包：

- [安装 SQL Server 包](sql-server-linux-setup.md)
- [安装全文搜索包](sql-server-linux-setup-full-text-search.md)
- [安装 SQL Server 代理包](sql-server-linux-setup-sql-agent.md)
- [安装 SQL Server Integration Services](sql-server-linux-setup-ssis.md)

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 14.0.1000.169-2 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[SQL Server 代理 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm)</br>[SSIS 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| SLES RPM 包 | 14.0.1000.169-2 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[SQL Server 代理 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm) | 
| Ubuntu 16.04 Debian 包 | 14.0.1000.169-2 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.1000.169-2_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.1000.169-2_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.1000.169-2_amd64.deb)</br>[SQL Server 代理 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.1000.169-2_amd64.deb)<br/>[SSIS 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="known-issues"></a>已知问题

以下部分介绍了 Linux 上 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 的正式发布版 (GA) 的已知问题。

#### <a name="general"></a>常规

- 安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的主机名的长度不能超过 15 个字符。 

    - **解决方法**：更改 /etc/hostname 中的名称，使其不超过 15 个字符。

- 手动将系统时间设置为时间倒移，会导致 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 停止更新 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的内部系统时间。

    - **解决方法**：重新启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。

- 仅支持单个实例安装。

    - **解决方法**：如果希望在给定主机上安装多个实例，请考虑使用 VM 或 Docker 容器。 

- [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Configuration Manager 无法连接到 Linux 上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。

- sa 登录名的默认语言是英语。

    - **解决方法**：使用 ALTER LOGIN 语句更改 sa 登录名的语言 。

#### <a name="databases"></a>数据库

- 不能使用 mssql-conf 实用工具移动 master 数据库。 可以使用 mssql-conf 移动其他系统数据库。

- 还原在 Windows 上 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中备份的数据库时，必须在 Transact-SQL 语句中使用 WITH MOVE 子句。

- 传输层安全性 (TLS) 的某些算法（密码套件）无法在 Linux 上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中正常运行。 这会在尝试连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 时导致连接失败，以及在高可用性组中的副本之间建立连接时出现问题。

   - **解决方法**：通过执行以下操作，修改 Linux 上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的 mssql.conf 配置脚本以禁用有问题的密码套件：

      1. 将以下项添加到 /var/opt/mssql/mssql.conf。

         ```
         [network]
         tlsciphers= AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:!ECDHE-RSA-AES128-GCM-SHA256:!ECDHE-RSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES128-GCM-SHA256:!ECDHE-ECDSA-AES256-SHA384:!ECDHE-ECDSA-AES128-SHA256:!ECDHE-ECDSA-AES256-SHA:!ECDHE-ECDSA-AES128-SHA:!ECDHE-RSA-AES256-SHA384:!ECDHE-RSA-AES128-SHA256:!ECDHE-RSA-AES256-SHA:!ECDHE-RSA-AES128-SHA:!DHE-RSA-AES256-GCM-SHA384:!DHE-RSA-AES128-GCM-SHA256:!DHE-RSA-AES256-SHA:!DHE-RSA-AES128-SHA:!DHE-DSS-AES256-SHA256:!DHE-DSS-AES128-SHA256:!DHE-DSS-AES256-SHA:!DHE-DSS-AES128-SHA:!DHE-DSS-DES-CBC3-SHA:!NULL-SHA256:!NULL-SHA
         ```

         > [!NOTE]
         > 在前面的代码中，`!` 对表达式进行求反。 这向 OpenSSL 指明不使用以下密码套件。  

      1. 使用以下命令重启 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。

         ```bash
         sudo systemctl restart mssql-server
         ```

- Windows 上使用内存中 OLTP 的 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 数据库无法在 Linux 上的 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] 上进行还原。 要还原使用内存中 OLTP 的 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 数据库，请首先将数据库升级到 Windows 上的 [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] 或 [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]，然后再通过备份/还原或分离/附加将数据库移至 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。

- 目前 Linux 不支持用户权限 ADMINISTER BULK OPERATIONS。

#### <a name="networking"></a>网络

如果满足以下两个条件，则涉及来自 sqlservr 进程的出站 TCP 连接（如链接服务器或可用性组）的功能可能不起作用：

1. 目标服务器被指定为主机名而不是 IP 地址。

1. 源实例已在内核中禁用 IPv6。 要验证系统是否在内核中启用了 IPv6，以下所有测试都必须通过：

   - `cat /proc/cmdline` 将输出当前内核的引导 cmdline。 输出不得包含 `ipv6.disable=1`。
   - /Proc/sys/net/ipv6/ 目录必须存在。
   - 调用 `socket(AF_INET6, SOCK_STREAM, IPPROTO_IP)` 的 C 程序应成功，即系统调用必须返回一个 fd != -1 并且不会因 EAFNOSUPPORT 而失败。

确切的错误取决于该功能。 对于链接服务器，这表现为登录超时错误。 对于可用性组，辅助节点上的 `ALTER AVAILABILITY GROUP JOIN` DDL 将在 5 分钟后因下载配置超时错误而失败。

要解决此问题，请执行以下操作之一：

1. 使用 IP 而不是主机名来指定 TCP 连接的目标。

1. 从引导 cmdline 中删除 `ipv6.disable=1`，在内核中启用 IPv6。 执行此操作的方法取决于 Linux 分发版和引导加载程序，例如 grub。 如果确实想要禁用 IPv6，仍可以通过在 `sysctl` 配置中设置 `net.ipv6.conf.all.disable_ipv6 = 1` 来禁用它（例如 `/etc/sysctl.conf`）。 这仍然会阻止系统的网络适配器获取 IPv6 地址，但允许 sqlservr 功能运行。

#### <a name="network-file-system-nfs"></a>网络文件系统 (NFS)
如果在生产中使用网络文件系统 (NFS) 远程共享，请注意以下支持要求：

- 使用 NFS 版本 4.2 或更高版本。 较早版本的 NFS 不支持新式文件系统常用的必需功能，例如 fallocate 和稀疏文件创建。
- 仅在 NFS 装载上查找 /var/opt/mssql 目录。 不支持其他文件，例如 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 系统二进制文件。
- 安装远程共享时，请确保 NFS 客户端使用“nolock”选项。

#### <a name="localization"></a>本地化

- 如果在安装过程中区域设置不是英语 (en_us)，则必须在 bash 会话/终端中使用 UTF-8 编码。 如果使用 ASCII 编码，可能会看到类似于以下内容的错误：

   ```
   UnicodeEncodeError: 'ascii' codec can't encode character u'\xf1' in position 8: ordinal not in range(128)
   ```

   如果无法使用 UTF-8 编码，请使用 MSSQL_LCID 环境变量运行安装程序以指定语言选择。

   ```bash
   sudo MSSQL_LCID=<LcidValue> /opt/mssql/bin/mssql-conf setup
   ```

- 运行 mssql-conf 安装程序并执行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的非英语安装时，在本地化文本“配置 SQL Server...”之后会显示错误的扩展字符。 或者，对于非拉丁语的安装，句子可能完全丢失。 丢失的句子应显示以下本地化字符串：“已成功处理授权 PID。 新版本为 [\<Name\>版本]”。 输出此字符串仅供参考，下一 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 累积更新将针对所有语言解决此问题。 这不会以任何方式影响 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的成功安装。 

#### <a name="full-text-search"></a>全文搜索

- 并非所有筛选器都适用于此版本，包括 Office 文档筛选器。 有关支持的筛选器列表，请参阅[在 Linux 上安装 SQL Server 全文搜索](sql-server-linux-setup-full-text-search.md#filters)。

#### <a name="sql-server-integration-services-ssis"></a><a id="ssis"></a> SQL Server Integration Services (SSIS)

- 此版本中的 SUSE 不支持 mssql-server-is 包。 目前仅 Ubuntu 和 Red Hat Enterprise Linux (RHEL) 支持该包。

- 由于 Linux CTP 2.1 刷新版和更高版本上有 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]，所以 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包可以使用 Linux 上的 ODBC 连接。 虽然已使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 和 MySQL ODBC 驱动程序测试过该功能，但也希望该功能可以与任何遵循 ODBC 规范的 Unicode ODBC 驱动程序搭配使用。 在设计阶段，可以提供 DSN 或连接字符串以连接到 ODBC 数据，还可以使用 Windows 身份验证。 有关详细信息，请参阅[宣布 Linux 上的 ODBC 支持的博客文章](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/)。

- 在 Linux 上运行 SSIS 包时，此版本不支持以下功能：
  - [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 目录数据库
  - SQL 代理计划的包执行
  - Windows 身份验证
  - 第三方组件
  - 变更数据捕获 (CDC)
  - [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 横向扩展
  - 适用于 SSIS 的 Azure 功能包
  - Hadoop 和 HDFS 支持
  - Microsoft Connector for SAP BW

有关当前不受支持或提供有限支持的内置 SSIS 组件的列表，请参阅 [Linux 上的 SSIS 的限制和已知问题](sql-server-linux-ssis-known-issues.md#components)。

有关 Linux 上的 SSIS 的详细信息，请参阅以下文章：
-   [宣布对 Linux 的 SSIS 支持的博客文章](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/)。
-   [在 Linux 上安装 SQL Server Integration Services (SSIS)](sql-server-linux-setup-ssis.md)
-   [使用 SSIS 在 Linux 上提取、转换和加载数据](sql-server-linux-migrate-ssis.md)

#### <a name="sql-server-management-studio-ssms"></a><a id="ssms"></a> SQL Server Management Studio (SSMS)

以下限制适用于 Windows 中连接到 Linux 上 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]。

- 不支持维护计划。

- 不支持管理数据仓库 (MDW) 和 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中的数据收集器。 

- 具有 Windows 身份验证或 Windows 事件日志选项的 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] UI 组件不适用于 Linux。 仍可通过其他选项（如 SQL 登录名）使用这些功能。 

- 不能修改要保留的日志文件数。

## <a name="next-steps"></a>后续步骤

要开始使用，请参阅以下快速入门：

- [在 Red Hat Enterprise Linux 上安装](quickstart-install-connect-red-hat.md)
- [在 SUSE Linux Enterprise Server 上安装](quickstart-install-connect-suse.md)
- [在 Ubuntu 上安装](quickstart-install-connect-ubuntu.md)
- [在 Docker 上运行](quickstart-install-connect-docker.md)
- [在 Azure 中预配 SQL VM](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)
- [运行和连接 - 云](quickstart-install-connect-clouds.md)

有关常见问题的解答，请参阅 [Linux 上的 SQL Server 常见问题解答](sql-server-linux-faq.md)。
