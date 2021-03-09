---
title: Linux 上的 SQL Server 2019 的发行说明
description: 本文包含 Linux 上运行的 SQL Server 2019 的发行说明和支持功能。 发行说明适用于最新版本和几个以前的版本。
author: VanMSFT
ms.author: vanto
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: c81ca4cb8d531626c7f5ae51f8b4e264d907d2ce
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464673"
---
# <a name="release-notes-for-sql-server-2019-on-linux"></a>Linux 上的 SQL Server 2019 的发行说明

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx-linuxonly.md](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx-linuxonly.md)]

以下发行说明适用于 Linux 上运行的 SQL Server 2019。 本文按各版本分为不同的部分。 每个版本都有介绍 CU 更改的支持性文章的链接，还有 Linux 包下载的链接。

> [!TIP]
> 若要了解 SQL Server 2019 中的新增 Linux 功能，请参阅 [SQL Server 2019 中的新增功能](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true#sql-server-on-linux)。

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

## <a name="tools"></a>工具

以 SQL Server 为目标的大多数现有客户端工具可无缝转为以 Linux 上运行的 SQL Server 为目标。 某些工具与 Linux 配合使用时可能有特定的版本要求。 有关 SQL Server 工具的完整列表，请参阅[适用于 SQL Server 的 SQL 工具和实用程序](../tools/overview-sql-tools.md)。

## <a name="release-history"></a>版本历史记录

下表列出了 SQL Server 2019 版本的发布历史记录。

| 发布                   | 版本       | 发布日期 |
|---------------------------|---------------|--------------|
| [CU9](#cu9)               | 15.0.4102.2   | 2021-02-10   |
| [CU8-GDR](#cu8-GDR)               | 15.0.4083.2  | 2021-01-12   |
| [CU8](#cu8)               | 15.0.4073.23  | 2020-10-07   |
| [CU7（已删除）](https://support.microsoft.com/help/4570012)     | 15.0.4063.15  | 2020-09-02   |
| [CU6](#cu6)               | 15.0.4053.23  | 2020-08-04   |
| [CU5](#cu5)               | 15.0.4043.16  | 2020-06-22   |
| [CU4](#cu4)               | 15.0.4033.1   | 2020 年 3 月 31 日   |
| [CU3](#cu3)               | 15.0.4023.6   | 2020-03-12   |
| [CU2](#cu2)               | 15.0.4013.40  | 2020-02-13   |
| [CU1](#cu1)               | 15.0.4003.23  | 2020-01-07   |
| [GA](#ga)                 | 15.0.2000.5   | 2019-11-04   |
| [候选发布](#rc)  | 15.0.1900.25  | 2019-08-21   |

## <a name="how-to-install-updates"></a><a id="cuinstall"></a> 如何安装更新

如果已配置 CU 存储库 (mssql-server-2019)，则在执行新安装时将获得 SQL Server 包的最新 CU。 如果需要 Docker 容器映像，请参阅[适用于 Docker 引擎的 Linux 上的 Microsoft SQL Server](https://hub.docker.com/r/microsoft/mssql-server/) 的官方映像。 有关存储库配置的详细信息，请参阅[为 Linux 上的 SQL Server 配置存储库](sql-server-linux-change-repo.md)。

如果要更新现有的 SQL Server 包，请为每个包运行相应的更新命令以获取最新的 CU。 有关每个包的特定更新说明，请参阅以下安装指南：

- [安装 SQL Server 包](sql-server-linux-setup.md#upgrade)
- [安装全文搜索包](sql-server-linux-setup-full-text-search.md)
- [安装 SQL Server Integration Services](sql-server-linux-setup-ssis.md)
- [在 Linux 上安装 SQL Server 2019 机器学习服务 R 和 Python 支持](sql-server-linux-setup-machine-learning.md)
- [安装 PolyBase 包](../relational-databases/polybase/polybase-linux-setup.md)
- [启用 SQL Server 代理](sql-server-linux-setup-sql-agent.md)

## <a name="cu9-february-2021"></a><a id="cu9"></a> CU9（2021 年 2 月）

这是 SQL Server 2019 (15.x) 的累积更新 9 (CU9) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4102.2。 若要了解修补程序和改进，请参阅 <https://support.microsoft.com/help/5000642>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 从 CU1 开始，Red Hat 的脱机包安装链接指向 RHEL 8 包。 如需 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2019/>。
>
> 自 CU3 起，SQL Server 2019 现已支持 Ubuntu 18.04。 Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包。 如需 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.4102.2-4 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4102.2-4.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4102.2-4.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4102.2-4.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4102.2-4.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4102.2-4.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4102.2-4.x86_64.rpm)|
| SLES RPM 包 | 15.0.4102.2-4 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4102.2-4.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4102.2-4.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4102.2-4.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4102.2-4.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4102.2-4.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4102.2-4.x86_64.rpm)|
| Ubuntu 18.04 Debian 包 | 15.0.4102.2-4 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4102.2-4_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4102.2-4_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4102.2-4_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4102.2-4_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4102.2-4_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4102.2-4_amd64.deb)|

## <a name="cu8-gdr-january-2021"></a><a id="cu8-GDR"></a> CU8-GDR（2021 年 1 月）

这是 SQL Server 2019 (15.x) 的累积更新 8-GDR (CU8-GDR) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4083.2。 若要了解修补程序和改进，请参阅 <https://support.microsoft.com/help/4577194>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 从 CU1 开始，Red Hat 的脱机包安装链接指向 RHEL 8 包。 如需 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2019/>。
>
> 自 CU3 起，SQL Server 2019 现已支持 Ubuntu 18.04。 Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包。 如需 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.4083.2-15 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4083.2-15.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4083.2-15.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4083.2-15.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4083.2-15.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4083.2-15.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4083.2-15.x86_64.rpm)|
| SLES RPM 包 | 15.0.4083.2-15 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4083.2-15.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4083.2-15.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4083.2-15.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4083.2-15.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4083.2-15.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4083.2-15.x86_64.rpm)|
| Ubuntu 18.04 Debian 包 | 15.0.4083.2-15 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4083.2-15_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4083.2-15_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4083.2-15_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4083.2-15_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4083.2-15_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4083.2-15_amd64.deb)|

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8（2020 年 9 月）

这是 SQL Server 2019 (15.x) 的累积更新 8 (CU8) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4073.23。 若要了解修补程序和改进，请参阅 <https://support.microsoft.com/help/4577194>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 从 CU1 开始，Red Hat 的脱机包安装链接指向 RHEL 8 包。 如需 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2019/>。
>
> 自 CU3 起，SQL Server 2019 现已支持 Ubuntu 18.04。 Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包。 如需 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.4073.23-4 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4073.23-4.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4073.23-4.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4073.23-4.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4073.23-4.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4073.23-4.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4073.23-4.x86_64.rpm)|
| SLES RPM 包 | 15.0.4073.23-4 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4073.23-4.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4073.23-4.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4073.23-4.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4073.23-4.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4073.23-4.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4073.23-4.x86_64.rpm)|
| Ubuntu 18.04 Debian 包 | 15.0.4073.23-4 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4073.23-4_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4073.23-4_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4073.23-4_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4073.23-4_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4073.23-4_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4073.23-4_amd64.deb)|

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6（2020 年 7 月）

这是 SQL Server 2019 (15.x) 的累积更新 6 (CU6) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4053.23。 若要了解修补程序和改进，请参阅 <https://support.microsoft.com/help/4563110>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 从 CU1 开始，Red Hat 的脱机包安装链接指向 RHEL 8 包。 如需 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2019/>。
>
> 自 CU3 起，SQL Server 2019 现已支持 Ubuntu 18.04。 Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包。 如需 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.4053.23-2 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4053.23-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4053.23-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4053.23-2.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4053.23-2.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4053.23-2.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4053.23-2.x86_64.rpm)|
| SLES RPM 包 | 15.0.4053.23-2 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4053.23-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4053.23-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4053.23-2.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4053.23-2.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4053.23-2.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4053.23-2.x86_64.rpm)|
| Ubuntu 18.04 Debian 包 | 15.0.4053.23-2 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4053.23-2_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4053.23-2_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4053.23-2_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4053.23-2_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4053.23-2_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4053.23-2_amd64.deb)|

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5（2020 年 6 月）

这是 SQL Server 2019 (15.x) 的累积更新 5 (CU5) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4043.16。 若要了解修补程序和改进，请参阅 <https://support.microsoft.com/help/4552255>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 从 CU1 开始，Red Hat 的脱机包安装链接指向 RHEL 8 包。 如需 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2019/>。
>
> 自 CU3 起，SQL Server 2019 现已支持 Ubuntu 18.04。 Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包。 如需 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.4043.16-4 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4043.16-4.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4043.16-4.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4043.16-4.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4043.16-4.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4043.16-4.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4043.16-4.x86_64.rpm)|
| SLES RPM 包 | 15.0.4043.16-4 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4043.16-4.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4043.16-4.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4043.16-4.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4043.16-4.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4043.16-4.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4043.16-4.x86_64.rpm)|
| Ubuntu 18.04 Debian 包 | 15.0.4043.16-4 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4043.16-4_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4043.16-4_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4043.16-4_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4043.16-4_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4043.16-4_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4043.16-4_amd64.deb)|

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4（2020 年 4 月）

