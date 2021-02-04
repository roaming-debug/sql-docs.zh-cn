---
title: 服务器和数据库连接问题疑难解答
description: 本文介绍如何诊断并修复连接到报表服务器时遇到的问题。 还将介绍“意外错误”消息。
ms.date: 12/16/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
ms.assetid: 8bbb88df-72fd-4c27-91b7-b255afedd345
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 74bf6f0da0daa0df248243ffbce505c75c55db5e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076319"
---
# <a name="troubleshoot-server--database-connection-problems-with-reporting-services"></a>Reporting Services 的服务器和数据库连接问题疑难解答
使用本主题可以排除在连接到报表服务器时所遇到的故障。 本主题还提供了与错误消息有关的信息。 有关数据源配置和配置报表服务器连接信息的详细信息，请参阅[指定报表数据源的凭据和连接信息](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)和[配置报表服务器数据库连接（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)。  
  
## <a name="cannot-create-a-connection-to-data-source-datasourcename-rserroropeningconnection"></a>无法与数据源“datasourcename”建立连接。 (rsErrorOpeningConnection)  
这是一个一般性错误，在报表服务器无法打开到为报表提供数据的外部数据源的连接时发生。 此错误和另外一条错误消息一起出现，后者指明了错误的根本原因。 与 **rsErrorOpeningConnection** 一起出现的可能还有以下错误：  
  
### <a name="login-failed-for-user-username"></a>用户“UserName”登录失败  
该用户无权访问该数据源。 如果使用的是 SQL Server 数据库，请验证该用户是否具有有效的数据库用户登录名。 有关如何创建数据库用户或 SQL Server 登录名的详细信息，请参阅 [创建数据库用户](../../relational-databases/security/authentication-access/create-a-database-user.md) 和 [创建 SQL Server 登录名](../../relational-databases/security/authentication-access/create-a-login.md)。  
  
### <a name="login-failed-for-user-nt-authorityanonymous-logon"></a>用户“NT AUTHORITY\ANONYMOUS LOGON”登录失败  
跨多个计算机连接传递凭据时会发生此错误。 如果使用 Windows 身份验证，并且未启用 Kerberos 5 协议，则在跨多个计算机连接传递凭据时将会出现此错误。 若要解除此错误，请考虑使用已存储凭据或提示的凭据。 有关如何解决此问题的详细信息，请参阅 [指定报表数据源的凭据和连接信息](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)。  
  
### <a name="an-error-has-occurred-while-establishing-a-connection-to-the-server"></a>在建立与服务器的连接时出错。  
在连接到 SQL Server 时，在默认的设置下 SQL Server 不允许远程连接可能会导致此失败。 （提供程序：命名管道提供程序，错误: 40 - 无法打开到 SQL Server 的连接）。 此错误由托管报表服务器数据库的数据库引擎实例返回。 大多数情况下，出现此错误的原因是 SQL Server 服务停止。 或者，如果使用的是具有高级服务的 SQL Server Express 或命名实例，那么，当报表服务器 URL 或报表服务器数据库的连接字符串不正确时，将发生此错误。 若要解决这些问题，请执行以下操作：  
  
* 验证 SQL Server (**MSSQLSERVER**) 服务是否正在运行。 在托管数据库引擎实例的计算机上，依次单击“开始”、“管理工具”和“服务”，然后滚动到 SQL Server (**MSSQLSERVER**)。 如果未启动，请右键单击该服务，选择“属性”，在“启动类型”中选择“自动”，然后依次单击“应用”、“启动”和“确定”。   
* 确保报表服务器 URL 和报表服务器数据库连接字符串正确。 如果 Reporting Services 或数据库引擎作为命名实例安装，则在安装过程中创建的默认连接字符串将包括相应的实例名称。 例如，如果在名为 DEVSRV01 的服务器上安装了具有高级服务的 SQL Server Express 的默认实例，则 Web 门户 URL 将为 DEVSRV01\Reports$SQLEXPRESS。 此外，连接字符串中的数据库服务器名称将类似于 DEVSRV01\SQLEXPRESS。 有关 SQL Server Express 的 URL 和数据源连接字符串的详细信息，请参阅 [具有高级服务的 SQL Server Express 中的 Reporting Services](/previous-versions/sql/sql-server-2008-r2/ms365166(v=sql.105))。 若要验证报表服务器数据库的连接字符串，请启动 Reporting Services 配置工具并查看“数据库安装”页。  
  
### <a name="a-connection-cannot-be-made-ensure-that-the-server-is-running"></a>无法进行连接。 请确保服务器正在运行。  
此错误由 ADOMD.NET 提供程序返回。 有多种原因可导致发生此错误。 如果已将服务器指定为“localhost”，请尝试改为指定服务器名称。 如果无法为新连接分配内存，也会发生此错误。 有关详细信息，请参阅 [知识库文章 912017 - 连接到 SQL Server 2005 Analysis Services 实例时收到错误消息：](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/912017)。  
  
