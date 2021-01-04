---
title: 管理 Reporting Services SharePoint 服务应用程序 | Microsoft Docs
description: 了解如何在 SharePoint 管理中心中管理 SQL Server Reporting Services 服务应用程序。
ms.date: 10/05/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: cbc258c5da9252a4463caf4b9b096c5a899d9e0a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97409683"
---
# <a name="manage-a-reporting-services-sharepoint-service-application"></a>管理 Reporting Services SharePoint 服务应用程序

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序通过 SharePoint 管理中心进行管理。 “管理”和“属性”页允许您更新服务应用程序的配置及常见管理任务。  

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

## <a name="open-service-application-properties-page"></a>打开服务应用程序属性页面

 要打开 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用属性页面，请完成以下步骤：  
  
1.  在管理中心内，在“应用程序管理”组中单击 **“管理服务应用程序”** 。  
  
2.  在服务应用程序名称附近单击或单击 **“类型”** 列，这将选择整行，然后在 SharePoint 功能区上单击 **“属性”** 。  
  
 有关服务应用程序属性的详细信息，请参阅[步骤 3：创建 Reporting Services 服务应用程序](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md#bkmk_create_serrviceapplication)中的“服务应用程序”一节。  
  
## <a name="open-service-application-management-pages"></a>打开服务应用程序管理页面

 要打开 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用管理页面，请完成以下步骤：  
  
1.  在管理中心内，在“应用程序管理”组中单击 **“管理服务应用程序”** 。  
  
2.  单击服务应用程序的名称，此时将打开 **“管理 Reporting Services 应用程序”** 页。  
  
3.  您也可以在服务应用程序名称附近单击或单击 **“类型”** 列，这将选择整行，然后在 SharePoint 功能区上单击 **“管理”** 。  
  
## <a name="system-settings-page"></a>系统设置页面

 系统设置页允许您配置服务应用程序的行为和用户体验，包括各种超时。
  
### <a name="report-settings"></a>报表设置
  
|设置|注释|  
|-------------|--------------|  
|外部图像超时|默认值为 600 秒。|  
|快照压缩|默认值为 SQL|  
|系统报表超时|默认值为 1800 秒。<br /><br /> 指定在给定的秒数之后报表服务器上的报表处理是否超时。 此值应用于报表服务器上的报表处理。 它不会影响为报表提供数据的数据库服务器上的数据处理。 报表处理计时器时钟在您选择报表时开始运行，而在报表打开时结束运行。 所指定的值必须足以完成数据处理和报表处理。|  
|系统快照限制|默认值为 -1，没有限制。<br /><br /> 为要保留的报表历史副本数设置站点范围默认值。 默认值是初始设置，用于确定可以为每个报表存储的快照数。 可以在特定报表的属性页中指定其他限定值。|  
|存储的参数的生存期|默认值为 180|  
|存储的参数的阈值|默认值为 1500 天。|  
  
### <a name="session-settings"></a>会话设置
  
|设置|注释|  
|-------------|--------------|  
|会话超时|默认值为 600 秒。|  
|使用会话 Cookie|默认值为 TRUE。|  
|EDLX 报表超时|默认值为 1800 秒。|  
  
### <a name="system-settings-for-logging"></a>日志记录的系统设置
  
|设置|注释|  
|-------------|--------------|  
|启用执行日志记录|默认值为 TRUE。<br /><br /> 指定报表服务器是否会生成跟踪日志及指定日志保留的天数。 . 这些日志存储在报表服务器计算机上的 \Microsoft SQL Server\MSSQL.n\ReportServer\Log 文件夹中。 每次重新启动该服务时都要启动新的日志文件。 有关日志文件的详细信息，请参阅 [Report Server Service Trace Log](../../reporting-services/report-server/report-server-service-trace-log.md)|  
|保留执行日志的天数|默认值为 60 天。|  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 都支持 SharePoint ULS 日志记录。  有关详细信息，请参阅 [为 SharePoint 跟踪日志 (ULS) 启用 Reporting Services 事件](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)  
  
### <a name="security-settings"></a>安全设置
  
|设置|注释|  
|-------------|--------------|  
|启用集成安全性|默认值为 TRUE。<br /><br /> 指定是否可以使用请求报表的用户的 Windows 安全令牌与报表数据源建立连接。|  
|启用加载报表定义|默认值为 TRUE。|  
|启用远程错误|默认值为 FALSE|  
|启用测试连接详细错误|默认值为 TRUE。|  
  
### <a name="client-settings"></a>客户端设置
  
|设置|注释|  
|-------------|--------------|  
|启用报表生成器下载|默认值为 TRUE。<br /><br /> 指定客户端是否能够看到可用于下载报告生成器应用程序的按钮。|  
|报表生成器启动 URL|当报表服务器不使用默认报表生成器 URL 时，可指定自定义 URL。 此设置是可选的。 如果不指定值，将使用默认 URL，它将启动报表生成器。 要将报表生成器 3.0 作为 Click-Once 应用程序启动，请输入以下值： https://\<computername>/ReportServer/ReportBuilder/ReportBuilder_3_0_0_0.application。|  
|启用客户端打印|默认值为 TRUE。<br /><br /> 指定用户是否可以下载提供打印选项的客户端控件。|  
|编辑会话超时|默认值为 7200 秒。|  
|编辑会话缓存限制|默认值为 5。|  
  
## <a name="manage-jobs"></a>管理作业

 您可以查看和删除正在运行的作业，例如报表订阅和数据驱动订阅所创建的作业。 该页不用于管理订阅，而是管理由订阅触发的作业。 例如，安排为每小时运行一次的订阅将每小时生成一个在 **“管理作业”** 页面上显示的作业。  
  
 ![管理正在运行的作业](../../reporting-services/report-server-sharepoint/media/ssrs-manage-jobs.gif "管理正在运行的作业")  
  
## <a name="key-management"></a>密钥管理
 下表总结了“密钥管理”页面  
  
> [!IMPORTANT]  
>  定期更改 Reporting Services 加密密钥是确保安全的好办法。 建议在升级 Reporting Services 主版本后立即更改密钥。 升级后更改密钥将使在升级周期外更改 Reporting Services 加密密钥所导致的额外服务中断降到最低程度。  
  
|页|说明|  
|----------|-----------------|  
|备份加密密钥|1) 在“密码:”和“确认密码:”框中键入密码，然后单击“导出”  。 如果您键入的密码不符合域策略的复杂性要求，将会看到一条警告。<br /><br /> 2) 系统会提示你指定保存密钥文件的位置。 你应该考虑将密钥文件存储在与运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的计算机不同的计算机上。 默认文件名称与服务应用程序名称相同。|  
|还原加密密钥|1) 在“文件位置”框中键入或浏览密钥文件<br /><br /> 2) 在“密码”框中，键入用于备份加密文件的密码。<br /><br /> 3) 单击“确定”|  
|更改加密密钥|此操作将创建一个新密钥，并且重新加密您已加密的内容。 如果有很多内容，此操作可能需要几个小时。<br /><br /> 完成更改加密密钥操作后，建议您备份新密钥。|  
|删除加密的内容|删除的内容无法恢复。<br /><br /> **\*\* 重要提示 \*\*** 删除和重新创建对称密钥的操作不能逆转或撤消。 删除或重新创建该密钥可能对您当前的安装产生重要影响。 如果删除对称密钥，则使用此密钥加密的所有现有数据也将被删除。 删除的数据包括指向外部报表数据源的连接字符串、存储的连接字符串和某些订阅信息。|  