这是 SQL Server 2019 (15.x) 的累积更新 4 (CU4) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4033.1。 若要了解修补程序和改进，请参阅 <https://support.microsoft.com/help/4548597>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 从 CU1 开始，Red Hat 的脱机包安装链接指向 RHEL 8 包。 如需 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2019/>。
>
> 自 CU3 起，SQL Server 2019 现已支持 Ubuntu 18.04。 Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包。 如需 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.4033.1-2 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4033.1-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4033.1-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4033.1-2.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4033.1-2.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4033.1-2.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4033.1-2.x86_64.rpm)|
| SLES RPM 包 | 15.0.4033.1-2 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4033.1-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4033.1-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4033.1-2.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4033.1-2.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4033.1-2.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4033.1-2.x86_64.rpm)|
| Ubuntu 18.04 Debian 包 | 15.0.4033.1-2 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4033.1-2_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4033.1-2_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4033.1-2_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4033.1-2_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4033.1-2_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4033.1-2_amd64.deb)|

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3（2020 年 3 月）

这是 SQL Server 2019 (15.x) 的累积更新 3 (CU3) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4023.6。 若要了解修补程序和改进，请参阅 <https://support.microsoft.com/help/4538853>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 从 CU1 开始，Red Hat 的脱机包安装链接指向 RHEL 8 包。 如需 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2019/>。
>
> 自 CU3 起，SQL Server 2019 现已支持 Ubuntu 18.04。 Ubuntu 的脱机包安装链接指向 Ubuntu 18.04 包。 如需 Ubuntu 16.04 包，请参阅下载路径 <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.4023.6-2 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4023.6-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4023.6-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4023.6-2.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4023.6-2.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4023.6-2.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4023.6-2.x86_64.rpm)|
| SLES RPM 包 | 15.0.4023.6-2 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4023.6-2.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4023.6-2.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4023.6-2.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4023.6-2.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4023.6-2.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4023.6-2.x86_64.rpm)|
| Ubuntu 18.04 Debian 包 | 15.0.4023.6-2 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4023.6-2_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4023.6-2_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4023.6-2_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4023.6-2_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4023.6-2_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4023.6-2_amd64.deb)|

