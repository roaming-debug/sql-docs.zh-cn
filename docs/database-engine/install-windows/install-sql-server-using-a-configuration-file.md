---
title: 使用配置文件安装 SQL Server | Microsoft Docs
description: 可使用 SQL Server 安装程序生成一个配置文件，以使用统一配置在整个组织中部署 SQL Server。
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: a832153a-6775-4bed-83f0-55790766d885
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: c67682305f0ed48c1d96cf5ca41ad0e88e421e06
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125899"
---
# <a name="install-sql-server-using-a-configuration-file"></a>使用配置文件安装 SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
 
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序提供了基于系统默认值和运行时输入生成配置文件的功能。 可以使用配置文件在整个企业中部署具有相同配置的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 通过创建一个启动 Setup.exe 的批处理文件，还可以使企业范围内的手动安装得以标准化。 
 
本文面向 SQL Server 2016 版及更高版本。 有关 SQL Server 较早版本的信息，请参阅[使用配置文件安装 SQL Server 2014](/previous-versions/sql/2014/database-engine/install-windows/install-sql-server-using-a-configuration-file?view=sql-server-2014&preserve-view=true)。
 
安装程序仅支持通过命令提示符使用配置文件。 下面列出了在使用配置文件时参数的处理顺序：  
  
-  配置文件覆盖包中的默认值  
  
-   命令行的值覆盖配置文件中的值  
  
 配置文件可以用来跟踪每个安装的参数和值。 这使得配置文件适合用于对安装进行验证和审核。 
  
## <a name="configuration-file-structure"></a>配置文件结构  
 ConfigurationFile.ini 文件是一个文本文件，其中具有参数（名称/值对）和描述性注释。 
  
 下面是 ConfigurationFile.ini 文件的一个示例：  
  
```  
; Microsoft SQL Server Configuration file  
[OPTIONS]  
; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE.  
; This is a required parameter.  
ACTION="Install"  
; Specifies features to install, uninstall, or upgrade.  
; The list of top-level features include SQL, AS, RS, IS, and Tools.  
; The SQL feature will install the database engine, replication, and full-text.  
; The Tools feature will install Management Tools, Books online,   
; SQL Server Data Tools, and other shared components.  
FEATURES=SQL,Tools  
```  
  
#### <a name="how-to-generate-a-configuration-file"></a>如何生成配置文件  
  
1. 插入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装介质， 然后双击根文件夹中的 Setup.exe。 若要从网络共享进行安装，请找到共享中的根文件夹，然后双击 Setup.exe。 
  
    > [!NOTE]  
    >  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express Edition 安装程序不会自动创建配置文件。 以下命令将启动安装程序并创建配置文件。 
    >   
    >  SETUP.exe /UIMODE=Normal /ACTION=INSTALL  
  
2. 按照向导操作，直到出现 **“准备安装”** 页。 配置文件的路径是在 **“准备安装”** 页的配置文件路径部分中指定的。 有关如何安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的详细信息，请参阅[使用安装向导安装 SQL Server（安装程序）](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)。 
  
3. 取消安装并且不要真正完成安装，以便生成 INI 文件。 
  
    > [!NOTE]  
    >  安装程序基础结构将写出已运行操作的所有适当参数，但不包括密码等敏感信息。 /IAcceptSQLServerLicenseTerms 参数也不写出到配置文件，它要求修改配置文件或在命令提示符下提供一个值。 有关详细信息，请参阅 [从命令提示符安装 SQL Server](./install-sql-server-from-the-command-prompt.md)。 另外，对于通常不通过命令提示符提供值的布尔参数，值将包括在内。 
  
## <a name="using-the-configuration-file-to-install-ssnoversion"></a>使用配置文件安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

只能在命令行安装中使用配置文件。 
  
> [!NOTE]  
> 如果需要对配置文件进行更改，建议您创建一个副本并对副本进行操作。 
  
### <a name="how-to-use-a-configuration-file-to-install-a-stand-alone-ssnoversion-instance"></a>如何使用配置文件安装独立的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例  
  
-   通过命令提示符运行安装，然后使用 *ConfigurationFile* 参数提供 ConfigurationFile.ini 文件。 
  