如果此错误还包含“无法识别这种主机”，则说明 Analysis Services 服务器不可用或拒绝连接。 如果 Analysis Services 服务器作为命名实例安装在远程计算机上，则可能必须运行 SQL Server Browser 服务来获取该实例使用的端口号。  
  
### <a name="report-services-soap-proxy-source"></a>（Report Services SOAP 代理源）  
如果在报表模型生成过程中出现此错误，并且其他信息部分还包含“SQL Server 不存在或访问被拒绝”，则可能是出现了下列情况：  
* 数据源的连接字符串包含“localhost”。  
* SQL Server 服务禁用 TCP/IP。  
  
若要解决此错误，您可以将连接字符串修改为使用服务器名称，也可针对服务启用 TCP/IP。 请执行下列步骤启用 TCP/IP：  
  
1. 启动 SQL Server 配置管理器。  
2. 展开“SQL Server 网络配置”  。  
3. 选择“MSSQLSERVER 的协议”  。  
4. 右键单击“TCP/IP”  ，并选择“启用”  。  
5. 选择“SQL Server 服务”  。  
6. 右键单击“SQL Server (MSSQLSERVER)”  ，并选择“重新启动”  。  
  
## <a name="wmi-error-when-connecting-to-a-report-server-in-management-studio"></a>在 Management Studio 中连接报表服务器时出现 WMI 错误  
默认情况下，Management Studio 使用 Reporting Services Windows Management Instrumentation (WMI) 提供程序来建立与报表服务器的连接。 如果未正确安装 WMI 提供程序，在尝试连接到报表服务器时将遇到以下错误：  
  
无法连接到 \<your server name>。 没有安装 Reporting Services WMI 提供程序，或者该提供程序配置不当 (Microsoft.SqlServer.Management.UI.RSClient)。  
  
若要解决此错误，请重新安装该软件。 对于所有其他情况，作为临时解决方法，可以通过 SOAP 端点连接到报表服务器：  
  
* 在 Management Studio 中的“连接到服务器”  对话框中，在“服务器名称”  中键入报表服务器的 URL。 默认为 `https://<your server name>/reportserver`。 如果使用的是具有高级服务的 SQL Server 2008 Express 版本，则为 `https://<your server name>/reportserver$sqlexpress`。  
  
若要解决该错误以便可以使用 WMI 提供程序进行连接，则应运行安装程序以修复 Reporting Services 或重新安装 Reporting Services。  
  
## <a name="connection-error-where-login-failed-due-to-unknown-user-name-or-bad-password"></a>连接错误，由于未知用户名或密码错误导致登录失败  
如果从报表服务器连接到报表服务器数据库时使用了域帐户，并且更改了该域帐户的密码，则可能会出现 **rsReportServerDatabaseLogonFailed** 错误。   
  
完整错误文本如下：“报表服务器无法打开与报表服务器数据库的连接。 登录失败 (**rsReportServerDatabaseLogonFailed**)。 登录失败: 用户名未知或密码错误。”  
  
如果重置密码，则必须更新该连接。 有关详细信息，请参阅[配置报表服务器数据库连接（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)。  
  
## <a name="the-report-server-cannot-open-a-connection-to-the-report-server-database-rsreportserverdatabaseunavailable"></a>报表服务器无法打开与报表服务器数据库的连接。 (rsReportServerDatabaseUnavailable)。  
完整消息：报表服务器无法打开与报表服务器数据库的连接。 所有请求和处理都要求与数据库建立连接 (rsReportServerDatabaseUnavailable)  
当报表服务器无法连接到为服务器提供内部存储的 SQL Server 关系数据库时，会发生此错误。 通过 Reporting Services 配置工具来管理与报表服务器数据库的连接。 您可以运行此工具，转到“数据库安装”页，更正连接信息。 使用此工具更新连接信息是最佳的方法；此工具可确保能够更新相关设置并重新启动服务。 有关详细信息，请参阅 [配置报表服务器数据库连接](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md) 和 [配置报表服务器服务帐户](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)。  
  
如果没有将托管报表服务器数据库的数据库引擎实例配置为启用远程连接，也会发生此错误。 在某些 SQL Server 版本中，默认情况下将启用远程连接。 若要验证是否已在你使用的 SQL Server 数据库引擎实例上启用远程连接，请运行 SQL Server 配置管理器工具。 必须同时启用 TCP/IP 和命名管道。 报表服务器同时使用这两种协议。 有关如何启用远程连接的说明，请参阅 [配置用于远程管理的报表服务器](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md)中的“如何配置与报表服务器数据库的远程连接”部分。  
  
如果此错误还包含以下文本，则说明用于运行数据库引擎实例的帐户的密码已过期：“与服务器建立连接时出错。 在连接到 SQL Server 时，在默认的设置下 SQL Server 不允许远程连接可能会导致此失败。 （**提供程序：SQL Server 网络接口，错误：26 - 定位指定的服务器/实例时出错）** 。” 若要解决此错误，请重置密码。   
  
