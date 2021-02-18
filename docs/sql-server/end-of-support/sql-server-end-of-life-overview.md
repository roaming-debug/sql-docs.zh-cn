---
title: 终止支持选项
description: 了解已达到支持结束时间的 SQL Server 产品的不同选项，如 SQL Server 2005、SQL Server 2008 和 SQL Server 2008 R2。
ms.date: 08/12/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c7560fbda04ef5a14c73ead8a7bbc8337e3e38ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352905"
---
# <a name="sql-server-end-of-support-options"></a>SQL Server 终止支持选项 
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

本文介绍了用于处理已达到支持结束时间的 SQL Server 产品的选项。

## <a name="understanding-the-sql-server-lifecycle"></a>了解 SQL Server 生命周期

每个版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 都有至少 10 年的支持期限，其中包括五年的主要支持和五年的扩展支持：
-  “主要支持”  包括功能、性能、可伸缩性和安全更新。 
-  “扩展支持”  仅包含安全更新。 

“终止支持”  （有时也称为生命周期结束）表示产品已达到其生命周期的结束时间，并且不再为该产品提供服务和支持。 有关 Microsoft 生命周期的详细信息，请参阅 [Microsoft 生命周期策略](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy)。



## <a name="options"></a>选项

一旦 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 达到终止支持阶段，你可以选择：
- 升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的当前版本。
- 购买[扩展安全更新订阅](https://www.microsoft.com/cloud-platform/extended-security-updates)。 
- 将工作负载按原样迁移到 Azure 虚拟机，以便获取[免费扩展安全更新](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)。
- 将工作负载迁移到 [Azure SQL 数据库服务](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)。 

有关计划和自动化升级或迁移的详细信息、指南和工具，请参阅 [SQL Server 2005 终止支持](https://www.microsoft.com/sql-server/sql-server-2005)和 [SQL Server 2008 终止支持](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)。  

![终止支持选项](media/sql-server-end-of-life-overview/sql-server-upgrade-paths.png)

本文介绍了每种方法的优点和注意事项，以及可帮助引导决策过程的其他资源。

## <a name="upgrade-sql-server"></a>升级 SQL Server

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 达到支持结束时间后，可以选择升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的更新版本和受支持的版本。 这为你提供了环境一致性，允许你使用最新的功能集，并采用新版本的支持生命周期。

### <a name="benefits"></a>优点
- **最新技术**：新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本引入了创新元素，其中包括性能、可伸缩性和高可用性功能，并提高了安全性。 
- **控件**：因为你同时管理硬件和软件，因此你对功能和可伸缩性具有最大控制权。
- **熟悉的环境**：如果从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的旧实例升级，这是最相似的环境。
- **广泛的适用性**：适用于任何类型的数据库应用程序，包括 OLTP 系统和数据仓库。
- **数据库应用程序风险较低**：通过保持与旧系统相同级别的数据库兼容性，可防止现有数据库应用程序的功能和性能更改，这些更改可能会造成不利影响。 仅当应用程序需要使用由较新的数据库兼容性设置封闭的功能时，才需要完全重新验证应用程序。 有关详细信息，请参阅[兼容性认证](../../database-engine/install-windows/compatibility-certification.md)。

### <a name="considerations"></a>注意事项

- **成本**：此方法需要最大的前期投资和最持续的管理。 你需要购买、维护和管理你自己的硬件和软件。
- **故障时间**：可能会有故障时间，具体取决于你的升级策略。 在就地升级过程中，可能会遇到问题。
- **复杂性**：如果你使用的是 Windows Server 2008 或 [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)]，还需要升级 OS，因为这些 Windows 版本可能不支持新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 在 OS 升级过程中会增加风险，因此，进行并排迁移可能更谨慎，但成本更高。 Windows Server 2008 或 [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)] 的故障转移群集实例不支持就地 OS 升级。 

  > [!NOTE]
  > 从 Windows Server 2016 开始，可以使用群集 OS 滚动升级。

### <a name="resources"></a>资源

