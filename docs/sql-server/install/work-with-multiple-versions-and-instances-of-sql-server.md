---
title: 处理多个版本和实例
description: 可安装 SQL Server 的多个实例，也可在已安装 SQL Server 早期版本的计算机上安装 SQL Server。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- concurrent installations [SQL Server]
- versions [SQL Server], multiple
- side-by-side installations [SQL Server]
- multiple SQL Server component versions
- installing SQL Server, side-by-side installations
- Setup [SQL Server], side-by-side installations
- 64-bit edition [SQL Server]
- 32-bit edition [SQL Server]
- editions [SQL Server], side-by-side installations
ms.assetid: 93acefa8-bb41-4ccc-b763-7801f51134e0
author: cawrites
ms.author: chadam
ms.openlocfilehash: ce7c337ccd2899afedbc28781c1ed822f04b8202
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765666"
---
# <a name="work-with-multiple-versions-and-instances-of-sql-server"></a>处理多个 SQL Server 版本和实例

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

可以安装 SQL Server 的多个实例，也可以在已安装 SQL Server 早期版本的计算机上安装 SQL Server。

与 SQL Server 相关的以下项与同一台计算机上安装的多个实例兼容：

- 数据库引擎

- Analysis Services

- Reporting Services（在 SQL Server 2016 及更低版本中）。 自 SQL Server 2016 起。 SQL Server Reporting Services (SSRS) 有单独的安装。 


可以在已安装其他 SQL Server 版本的计算机上升级 SQL Server 的早期版本。 有关支持的升级方案，请参阅 [支持的版本和版本升级](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)。
  
## <a name="version-components-and-numbering"></a>版本组件和编号

 下面的概念对于理解 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的并行实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的行为十分有用。
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的标准产品版本格式为 MM.nn.bbbb.rr，其中每一片断定义为：
  
 MM - 主版本  
  
 nn - 次版本  
  
 bbbb - 内部版本号  
  
 rr - 内部修订版本号  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的每个主版本或次版本中，都会增加该版本号，以便与之前的版本区分。 这一对版本的更改出于多种目的。 这包括在用户界面中显示版本信息、控制升级过程中的文件替换方式、应用服务包，以及作为后续版本的功能区分机制。
  
### <a name="components-shared-by-all-versions-of-ssnoversion"></a>由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

 某些组件由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的所有已安装版本的所有实例共享。 在同一台计算机上并行安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的不同版本时，这些组件将自动升级到最新版本。 此类组件通常会在卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的最后的实例时自动卸载。
  
 示例：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 和 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] VSS Writer。
  
### <a name="components-shared-across-all-instances-of-the-same-major-version-of-ssnoversion"></a>在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本在所有实例之间共享某些组件。 如果在升级过程中选择了这些共享的组件，现有组件将升级到最新版本。
  
示例： [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]、 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书。
  
### <a name="components-shared-across-minor-versions"></a>跨次要版本共享的组件

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本共享组件。
  
示例：安装程序支持文件。
  
### <a name="components-specific-to-an-instance-of-ssnoversion"></a>特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

某些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件或服务特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例。 它们也称为识别实例的组件或服务。 它们与托管它们的实例共用相同的版本，并且仅用于相应实例。
  
示例： [!INCLUDE[ssDE](../../includes/ssde-md.md)]、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]。  
  
### <a name="components-that-are-independent-of-the-ssnoversion-versions"></a>独立于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本的组件

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中将安装某些组件，但这些组件独立于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的版本。 它们可在主版本之间共享，或者由所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本共享。  

示例：Microsoft Sync Framework、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact。  
  
有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 安装的详细信息，请参阅 [使用安装向导安装 SQL Server 2016（安装程序）](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)。 有关如何卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 的详细信息，请参阅[卸载现有 SQL Server 实例（安装程序）](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)。  
  
## <a name="using-ssnoversion-side-by-side-with-previous-versions-of-ssnoversion"></a>并行使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与其早期版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

可以在已运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本实例的计算机上安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果计算机上已存在默认实例，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须作为命名实例安装。  

下表显示了对安装有必需版本 .NET 的通常受支持版本 Windows 上的每个 SQL Server 版本的并行支持：

| 现有实例 | 并行支持| 
|-------------------|----------------------------|
| SQL Server 2019 | SQL Server 2008 - SQL Server 2017| 
| SQL Server 2017 | SQL Server 2008 - SQL Server 2016| 
| SQL Server 2016 | SQL Server 2008 - SQL Server 2014| 

有关详细信息，请参阅[在 Windows 8 及更高版本中使用 SQL Server](https://support.microsoft.com/help/2681562/using-sql-server-in-windows-8-and-later-versions-of-windows-operating)。 

  
> [!CAUTION]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep 不支持在同一台计算机上并行安装 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的已准备实例和早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 但是，可以在同一台计算机上并行安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的相同主版本的多个已准备实例。 有关详细信息，请参阅 [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)。  
>
> 在运行 Windows Server 2008 R2 Server Core SP1 的计算机上，SQL Server 2016 及更高版本无法与旧版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并行安装。 有关 Server Core 安装的详细信息，请参阅 [在服务器核心上安装 SQL Server 2016](../../database-engine/install-windows/install-sql-server-on-server-core.md)。  
  


## <a name="preventing-ip-address-conflicts"></a>防止 IP 地址冲突

并行安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例与 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的独立实例时，请注意避免 IP 地址上的 TCP 端口号冲突。 当 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的两个实例都配置为使用默认 TCP 端口 (1433) 时，通常会发生冲突。 要避免冲突，请将一个实例配置为使用非默认的固定端口。 在独立实例上配置固定端口通常是最简单的。 若将 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 配置为使用不同的端口，则在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例失败到备用节点时，将防止出现会阻止实例启动的意外 IP 地址/TCP 端口冲突。
  
## <a name="see-also"></a>另请参阅

* [安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
* [使用安装向导安装 SQL Server（安装程序）](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)
* [支持的版本和版本升级](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)
* [升级 SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)
* [SQL Server 2019 各个版本及其支持的功能](../../sql-server/editions-and-components-of-sql-server-version-15.md) 
* [版本和 SQL Server 2017 支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)
* [版本和 SQL Server 2016 支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)
* [向后兼容性_已删除](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))