---
title: SQL Server 2017 发行说明 | Microsoft Docs
description: 本文介绍 SQL Server 2017 中的限制和问题，并提供相关信息的链接。
ms.custom: ''
ms.date: 11/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-2017
ms.openlocfilehash: cd13f9fea3ade8f0bcb92e77470ab74f40d69ae2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98167669"
---
# <a name="sql-server-2017-release-notes"></a>SQL Server 2017 发行说明
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]
本主题介绍 SQL Server 2017 的限制和问题。 如需相关信息，请参阅：
- [SQL Server 2017 的新增功能](../sql-server/what-s-new-in-sql-server-2017.md)
- [SQL Server on Linux 发行说明](../linux/sql-server-linux-release-notes.md)
- [SQL Server 2017 累积更新](https://aka.ms/sql2017cu)，了解有关最新累积更新 (CU) 版本的信息

**试用 SQL Server！**
- [![从评估中心下载](../includes/media/download2.png)](https://go.microsoft.com/fwlink/?LinkID=829477) [下载 SQL Server 2017](https://go.microsoft.com/fwlink/?LinkID=829477)
- [![创建虚拟机](../includes/media/azure-vm.png)](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm) [启动已安装有 SQL Server 2017 的虚拟机](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm)

> [!NOTE]
> SQL Server 2019 预览版现已推出。 有关详细信息，请参阅 [SQL Server 2019 中的新增功能](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true)。

## <a name="sql-server-2017---general-availability-release-october-2017"></a>SQL Server 2017 - 正式发布版（2017 年 10 月）
### <a name="database-engine"></a>数据库引擎

- **问题及其对客户的影响：** 升级后，现有的 FILESTREAM 网络共享可能不再可用。

- **解决方法：** 首先，重启计算机并检查 FILESTREAM 网络共享是否可用。 如果该共享仍不可用，请完成以下步骤：

    1. 在 SQL Server 配置管理器中，右键单击 SQL Server 实例，然后单击“属性”  。 
    2. 在“FILESTREAM”选项卡中，清除“针对文件 I/O 流访问启用 FILESTREAM”，然后单击“应用”    。
    3. 再次对原始共享名称选中“针对文件 I/O 流访问启用 FILESTREAM”  ，然后单击“应用”  。

### <a name="master-data-services-mds"></a>Master Data Services (MDS)
- **问题及其对客户的影响：** 在用户权限页上，当向实体树视图中的根级别授予权限时，看到以下错误：`"The model permission cannot be saved. The object guid is not valid"`

- **解决方法：** 
  - 向树视图中的子节点而非根级别授予权限。

### <a name="analysis-services"></a>Analysis Services
- **问题及其对客户的影响：** 对于 1400 兼容性级别的表格模型，适用于以下源的数据连接器尚不可用。
  - Amazon Redshift
  - IBM Netezza
  - Impala
- **解决方法：** 无。   

- **问题及其对客户的影响：** 具有透视的 1400 兼容性级别的直接查询模型可能在查询或发现元数据时失败。
- **解决方法：** 删除透视并重新部署。

### <a name="tools"></a>工具
- **问题及其对客户的影响：** 运行 DReplay  失败，并显示以下消息：“DReplay 出错，发生了意外错误!”。
- **解决方法：** 无。

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc2---august-2017"></a>SQL Server 2017 候选发布（RC2 - 2017 年 8 月）
对于 Windows 上的 SQL Server，没有与此版本相关的发行说明。 请参阅 [Linux 版 SQL Server 的发行说明](../linux/sql-server-linux-release-notes.md)。


![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc1---july-2017"></a>SQL Server 2017 候选发布（RC1 - 2017 年 7 月）
### <a name="sql-server-integration-services-ssis-rc1---july-2017"></a>SQL Server Integration Services (SSIS)（RC1 - 2017 年 7 月）
- **问题及其对客户的影响：** 存储过程 **[catalog].[create_execution]** 的参数 *runincluster* 重命名为 *runinscaleout*，以保持一致性和可读性。
- **解决方法：** 如果现有脚本在 Scale Out 中运行包，必须将参数名称从 runincluster 更改为 runinscaleout，以使脚本在 RC1 中正常运行   。

- **问题及其对客户的影响：** SQL Server Management Studio (SSMS) 17.1 及更早版本无法在 RC1 的 Scale Out 中触发包执行。 错误消息为：“\@runincluster 不是过程 create_execution 的参数。” SSMS 的下一个版本，即版本 17.2 中修复了此问题。 SSMS 的版本 17.2 及更高版本支持 Scale Out 中的新参数名称和包执行。 
- **解决方法：** 在 SSMS 版本 17.2 发布前：
  1. 使用现有版本 SSMS 生成包执行脚本。
  2. 将脚本中 runincluster  参数的名称更改为 runinscaleout  。
  3. 运行该脚本。

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-21-may--2017"></a>SQL Server 2017 CTP 2.1（2017 年 5 月）
### <a name="documentation-ctp-21"></a>文档 (CTP 2.1)
- **问题及其对客户的影响：** [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] 的文档受到限制，且其内容包含在 [!INCLUDE[ssSQL15_md](../includes/sssql16-md.md)] 文档集中。  文章中的 [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] 专属内容使用“适用于”  进行标注。 
- **问题及其对客户的影响：** [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] 没有可用的脱机内容。

### <a name="sql-server-reporting-services-ctp-21"></a>SQL Server Reporting Services (CTP 2.1)

- **问题及其对客户的影响：** 如果同一台计算机上同时安装了 SQL Server Reporting Services 和 Power BI 报表服务器，并卸载了其中之一，那么将无法通过报表服务器配置管理器连接到剩余的报表服务器。
- **解决方法**：若要解决此问题，必须在卸载其中一个服务器后执行以下操作。

    1. 在管理员模式下启动命令提示符。
    2. 转到安装其余报表服务器的目录。

        Power BI 报表服务器的默认位置：  C:\Program Files\Microsoft Power BI Report Server

        SQL Server Reporting Services 的默认位置：  C:\Program Files\Microsoft SQL Server Reporting Services

    3. 然后，转到下一个文件夹（SSRS  或 PBIRS  ，具体视剩余报表服务器而定）
    4. 转到 WMI 文件夹。
    5. 运行以下命令：

        ```console
        regsvr32 /i ReportingServicesWMIProvider.dll
        ```

        如果看到以下错误，请予以忽略。

        ```
        The module "ReportingServicesWMIProvider.dll" was loaded but the entry-point DLLInstall was not found. Make sure that "ReportingServicesWMIProvider.dll" is a valid DLL or OCX file and then try again.
        ```

### <a name="tsqllanguageservicemsi-ctp-21"></a>TSqlLanguageService.msi (CTP 2.1)

- **问题及其对客户的影响：** 在已安装 2016 版 TSqlLanguageService.msi 的计算机上安装（通过 SQL 安装程序或作为独立可再发行组件安装）后，系统将删除 v13.* (SQL 2016) 版 Microsoft.SqlServer.Management.SqlParser.dll 和 Microsoft.SqlServer.Management.SystemMetadataProvider.dll    。 在 2016 版程序集上具有依赖项的任何应用程序停止运行并生成类似以下内容的错误：错误:无法加载文件或程序集“Microsoft.SqlServer.Management.SqlParser, Version=13.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91”或其任何依赖项。系统找不到指定文件。 

   另外，尝试重新安装 2016 版 TSqlLanguageService.msi 失败，并显示以下消息：安装 Microsoft SQL Server 2016 T-SQL 语言服务失败，因为计算机上已存在更高版本  。

- **解决方法**：要解决此问题并修复依赖于 v13 版程序集的应用程序，请执行下列步骤：

   1. 转到“添加/删除程序” 
   2. 找到 Microsoft SQL Server 2019 T-SQL 语言服务 CTP2.1  ，右键单击它，然后选择“卸载”  。
   3. 删除组件后，修复已损坏的应用程序，或重新安装适当版本的 TSqlLanguageService.MSI  。

   此解决方法将删除这些程序集的 v14 版本，因此任何依赖 v14 版本的应用程序都无法再正常运行。 如果需要这些程序集，则需单独安装，无需并行安装任何 2016 版。

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-20-april--2017"></a>SQL Server 2017 CTP 2.0（2017 年 4 月）
### <a name="documentation-ctp-20"></a>文档 (CTP 2.0)
- **问题及其对客户的影响：** [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] 的文档受到限制，且其内容包含在 [!INCLUDE[ssSQL15_md](../includes/sssql16-md.md)] 文档集中。  文章中的 [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] 专属内容使用“适用于”  进行标注。 
- **问题及其对客户的影响：** [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] 没有可用的脱机内容。

### <a name="always-on-availability-groups"></a>AlwaysOn 可用性组

- **问题及其对客户的影响：** 如果 SQL Server 主版本低于托管主要副本的实例，托管可用性组次要副本的 SQL Server 实例将崩溃。 如果从托管可用性组且受支持的任意 SQL Server 版本升级到 [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] CTP 2.0，则这些升级将受到影响。 在下列情况下会发生此问题。 

> 1. 用户根据[最佳做法](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md)升级托管次要副本的 SQL Server 实例。
> 2. 升级后，便会发生故障转移，并且在升级完可用性组中的所有次要副本之前，新升级的次要副本就会变成主要副本。 旧的主要副本现在是次要副本，即版本低于主要副本。
> 3. 可用性组处于不受支持的配置中，任何剩余的辅助副本可能容易崩溃。 

- **解决方法**：连接到托管新的主要副本的 SQL Server 实例并从配置中删除有问题的次要副本。

   ```sql
   ALTER AVAILABILITY GROUP agName REMOVE REPLICA ON NODE instanceName;
   ```

   托管次要副本的 SQL Server 实例即可恢复。

## <a name="more-information"></a>详细信息
- [SQL Server Reporting Services 发行说明](../reporting-services/release-notes-reporting-services.md)的限制和问题。
- [机器学习服务的已知问题](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md)
- [SQL Server 更新中心 - 链接和有关所有受支持版本的信息](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png)