## <a name="execution-account"></a>执行帐户

 使用此页可以配置用于无人参与处理的帐户。 只有在其他凭据源不可用的以下特殊情况时，才使用此帐户：  
  
-   当报表服务器连接到不需要凭据的数据源时。 可能不需要凭据的数据源示例包括 XML 文档和一些客户端数据库应用程序。  
  
-   当报表服务器连接到其他服务器以检索报表中所引用的外部图像文件或其他资源时。  

 设置此帐户是可选的，但如果不设置此帐户，则将限制您使用外部图像以及与一些数据源的连接。 在检索外部图像文件时，报表服务器将检查是否可以建立匿名连接。 如果连接受密码保护，报表服务器将使用无人参与的报表处理帐户来连接到远程服务器。 在为报表检索数据时，报表服务器或者模拟当前用户、提示用户提供凭据或使用存储的凭据，或者（如果数据源连接指定 **“无”** 作为凭证类型）使用无人参与的处理帐户。 在连接到其他计算机时，报表服务器不允许对其服务帐户凭据进行委托或模拟，因此，如果不能使用其他凭据，报表服务器就必须使用无人参与的处理帐户。  

 您指定的帐户不能是用于运行服务帐户的帐户。 如果将在扩展部署中运行该报表服务器，则必须在每个报表服务器上以相同的方式配置此帐户。  

 您可以使用任何一个 Windows 用户帐户。 为获得最佳结果，请选择拥有读取权限和网络登录权限的帐户，以支持与其他计算机的连接。 对于您希望在报表中使用的任何外部图像或数据文件，它必须拥有对这些文件的读取权限。 切勿指定本地帐户，除非所有报表数据源和外部图像均存储在报表服务器计算机中。 只可将该帐户用于无人参与报表处理。  

 ### <a name="powershell-command"></a>PowerShell 命令

 以下是一个 PowerShell 命令示例，用于通过 UEAccount 属性返回 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序列表。  