[安装媒体](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm)   
[使用安装向导升级 SQL Server](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

新增功能：
- [SQL Server 2016](../what-s-new-in-sql-server-2016.md)
- [SQL Server 2017](../what-s-new-in-sql-server-2017.md) 
- [SQL Server 2019](../what-s-new-in-sql-server-ver15.md)   

硬件要求：
- [SQL Server 2017 及更低版本](../install/hardware-and-software-requirements-for-installing-sql-server.md)  
- [SQL Server 2019](../install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)    

支持的版本和版本升级：
- [SQL Server 2016](../../database-engine/install-windows/supported-version-and-edition-upgrades.md?view=sql-server-2016&preserve-view=true) 
- [SQL Server 2017](../../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md)
- [SQL Server 2019](../../database-engine/install-windows/supported-version-and-edition-upgrades-version-15.md)

工具：
-  [数据库实验助手](../../dea/database-experimentation-assistant-overview.md)可帮助评估特定工作负载的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的目标版本。 
-  [数据迁移助手](../../dma/dma-overview.md)有助于检测可能会影响新版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的数据库功能的兼容性问题。 
-  [查询优化助手](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)有助于优化在升级数据库兼容性时可能会遇到不利影响的工作负载。

下图提供了多年来 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的各种版本的创新示例： 

![25 年的 SQL Server 创新](media/sql-server-end-of-life-overview/sql-server-version-improvements.png)

## <a name="extend-support"></a>扩展支持 

如果尚未准备好进行升级，并且尚未准备好迁移到云，可以购买扩展安全更新订阅，以接收支持日期结束后三年的重要  安全更新。  

### <a name="benefits"></a>优点 

- **应用程序支持**：如果应用程序需要在新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上重新认证，那么这是最佳选项。 这对于不使用[兼容性认证](../../database-engine/install-windows/compatibility-certification.md)的应用程序很常见。 
- **一致的基础结构**：无需以任何方式更改基础结构。 
- **技术支持**：如果你有软件保障或其他支持计划，可以继续从终止支持的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 中获得技术支持。 这是获取 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 支持的唯一方法。 
- **Time**：此选项有效期为三年，为你提供额外的时间来验证应用程序。 

### <a name="considerations"></a>注意事项 

- **有限的可用性**：此选项仅适用于具有软件保障或订阅许可证的客户。 
- **成本**：此选项的成本可能很高，因为扩展安全更新每年大约占据本地许可成本的 75%。
- **有限的时间框架**：此选项有效期为三年，因此，如果想要确保安全性和合规性，则在三年期满时仍需要升级或迁移。
- **无 bug 修复**：如果遇到与产品有关的非安全 bug，[!INCLUDE[msCoName](../../includes/msconame-md.md)] 将不会为其发布修补程序。 
- **有限的支持**：扩展安全更新不包括新功能、功能改进或客户请求的修补程序。 安全修补程序仅限于 [Microsoft 安全响应中心 (MSRC)](https://portal.msrc.microsoft.com/) 评级为“重要”的安全更新。

### <a name="resources"></a>资源

[扩展安全更新 (ESU) 概述](sql-server-extended-security-updates.md)       
[详细的 ESU 常见问题](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[通过按原样迁移到 Azure VM 来免费扩展支持](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)       
[软件保障](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default)       

## <a name="azure-virtual-machine"></a>Azure 虚拟机

另一种方法是将工作负载迁移到[运行 SQL Server 的 Azure 虚拟机](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)。 可以按原样迁移系统，并保持终止支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，也可以升级到新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 对于需要 OS 级别访问权限的迁移和应用程序，这是最佳选择。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 虚拟机已针对需要快速迁移到云中且只需进行极少量更改甚至不需要任何更改的现有应用程序准备好进行直接迁移。 

### <a name="benefits"></a>优点

- **免费扩展安全更新**：如果选择使用 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 或 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 按原样保持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，即使没有软件保障，你也可以在超过支持日期结束后的三年内获取免费扩展安全更新。 
- **节省成本**：节省硬件和服务器软件的成本，只需支付每小时使用量。 
- **直接迁移**：可以将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和应用程序基础结构直接转移到云中，只需进行少量更改甚至不需要任何更改。 
- **托管环境**：你将获得托管环境的好处，如卸载硬件和软件维护。 
- **自动化**：如果使用的是 [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)] 和更高版本，你将获得自动修补和自动备份的好处。 
- **OS 控制**：可以控制操作系统环境，但使用熟悉的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能集。 
- **快速部署**：可以从虚拟机映像库快速进行部署。 
- **许可移动性**：可以提供许可证，以便降低运营成本。 
- **高可用性**：Azure 基础结构提供了高达 99.99% 的可用性，不仅可以从内置的虚拟机可用性中获益，还可以利用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 高可用性选项，例如故障转移群集实例和 Always On 可用性组。 
- **数据库应用程序风险较低**：通过将数据库兼容性保持在与旧数据库相同的级别，可以防止现有数据库应用程序的功能和性能更改，这些更改可能会造成不利影响。 仅当应用程序需要使用由较新的数据库兼容性设置封闭的功能时，才需要完全重新验证应用程序。 有关详细信息，请参阅[兼容性认证](../../database-engine/install-windows/compatibility-certification.md)。

### <a name="considerations"></a>注意事项

- **可管理性**：仍需要同时管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和操作系统软件。 
- **网络**：必须将虚拟机配置为与网络和 Active Directory 基础结构集成，这是新增的复杂性层。 
- **共享存储 FCI**：Azure 虚拟机仅支持使用存储空间直通或高级文件共享的故障转移群集实例，并且不支持使用共享存储的故障转移群集实例。 因此，当使用 Windows Server 2012 或更高版本时，Azure 虚拟机仅支持故障转移群集实例。
- **可伸缩性故障时间**：更改 CPU 和存储资源会造成停机。 
- **大小限制**：尽管 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例可以支持任意数量的数据库，但是，单个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的所有数据库的累积总数为 256 TB，而对于本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 则是 524 PB。 

### <a name="resources"></a>资源

[SQL Server VM 概述](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)       
[选择 Azure SQL 选项](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[将 SQL Server 迁移到 Azure VM](/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql)       
[用于按原样迁移到 Azure 的免费扩展安全更新 (ESU)](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)       
[扩展安全更新 (ESU) 概述](sql-server-extended-security-updates.md)       
[详细的 ESU 常见问题](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[SQL 虚拟机自动修补](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)       
[SQL 虚拟机自动备份](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2)       
[SQL 虚拟机高可用性](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr)       
[SQL 虚拟机常见问题](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-faq)       

## <a name="azure-sql-database-single-database"></a>Azure SQL 数据库单一数据库

如果希望日后减轻维护负担、降低成本，并不再需要升级，则可以将工作负载移动到 [Azure SQL 数据库单一数据库](/azure/sql-database/sql-database-single-database)中。 对于需要使用最新的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 稳定功能，且开发与面市时间有限的新式云应用程序，这是最佳选项。 

### <a name="benefits"></a>优点

- **成本**：单一数据库可能经济高效，因为硬件、软件和维护成本已减轻，并且可以按秒或小时的使用情况付费。 
- **灵活性**：如果开发人员工作效率和解决方案快速上市时间至关重要，或者需要提供外部访问权限，则单一数据库特别适用于云设计的应用程序。  
- **常见功能**：提供最常用的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 功能，但不及 SQL 托管实例那么多。  
- **快速部署**：可以快速部署单一数据库。 
- **可伸缩性**：可以根据业务需要快速轻松地扩展和缩减，从而提供额外的节省成本优势。 
- **可用性**：服务成本包括存储和高可用性，同时保证 99.995% 的可用性。  
- **自动化**：修补和备份会自动进行，从而节省了宝贵的维护时间。  
- **智能见解**：利用内置的智能分析来深入了解数据库性能。  
- **无版本**：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 无版本，这意味着你始终使用的是最新版本，并且不必担心升级或停机问题。 此外，你始终使用的是最新和最出色的版本，其中包含我们最先发布到云中的最新稳定功能。
- **数据库应用程序风险较低**：通过将数据库兼容性保持在与本地数据库相同的级别，可以防止现有应用程序的功能和性能更改，这些更改可能会造成不利影响。 仅当应用程序需要使用由较新的数据库兼容性设置封闭的功能时，才需要完全重新验证应用程序。 有关详细信息，请参阅[兼容性认证](../../database-engine/install-windows/compatibility-certification.md)。

### <a name="considerations"></a>注意事项

- **有限的迁移选项**：一次只能迁移一个数据库，而不是整个实例。   
- **功能限制**：尽管可以使用最常用的 Azure SQL 数据库功能，但单一数据库的功能集并不像 Azure SQL 托管实例或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 那样完整。 
- **Transact-SQL 差异**：单一数据库与本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之间存在一些 [!INCLUDE[tsql](../../includes/tsql-md.md)] (T-SQL) 差异。 
- **大小限制**：单一数据库的最大数据库大小为 100 TB，而 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的最大数据库大小为 524 PB。 
- **维护时间**：虽然几乎是透明的，但无法保证准确的维护时间。 

### <a name="resources"></a>资源

[Azure SQL 数据库概述](/azure/sql-database/sql-database-technical-overview)       
[选择 Azure SQL 选项](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[SQL 数据库功能比较](/azure/sql-database/sql-database-features)       
[将 SQL Server 迁移到单一数据库](/azure/sql-database/sql-database-single-database-migrate)       
[更广泛的迁移过程](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)       
[单一数据库 T-SQL 差异](/azure/sql-database/sql-database-transact-sql-information)       
[vCore](/azure/sql-database/sql-database-vcore-resource-limits-single-databases) 和 [DTU](/azure/sql-database/sql-database-dtu-resource-limits-single-databases) 资源限制       
[智能见解](/azure/sql-database/sql-database-intelligent-insights)       

工具：
- [Data Migration Assistant](../../dma/dma-overview.md)
- [数据库迁移服务](/azure/dms/dms-overview)

## <a name="sql-managed-instance"></a>SQL 托管实例

如果想要充分利用减轻维护和成本负担，但发现 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 单一数据库的功能集过于限制，则可以迁移到 [SQL 托管实例](/azure/sql-database/sql-database-managed-instance)。 托管实例与本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 非常相似，无需担心硬件故障或修补问题。 托管实例是系统和用户数据库的集合，附带一组共享资源，这些资源可直接迁移，并且可用于大多数到云的迁移。 对于需要使用最新的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 稳定功能，并且在进行极少量更改的情况下迁移到云中的新应用程序或现有本地应用程序，这是最佳选项。 

### <a name="benefits"></a>优点

- **成本**：可以通过卸载软件和硬件维护来节约成本。  
- **直接迁移**：可以将整个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本地实例直接迁移到托管实例，其中包括所有数据库，只需对数据库进行极少量更改甚至不需要任何更改。 
- **功能**：托管实例的功能集与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地实例的功能集密切匹配，如跨数据库查询、事务复制发布和分发、SQL 作业计划和 CLR 支持。 
- **可伸缩性**：托管实例中的所有数据库共享资源，并且可以在无停机的情况下随时扩展和缩减。   
- **自动化**：修补和备份会自动进行，从而节省了宝贵的维护时间。  
- **可用性**：服务成本包括存储和高可用性，同时保证 99.99% 的可用性。  
- **智能见解**：利用内置的智能分析来深入了解数据库性能。  
- **无版本**：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 无版本，这意味着你始终使用的是最新版本，并且不必担心升级或停机问题。 此外，你始终使用的是最新和最出色的版本，其中包含我们最先发布到云中的最新稳定功能。
- **数据库应用程序风险较低**：通过将数据库兼容性保持在与本地数据库相同的级别，可以防止现有数据库应用程序的功能和性能更改，这些更改可能会造成不利影响。 仅当应用程序需要使用由较新的数据库兼容性设置封闭的功能时，才需要完全重新验证应用程序。 有关详细信息，请参阅[兼容性认证](../../database-engine/install-windows/compatibility-certification.md)。

### <a name="considerations"></a>注意事项

- **成本**：与单一数据库选项相比，托管实例选项的成本可能更高。  
- **Transact-SQL 差异**：单一数据库与本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之间存在一些 [!INCLUDE[tsql](../../includes/tsql-md.md)] (T-SQL) 差异。  
- **部署**：与单一数据库相比，部署托管实例可能需要更多的时间。  
- **功能限制**：尽管托管实例与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 共享大多数功能，但仍存在一些不受支持的功能。 
- **大小限制**：托管实例中所有数据库的总存储大小限制为 8 TB，而本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的总存储大小限制为 524 PB。  
- **网络**：托管实例的网络要求向基础结构增加了额外的复杂性，并需要 Azure ExpressRoute 或 VPN 网关。
- **维护时间**：虽然几乎是透明的，但无法保证准确的维护时间。 

### <a name="resources"></a>资源

[SQL 托管实例概述](/azure/sql-database/sql-database-managed-instance)       
[选择 Azure SQL 选项](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[SQL 数据库功能比较](/azure/sql-database/sql-database-features)       
[将 SQL Server 迁移到 Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance-migrate)       
[更广泛的迁移过程](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)       

工具：
- [Data Migration Assistant](../../dma/dma-overview.md)
- [数据库迁移服务](/azure/dms/dms-overview)

## <a name="non-sql-options"></a>非 SQL 选项

对于某些类型的应用程序，你可能还需要考虑使用非关系或 NoSQL 解决方案，如 Azure Cosmos DB 或 Azure 表存储。

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

对于新式、可伸缩、移动和 Web 应用程序（使用 JSON 数据并要求结合使用功能强大的查询和事务数据处理），请考虑使用 Azure Cosmos DB。 有关详细信息，请参阅 [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)。 有关导入数据的信息，请参阅[将数据导入到 Cosmos DB](/azure/cosmos-db/import-data/)。

Azure Cosmos DB 具有以下优点：
- 已为你的文档编制索引，你可以使用熟悉的 SQL 语法来查询它们。
- 该数据库未设计架构。
- 无需重新生成索引即可向文档添加属性。
- 直接在数据库引擎内获取 JSON 和 JavaScript 支持。
- 获取对地理空间数据和集成其他 Azure 服务（包括 Azure Search、HDInsight 和 Data Factory）的本机支持。
- 获得低延迟、高性能存储并保留吞吐量级别。

### <a name="azure-table-storage"></a>Azure 表存储

请考虑使用 Azure 表存储以使用经济高效的解决方案存储数千兆半结构化数据。 有关详细信息，请参阅 [表存储](https://azure.microsoft.com/services/storage/tables/)。

Azure 表存储具有以下优点：
- 无需使数据离线即可扩展你的应用和表架构。
- 无需分片数据集即可纵向扩展。
- 获取跨多个区域复制数据的异地冗余存储。

## <a name="lifecycle-dates"></a>生命周期日期

下表提供了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品的近似生命周期日期。 有关更多详细信息和准确性，请参阅 [Microsoft 生命周期策略](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy)页。 

| **版本**     | **发布年份** | **主要支持结束年份** | **延长支持结束年份** |
| :---------------| :--------------- | :------------------------------ | :---------------------------- |
| [SQL Server 2019](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202019) | 2019 | 2025 | 2030 |
| [SQL Server 2017](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202017) | 2017 | 2022 | 2027 |
| [SQL Server 2016](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202016) | 2016 | 2021 | 2026 |
| [SQL Server 2014](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202014) | 2014 | 2019 | 2024 |
| [SQL Server 2012](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202012) | 2012 | 2017 | 2022 |
| [SQL Server 2008 R2](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008%20R2) | 2010 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2008](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008) | 2008 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2005](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202005) | 2006 | 2011 | [2016](https://www.microsoft.com/sql-server/sql-server-2005) |
| [SQL Server 2000](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202000) | 2000 | 2005 | [2013](/archive/blogs/cdnitmanagers/sql-server-2000-end-of-support-april-2013) |

> [!IMPORTANT]
> 如果该表与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 生命周期页之间存在任何差异，则 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 生命周期将取代该表，因为该表将用作近似引用。  

## <a name="next-steps"></a>后续步骤  
[SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019)   
[SQL Server 2005 终止支持](https://www.microsoft.com/sql-server/sql-server-2005)   
[SQL Server 2008 终止支持](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)   
[扩展安全更新 (ESU) 概述](sql-server-extended-security-updates.md)   
[用于按原样迁移到 Azure 的免费扩展安全更新 (ESU)](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)   
[SQL Server VM 概述](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)   
[Azure SQL 数据库概述](/azure/sql-database/sql-database-technical-overview)