## <a name="rpc-server-is-not-listening"></a>“RPC 服务器未在监听”  
报表服务器服务对某些操作使用远程过程调用 (RPC) 服务器。 如果遇到“RPC 服务器未在监听”错误，请验证报表服务器服务是否正在运行。  
  
## <a name="unexpected-error-general-network-error"></a>错误（常规网络错误）  
此错误表示数据源连接错误。 您应该查看连接字符串，验证您是否拥有访问该数据源的权限。 如果使用 Windows 身份验证访问数据源，则必须拥有访问承载该数据源的计算机的权限。  
  
## <a name="unable-to-grant-database-access-in-sharepoint-central-administration"></a>无法在 SharePoint 管理中心授予数据库访问权限  
在 Windows Vista 或 Windows Server 2008 上对 Reporting Services 进行配置以便与 SharePoint 产品或技术集成时，尝试在 SharePoint 管理中心的“授予数据库访问权限”  页上授予访问权限时，可能会收到下列错误消息：“无法与计算机建立连接。”  
  
发生这种情况是因为在执行需要管理员权限的任务时，Windows Vista 和 Windows Server 2008 中的用户帐户控制 (UAC) 要求管理员显式接受才能提升和使用管理员标记。 但是在这种情况下，无法提升 Windows SharePoint Services 管理服务来授予 Reporting Services 服务帐户对 SharePoint 配置和内容数据库的访问权限。  
  
在 SQL Server 2008 Reporting Services 中，只有报表服务器服务帐户需要数据库访问权限；在 SQL Server 2005 Reporting Services SP2 中，报表服务器 Windows 服务帐户和报表服务器 Web 服务帐户都要求数据库访问权限。 有关 SQL Server 2008 中的报表服务器服务帐户的详细信息，请参阅服务帐户（Reporting Services 配置）。  
  
对于此问题有两种解决方法。   
1.  一种解决方法是：可以暂时关闭 UAC 而使用 SharePoint 管理中心来授予访问权限。  
> [!IMPORTANT]  
> 如果关闭 UAC 解决此问题要谨慎，在 SharePoint 管理中心授予数据库访问权限后要立即打开 UAC。 如果不想关闭 UAC，请使用本节提供的另一种解决方法。 有关 UAC 的信息，请参阅 Windows 产品文档。  
2. 第二种解决方法是：可以手动对 Reporting Services 服务帐户授予数据库的访问权限。 可以使用以下过程通过将 Reporting Services 服务帐户添加到正确的 Windows 组和数据库角色来授予访问权限。 此过程适用于 SQL Server 2008 Reporting Services 中的报表服务器服务帐户；如果运行 SQL Server 2005 Reporting Services，请为报表服务器 Windows 服务帐户和报表服务器 Web 服务帐户执行此过程。   
  
### <a name="to-manually-grant-database-access"></a>手动授予数据库访问权限  
  
1. 将报表服务器服务帐户添加到 Reporting Services 计算机上的 WSS_WPG Windows 组。  
2. 连接到承载 SharePoint 配置和内容数据库的数据库实例，为报表服务器服务帐户创建 SQL 数据库登录名。  
3. 将 SQL 数据库登录名添加到以下数据库角色中：  
  
* WSS 内容数据库中 db_owner 角色  
* SharePoint_Config 数据库中 WSS_Content_Application_Pools 角色  
  
## <a name="unable-to-connect-to-the-reports-and-reportserver-directories-when-the-report-server-databases-are-created-on-a-virtual-sql-server-that-runs-in-a-microsoft-cluster-services-mscs-cluster"></a>在运行于 Microsoft 群集服务 (MSCS) 群集中的虚拟 SQL Server 上创建报表服务器数据库时，无法连接到 /reports 和 /reportserver 目录  
在 MSCS 群集中运行的虚拟 SQL Server 上创建报表服务器数据库 **ReportServer** 和 **ReportServerTempDB** 时，远程名称（格式为 `<domain>\<computer_name>$` ）可能没有向 SQL Server 注册为登录名。 如果将报表服务器服务帐户配置为需要此远程名称才能进行连接的帐户，则用户在 Reporting Services 中无法连接到 /reports 和 /reportserver 目录。 例如，内置的 Windows 帐户 NetworkService 要求此远程名称。 要避免此问题，请使用显式域帐户或 SQL Server 登录名连接到报表服务器数据库。  
    
  ## <a name="see-also"></a>另请参阅  
[Reporting Services 和 Power View 的浏览器支持](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)  
[错误和事件 (Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
[排查与 Reporting Services 报表相关的数据检索问题](../../reporting-services/troubleshooting/troubleshoot-data-retrieval-issues-with-reporting-services-reports.md)  
[Reporting Services 订阅和传递的疑难解答](../../reporting-services/troubleshooting/troubleshoot-reporting-services-subscriptions-and-delivery.md)  
  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect-md.md)]