## <a name="cu2-february-2020"></a><a id="cu2"></a> CU2（2020 年 2 月）

这是 SQL Server 2019 (15.x) 的累积更新 2 (CU2) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4013.40。 若要了解修补程序和改进，请参阅 <https://support.microsoft.com/help/4536075>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 从 CU1 开始，Red Hat 的脱机包安装链接指向 RHEL 8 包。 如需 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2019/>。

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.4013.40-8 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4013.40-8.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4013.40-8.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4013.40-8.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4013.40-8.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4013.40-8.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4013.40-8.x86_64.rpm)|
| SLES RPM 包 | 15.0.4013.40-8 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4013.40-8.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4013.40-8.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4013.40-8.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4013.40-8.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4013.40-8.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4013.40-8.x86_64.rpm)|
| Ubuntu 16.04 Debian 包 | 15.0.4013.40-8 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4013.40-8_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4013.40-8_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4013.40-8_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4013.40-8_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4013.40-8_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4013.40-8_amd64.deb)|

## <a name="cu1-january-2020"></a><a id="cu1"></a> CU1 （2020 年 1 月）

这是 SQL Server 2019 (15.x) 的累积更新 1 (CU1) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4003.23。 若要了解此发行版中的修补程序和改进，请参阅 <https://support.microsoft.com/en-us/help/4527376>。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