```
Get-SPRSServiceApplication | select typename, name, service, ueaccountname  
```

 有关详细信息，请参阅 [用于 Reporting Services SharePoint 模式的 PowerShell cmdlet](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md)。  

### <a name="options"></a>选项

 **指定执行帐户**  
 选择此选项可指定一个帐户。  
  
 **帐户**  
 输入一个 Windows 域用户帐户。 使用以下格式：\<domain>\\<user account\>。  
  
 **密码**  
 键入密码。  
  
 **确认密码**  
 重新键入密码。  

## <a name="e-mail-settings"></a>电子邮件设置

 使用此页可指定简单邮件传输协议 (SMTP) 设置，这些设置用于启用报表服务器的报表服务器电子邮件传递功能。 可使用报表服务器电子邮件传递扩展插件通过电子邮件订阅来分发报表或报表处理通知。 报表服务器电子邮件传递扩展插件需要使用 SMTP 服务器并在“发件人:”字段中使用电子邮件地址。  

### <a name="options"></a>选项

 **使用 SMTP 服务器**  
 指定报表服务器电子邮件通过 SMTP 服务器进行路由。  
  
 **出站 SMTP 服务器**  
 指定要使用的 SMTP 服务器或网关。 可以使用网络中的本地服务器或 SMTP 服务器。  
  
 **发件人地址**  
 指定要在所生成电子邮件的“发件人:”字段中使用的电子邮件地址。 您必须指定一个有权从 SMTP 服务器中发送邮件的用户帐户。  

## <a name="provision-subscriptions-and-alerts"></a>预配订阅和警报

 使用该页验证 SQL Server 代理是否正在运行，并将对报表服务的访问设置为使用 SQL Server 代理。 SQL Server 代理是 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅、计划和数据警报所必需的。 [用于 SSRS 服务应用程序的设置订阅和警报](../../reporting-services/install-windows/provision-subscriptions-and-alerts-for-ssrs-service-applications.md)  

## <a name="proxy-association"></a>代理关联

 在创建 Reporting Services 服务应用程序时，您选择了要关联的 Web 应用程序，并且为通过 Reporting Services 服务应用程序访问设置了权限。 如果您选择不关联或要更改关联，则可以使用以下步骤：  
  
1.  在 SharePoint 管理中心内，在“应用程序管理”中单击 **“配置服务应用程序关联”** 。  
  
2.  在“服务应用程序关联”页中，将视图更改为 **“服务应用程序”** 。  
  
3.  找到并单击新 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序的名称。 也可以单击应用程序代理组名称 **默认值** 以将代理添加到默认组，而不是完成以下步骤。  
  
4.  在 **“编辑以下连接组”** 选择框中，选择 **自定义**。  
  
5.  选中代理对应的框，然后单击 **“确定”** 。  
  
更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
