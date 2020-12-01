---
title: 什么是外延安全更新程序？
description: 了解如何使用 SQL Server 注册表来获取终止支持且生命周期结束的 SQL Server 产品（如 SQL Server 2008 和 SQL Server 2008 R2）的外延安全更新程序。
ms.custom: ''
ms.date: 11/24/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: f3a337395be09743be335dd01ac80caf9dc98be0
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96121277"
---
# <a name="what-are-extended-security-updates-for-sql-server"></a>什么是 SQL Server 的外延安全更新程序？
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

本文介绍了如何使用 SQL Server 注册表服务来接收 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 的外延安全更新程序。 若要详细了解其他选项，请参阅[终止支持选项](sql-server-end-of-life-overview.md)。 

## <a name="overview"></a>概述
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的支持生命周期结束后，你可以视需要为服务器注册外延安全更新程序 (ESU) 订阅，最长可继续获得三年保护，直到可以升级到新版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或迁移到 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。 该订阅以两种方式提供：
-  可为本地或托管环境服务器购买。
-  在将本地服务器迁移到 Azure 虚拟机时，默认情况下是免费的，且已经启用。 然后，可以使用 Azure 门户中的 SQL Server 注册表  服务，以注册终止支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，并下载更新（若有）。 

为了继续保护你的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，Microsoft 建议尽快应用 ESU 修补程序。 若要详细了解 ESU，请参阅 [ESU FAQ 页](https://www.microsoft.com/cloud-platform/extended-security-updates)。

> [!IMPORTANT]
> [SQL Server 2008 和 SQL Server 2008 R2 的外延支持已于 2019 年 7 月 10 日停止提供](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)。 对于这些版本，建议使用本文中介绍的外延安全更新程序或其他迁移选项。 有关详细信息，请参阅[终止支持选项](sql-server-end-of-life-overview.md)。

## <a name="what-are-extended-security-updates"></a>什么是外延安全更新程序
[!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 的外延安全更新程序 (ESU) 包括，为已购买外延支持更新订阅的客户预配安全更新程序。

一旦发现安全漏洞并被 [Microsoft 安全响应中心 (MSRC)](https://portal.msrc.microsoft.com) 评为“关键”，便会按需提供 ESU（若需要）   。 因此，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ESU 是不定期发布。

ESU 不包括：
- 新增功能
- 功能改进
- 客户请求的修补程序

### <a name="support"></a>支持
ESU 不包括技术支持，但你可以使用有效支持合同（如 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] / [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 上的[软件保障](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3)或顶级支持/统一支持），以获取 ESU 所涵盖的工作负载的技术支持（如果你选择留在本地的话）。 或者，如果是在 Azure 上托管，你可以使用 Azure 支持计划来获取技术支持。 

  > [!NOTE]
  > Microsoft 无法提供 ESU 订阅未涵盖的 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 实例（无论是在本地，还是在托管环境中）的技术支持。 

## <a name="esu-availability-and-deployment"></a>ESU 可用性和部署
在 Azure、本地或托管环境中运行工作负载的客户可以使用 ESU。

### <a name="azure-virtual-machines"></a>Azure 虚拟机
如果将工作负载迁移到 Azure 虚拟机 (IaaS)，最长可以在终止支持后三年内使用 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 的外延安全更新程序，除了运行虚拟机的成本外，没有任何额外费用。  。 客户无需软件保障，即可在 Azure 中接收外延安全更新程序。 

如果虚拟机配置为使用[自动修补](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)，则在 Windows Server 2008 R2 及更高版本上运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Azure 虚拟机将通过现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更新通道自动接收 ESU  。

需要手动下载在 Windows Server 2008 上运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Azure 虚拟机 (VM) 或未配置[自动修补](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)的 VM，并对其部署 ESU 修补程序（如[本地或托管环境](#on-premises-or-hosted-environments)部分所述）   。

### <a name="on-premises-or-hosted-environments"></a>本地或托管环境
如果有软件保障，可以根据企业协议 (EA)、企业订阅协议 (EAS)、服务器和云合约 (SCE) 或教育解决方案合约 (EES)，最长在终止支持后三年内购买外延安全更新程序 (ESU) 订阅。 只能为需要覆盖的服务器购买 ESU。 可以直接从 Microsoft 或 Microsoft 授权合作伙伴处购买 ESU。 

ESU 协议所涵盖的客户必须按照以下步骤下载并部署 ESU 修补程序：
-  向 [SQL Server 注册表](#create-sql-server-registry)[注册符合条件的实例](#register-instances-for-esus)  。 
-  注册后，只要发布 ESU 修补程序，就可通过 Azure 门户中的下载链接下载该包。 
-  可手动或通过组织中使用的任何更新业务流程解决方案，例如 Microsoft Endpoint Configuration Manager（以前称为 System Center Configuration Manager），将下载的包部署到本地或托管环境。 

> [!NOTE]
> 对于未配置为接收自动更新的 Azure Stack 和 Azure 虚拟机，客户也需要遵循此流程。

有关详细信息，请参阅[外延安全更新程序常见问题解答](https://www.microsoft.com/cloud-platform/extended-security-updates)。 

## <a name="create-sql-server-registry"></a>创建 SQL Server 注册表
若要注册已启用 ESU 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，必须先在 Azure 门户中创建 SQL Server 注册表。 

> [!IMPORTANT]
> 运行已配置[自动更新](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)的 Azure 虚拟机时，无需为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例注册 ESU。 

若要创建 SQL Server 注册表，请按照以下步骤操作：

1. 登录到 [Azure 门户](https://portal.azure.com)。 
1. 选择“创建资源”  选项。 
1. 在搜索框中键入“`SQL Server registry`”。  
1. 依次选择 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 发布的“SQL Server 注册表”  选项和“创建”  。 

   ![Azure 门户的屏幕截图，其中显示了如何创建 SQL Server 注册表。](media/sql-server-extended-security-updates/sql-server-registry-service.png)

1. 在“项目详细信息”  下，从下拉列表中选择你的订阅。 然后，选择现有资源组  ，或选择“新建”  ，以为新 SQL Server 注册表服务新建资源组。 
1. 在“服务详细信息”  下，提供新“SQL Server 注册表”  资源的名称和区域： 

   ![SQL Server 注册表的屏幕截图，其中显示了“基本信息”选项卡。](media/sql-server-extended-security-updates/create-new-sql-server-registry.png)

1. 选择“审阅 + 创建”  ，以审阅 SQL Server 注册表  的详细信息。 通过验证后，选择“创建”  。 

## <a name="register-instances-for-esus"></a>为实例注册 ESU

在“SQL Server 注册表”  资源部署后，可以选择注册[单个](#single-sql-server-instance) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，也可以[批量](#multiple-sql-server-instances-in-bulk)注册多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 必须在 SQL Server 注册表范围内注册至少一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，才能下载任何 ESU 包。 

### <a name="single-sql-server-instance"></a>单个 SQL Server 实例

若要注册单个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，请按照以下步骤操作：

1. 登录到 [Azure 门户](https://portal.azure.com)。 
1. 转到“SQL Server 注册表”  资源。 
1. 在“概览”  窗格中，选择“+ 注册”  ： 

   ![选择“注册”，以注册单个 SQL Server 实例](media/sql-server-extended-security-updates/register-single-sql-server-instance.png)

1. 提供必填信息（如下表所详述），然后选择“注册”  ： 

   |**值**| **说明**|
   | :-------| :------------- |
   | **实例** | 输入命令 `SELECT @@SERVERNAME` 的输出（如 `MyServer\Instance01`）。 | 
   | **SQL 版本** | 从下拉列表中选择“2008”或“2008 R2”。 | 
   | **版本(Edition)** | 从下拉列表中选择适用版本：“Datacenter”、“Developer”（如果已购买 ESU，可以免费部署）、“Enterprise”、“Standard”、“Web”、“Workgroup”。 | 
   | **核心数** | 输入此实例的核心数 | 
   | **主机类型** | 从下拉列表中选择适用主机类型：“虚拟机(本地)”、“物理服务器(本地)”、“Azure 虚拟机”、“Amazon EC2”、“Google Compute Engine”、“其他”。 |
   | **SubscriptionID**<sup>1</sup> | 输入在其中创建 VM 的 SubscriptionID。  |
   | **资源组**<sup>1</sup> | 输入在其中创建 VM 的资源组。  | 
   | **Azure VM 名称**<sup>1</sup>  | 输入 VM 资源名称。  | 
   | **Azure VM 操作系统**<sup>1</sup> | 从下拉列表中选择适用 Windows Server 操作系统版本。 | 

   <sup>1</sup>仅对 Azure 虚拟机是必需的。 

此时，新注册的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例显示在“概览”  窗格的“注册 SQL Server 实例”  部分中： 

![已注册的 SQL Server 实例](media/sql-server-extended-security-updates/registered-sql-instance.png)

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例注册后，“安全更新程序”  部分就可用了。 任何可用的 ESU 都将发布在其中。 

### <a name="multiple-sql-server-instances-in-bulk"></a>多个 SQL Server 实例（批量）

可以通过上传 .CSV 文件来批量注册多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 [正确设置 CSV 文件的格式](#formatting-requirements-for-csv-file)后，可以按照以下步骤操作，使用“SQL Server 注册表”资源批量注册 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例： 

1. 登录到 [Azure 门户](https://portal.azure.com)。 
1. 转到“SQL Server 注册表”  资源。 
1. 在“概览”  窗格中，选择“批量注册”  ：  

   ![选择“批量注册”，以注册多个 SQL Server 实例](media/sql-server-extended-security-updates/bulk-register-sql-server-instances.png)

1. 选择文件图标，以转到 .CSV 文件位置。 选择 .CSV 文件。 然后，选择“注册”  ，以上传文件并注册多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 

   ![上传 CSV 文件，以注册多个 SQL Server 实例](media/sql-server-extended-security-updates/upload-csv-file-for-bulk-registration.png)

### <a name="formatting-requirements-for-csv-file"></a>CSV 文件的格式设置要求
- 值以逗号分隔
- 值不含单引号或双引号
- 列名不区分大小写，但必须按如下命名  ： 
  - name
  - 版本
  - edition
  - cores
  - hostType
  - subscriptionID<sup>1</sup>
  - resourceGroup<sup>1</sup>
  - azureVmName<sup>1</sup>
  - AzureVmOS<sup>1</sup>

<sup>1</sup>仅对 Azure 虚拟机是必需的。 

#### <a name="csv-example-1---on-premises"></a>CSV 示例 1 - 本地

对于本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，CSV 文件应如下所示： 

```csv
name,version,edition,cores,hostType
Server1\SQL2008,2008,Enterprise,12,Physical Server
Server1\SQL2008R2,2008 R2,Enterprise,12,Physical Server
Server2\SQL2008R2,2008 R2,Enterprise,24,Physical Server
Server3\SQL2008R2,2008 R2,Enterprise,12,Virtual Machine
Server4\SQL2008,2008,Developer,8,Physical Server  
```

有关 CSV 文件示例，请参阅 [MyPhysicalServers.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyPhysicalServers.csv)。

#### <a name="csv-example-2---azure-vm"></a>CSV 示例 2 - Azure VM

对于Azure 虚拟机 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，CSV 文件应如下所示： 

```csv
name,version,edition,cores,hostType,subscriptionId,resourceGroup,azureVmName,azureVmOS    
ProdServerUS1\SQL01,2008 R2,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ProdServerUS1\SQL02,2008 R2,Enterprise,24,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ServerUS2\SQL01,2008,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
ServerUS2\SQL02,2008,Enterprise,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
SalesServer\SQLProdSales,2008 R2,Developer,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM3,2008 R2  
```

有关定目标到 Azure VM 的 CSV 文件示例，请参阅 [MyAzureVMs.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyAzureVMs.csv)。 


> [!TIP]
> 对于可以将必需 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例注册信息生成到 .CSV 文件中的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和 PowerShell 示例脚本，请参阅 [ESU 注册脚本示例](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts.md)。 

## <a name="download-esus"></a>下载 ESU

使用 SQL Server 注册表服务注册 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例后，便可以使用 Azure 门户中的链接下载外延安全更新程序包（若有）。 

若要下载 ESU，请按照以下步骤操作： 

1. 登录到 [Azure 门户](https://portal.azure.com)。 
1. 转到“SQL Server 注册表”  资源。 
1. 选择导航窗格中的“安全更新程序”  。 

   ![检查“安全更新程序”窗格中是否有更新程序](media/sql-server-extended-security-updates/security-updates-sql-registry.png)

1. 从其中下载安全更新程序（若有）。 

## <a name="supported-regions-and-data-residency"></a>支持的区域和数据驻留

一部分 Azure 区域提供了 SQL Server 注册表服务（预览版）。 下表显示了支持的区域以及每个区域的数据驻留类型。

| **区域** | **数据驻留** |
|:--|:--|
|澳大利亚东部|地域|
|Australia Southeast|地域|
|加拿大中部|地域|
|法国中部|地域|
|Japan East|地域|
|日本西部|地域|
|韩国中部|地域|
|韩国南部|地域|
|美国中北部|地域|
|北欧|地域|
|美国中南部|地域|
|Southeast Asia|单区域|
|印度南部|地域|
|南非北部|地域|
|英国南部|地域|
|英国西部|地域|
|美国西部|地域|
|美国东部|地域|
|美国中部|地域|
|东亚|地域|
|西欧|地域|
|美国中西部|地域|
|美国西部 2|地域|
|美国东部 2|地域|

在带有地理驻留的区域中，SQL 注册表服务维护异地冗余存储帐户 (GRS) 中的数据备份。  在带有单个区域驻留的区域中，SQL 注册表服务维护区域冗余存储帐户 (ZRS) 中的数据备份。 有关详细信息，请参阅[信任中心](https://azuredatacentermap.azurewebsites.net/)。

## <a name="configure-regional-redundancy"></a>配置区域冗余 

如果需要为自己的 SQL Server 注册表  提供区域冗余，客户可以在两个不同的区域中创建注册数据。 然后，客户可以根据 SQL Server 注册表  服务可用性，从两个区域中的任何一个下载安全更新程序。 

为了实现区域冗余，必须在两个不同的区域中创建 SQL Server 注册表  服务，且必须在这两个服务之间拆分 SQL Server 清单。 这样，一半的 SQL Server 是向一个区域中的注册表服务注册，另一半的 SQL Server 是向另一个区域中的注册表服务注册。 

若要配置区域冗余，请按照以下步骤操作：

1. 将 SQL Server 2008 或 2008 R2 清单拆分为两个文件（如 upload1.csv 和 upload2.csv）。 
  
   :::image type="content" source="media/sql-server-extended-security-updates/two-upload-files-for-regional-redundancy.png" alt-text="示例上传文件":::

1. 在一个区域中创建第一个 SQL Server 注册表  服务，然后向它批量注册其中一个 csv 文件。 例如，在“美国西部”  区域中创建第一个 SQL Server 注册表  服务，然后批量注册使用 upload1.csv 文件的 SQL Server。 
1. 在第二个区域中创建第二个 SQL Server 注册表  服务，然后向它批量注册另一个 csv 文件。 例如，在“美国东部”  区域中创建第二个 SQL Server 注册表  服务，然后批量注册使用 upload2.csv 文件的 SQL Server。 


在向两个不同的 SQL Server 注册表  资源注册数据后，可以根据服务可用性从两个区域中的任何一个下载安全更新程序。 


## <a name="faq"></a>常见问题解答

有关外延安全更新程序的一般常见问题解答，可以参阅[外延安全更新程序 FAQ](https://www.microsoft.com/cloud-platform/extended-security-updates)。 下面列出了特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的常见问题解答。 

**SQL Server 2008 和 2008 R2 终止支持是哪一天？**

[!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 的终止支持日期为 2019 年 7 月 9 日。 

**终止支持意味着什么？**

对于 Business 和 Developer 产品（如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Windows Server），Microsoft 生命周期策略提供 10 年支持（主要支持为 5 年，外延支持为 5 年）。 根据策略，在外延支持期结束后，就不会提供修补程序或安全更新程序，这可能会导致安全性和合规性问题出现，并给客户的应用程序和业务带来严重的安全风险。

**什么版本的 SQL Server 有资格获得外延安全更新程序？**

[!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 的 Enterprise、Datacenter、Standard、Web 和 Workgroup 版本有资格获得适用于 x86 和 x64 版本的外延安全更新程序。 

**外延安全更新程序产品/服务何时可用？**

外延安全更新程序现可供购买，可以从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 许可合作伙伴处订购。 外延安全更新程序（若有）会在终止支持后开始按需交付。 有意迁移到 Azure 的客户可以立即执行此操作。 

**外延安全更新程序包括什么？** 

外延安全更新程序包括，最长可以在 2019 年 7 月 9 日后三年内预配 [Microsoft 安全响应中心 (MSRC)](https://portal.msrc.microsoft.com/) 评为“关键”  的安全更新程序和公告。 外延安全更新程序（若有）会按需分发。 外延安全更新程序不包括技术支持，但你可以使用其他 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 支持计划来获取有关 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 问题的帮助，这些问题涉及外延安全更新程序所涵盖的工作负载。 外延安全更新程序不包括新功能、功能改进和客户请求的修补程序。 不过，[!INCLUDE[msCoName](../../includes/msconame-md.md)] 可以根据需要包括非安全修补程序。

**SQL Server 2008 和 2008 R2 的外延安全更新程序为什么只提供“关键”更新程序？**

对于过去的终止支持事件，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 只提供关键安全更新程序，这符合企业客户的合规性标准。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不分发常规每月安全更新程序。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 只为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 被标识为受影响产品的 MSRC 公告按需提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全更新程序 (GDR)。
如果新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 重要更新程序不提供，但是被客户（而不是 MSRC）视为关键更新程序，我们将根据具体情况与客户合作，以建议适当的缓解措施。

**什么许可计划有资格获得外延安全更新程序？**

软件保障客户可以根据企业协议 (EA)、企业订阅协议 (EAS)、服务器和云合约(SCE) 或教育解决方案合约 (EES) 在本地购买外延安全更新程序。 软件保障不需要在相同的合约中。

**SQL Server 客户是否必须运行最新 Service Pack 才能受益于外延安全更新程序？**

是，客户必须运行带最新 Service Pack 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Windows Server 2008 和 2008 R2，才能应用外延安全更新程序。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 只生成可应用于 Service Pack 的更新程序。

**什么方法适合没有软件保障的 SQL Server 客户？** 

对于没有软件保障的客户，获得外延安全更新程序的另一种方法是迁移到 Azure。 对于可变工作负载，建议客户通过即用即付迁移到 Azure，这样就能随时伸缩。 对于可预测的工作负载，建议客户通过服务器订阅和预留实例迁移到 Azure。
  
**此产品/服务是否也适用于 SQL Server 2005？**

不是。 对于旧版本，建议升级到最新版，但客户可以升级到 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 或 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 版本来使用此产品/服务。

**我能否在 Azure 上部署全新的 SQL Server 2008 或 2008 R2 实例，且仍获得外延安全更新程序？**

能，客户可以在 Azure 虚拟机上启动新的 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 或 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 实例，同时获得外延安全更新程序。

**在终止支持日期后，我能否在本地获取 SQL Server 2008 或 2008 R2 的技术支持，而无需购买外延安全更新程序？**

不是。 如果客户有 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 或 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]，并选择在迁移期间留在本地，而不购买外延安全更新程序，那么即使他们有支持计划，也无法记录支持票证。 不过，如果他们迁移到 Azure，则可以使用 Azure 支持计划来获得支持。

**如果 SQL Server 2008 和 2008 R2 客户想要自带许可 (BYOL)，是否必须有软件保障？**

是，客户必须有软件保障，才能利用 Azure 虚拟机上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] BYOL 计划作为许可移动性计划的一部分。 对于没有软件保障的客户，建议客户迁移到 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 环境的 Azure SQL 托管实例。 客户还可以迁移到即用即付 Azure 虚拟机。 按核心许可 SQL 的软件保障客户也可以使用 Azure 混合权益 (AHB) 迁移到 Azure。

Azure SQL 托管实例是 Azure 中几乎与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本地完全兼容的一项服务。 托管实例提供内置的高可用性/灾难恢复功能，以及智能性能功能和动态缩放功能。 托管实例还提供无需手动安全修补和升级的无版本体验。 若要详细了解 BYOL 计划，请参阅 Azure 定价指南页。

**客户可以使用什么方法在 Azure 中运行 SQL Server？**

客户可以将旧的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 环境迁移到 Azure SQL 托管实例（完全托管的数据平台服务 (PaaS)，它提供“无版本”选项来消除终止支持日期方面的顾虑），也可以迁移到 Azure 虚拟机来获得安全更新程序。 迁移的数据库会继续与旧系统兼容。 有关详细信息，请参阅[兼容性认证](../../database-engine/install-windows/compatibility-certification.md)。

在终止支持日期 2019 年 7 月 9 日之后的三年内，外延安全更新程序将提供给 Azure 虚拟机中的 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]。 对于要从 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 升级的客户，支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的所有后续版本。 对于 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 至 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]，客户必须使用受支持的最新 Service Pack。 自 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 起，建议客户使用最新累积更新。 请注意，自 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 起，将不提供 Service Pack，而只提供累积更新和常规分发版本 (GDR)。

Azure SQL 托管实例是 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 中实例范围内部署选项，它提供最广泛的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 引擎兼容性和本机虚拟网络 (VNET) 支持，因此你可以将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库迁移到托管实例，而无需更改应用。 它将丰富的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 外围应用与完全托管智能服务的运营和财务优势相结合。 利用新的 Azure 数据库迁移服务，将 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 迁移到 Azure SQL 托管实例，几乎不需要更改应用程序代码。

**客户能否对 SQL Server 2008 和 2008 R2 版本利用 Azure 混合权益？**

能，具有有效软件保障或等效服务器订阅的客户可以通过现有本地许可投资来利用 Azure 混合权益，在运行于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和 Azure 虚拟机上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上进行折扣定价。

**客户能否在 Azure 政府区域获取免费的外延安全更新程序？**

能，在 Azure 政府区域的 Azure 虚拟机上，可以使用外延安全更新程序。

**客户能否在 Azure Stack 上获取免费的外延安全更新程序？**

能，客户可以将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Windows Server 2008 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 迁移到 Azure Stack，并在终止支持日期后接收外延安全更新程序，而无需额外支付费用。

**对于使用共享存储的 2008 和 2008 R2 SQL 群集客户，迁移到 Azure 的指导是什么？**

Azure 暂不支持共享存储群集。 有关如何在 Azure 上配置高度可用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的建议，请参阅 [SQL Server 高可用性指南](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr)。

**客户能否将 SQL Server 的外延安全更新程序用于第三方主机托管服务提供商？**

如果客户将自己的 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 环境迁移到其他云提供商上的 PaaS 实现中，便无法利用外延安全更新程序。 如果客户想要迁移到虚拟机 (IaaS)，可以通过软件保障利用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 许可移动性计划进行迁移，并从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 购买外延安全更新程序，以手动将修补程序应用于在授权的 SPLA 主机托管服务提供商服务器上的 VM (IaaS) 中运行的 [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] 实例。 不过，Azure 中的免费更新程序更有吸引力。

**在 Azure 虚拟机中增强 SQL Server 性能的最佳做法是什么？** 

有关如何在 Azure 虚拟机上优化 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 性能的建议，请参阅 [SQL Server 优化指南](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)。 

## <a name="see-also"></a>另请参阅

- [SQL Server 2008/2008 R2 生命周期页](https://support.microsoft.com/lifecycle/search?alpha=sql%20server%202008)
- [SQL Server 2008/2008 R2 终止支持页](https://aka.ms/sqleos)
- [外延安全更新程序常见问题解答 (FAQ)](https://aka.ms/sqleosfaq)
- [Microsoft 安全响应中心 (MSRC)](https://portal.msrc.microsoft.com/security-guidance/summary)
- [使用 Azure 自动化管理 Windows 更新](/azure/automation/update-management/overview)
- [SQL Server 自动修补](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)
- [Microsoft 数据迁移指南](https://datamigration.microsoft.com/)
- [Azure 迁移：将当前 SQL Server 2008/2008 R2 迁移到 Azure VM 的直接迁移选项](https://azure.microsoft.com/services/azure-migrate/)
- [用于 SQL 迁移的云采用框架](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)
- [GitHub 上的 ESU 相关脚本](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/sql-server-extended-security-updates/scripts)

