---
title: 安装向导帮助 | Microsoft Docs
description: 指定是使用 SQL Server 安装向导中的实例配置创建 SQL Server 的默认实例还是其命名实例。
ms.custom: ''
ms.date: 08/16/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
f1_keywords:
- instance configuration, Setup
helpviewer_keywords:
- Instance Name page [SQL Server Installation Wizard]
- SQL Server Installation Wizard, Instance Name page
ms.assetid: 5bf822fc-6dec-4806-a153-e200af28e9a5
author: cawrites
ms.author: chadam
robots: noindex,nofollow
ms.openlocfilehash: 8a136b1158f31cf6369bb29f404f4f6f58d6901c
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172409"
---
# <a name="installation-wizard-help"></a>安装向导帮助

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本文介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导中的一些配置页。

## <a name="instance-configuration-page"></a>“实例配置”页

使用 **安装向导的** “实例配置” [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 页面可指定是创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的默认实例还是其命名实例。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例尚未安装，系统就会创建默认实例，除非你指定命名实例。  
  
每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例都由一组具有排列规则及其他选项特定设置的非重复的服务组成。 目录结构、注册表结构和服务名称全都反映在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中创建的实例名称和具体实例 ID。  
  
实例是默认实例或命名实例。 默认实例名为 MSSQLSERVER。 使用默认名称，客户端无需指定实例名称，即可进行连接。 命名实例在安装过程中由用户决定。 您可以将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 作为命名实例安装，无需先安装默认实例。 一次只能有一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装是默认实例，无论版本如何。  
  
> [!NOTE]  
> 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep，您可以在 **“实例配置”** 页上完成已准备实例时指定实例名称。 如果计算机上现无 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 默认实例，可以将要完成的已准备实例配置为默认实例。  
  
### <a name="multiple-instances"></a>多个实例
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持在单个服务器或处理器上存在多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，但只能有一个实例为默认实例。 所有其他实例必须为命名实例。 一台计算机可同时运行多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，每个实例都独立于其他实例运行。  
  
 有关详细信息，请参阅 [SQL Server 最大容量规范](../maximum-capacity-specifications-for-sql-server.md)。  
  
### <a name="options"></a>选项

**仅故障转移群集实例**：指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集网络名称。 此名称可用来在网络上标识故障转移群集实例。  
  
**决定使用默认实例还是命名实例**：决定是安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 默认实例还是命名实例时，请注意以下几点：  
  
* 如果计划在数据库服务器上安装单个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，则该实例应为默认实例。  
  
* 如果您打算在同一台计算机上安装多个实例，请使用命名实例。 一台服务器只能承载一个默认实例。  
* 任何安装 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的应用程序都应将其作为命名实例安装。 如果同一台计算机中安装有多个应用程序，这种做法可以最大限度地减少冲突。
  
**默认实例**：选择该选项可安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的默认实例。 一台计算机只能托管一个默认实例；所有其他实例必须是命名实例。 但是，如果您已经安装了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的默认实例，则可以向同一台计算机添加 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的默认实例。  
  
**命名实例**：选择该选项可创建新的命名实例。 命名 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，请注意以下几点：  
  
* 实例名称不区分大小写。  
  
* 实例名称不得以下划线 (_) 开头或结尾。  
  
* 实例名称不得包含“Default”一词或其他保留关键字。 如果在实例名称中使用了保留关键字，就会看到安装程序错误。 有关详细信息，请参阅[保留关键字 &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md)。  
  
* 如果为实例名称指定 MSSQLSERVER，创建的是默认实例。  
  
* [!INCLUDE[ssGeminiLong](../../includes/ssgeminilong-md.md)] 安装始终安装为“[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]”的命名实例。 不得为此功能角色指定其他实例名称。  
  
* 实例名限制为 16 个字符。  
  
* 实例名中的第一个字符必须是字母。 可接受的字母由 Unicode 标准 2.0 定义。 这些字母包括拉丁字符 a-z、A-Z 和其他语言中的字母字符。  
  
* 后续字符可以是 Unicode 标准 2.0 定义的字母、源于基本拉丁语或其他国家/地区书写符号的十进制数字、美元符号 ($) 或者下划线 (_)。  
  
* 禁止在实例名称中使用嵌入空格或其他特殊字符。 也禁止使用反斜杠 (\\)、逗号 (,)、冒号 (:)、分号 (;)、单引号 (')、& 号 (&)、连字号 (-) 和 at 符号 (@)。  
  
  只有在当前 Windows 代码页中有效的字符才能用在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例名称中。 如果使用不受支持的 Unicode 字符，就会看到安装程序错误。  
  
**检测到的实例和功能**：查看正在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的计算机中安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例和组件的列表。  
  
**实例 ID**：默认情况下，实例名称用作实例 ID。 此 ID 用于标识 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的安装目录和注册表项。 对于默认实例和命名实例，行为是相同的。 对于默认实例，实例名称和实例 ID 为 MSSQLSERVER。 若要使用非默认实例 ID，请在“实例 ID”字段中指定它。  
  
> [!IMPORTANT]  
>  如果使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep，“实例配置”页上显示的实例 ID 是，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep 过程的准备映像步骤中指定的实例 ID。 无法在完成映像步骤中指定其他实例 ID。

> [!NOTE]  
>  不支持以下划线 (_) 开头或包含数字符号 (#) 或美元符号 ($) 的实例 ID。  
  
若要详细了解目录、文件位置和实例 ID 命名，请参阅 [SQL Server 默认实例和命名实例的文件位置](file-locations-for-default-and-named-instances-of-sql-server.md)。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的给定实例的所有组件作为一个单元进行管理。 所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Pack 和升级都适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的每个组件。  
  
共用同一实例名称的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件都必须满足以下条件：  
  
* 版本相同
* 版本类别相同
* 语言设置相同
* 聚集状态相同
* 操作系统相同  
  
> [!NOTE]  
> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 不是群集感知应用程序。  

## <a name="analysis-services-configuration---account-provisioning-page"></a>“Analysis Services 配置 - 帐户预配”页
  
此页可用于设置服务器节点，并向需要不受限制地访问 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的用户或服务授予管理权限。 安装程序不会自动将本地 Windows 组 BUILTIN\Administrators 添加到要安装的实例的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务器管理员角色。 如果要将本地管理员组添加到服务器管理员角色，则必须显式指定该组。  
  
若要安装 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]，请务必向负责在 [!INCLUDE[SPS2010](../../includes/sps2010-md.md)] 场中部署 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 服务器的 SharePoint 场管理员或服务管理员授予管理权限。  
  
### <a name="options"></a>选项

**服务器模式**：服务器模式指定可部署到服务器的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据库的类型。 服务器模式是在你使用安装程序期间进行确定，之后便无法再修改。 每种模式都是互斥的；也就是说，需要两个 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例，并为每个实例配置不同的模式，以同时支持典型联机分析处理 (OLAP) 和表格模型解决方案。  
  
**指定管理员**：必须为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例指定至少一个服务器管理员。 你指定的用户或组成为要安装的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例的服务器管理员角色成员。 此类成员必须拥有 Windows 域用户帐户，这些帐户与要在其上安装软件的计算机处于同一域中。  
  
> [!NOTE]  
> 用户帐户控制 (UAC) 是一项 Windows 安全功能，它要求管理操作或应用程序必须先获得管理员专门批准，然后才能运行。 由于 UAC 默认处于启用状态，因此系统会提示你允许执行需要提升的权限的特定操作。 可以将 UAC 配置为更改默认行为，也可以为特定程序自定义 UAC。 若要详细了解 UAC 和 UAC 配置，请参阅[用户帐户控制分步指南](https://go.microsoft.com/fwlink/?linkid=196350)和[用户帐户控制 (Wikipedia)](https://go.microsoft.com/fwlink/?linkid=196351)。  
  
### <a name="see-also"></a>另请参阅
  
* [配置服务帐户 &#40;Analysis Services&#41;](https://docs.microsoft.com/analysis-services/instances/configure-service-accounts-analysis-services)
* [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  

## <a name="analysis-services-configuration---data-directories-page"></a>“Analysis Services 配置 - 数据目录”页

下表中的默认目录是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中可由用户配置的目录。 对这些文件的访问权限授予给本地管理员，以及在使用安装程序期间创建并预配的 SQLServerMSASUser$\<instance> 安全组的成员。  
  
### <a name="ui-element-list"></a>UI 元素列表  
  
|说明|默认目录|建议|  
|-----------------|-----------------------|---------------------|  
|**数据根目录**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Data\ |确保通过限制权限对 \Program files\Microsoft SQL Server\ 文件夹进行保护。 在许多配置中，[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 性能取决于数据目录所在的存储区的性能。 请将此目录置于已附加到系统的性能最高存储中。 对于故障转移群集安装，应确保数据目录位于共享磁盘上。|  
|**日志文件目录**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Log\ |此目录用于 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 日志文件，其中包括 FlightRecorder 日志。 如果要延长网络流量记录器持续时间，请确保该日志目录有足够的空间。|  
|**临时目录**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Temp\ |请将临时目录置于高性能存储子系统中。|  
|**备份目录**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Backup\ |此目录用于 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 默认备份文件。 对于 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 安装，此目录也是 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 系统服务缓存 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据文件的位置。<br /><br /> 请确保适当权限已设置以防数据丢失，并确保 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的用户组拥有足够的权限，可以对备份目录执行写入操作。 不支持对备份目录使用映射驱动器。|  
  
### <a name="considerations"></a>注意事项  
  
* 在部署到 SharePoint 场后，[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例将应用程序文件、数据文件和属性存储在内容数据库和服务应用程序数据库中。  
  
* 向现有安装添加功能时，既不得更改以前安装的功能的位置，也不得为新功能指定位置。  

* 可能需要安装扫描软件（如防病毒应用程序和反间谍应用程序）以排除 SQL Server 文件夹和文件类型。 有关详细信息，请参阅以下支持文章：[运行 SQL Server 的计算机上的防病毒软件](https://support.microsoft.com/kb/309422)。
  
* 如果指定非默认安装目录，请确保安装文件夹对此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是唯一的。 不要将此对话框中的目录分与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的目录。 还请在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中安装 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 组件，以分隔目录。  
  
* 在下列情况下，无法安装程序文件和数据文件：  
  
  * 在可移动磁盘驱动器上  
  * 在使用压缩的文件系统上  
  * 在系统文件所在的目录上  
  
### <a name="see-also"></a>另请参阅

若要详细了解目录、文件位置和实例 ID 命名，请参阅 [SQL Server 默认实例和命名实例的文件位置](file-locations-for-default-and-named-instances-of-sql-server.md)。  
  
### <a name="analysis-services-configuration---data-directories-page"></a>“Analysis Services 配置 - 数据目录”页

下表中的默认目录是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中可由用户配置的目录。 对这些文件的访问权限授予给本地管理员，以及在使用安装程序期间创建并预配的 SQLServerMSASUser$\<instance> 安全组的成员。  
  
#### <a name="ui-element-list"></a>UI 元素列表
  
|说明|默认目录|建议|  
|-----------------|-----------------------|---------------------|  
|**数据根目录** |\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Data |确保通过限制权限对 \Program files\Microsoft SQL Server\ 文件夹进行保护。 在许多配置中，[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 性能取决于数据目录所在的存储区的性能。 请将此目录置于已附加到系统的性能最高存储中。 对于故障转移群集安装，应确保数据目录位于共享磁盘上。|  
|**日志文件目录**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Log |此目录用于 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 日志文件，其中包括 FlightRecorder 日志。 如果延长外部测试版记录器持续时间，请确保日志目录有足够空间。|  
|**临时目录**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Temp |请将临时目录置于高性能存储子系统中。|  
|**备份目录**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Backup |此目录用于 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 默认备份文件。 对于 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 安装，这也是 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 系统服务缓存 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据文件的位置。<br /><br /> 确保设置合适的权限以防止数据丢失，并确保 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务的用户组具有写入备份目录的足够权限。 不支持对备份目录使用映射驱动器。|  
  
#### <a name="considerations"></a>注意事项
  
* 在部署到 SharePoint 场后，[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例将应用程序文件、数据文件和属性存储在内容数据库和服务应用程序数据库中。  
  
* 向现有安装添加功能时，既不得更改以前安装的功能的位置，也不得为新功能指定位置。  

* 可能需要安装扫描软件（如防病毒应用程序和反间谍应用程序）以排除 SQL Server 文件夹和文件类型。 有关详细信息，请参阅[运行 SQL Server 的计算机上的防病毒软件](https://support.microsoft.com/kb/309422)。
  
* 如果指定非默认安装目录，请确保安装文件夹对此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是唯一的。 不要将此对话框中的目录分与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的目录。 还请在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中安装 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 组件，以分隔目录。  
  
* 在下列情况下，无法安装程序文件和数据文件：  
  
  * 在可移动磁盘驱动器上  
  * 在使用压缩的文件系统上  
  * 在系统文件所在的目录上  
  
#### <a name="see-also"></a>另请参阅

* 若要详细了解目录、文件位置和实例 ID 命名，请参阅 [SQL Server 默认实例和命名实例的文件位置](file-locations-for-default-and-named-instances-of-sql-server.md)  
* [文件服务器上的共享权限和 NTFS 权限](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/configuring-share-and-ntfs-permissions)

## <a name="database-engine-configuration---server-configuration-page"></a><a name="serverconfig"></a>“数据库引擎配置 - 服务器配置”页

此页可用于设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全模式，并将 Windows 用户或组添加为 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]管理员。  
  
### <a name="considerations-for-running-sscurrent"></a>关于运行 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的注意事项

在旧版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，BUILTIN\Administrators 组被预配为 [!INCLUDE[ssDE](../../includes/ssde-md.md)]中的登录名，本地 Administrators 组的成员可以使用自己的管理员凭据登录。 不过，使用提升的权限并不是最佳做法。

在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中，BUILTIN\Administrators 组未被预配为登录名。 请务必为每个管理用户都创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，并在安装 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 新实例的过程中将相应登录名添加到 sysadmin 固定服务器角色中。 对运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业（包括复制代理作业）的 Windows 帐户执行相同操作。  
  
### <a name="options"></a>选项

**安全模式**：为安装选择“Windows 身份验证”或“混合模式身份验证”。  
  
**Windows 主体预配**：在旧版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，Windows BUILTIN\Administrators 本地组被添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sysadmin 服务器角色中，有效地向 Windows 管理员授予对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的访问权限。 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中，BUILTIN\Administrators 组未在 sysadmin 服务器角色中进行预配。 而是改为由您在安装过程中为新安装显式设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员。  
  
> [!IMPORTANT]  
> 您在安装过程中必须为新安装显式设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员。 除非你完成这一步，否则安装程序不会允许你继续操作。
  
**指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员**：必须为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例指定至少一个 Windows 主体。 若要添加正在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的帐户，请选择“添加当前用户”按钮。 若要在系统管理员列表中添加或删除帐户，请先选择“添加”或“删除”，再编辑对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例拥有管理员权限的用户、组或计算机列表。  
  
编辑完列表后，选择“确定”，然后验证此配置对话框中的管理员列表。 如果列表是完整的，请单击“下一步”。  
  
如果选择“混合模式身份验证”，必须为内置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统管理员 (sa) 帐户提供登录凭据。  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
**Windows 身份验证模式**：当用户通过 Windows 用户帐户连接时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用操作系统中的 Windows 主体令牌来验证帐户名和密码。 Windows 身份验证是默认身份验证模式，比混合模式身份验证安全得多。 Windows 身份验证使用 Kerberos 安全协议，在验证强密码复杂性方面强制执行密码策略，并支持帐户锁定和密码到期。  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  

> [!IMPORTANT]  
> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] 切勿设置空白或弱 sa 密码。  
  
**混合模式(Windows 身份验证或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证)** ：允许用户使用 Windows 身份验证或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接。 通过 Windows 用户帐户进行连接的用户可以使用经过 Windows 验证的信任连接。  
  
如果必须选择混合模式身份验证，且需要使用 SQL 登录名来适应旧版应用程序，必须为所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户设置强密码。  
  
> [!NOTE]  
> 提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证只是为了实现后向兼容性。 [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
**输入密码**：输入并确认系统管理员 (sa) 登录名。 密码是抵御入侵者的第一道防线，因此设置强密码对于系统安全是绝对必要的。 切勿设置空白或弱 sa 密码。  
  
> [!NOTE]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 密码可包含 1 到 128 个字符，包括字母、符号和数字的任意组合。 如果选择混合模式身份验证，必须先输入强 sa 密码，然后才能继续转到安装向导的下一页。  
  
#### <a name="strong-password-guidelines"></a>强密码指南
  
强密码不易被人猜出，也不易被计算机程序破解。 强密码不得使用禁止的条件或字词，具体包括：  
  
* 空条件或 NULL 条件
* “Password”
* “Admin”
* “Administrator”
* “sa”
* “sysadmin”

强密码不得是下列与安装计算机相关联的字词：  
  
* 当前登录计算机的用户的用户名
* 计算机名称  
  
强密码的长度必须多于 8 个字符，并且强密码至少要满足下列四个条件中的三个：  
  
* 它必须包含大写字母。
* 它必须包含小写字母。  
* 必须包含数字。
* 必须包含非字母数字字符；例如 #、% 或 ^。  
  
你在此页上输入的密码必须符合强密码策略要求。 如果有任何使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的自动化过程，请确保密码符合强密码策略要求。  
  
### <a name="see-also"></a>另请参阅

若要详细了解如何选择 Windows 身份验证与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，请参阅[选择身份验证模式](../../relational-databases/security/choose-an-authentication-mode.md)。  

若要详细了解如何选择用于运行 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的帐户，请参阅[配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。

## <a name="database-engine-configuration---data-directories-page"></a><a name ="datadir"></a>“数据库引擎配置 - 数据目录”页

此页面可用于指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 程序和数据文件的安装位置。 根据安装类型，支持的存储可能包括本地磁盘、共享存储或 SMB 文件服务器。  
  
若要将 SMB 文件共享指定为目录，您必须手动键入支持的 UNC 路径。 不支持转到 SMB 文件共享。 下面的示例展示了受支持的 SMB 文件共享 UNC 路径格式：

`\\<ServerName>\<ShareName>\...`

### <a name="standalone-instance-of-ssnoversion"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立实例
  
对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立实例，下表列出了受支持的存储类型，以及你可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中配置的默认目录：  
  
### <a name="ui-element-list"></a>UI 元素列表
  
|说明|受支持的存储类型|默认目录|建议|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**数据根目录**|本地磁盘、SMB 文件服务器、共享存储* |\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\ |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目录配置访问控制列表 (ACL)，并在配置过程中中断继承。|  
|**用户数据库目录**|本地磁盘、SMB 文件服务器、共享存储*|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data |用户数据目录的最佳实践取决于工作量和性能要求。|  
|**用户数据库日志目录**|本地磁盘、SMB 文件服务器、共享存储*|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|确保日志目录有足够的空间。|  
|**备份目录**|本地磁盘、SMB 文件服务器、共享存储*|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Backup|设置合适的权限以防止数据丢失，并确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的用户帐户具有写入备份目录的足够权限。 不支持对备份目录使用映射驱动器。|  
  
\*尽管共享磁盘受支持，但不建议将共享磁盘用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立实例。  
  
### <a name="failover-cluster-instance-of-ssnoversion"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例

对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例，下表列出了受支持的存储类型，以及你可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中配置的默认目录。  
  
|说明|受支持的存储类型|默认目录|建议|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**数据根目录**|共享存储、SMB 文件服务器|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\<br /><br /> **提示**：如果你选择“群集磁盘选择”页上的“共享磁盘”，系统默认选择第一个共享磁盘。 如果未在“群集磁盘选择”页上进行任何选择，此字段默认为空白。|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目录配置 ACL，并在配置过程中中断继承。|  
|**用户数据库目录**|共享存储、SMB 文件服务器|\<Drive:>Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **提示**：如果你选择“群集磁盘选择”页上的“共享磁盘”，系统默认选择第一个共享磁盘。 如果未在“群集磁盘选择”页上进行任何选择，此字段默认为空白。|用户数据目录的最佳实践取决于工作量和性能要求。|  
|**用户数据库日志目录**|共享存储、SMB 文件服务器|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **提示**：如果你选择“群集磁盘选择”页上的“共享磁盘”，系统默认选择第一个共享磁盘。 如果未在“群集磁盘选择”页上进行任何选择，此字段默认为空白。|确保日志目录有足够的空间。|  
|**备份目录**|本地磁盘、共享存储、SMB 文件服务器|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Backup<br /><br /> **提示**：如果你选择“群集磁盘选择”页上的“共享磁盘”，系统默认选择第一个共享磁盘。 如果未在“群集磁盘选择”页上进行任何选择，此字段默认为空白。|设置合适的权限以防止数据丢失，并确保 SQL Server 服务的用户帐户具有写入备份目录的足够权限。 不支持对备份目录使用映射驱动器。|  
  
### <a name="security-considerations"></a>安全注意事项
  
安装程序将为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目录配置 ACL 并在配置过程中中断继承。  
  
以下建议适用于 SMB 文件服务器：  
  
* 使用 SMB 文件服务器时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户必须是域帐户。  
  
* 用于安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的帐户应对用作数据目录的 SMB 文件共享文件夹拥有完全控制 NTFS 权限。  
  
* 用于安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的帐户应具有对 SMB 文件服务器的 SeSecurityPrivilege 特权。 若要授予此特权，请使用文件服务器上的“本地安全策略”控制台将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装帐户添加到 **“管理审核和安全日志”** 策略中。 在“本地安全策略”控制台中，可以在“用户权限分配”部分中的“本地策略”下找到此设置。

### <a name="considerations"></a>注意事项
  
* 向现有安装添加功能时，既不得更改以前安装的功能的位置，也不得为新功能指定位置。  
  
* 如果指定非默认安装目录，请确保安装文件夹对此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是唯一的。 不要将此对话框中的目录分与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的目录。 还请在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中安装 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 组件，以分隔目录。  
  
* 在下列情况下，无法安装程序文件和数据文件：  
  
  * 在可移动磁盘驱动器上
  * 在使用压缩的文件系统上
  * 在系统文件所在的目录上
  * 在故障转移群集实例的映射网络驱动器上  
  
## <a name="a-nametempdb-database-engine-configuration---tempdb-page"></a><a name="tempdb"><a/>“数据库引擎配置 - TempDB”页

此页可用于指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的 tempdb 数据和日志文件位置、大小、增长设置和文件数量。 根据安装类型，支持的存储可能包括本地磁盘、共享存储或 SMB 文件服务器。  
  
若要将 SMB 文件共享指定为目录，您必须手动键入支持的 UNC 路径。 不支持转到 SMB 文件共享。 下面的示例展示了受支持的 SMB 文件共享 UNC 路径格式：

`\\<ServerName>\<ShareName>\....`
  
### <a name="data-and-log-directories-for-a-standalone-instance-of-ssnoversion"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立实例的数据目录和日志目录

对于[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立实例，下表列出了受支持的存储类型，以及你可以在使用安装程序期间配置的默认目录。  
  
|说明|受支持的存储类型|默认目录|建议|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**数据目录**|本地磁盘、SMB 文件服务器、共享存储* |\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目录配置 ACL，并在配置过程中中断继承。<br /><br /> tempdb 目录的最佳做法取决于工作负荷和性能需求。 若要跨多个卷分散数据文件，请指定多个文件夹或驱动器。|  
|**日志目录**|本地磁盘、SMB 文件服务器、共享存储*|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|确保日志目录有足够的空间。|  
  
\*尽管共享磁盘受支持，但不建议将共享磁盘用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立实例。  
  
### <a name="data-and-log-directories-for-a-failover-cluster-instance-of-ssnoversion"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例的数据和日志目录

对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例，下表列出了受支持的存储类型，以及你可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中配置的默认目录。  
  
|说明|受支持的存储类型|默认目录|建议|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**tempdb 数据目录**|本地磁盘、共享存储、SMB 文件服务器|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\Data<br /><br /> **提示**：如果你选择“群集磁盘选择”页上的“共享磁盘”，系统默认选择第一个共享磁盘。 如果未在“群集磁盘选择”页上进行任何选择，此字段默认为空白。|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目录配置 ACL，并在配置过程中中断继承。<br /><br /> 请确保指定的一个或多个目录（如果指定了多个文件）对所有群集节点都有效。 在故障转移期间，如果 tempdb 目录对故障转移目标节点不可用，SQL Server 资源将无法联机。|  
|**tempdb 日志目录**|本地磁盘、共享存储、SMB 文件服务器|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **提示**：如果你选择“群集磁盘选择”页上的“共享磁盘”，系统默认选择第一个共享磁盘。 如果未在“群集磁盘选择”页上进行任何选择，此字段默认为空白。|用户数据目录的最佳实践取决于工作量和性能要求。<br /><br /> 确保指定的目录对所有群集节点都有效。 在故障转移期间，如果 tempdb 目录对故障转移目标节点不可用，SQL Server 资源将无法联机。<br /><br /> 确保日志目录有足够的空间。|  
  
### <a name="ui-element-list"></a>UI 元素列表

根据工作负荷和要求配置 **tempdb** 的设置。 下列设置适用于 **tempdb** 数据文件：  
  
* “文件数” 是 **tempdb** 的数据文件总数。 默认值是 8 和安装程序检测到的逻辑内核数中的较小值。 作为一般准则，如果逻辑处理器数小于或等于 8，则使用与逻辑处理器数相同的数据文件数。 如果逻辑处理器数大于 8，请指定 8 个数据文件。 如果存在争用，请以 4（逻辑处理器数上限）为倍数增加数据文件数，直到争用减少到可接受的水平，或请更改工作负荷或代码。
  
* **初始大小(MB)** ：每个 tempdb 数据文件的初始大小（以 MB 为单位）。 默认值为 8 MB（对于 [!INCLUDE[ssexpress](../../includes/ssexpress_md.md)] 为 4 MB）。 [!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] 引入的最大初始文件大小为 262,144 MB (256 GB)。 [!INCLUDE[sssql15](../../includes/sssql16-md.md)] 的初始文件大小上限为 1024MB。 所有 **tempdb** 数据文件初始大小相同。 由于 tempdb 在 SQL Server 每次启动或进行故障转移时都会重新创建，因此请指定与工作负荷正常运行所需大小接近的大小。 若要在启动过程中进一步优化 tempdb 创建，请启用[数据库即时文件初始化](../../relational-databases/databases/database-instant-file-initialization.md)。  
  
* **总初始大小(MB)** ：所有 tempdb 数据文件的累积大小。  
  
* **自动增长(MB)** ：每个 tempdb 数据文件在空间不足时自动增长的空间量（以 MB 为单位）。 在 [!INCLUDE[sssql15](../../includes/sssql16-md.md)] 及更高版本中，所有数据文件都按此设置中指定的量同时增长。  
  
* **总自动增长 (MB)** 是每个自动增长事件的累积大小。  
* **数据目录**：显示保留 tempdb 数据文件的所有目录。 当存在多个目录时，以循环方式将数据文件放置在目录中。 例如，如果你创建 3 个目录，并指定 8 个数据文件，那么数据文件 1、4 和 7 在第一个目录中创建。 数据文件 2、5 和 8 在第二个目录中创建。 数据文件 3 和 6 在第三个目录中创建。  
  
* 若要添加目录，请选择“添加…”。  
  
* 若要删除目录，请依次选择目录和“删除”。  
  
“tempdb 日志文件”是日志文件名。 此文件是自动创建的。 下列设置仅适用于 **tempdb** 日志文件：  
  
* **初始大小 (MB)** 是 **tempdb** 日志文件的初始大小。 默认值为 8 MB（对于 [!INCLUDE[ssexpress](../../includes/ssexpress_md.md)] 为 4 MB）。 [!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] 引入的最大初始文件大小为 262,144 MB (256 GB)。 [!INCLUDE[sssql15](../../includes/sssql16-md.md)] 的初始文件大小上限为 1024MB。 由于 tempdb 在 SQL Server 每次启动或进行故障转移时都会重新创建，因此请指定与工作负荷正常运行所需大小接近的大小。 若要在启动过程中进一步优化 tempdb 创建，请启用[数据库即时文件初始化](../../relational-databases/databases/database-instant-file-initialization.md)。  
  
  > [!NOTE]
  > tempdb 使用最小日志记录。 无法备份 tempdb 日志文件。 它在 SQL Server 每次启动或群集实例进行故障转移时重新创建。

* **自动增长 (MB)** 是 **tempdb** 日志以 MB 为单位的增长量。  默认值 64 MB 在初始化期间创建最优数量的虚拟日志文件。  

  > [!NOTE]
  > tempdb 日志文件不使用数据库即时文件初始化。
  
* “日志目录” 是其中创建 **tempdb** 日志文件的日志目录。 只有一个 tempdb 日志目录。  
  
### <a name="security-considerations"></a>安全注意事项
  
安装程序为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目录配置 ACL，并在配置过程中中断继承。  

以下建议适用于 SMB 文件服务器：  
  
* 使用 SMB 文件服务器时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户必须是域帐户。  
  
* 如果帐户用于安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，应对用作数据目录的 SMB 文件共享文件夹拥有完全控制 NTFS 权限。  
  
* 用于安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的帐户应具有对 SMB 文件服务器的 SeSecurityPrivilege 特权。 若要授予此特权，请使用文件服务器上的“本地安全策略”控制台将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装帐户添加到 **“管理审核和安全日志”** 策略中。 在“本地安全策略”控制台中，可以在“用户权限分配”部分中的“本地策略”下找到此设置。  
  
> [!NOTE]
> 如果指定非默认安装目录，请确保安装文件夹对此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是唯一的。 此对话框中的任何目录都不应与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的目录共享。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例中的[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件也应安装到单独的目录。
  
### <a name="see-also"></a>另请参阅

* [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)
* [文件服务器上的共享权限和 NTFS 权限](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/configuring-share-and-ntfs-permissions)  

<!--
The MaxDOP setting applies only to SQL Server 2019 and later.
-->

::: moniker range=">=sql-server-ver15"

## <a name="a-namemaxdop-database-engine-configuration---maxdop-page"></a><a name="maxdop"><a/>“数据库引擎配置 - MaxDOP”页

“最大并行度(MaxDOP)”决定了一个语句最多可以使用多少个处理器。 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 引入了在安装过程中配置此选项的功能。 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 还可以根据内核数自动为服务器检测建议的 MaxDOP 设置。  

如果在安装过程中跳过此页，默认 MaxDOP 值就是此页中显示的建议值，而不是旧版本 (0) 的默认 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 值。 你还可以在此页上手动配置这一设置，并在安装后修改这一设置。 

### <a name="ui-element-list"></a>UI 元素列表

* “最大并行度 (MaxDOP)”是在并行执行一个语句期间使用的最大处理器数的值。 默认值遵循[配置服务器配置选项“最大并行度”](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines)中的最大并行度准则。

## <a name="a-namememory-database-engine-configuration---memory-page"></a><a name="memory"><a/>“数据库引擎配置 - 内存”页

“最小服务器内存”决定了 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 将用于缓冲池和其他缓存的内存下限。 默认值和建议值均为 0。 若要详细了解“最小服务器内存”的影响，请参阅[内存管理体系结构指南](../../relational-databases/memory-management-architecture-guide.md#effects-of-min-and-max-server-memory)。

“最大服务器内存”决定了 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 将用于缓冲池和其他缓存的内存上限。 根据现有系统内存，默认值为 2,147,483,647MB，计算出的建议值遵循独立 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的[服务器内存配置选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually)中的内存配置准则。 若要详细了解“最大服务器内存”的影响，请参阅[内存管理体系结构指南](../../relational-databases/memory-management-architecture-guide.md#effects-of-min-and-max-server-memory)。

如果在安装过程中跳过此页，使用的“最大服务器内存”默认值为 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 默认值 (2,147,483,647MB)。 选中“建议”单选按钮后，即可在此页上手动配置这些设置，并在安装后修改这些设置。 有关详细信息，请参阅 [服务器内存配置选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)。

### <a name="ui-element-list"></a>UI 元素列表
  
**默认**：默认情况下，此单选按钮处于选中状态，并将“最小服务器内存”和“最大服务器内存”设置设为 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 默认值。 

**建议**：必须选中此单选按钮，才能接受计算出的建议值，或将计算出的值更改为用户配置的值。  
  
**最小服务器内存(MB)** ：若要从计算出的建议值更改为用户配置的值，请输入“最小服务器内存”值。  
  
**最大服务器内存(MB)** ：若要从计算出的建议值更改为用户配置的值，请输入“最大服务器内存”值。  

**单击此处接受用于 SQL Server 数据库引擎的建议内存配置**：选中此复选框，可以接受此服务器上计算出的建议内存配置。 如果“建议”单选按钮处于选中状态，而此复选框处于未选中状态，那么安装程序将无法继续运行。

::: moniker-end

## <a name="database-engine-configuration---filestream-page"></a>“数据库引擎配置 - FILESTREAM”页

使用此页可针对此 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]安装启用 FILESTREAM。 FILESTREAM 通过将 varbinary(max) 二进制大型对象 (BLOB) 数据作为文件存储在 NTFS 文件系统中，将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]与此文件系统集成在一起。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句可插入、更新、查询、搜索和备份 FILESTREAM 数据。 Microsoft Win32 文件系统接口提供对数据的流访问权限。 
  
### <a name="ui-element-list"></a>UI 元素列表
  
**为 Transact-SQL 访问启用 FILESTREAM**：选中此项可针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问启用 FILESTREAM。 必须先选中此复选框，然后才能配置其他选项。  
  
**为文件 I/O 流访问启用 FILESTREAM**：选中此项可针对 FILESTREAM 启用 Win32 流访问。  
  
**Windows 共享名**：输入用来存储 FILESTREAM 数据的 Windows 共享名。  
  
**允许远程客户端拥有对 FILESTREAM 数据的流访问权限**：选中这个复选框可允许远程客户端访问此服务器上的 FILESTREAM 数据。  
  
### <a name="see-also"></a>另请参阅

* [启用和配置 FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md)
* [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  

## <a name="database-engine-configuration---user-instance-page"></a>“数据库引擎配置 - 用户实例”页

“用户实例”页可用于：

* 为没有管理员权限的用户单独生成[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例。
* 将用户添加到管理员角色中。  
  
### <a name="options"></a>选项
  
**启用用户实例**：默认处于启用状态。 若要禁用“启用用户实例”，请取消选中此复选框。  
  
用户实例也称为子实例或客户端实例，是由父实例（作为服务运行的主实例，如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ）代表用户生成的 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]实例。 用户实例作为用户进程在该用户的安全上下文下运行。 用户实例与父实例和计算机上运行的其他任何用户实例隔离开来。 用户实例功能亦称为“以普通用户身份运行 (RANU)”。  
  
> [!NOTE]  
> 在使用安装程序期间预配为 sysadmin 固定服务器角色成员的登录名在模板数据库中预配为管理员。 除非遭删除，否则它们是用户实例的 sysadmin 固定服务器角色成员。  
  
**将用户添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员角色中**：默认处于禁用状态。 若要将当前安装程序用户添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员角色中，请选中此复选框。  
  
作为 BUILTIN\Administrators 成员的 [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] 用户在连接到 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 时，不会自动添加到 sysadmin 固定服务器角色中。 只有已显式添加到服务器级别管理员角色中的 [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] 用户才能管理 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]。 Built-In\Users 组成员可以连接到 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 实例，但他们执行数据库任务的权限有限。 因此，对于从旧版 Windows 的 BUILTIN\Administrators 和 Built-In\Users 继承 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 权限的用户，必须在 [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] 上运行的 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 实例中显式获得管理权限。  
  
若要在安装程序结束后更改用户角色，请使用 [SQL Server Management Studio](../../relational-databases/security/authentication-access/join-a-role.md) 或 [TRANSACT-SQL](../../t-sql/statements/alter-role-transact-sql.md)。
