---
title: 使用 SysPrep 安装 SQL Server | Microsoft Docs
description: 本文介绍如何通过在 SQL Server 安装中使用 SysPrep 来准备和完成映像。
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 11f4ed8a-aaa9-417b-bdd5-204f551c6bb6
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: bcf581a18af1f8f3024cd94d712359396f64bbfd
ms.sourcegitcommit: 3ec49252e82590de0fe559a8574606ae213f6f3b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/07/2021
ms.locfileid: "97975424"
---
# <a name="install-sql-server-with-sysprep"></a>使用 SysPrep 安装 SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep 相关的安装操作可以通过安装中心来访问。 “安装中心”的“高级”页具有两个选项 -“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的独立实例的映像准备”和“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的已准备独立实例的映像完成”。 [准备](#prepare) 和 [完成](#complete) 部分将详细说明安装过程。 有关详细信息，请参阅 [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)。 
  
还可以使用命令提示符或配置文件准备和完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例。 有关详细信息，请参阅：  
  
- [从命令提示符安装 SQL Server](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)  
  
- [使用配置文件安装 SQL Server](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)  
  
## <a name="prerequisites"></a>先决条件  
安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之前，请查阅[计划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)中的文章。 
  
有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本及硬件和软件要求的详细信息，请参阅[安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。 
    
##  <a name="ssnoversion-sysprep-cluster-support"></a><a name="sysprep"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep 群集支持  
 从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]开始，SysPrep 支持从命令行安装群集 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 有关详细信息，请参阅 [什么是 Sysprep？](https://msdn.microsoft.com/library/cc721940\(v=WS.10\).aspx)。 
  
### <a name="to-prepare-a-ssnoversion-failover-cluster-unattended"></a>准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集（无人参与）  
  
1. 准备映像（如 [使用 SysPrep 安装 SQL Server 的注意事项](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)中所述）并通过 SysPrep 一般化捕获 Windows 映像。 以下示例准备该映像：  
  
    ```  
    Setup.exe /q /ACTION=PrepareImage l /FEATURES=SQLEngine /InstanceID =<MYINST> /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     然后运行 Windows SysPrep 一般化。 
  
2. 通过运行 Windows SysPrep 特殊化来部署该映像。 
  
3. 创建 Windows 故障转移群集。 
  
4. 使用 **/ACTION=PrepareFailoverCluster** 所有节点运行 setup.exe。 例如：  
  
    ```  
    setup.exe /q /ACTION=PrepareFailoverCluster /InstanceName=<InstanceName> /Features=SQLEngine  /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx"  /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
### <a name="complete-a-ssnoversion-failover-cluster-unattended"></a>完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集（无人参与）  
  
1. 使用在拥有可用存储组的节点上的 **/ACTION=CompleteFailoverCluster** 运行 setup.exe：  
  
    ```  
    setup.exe /q /ACTION=CompleteFailoverCluster /InstanceName=<InstanceName>  /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'>:" /FAILOVERCLUSTERNETWORKNAME="<Insert FOI Network Name>" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;Cluster Network;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="MSSQLSERVER" /INSTALLSQLDATADIR="<Drive>:\<Path>\MSSQLSERVER" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSYSADMINACCOUNTS="<DomainName\UserName>"  
    ```  
  
### <a name="adding-a-node-to-an-existing-ssnoversion-failover-cluster-unattended"></a>向现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集添加节点（无人参与）  
  
1. 通过运行 Windows SysPrep 特殊化来部署该映像。 
  
2. 加入 Windows 故障转移群集。 
  
3. 使用在所有节点上的 **/ACTION=AddNode** 运行 setup.exe：  
  
    ```  
    setup.exe /q /ACTION=AddNode /InstanceName=<InstanceName> /Features=SQLEngine  /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx"  /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
##  <a name="prepare-image"></a><a name="prepare"></a> 准备映像  
  
### <a name="prepare-a-stand-alone-instance-of-ssnoversion"></a>准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的独立实例。 
  
1. 插入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装介质， 然后双击根文件夹中的 Setup.exe。 若要从网络共享进行安装，请找到共享中的根文件夹，然后双击 Setup.exe。 
  
2. 安装向导将运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装中心。 若要准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，请单击“高级”页上的“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的独立实例的映像准备”。 
  
3. 系统配置检查器将在您的计算机上运行发现操作。 若要继续，请单击 **“确定”** 。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。 
  
4. 在“产品更新”页中，将显示最近提供的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品更新。 如果你不想包括更新，则取消选中“包括 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品更新”复选框。 如果未发现任何产品更新， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将不会显示该页并且自动前进到 **“安装安装程序文件”** 页。 
  
5. 在“安装安装程序文件”页上，安装程序将提供下载、提取和安装这些安装程序文件的进度。 如果找到了针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的更新，并且指定了包括该更新，则也将安装该更新。 
  
6. 系统配置检查器将在安装继续之前验证计算机的系统状态。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。 
  
7. 在“准备映像类型”页中，选择“准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新实例”。 
  
     仅当计算机上存在未配置的已准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，才显示“准备映像类型”页。 您可以选择准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新实例，或者将 sys prep 支持的功能添加到计算机上 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的现有已准备实例。 有关如何向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的已准备实例添加功能的详细信息，请参阅 [向已准备实例添加功能](#AddFeatures)。 
  
8. 在 **“许可条款”** 页上阅读许可协议，然后选中相应的复选框以接受许可条款和条件。 为了帮助改进 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，您还可以启用功能使用情况选项并将报告发送给 [!INCLUDE[msCoName](../../includes/msconame-md.md)]。 
  
9. 在 **“功能选择”** 页上，选择要安装的组件：  
  
    |安装|组件|  
    |-|-|  
    |[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] SysPrep|[!INCLUDE[ssDE](../../includes/ssde-md.md)]<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Replication<br /><br /> 全文功能<br /><br /> 数据库引擎服务<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式下的<br /><br /> [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]<br /><br /> 可再发行的功能<br /><br /> 共享功能|  
  
     突出显示功能名称时，右侧窗格中会显示每个组件组的说明。 您可以选中任意一些复选框。 有关详细信息，请参阅 [SQL Server 2017 各个版本支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)和 [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md)
  
     在右侧窗格中显示所选功能的必备组件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将在本过程后面所述的安装步骤中安装尚未安装的必备组件。 
  
10. 在 **“准备映像规则”** 页上，系统配置检查器将在安装继续之前验证计算机的系统状态。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。 
  
11. 在“实例配置”页上，指定实例的实例 ID。 单击“下一步”以继续。 
  
     **实例 ID** - 实例 ID 用于标识 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的安装目录和注册表项。 默认实例和命名实例都是如此。 如果已准备实例在“完成”步骤中作为默认实例完成，则该实例名称将被覆盖为 MSSQLSERVER。 实例 ID 仍保持指定的 ID。 
  
     **实例根目录** - 默认情况下，实例根目录为 [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]。 若要指定一个非默认的根目录，请使用所提供的字段，或单击“浏览”以找到一个安装文件夹。 在“完成”步骤的配置过程中，将使用准备步骤中指定的目录。 
  
     所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Pack 和升级都将应用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的每个组件。 
  
     **已安装的实例** — 该网格显示安装程序正在其中运行的计算机上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 
  
12. **“磁盘空间要求”** 页计算指定的功能所需的磁盘空间， 然后将所需空间与可用磁盘空间进行比较。 
  
13. 系统配置检查器将运行准备映像规则来针对您指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能验证您的计算机配置。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。 
  
14. **“已可以准备映像”** 页显示您在安装过程中指定的安装选项的树视图。 在此页上，安装程序指示是启用还是禁用产品更新功能以及最终的更新版本。 若要继续，请单击 **“准备”** 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将首先安装所选功能的必备组件，然后安装所选功能。 
  
15. 在安装过程中， **“准备映像进度”** 页会提供相应的状态，因此您可以在安装过程中监视安装进度。 
  
16. 安装完成后， **“完成”** 页会提供指向安装摘要日志文件以及其他重要说明的链接。 若要完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程，请单击 **“关闭”** 。 
  
17. 如果安装程序指示您重新启动计算机，请立即重新启动。 安装完成后，请务必阅读来自安装向导的消息。 有关详细信息，请参阅 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。 
  
18. 这将完成准备步骤。 您可以完成该映像或者按 [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)中所述部署准备的映像。 
  
##  <a name="complete-image"></a><a name="complete"></a> 完成映像  
  
### <a name="complete-a-prepared-instance-of-ssnoversion"></a>完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. 如果在您的计算机的映像中包括了一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的已准备实例，则在“开始”菜单中会看到一个相应的快捷方式。 你也可以启动安装中心，然后在“高级”页上单击“已准备独立实例的映像完成”。 
  
2. 系统配置检查器将在您的计算机上运行发现操作。 若要继续，请单击 **“确定”** 。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。 
  
3. 在 **“安装程序支持文件”** 页，单击 **“安装”** 以安装安装程序支持文件。 
  
4. 系统配置检查器将在安装继续之前验证计算机的系统状态。 检查完成后，请单击 **“下一步”** 继续。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。 
  
5. 在 **“产品密钥”** 页上，选择某一选项按钮，该按钮指示您是安装免费版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，还是安装具有 PID 密钥的产品的生产版本。 有关详细信息，请参阅 [SQL Server 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)。 如果您在安装 Evaluation 版，则 180 天的试用期将从您完成此步骤时开始。 
  
6. 在 **“许可条款”** 页上阅读许可协议，然后选中相应的复选框以接受许可条款和条件。 为了帮助改进 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，您还可以启用功能使用情况选项并将报告发送给 [!INCLUDE[msCoName](../../includes/msconame-md.md)]。 
  
7. 在 **“选择已准备实例”** 页上，从下拉框中选择您要完成的已准备实例。 从“实例 ID”列表中选择未配置的实例。 
  
     **已安装的实例：** 该网格显示所有实例，包括此计算机上的任何已准备实例。 
  
8. 在 **“查看功能”** 页上，您将看到在安装的准备步骤中包括的所选功能和组件。 如果您要向未在已准备实例中包括的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例添加更多功能，则必须首先完成此步骤以完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，然后从 **“安装中心”** 的 **“添加功能”** 上添加功能。 
  
    > [!NOTE]  
    >  您可以添加可用于您正在安装的产品版本的功能。 有关详细信息，请参阅 [SQL Server 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)。  
  
9. 在“实例配置”页上，指定已准备实例的实例名称。 这是您完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的配置后的实例的名称。 单击“下一步”以继续。 
  
     **实例 ID** - 实例 ID 用于标识 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的安装目录和注册表项。 默认实例和命名实例都是如此。 如果已准备实例在“完成”步骤中作为默认实例完成，则该实例名称将被覆盖为 MSSQLSERVER。 实例 ID 仍保持准备阶段中指定的 ID。 
  
     **实例根目录** - 将使用在准备步骤中指定的目录，并且不能在此步骤中修改。 
  
     所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Pack 和升级都将应用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的每个组件。 
  
     **已安装的实例** - 该网格显示安装程序正在其中运行的计算机上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 
  
10. 本文中的其余工作流取决于在准备步骤中已选择的功能。 您可能不会看到所有的页面，具体取决于进行的选择。 
  
11. 在“服务器配置 - 服务帐户”页上指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的登录帐户。 此页上配置的实际服务取决于您选择安装的功能。 
  
     您可以为所有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务分配相同的登录帐户，也可以单独配置各个服务帐户。 您还可以指定是自动启动、手动启动还是禁用服务。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议您逐个配置服务帐户，以便为每项服务提供最低权限，其中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务将被授予完成其任务所必须具备的最低权限。 有关详细信息，请参阅 [服务器配置 - 服务帐户](./install-sql-server.md) 和 [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。 
  
     若要为此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例中的所有服务帐户指定同一个登录帐户，请在该页底部的字段中提供凭据。 
  
     **安全说明** [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
     为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务指定登录信息后，请单击 **“下一步”** 。 
  
12. 使用“服务器配置 - 排序规则”选项卡为 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 指定非默认的排序规则。 有关详细信息，请参阅[服务器配置 - 排序规则](./install-sql-server.md)。 
  
13. 使用“ [!INCLUDE[ssDE](../../includes/ssde-md.md)] 配置 - 帐户设置”页指定以下各项：  
  
    - 安全模式 - 为您的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例选择 Windows 身份验证或混合模式身份验证。 如果选择“混合模式身份验证”，则必须为内置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统管理员帐户提供一个强密码。 
  
         在设备与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]成功建立连接之后，用于 Windows 身份验证和混合模式身份验证的安全机制是相同的。 有关详细信息，请参阅 [数据库引擎配置 - 服务器配置](./install-sql-server.md)。 
  
    - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员 - 必须为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例至少指定一个系统管理员。 若要添加用以运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的帐户，请单击 **“添加当前用户”** 。 若要向系统管理员列表中添加帐户或从中删除帐户，请单击 **“添加”** 或 **“删除”** ，然后编辑将拥有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的管理员特权的用户、组或计算机的列表。 有关详细信息，请参阅 [数据库引擎配置 - 服务器配置](./install-sql-server.md)。 
  
     完成对该列表的编辑后，请单击 **“确定”** 。 验证配置对话框中的管理员列表。 完成此列表后，请单击 **“下一步”** 。 
  
14. 使用“ [!INCLUDE[ssDE](../../includes/ssde-md.md)] 配置 - 数据目录”页指定非默认的安装目录。 若要安装到默认目录，请单击 **“下一步”** 。 
  
    > [!IMPORTANT]  
    >  如果指定非默认的安装目录，请确保安装文件夹对于此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例是唯一的。 此对话框中的任何目录都不应与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的目录共享。 
  
     有关详细信息，请参阅 [数据库引擎配置 - 数据目录](./install-sql-server.md)。 
  
15. 使用“ [!INCLUDE[ssDE](../../includes/ssde-md.md)] 配置 - FILESTREAM”页对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例启用 FILESTREAM。 有关详细信息，请参阅 [数据库引擎配置 - 文件流](./install-sql-server.md)。 
  
16. 使用“ [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置”页指定要创建的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装类型。 有关 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置模式的详细信息，请参阅 [Reporting Services 配置选项 (SSRS)](./install-sql-server.md)。 
  
17. 在 **“错误报告”** 页上，指定要发送到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 以帮助改进 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的信息。 默认情况下，将启用用于错误报告的选项。 
  
18. 在 **“完整映像规则”** 页上，系统配置检查器将运行完整的映像规则，以便使用您所指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置来验证您的计算机配置。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。 
  
19. **“已准备好完成映像”** 页显示您在安装过程中指定的安装选项的树视图。 若要继续，请单击 **“安装”** 。 
  
20. 在安装过程中， **“完成映像进度”** 页会提供相应的状态，因此您可以在安装过程中监视安装进度。 
  
21. 安装完成后， **“完成”** 页会提供指向安装摘要日志文件以及其他重要说明的链接。 若要完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程，请单击 **“关闭”** 。 
  
22. 如果安装程序指示您重新启动计算机，请立即重新启动。 安装完成后，请务必阅读来自安装向导的消息。 有关详细信息，请参阅 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。 
  
23. 此步骤完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的已准备实例的配置并且您已完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的安装。 
  
##  <a name="add-features-to-a-prepared-instance"></a><a name="AddFeatures"></a> Add Features to a Prepared Instance  
  
### <a name="add-features-to-a-prepared-instance-of-ssnoversion"></a>向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. 插入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装介质， 然后双击根文件夹中的 Setup.exe。 若要从网络共享进行安装，请找到共享中的根文件夹，然后双击 Setup.exe。 
  
2. 安装向导将运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装中心。 若要向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的已准备实例添加功能，请单击“高级”页上的“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的独立实例的映像准备”。 
  
3. 系统配置检查器将在您的计算机上运行发现操作。 若要继续，请单击 **“确定”** 。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。 
  
4. 在“安装程序支持文件”页，单击 **“安装”** 以安装安装程序支持文件。 
  
5. 在“准备映像类型”页上，选择“向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的现有已准备实例中添加功能”选项。 从可用的已准备实例的下拉列表中，选择要将功能添加到的特定的已准备实例。 
  
6. 在 **“功能选择”** 页上，指定要添加到指定的已准备实例的功能。 
  
     在右侧窗格中显示所选功能的必备组件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将在本过程后面所述的安装步骤中安装尚未安装的必备组件。 
  
7. 在 **“准备映像规则”** 页上，系统配置检查器将在安装继续之前验证计算机的系统状态。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。 
  
8. “磁盘空间要求”页将计算指定的功能所需的磁盘空间， 然后将所需空间与可用磁盘空间进行比较。 
  
9. 在 **“准备映像规则”** 页上，系统配置检查器将运行准备映像规则，以便使用您所指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能验证您的计算机配置。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。 
  
10. **“已可以准备映像”** 页显示您在安装过程中指定的安装选项的树视图。 若要继续，请单击 **“安装”** 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将首先安装所选功能的必备组件，然后安装所选功能。 
  
11. 在安装过程中， **“准备映像进度”** 页会提供相应的状态，因此您可以在安装过程中监视安装进度。 
  
12. 安装完成后， **“完成”** 页会提供指向安装摘要日志文件以及其他重要说明的链接。 若要完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程，请单击 **“关闭”** 。 
  
13. 如果安装程序指示您重新启动计算机，请立即重新启动。 安装完成后，请务必阅读来自安装向导的消息。 有关详细信息，请参阅 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。 
  
##  <a name="remove-features-from-a-prepare-instance"></a><a name="RemoveFeatures"></a> 从已准备实例删除功能  
  
### <a name="removing-features-from-a-prepared-instance-of-ssnoversion"></a>从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. 若要开始卸载过程，请从 **“开始”** 菜单单击 **“控制面板”** ，然后双击 **“程序和功能”** 。 
  
2. 双击要卸载的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件，再单击 **“删除”** 。 
  
3. 将运行安装程序支持规则以验证您的计算机配置。 单击 **“确定”** 继续。 
  
4. 在 **“选择实例”** 页中，选择要修改的已准备实例。 已准备实例的名称将显示为“未配置 PreparedInstanceID”，其中 PreparedInstanceID 是您选择的实例。 
  
5. 在 **“选择功能”** 页上，指定要从指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中删除的功能。 单击“下一步”以继续。 
  
6. 将运行删除规则以验证是否可以成功完成删除操作。 
  
7. 在 **“准备删除”** 页上查看要卸载的组件和功能的列表。 
  
8. **“删除进度”** 页将显示该操作的状态。 
  
9. 在 **“完成”** 页上可以查看操作的完成状态。 单击 **“关闭”** 以退出安装向导。 
  
##  <a name="uninstalling-a-prepared-instance"></a><a name="Uninstall"></a> 卸载已准备实例  
  
### <a name="uninstall-a-prepared-instance-of-ssnoversion"></a>卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. 若要开始卸载过程，请从 **“开始”** 菜单单击 **“控制面板”** ，然后双击 **“程序和功能”** 。 
  
2. 双击要卸载的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件，再单击 **“删除”** 。 
  
3. 将运行安装程序支持规则以验证您的计算机配置。 单击 **“确定”** 继续。 
  
4. 在 **“选择实例”** 页中，选择要修改的已准备实例。 已准备实例的名称将显示为“未配置 PreparedInstanceID”，其中 PreparedInstanceID 是您选择的实例。 
  
5. 在 **“选择功能”** 页上，指定要从指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中删除的功能。 单击“下一步”以继续。 
  
6. 在 **“删除规则”** 页上，安装程序将运行规则以验证是否可以成功完成操作。 
  
7. 在 **“准备删除”** 页上查看要卸载的组件和功能的列表。 
  
8. **“删除进度”** 页将显示该操作的状态。 
  
9. 在 **“完成”** 页上可以查看操作的完成状态。 单击 **“关闭”** 以退出安装向导。 
  
10. 重复步骤 1 到 9，直到删除所有 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 组件。 
  
##  <a name="modifying-or-uninstalling-a-completed-instance-of-ssnoversion"></a><a name="bk_Modifying_Uninstalling"></a> 修改或卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的已完成实例。 
 添加或删除功能或者卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的已完成实例的过程类似于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的已安装实例的过程。 有关详细信息，请参阅以下文章：  
  
- [向 SQL Server 实例添加功能（安装程序）](./add-features-to-an-instance-of-sql-server-setup.md)  
  
- [卸载现有 SQL Server 实例（安装程序）](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)  
  
## <a name="see-also"></a>另请参阅  
 [什么是 Windows SysPrep](/previous-versions/windows/it-pro/windows-vista/cc721940(v=ws.10))   
 [Windows SysPrep 工作原理](/previous-versions/windows/it-pro/windows-vista/cc766514(v=ws.10))  
  