### <a name="how-to-use-a-configuration-file-to-prepare-and-complete-an-image-of-a-stand-alone-ssnoversion-instance-sysprep"></a>如何使用配置文件准备和完成独立 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的映像 (SysPrep)  
  
1. 准备一个或多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例并在同一计算机上配置它们。 
  
    - 从安装中心的“高级”页运行“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的独立实例的映像准备”，并捕获准备映像配置文件。 
  
    - 将同一个准备映像配置文件用作准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的多个实例的模板。 
  
    - 从安装中心的“高级”页运行“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的已准备独立实例的映像完成”，以便在计算机上配置准备的实例。 
  
2. 使用 Windows SysPrep 工具准备操作系统的映像，包括未配置的、已准备的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 
  
    -   从安装中心的“高级”页运行“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的独立实例的映像准备”，并捕获准备映像配置文件。 
  
    -   从安装中心的“高级”页运行“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的已准备独立实例的映像完成”，但在捕获完全的配置文件之后，在“已准备好完成”页上取消它。 
  
    -   可以将完全的映像配置文件随 Windows 映像一起存储，以便自动执行已准备实例的配置。 
  
### <a name="how-to-install-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>如何使用配置文件安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集  
  
1. “集成安装”选项（在一个节点上创建单节点故障转移群集并在其他节点上运行 AddNode）：  
  
    -   运行“安装故障转移群集”选项，并捕获列出所有安装设置的配置文件。 
  
    -   通过提供 *ConfigurationFile* 配置文件参数运行命令行故障转移群集安装。 
  
    -   在要添加的其他节点上，运行 AddNode 以捕获适用于现有故障转移群集的 ConfigurationFile.ini 文件。 
  
    -   通过使用 ConfigurationFile 参数提供相同的配置文件，在将要加入故障转移群集的所有其他节点上运行命令行 AddNode。 
  
2. 高级安装选项（在所有故障转移群集节点上准备故障转移群集，接着在准备好所有节点后，在拥有共享磁盘的节点上运行完成）：  
  
    -   在其中一个节点上运行 **Prepare** ，然后捕获 ConfigurationFile.ini 文件。 
  
    -   在将为故障转移群集准备的所有节点上，为安装程序提供相同的 ConfigurationFile.ini 文件。 
  
    -   准备好所有节点后，在拥有共享磁盘的节点上运行完成故障转移群集操作，然后捕获 ConfigurationFile.ini 文件。 
  
    -   接着，您可以提供此 ConfigurationFile.ini 文件以完成故障转移群集。 
  
### <a name="how-to-add-or-remove-a-node-to-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>如何使用配置文件在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集中添加或删除节点  
  
-   如果您有以前用来在故障转移群集中添加或删除节点的配置文件，可以重复使用这个文件来添加或删除其他节点。 
  
### <a name="how-to-upgrade-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>如何使用配置文件升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集  
  
1. 在被动节点上运行升级，然后捕获 ConfigurationFile.ini 文件。 您可以通过执行真正的升级，或者可以通过在升级过程结束时退出而不进行真正的升级来达到此目的。 
  
2. 在要升级的其他节点上，提供 ConfigurationFile.ini 文件以完成升级过程。 
  
## <a name="sample-syntax"></a>示例语法  
 下面提供了有关如何使用配置文件的一些示例：  
  
-   在命令提示符处指定配置文件：  
  
```  
Setup.exe /ConfigurationFile=MyConfigurationFile.INI  
```  
  
-   在命令提示符处而不是配置文件中指定密码：  
  
```  
Setup.exe /SQLSVCPASSWORD="************" /AGTSVCPASSWORD="************" /ASSVCPASSWORD="************" /ISSVCPASSWORD="************" /RSSVCPASSWORD="************" /ConfigurationFile=MyConfigurationFile.INI  
```  
  
## <a name="see-also"></a>另请参阅  
 [从命令提示符安装 SQL Server](./install-sql-server-from-the-command-prompt.md)   
 [SQL Server 故障转移群集安装](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)   
 [升级 SQL Server 故障转移群集实例](../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)  
  