> [!NOTE]
> 从 CU1 开始，Red Hat 的脱机包安装链接指向 RHEL 8 包。 如需 RHEL 7 包，请参阅下载路径 <https://packages.microsoft.com/rhel/7/mssql-server-2019/>。

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.4003.23-3 | [引擎 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4003.23-3.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4003.23-3.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4003.23-3.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4003.23-3.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4003.23-3.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4003.23-3.x86_64.rpm)|
| SLES RPM 包 | 15.0.4003.23-3 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4003.23-3.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4003.23-3.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4003.23-3.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4003.23-3.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4003.23-3.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4003.23-3.x86_64.rpm)|
| Ubuntu 16.04 Debian 包 | 15.0.4003.23-3 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4003.23-3_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4003.23-3_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4003.23-3_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4003.23-3_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4003.23-3_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4003.23-3_amd64.deb)|

## <a name="ga-november-2019"></a><a id="ga"></a> GA（2019 年 11 月）

这是 SQL Server 2019 (15.x) 的正式发布版 (GA)。 此次发布的 SQL Server 数据库引擎版本是 15.0.2000.5。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.2000.5-5 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-15.0.2000.5-5.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-ha-15.0.2000.5-5.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-fts-15.0.2000.5-5.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-extensibility-15.0.2000.5-5.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-extensibility-java-15.0.2000.5-5.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-polybase-15.0.2000.5-5.x86_64.rpm)|
| SLES RPM 包 | 15.0.2000.5-5 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.2000.5-5.x86_64.rpm)</br>[高可用性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.2000.5-5.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.2000.5-5.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.2000.5-5.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.2000.5-5.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.2000.5-5.x86_64.rpm)|
| Ubuntu 16.04 Debian 包 | 15.0.2000.5-5 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.2000.5-5_amd64.deb)</br>[高可用性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.2000.5-5_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.2000.5-5_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.2000.5-5_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.2000.5-5_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.2000.5-5_amd64.deb)|

## <a name="release-candidate-august-2019"></a><a id="rc"></a> 候选发布（2019 年 8 月）

下面几个部分介绍了候选发布的包位置和已知问题。 若要详细了解 SQL Server 2019 上 Linux 的新功能，请参阅 [SQL Server 2019 中的新增功能](../sql-server/what-s-new-in-sql-server-ver15.md)。

### <a name="package-details"></a>包详细信息

对于手动安装或脱机包安装，可以下载 RPM 和 Debian 包，其信息如下表所示：

