---
description: 安装 SQL Server Reporting Services
title: 安装 SQL Server Reporting Services | Microsoft Docs
ms.date: 12/11/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 35924e9e1f5a72533ef1b30d983b99493858274d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484229"
---
# <a name="install-sql-server-reporting-services"></a>安装 SQL Server Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

SQL Server Reporting Services 安装涉及到用于存储报表项、呈现报表以及处理订阅和其他报表服务的服务器组件。 

::: moniker range=">=sql-server-ver15"
通过 Microsoft 下载中心下载 [SQL Server 2019 Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122)。

::: moniker-end

::: moniker range="=sql-server-2017"
通过 Microsoft 下载中心下载 [SQL Server 2017 Reporting Services](https://www.microsoft.com/download/details.aspx?id=55252)。

::: moniker-end

> [!NOTE]
> 要查找 Power BI 报表服务器？ 请参阅[安装 Power BI 报表服务器](https://powerbi.microsoft.com/documentation/reportserver-install-report-server/)。
> 
> 从 Reporting Services 的 SQL Server 2016 或更早版本进行升级或迁移？ 请参阅[升级和迁移 Reporting Services](upgrade-and-migrate-reporting-services.md)。

## <a name="before-you-begin"></a>开始之前

安装 Reporting Services 前，请查看[安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。

## <a name="install-your-report-server"></a>安装报表服务器

安装报表服务器非常简单。 只需几个步骤即可安装文件。

> [!NOTE]
> 安装时无需有可用的 SQL Server 数据库引擎服务器。 安装后需要有可用的该服务器才可配置 Reporting Services。

1. 查找 SQLServerReportingServices.exe 的位置并启动安装程序。

2. 选择“安装 Reporting Services”  。

3. 选择要安装的版本，然后选择“下一步”  。

    对于免费版本，可从下拉列表中选择评估版或开发人员版。

    ![评估版或开发人员版](media/install-reporting-services/report-server-install-edition-select.png)

    或者，输入产品密钥。 [查找 SQL Server Reporting Services 的产品密钥](find-reporting-services-product-key-ssrs.md)。

4. 阅读并同意许可条款，然后选择“下一步”  。

5. 需要有可用的数据库引擎才可存储报表服务器数据库。 选择“下一步”，仅安装报表服务器  。

6. 指定报表服务器的安装位置。 选择“安装”以继续操作  。

    > [!NOTE]
    > 默认路径为 C:\Program Files\Microsoft SQL Server Reporting Services。

7. 安装成功后，选择“配置报表服务器”，启动报表服务器配置管理器。

## <a name="configure-your-report-server"></a>配置报表服务器

在安装程序中选择“配置报表服务器”后，可看到“报表服务器配置管理器”   。 有关详细信息，请参阅[报表服务器配置管理器](reporting-services-configuration-manager-native-mode.md)。

需要[创建报表服务器数据库](ssrs-report-server-create-a-report-server-database.md)，以便完成 Reporting Services 的初始配置。 完成此步骤需要 SQL Server 数据库服务器。

### <a name="creating-a-database-on-a-different-server"></a>在不同服务器上创建数据库

如果要在其他计算机上的数据库服务器中创建报表服务器数据库，需要将报表服务器的服务帐户更改为数据库服务器上已识别的凭据。

默认情况下，报表服务器使用虚拟服务帐户。 如果尝试在不同服务器上创建数据库，则可能会在“应用连接权限”步骤中收到以下错误消息。

`System.Data.SqlClient.SqlException (0x80131904): Windows NT user or group '(null)' not found. Check the name again.`

要解决此错误，可将服务帐户更改为网络服务或域帐户。 将服务帐户更改为网络服务会在报表服务器计算机帐户的上下文中应用权限。

有关详细信息，请参阅[配置报表服务器服务帐户](configure-the-report-server-service-account-ssrs-configuration-manager.md)。

## <a name="windows-service"></a>Windows 服务

Windows 服务是在安装过程中创建的。 它显示为 SQL Server Reporting Services  。 服务名称为 SQLServerReportingServices  。

## <a name="default-url-reservations"></a>默认 URL 预留

URL 预留由前缀、主机名、端口和虚拟目录组成：

|组成部分|说明|
|----------|-----------------|
|前缀|默认的前缀为 HTTP。 如果你以前安装过传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”）证书，安装程序会尝试创建使用 HTTP 前缀的 URL 预留。|
|主机名|默认主机名为强通配符 (+)。 它指定对于解析为计算机的任何主机名，报表服务器均会接受指定端口上的任何 HTTP 请求，包括 `https://<computername>/reportserver`、`https://localhost/reportserver` 或 `https://<IPAddress>/reportserver.`|
|端口|默认端口为 80。 如果使用端口 80 以外的其他任何端口，则在浏览器窗口中打开 Web 门户时，必须将该端口显式添加至 URL 中。|
|虚拟目录|默认情况下，虚拟目录创建时的格式为 ReportServer（适用于报表服务器 Web 服务）和 Reports（适用于 Web 门户）。 对于报表服务器 Web 服务，默认的虚拟目录为 **reportserver**。 对于 Web 门户，默认的虚拟目录为 reports  。|

完整的 URL 字符串示例如下所示：

- `https://+:80/reportserver` 用于访问报表服务器。

- `https://+:80/reports`，用于访问 Web 门户。

## <a name="firewall"></a>防火墙

从远程计算机访问报表服务器时，如果存在防火墙，需要确保配置了任意防火墙规则。

需要打开为 Web 服务 URL 和 Web 门户 URL 配置的 TCP 端口。 默认情况下，在 TCP 端口 80 上进行配置。

## <a name="additional-configuration"></a>其他配置

- 要使用 Power BI 服务配置集成，以便可以将报表项固定到 Power BI 仪表板，请参阅[使用 Power BI 服务集成](power-bi-report-server-integration-configuration-manager.md)。

- 要为订阅处理配置电子邮件，请参阅[电子邮件设置](e-mail-settings-reporting-services-native-mode-configuration-manager.md)和[报表服务器中的电子邮件传递](../subscriptions/e-mail-delivery-in-reporting-services.md)。

- 要配置 Web 门户 以便可在远程计算机上对其进行访问，以查看和管理报表，请参阅[将防火墙配置为允许报表服务器访问](../report-server/configure-a-firewall-for-report-server-access.md)和[配置报表服务器以进行远程管理](../report-server/configure-a-report-server-for-remote-administration.md)。

## <a name="related-information"></a>相关信息

有关如何安装 SQL Server Reporting Services 本机模式的信息，请参阅[安装 Reporting Services 本机模式报表服务器](install-reporting-services-native-mode-report-server.md)。 

::: moniker range="=sql-server-2016"

有关如何在 SharePoint 集成模式下安装 SQL Server 2016 Reporting Services（及更早版本）的信息，请参阅[在 SharePoint 模式下安装第一个报表服务器](install-the-first-report-server-in-sharepoint-mode.md)。

::: moniker-end

## <a name="next-steps"></a>后续步骤

安装报表服务器后，开始创建报表并将其部署到报表服务器。 有关如何开始使用报表生成器的信息，请参阅[安装报表生成器](../../reporting-services/install-windows/install-report-builder.md)。

要使用 SQL Server Data Tools 创建报表，请[下载 SQL Server Data Tools](https://go.microsoft.com/fwlink/?LinkID=616714)。

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