| 程序包 | 包版本 | 下载 |
|-----|-----|-----|
| Red Hat RPM 包 | 15.0.1900.25-1 | [引擎 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-15.0.1900.25-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-fts-15.0.1900.25-1.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-extensibility-15.0.1900.25-1.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-extensibility-java-15.0.1900.25-1.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-polybase-15.0.1900.25-1.x86_64.rpm)|
| SLES RPM 包 | 15.0.1900.25-1 | [mssql-server 引擎 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-15.0.1900.25-1.x86_64.rpm)</br>[全文搜索 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-fts-15.0.1900.25-1.x86_64.rpm)</br>[扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-extensibility-15.0.1900.25-1.x86_64.rpm)</br>[Java 扩展性 RPM 包](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-extensibility-java-15.0.1900.25-1.x86_64.rpm)</br>[PolyBase RPM 包](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-polybase-15.0.1900.25-1.x86_64.rpm)|
| Ubuntu 16.04 Debian 包 | 15.0.1900.25-1 | [引擎 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server/mssql-server_15.0.1900.25-1_amd64.deb)</br>[全文搜索 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.1900.25-1_amd64.deb)</br>[扩展性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.1900.25-1_amd64.deb)</br>[Java 扩展性 Debian 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.1900.25-1_amd64.deb)</br>[PolyBase RPM 包](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.1900.25-1_amd64.deb)|

## <a name="known-issues"></a>已知问题

以下部分介绍了 Linux 上 SQL Server 2019 (15.x) 的正式发布版 (GA) 的已知问题。

### <a name="general"></a>常规

- 安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的主机名的长度不能超过 15 个字符。 

    - **解决方法**：更改 /etc/hostname 中的名称，使其不超过 15 个字符。

- 手动将系统时间设置为时间倒移，会导致 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 停止更新 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的内部系统时间。

    - **解决方法**：重新启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。

- 仅支持单个实例安装。

    - **解决方法**：如果希望在给定主机上安装多个实例，请考虑使用 VM 或 Docker 容器。 

- [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Configuration Manager 无法连接到 Linux 上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。

- sa 登录名的默认语言是英语。

    - **解决方法**：使用 ALTER LOGIN 语句更改 sa 登录名的语言 。

- OLEDB 提供程序记录以下警告：`Failed to verify the Authenticode signature of 'C:\binn\msoledbsql.dll'. Signature verification of SQL Server DLLs will be skipped. Genuine copies of SQL Server are signed. Failure to verify the Authenticode signature might indicate that this is not an authentic release of SQL Server. Install a genuine copy of SQL Server or contact customer support.`

   - **解决方法**：不需要执行任何操作。 OLEDB 提供程序使用 SHA256 进行签名。 SQL Server 数据库引擎无法正确验证已签名的 .dll。

### <a name="databases"></a>数据库

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

- Windows 上使用内存中 OLTP 的 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 数据库无法在 Linux 上的 SQL Server 2019 (15.x) 上进行还原。 要还原使用内存中 OLTP 的 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 数据库，请首先将数据库升级到 Windows 上的 [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]、SQL Server 2017 或 SQL Server 2019，然后再通过备份/还原或分离/附加将数据库移至 Linux 上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。

- 目前 Linux 不支持用户权限 ADMINISTER BULK OPERATIONS。

### <a name="networking"></a>网络

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

- 使用 NFS 版本 4.2 或更高版本。 较早版本的 NFS 不支持现代文件系统常用的必需功能，例如 `fallocate` 和稀疏文件创建。
- 仅在 NFS 装载上查找 /var/opt/mssql 目录。 不支持其他文件，例如 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 系统二进制文件。
- 安装远程共享时，请确保 NFS 客户端使用 `nolock` 选项。

### <a name="localization"></a>本地化

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

### <a name="sql-server-integration-services-ssis"></a><a id="ssis"></a> SQL Server Integration Services (SSIS)

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

### <a name="sql-server-management-studio-ssms"></a><a id="ssms"></a> SQL Server Management Studio (SSMS)

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
- [在 Docker 上运行](quickstart-install-connect-ubuntu.md)
- [在 Azure 中预配 SQL VM](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)
- [运行和连接 - 云](quickstart-install-connect-clouds.md)

有关常见问题的解答，请参阅 [Linux 上的 SQL Server 常见问题解答](sql-server-linux-faq.yml)。
