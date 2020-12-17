---
title: 在网络负载均衡群集上配置报表服务器 | Microsoft Docs
description: 了解如何配置报表服务器横向扩展，以便在 NLB 上运行。 实现 NLB 群集解决方案以支持 Reporting Services 扩展部署。
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 674e549b48cdb96a6ecae1b8630353751195085f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461388"
---
# <a name="configure-a-report-server-on-a-network-load-balancing-cluster"></a>在网络负载平衡群集上配置报表服务器

  如果要将报表服务器扩展配置为在网络负载平衡 (NLB) 群集上运行，必须执行以下操作：  
  
- 确保可通过映射到虚拟服务器 IP 地址的虚拟服务器名称来访问 NLB 群集。 必须提供虚拟服务器名称，才能配置 NLB 群集的单入口点。 在为每个报表服务器实例配置 URL 时，会将虚拟服务器名称指定为主机。  
  
- 配置视图状态验证以支持查看交互式报表。 交互式报表通常会在单个用户会话期间多次呈现，以便为了响应用户操作而对新数据或不同数据进行可视化。 通过配置视图状态验证，可以在用户会话期间保持连续性，而与实际请求由哪个报表服务器提供服务无关。  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 不支持对扩展部署进行负载平衡，也不支持定义通过共享 URL 进行单点访问。 必须实现单独的软件或硬件 NLB 群集解决方案以支持 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 扩展部署。  
  
 可以将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装在已经是 NLB 群集一部分的节点上，也可以先配置扩展部署，然后再安装群集软件。  
  
## <a name="steps-for-report-server-deployment-on-an-nlb-cluster"></a>报表服务器在 NLB 群集上的部署步骤

 可以使用下面的指南来安装和配置部署：  
  
|步骤|说明|更多信息|  
|----------|-----------------|----------------------|  
|1|在将 Reporting Services 安装在 NLB 群集内的服务器节点上之前，请检查扩展部署的要求。|[配置本机模式报表服务器扩展部署](../install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)|  
|2|配置 NLB 群集并验证它是否正常工作。<br /><br /> 确保将主机标头名称映射到 NLB 群集的虚拟服务器 IP。 主机标头名称用在报表服务器 URL 中，它比 IP 地址便于记忆和键入。|有关详细信息，请参阅您运行的 Windows 操作系统版本的 Windows Server 产品文档。|  
|3|将主机标头的 NetBIOS 和完全限定域名 (FQDN) 添加到 Windows 注册表中存储的 **BackConnectionHostNames** 列表中。<br /><br /> 例如，如果主机标头名称 \<MyServer> 是 Windows 计算机名称“contoso”的虚拟名称，可以将该 FQDN 形式引用为“contoso.domain.com”。 需要将主机标头名称 (MyServer) 与 FQDN 名称 (contoso.domain.com) 一起添加到 **BackConnectionHostNames** 的列表中。  <br /><br /> 然后重新启动计算机以确保更改生效。|如果您的服务器环境涉及本地计算机上的 NTLM 身份验证并且造成环回连接，则此步骤是必需的。<br /><br /> 如果出现这种情况，将会出现报表管理器和报表服务器之间的请求失败，错误号为 401（未经授权）。|  
|4|在已是 NLB 群集一部分的节点上以“仅文件”模式安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]，并为扩展部署配置报表服务器实例。<br /><br /> 所配置的扩展部署将不响应那些定向到虚拟服务器 IP 的请求。 在配置视图状态验证之后，会在以后的步骤中将扩展部署配置为使用虚拟服务器 IP。|[配置本机模式报表服务器横向扩展部署（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)|  
|5|配置视图状态验证。<br /><br /> 为了达到最佳效果，请在配置扩展部署之后、配置报表服务器实例以使用虚拟服务器 IP 之前执行此步骤。 通过先配置视图状态验证，可以避免在用户试图访问交互式报表时出现有关状态验证失败的异常。|本主题中的[如何配置视图状态验证](#ViewState) 。|  
|6|将 **Hostname** 和 **UrlRoot** 配置为使用 NLB 群集的虚拟服务器 IP。|本主题中的[如何配置 Hostname 和 UrlRoot](#SpecifyingVirtualServerName) 。|  
|7|验证能否通过指定的主机名来访问服务器。|本主题中的[验证报表服务器访问权限](#Verify) 。|  
  
## <a name="how-to-configure-view-state-validation"></a><a name="ViewState"></a> 如何配置视图状态验证

::: moniker range="=sql-server-2016"
若要在 NLB 群集上运行扩展部署，必须配置视图状态验证，以便用户可以查看交互式 HTML 报表。  必须对报表服务器 Web 服务执行该操作。
::: moniker-end

::: moniker range=">=sql-server-2017"
若要在 NLB 群集上运行扩展部署，必须配置视图状态验证，以便用户可以查看交互式 HTML 报表。
::: moniker-end
  
 视图状态验证由 ASP.NET 控制。 默认情况下，将启用视图状态验证并使用 Web 服务的标识来执行验证。 但是，在 NLB 群集方案中，存在运行于不同计算机上的多个服务实例和 Web 服务标识。 因为每个节点的服务标识都各不相同，所以您无法依赖单个进程标识来执行验证。  
  
 若要解决此问题，可以生成一个任意验证密钥来支持视图状态验证功能，然后将每个报表服务器节点手动配置为使用同一密钥。 可以使用随机生成的十六进制序列。 十六进制序列的长度由验证算法（如 SHA1）确定。  

::: moniker range="=sql-server-2016"

1. 通过使用由 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]提供的 autogenerate 功能，生成一个验证密钥和解密密钥。 最后，必须具有单个 <`machineKey`> 条目，以便将该条目粘贴到扩展部署中每个报表服务器实例的 Web.config 文件中。  
  
    下面的示例说明了您必须获得的值。 请不要将该示例复制到配置文件中，其中的密钥值是无效的。  
  
    ```xml
    <machineKey ValidationKey="123455555" DecryptionKey="678999999" Validation="SHA1" Decryption="AES"/>  
    ```  
  
2. 打开报表服务器的 Web.config 文件，并在 <`system.web`> 部分粘贴所生成的 <`machineKey`> 元素。 默认情况下，报表管理器的 Web.config 文件位于 \Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\Reportserver\Web.config 中。  
  
3. 保存文件。  
  
4. 对扩展部署中的每个报表服务器重复上述步骤。  
  
5. 确保 \Reporting Services\Reportserver 文件夹中所有 Web.Config 文件的 <`system.web`> 部分都包含相同的 <`machineKey`> 元素。  

::: moniker-end

::: moniker range=">=sql-server-2017"

1. 通过使用由 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]提供的 autogenerate 功能，生成一个验证密钥和解密密钥。 最后，必须具有单个 \<**MachineKey**> 条目，以便将该条目粘贴到横向扩展部署中每个报表服务器实例的 RSReportServer.config 文件中。

    下面的示例说明了您必须获得的值。 请不要将该示例复制到配置文件中，其中的密钥值是无效的。 报表服务器需要正确的大小写。

    ```xml
    <MachineKey ValidationKey="123455555" DecryptionKey="678999999" Validation="SHA1" Decryption="AES"/>
    ```

2. 保存文件。

3. 对扩展部署中的每个报表服务器重复上述步骤。  

4. 确保 \Reporting Services\Report Server 文件夹中所有 RSReportServer.Config 文件都包含相同的 \<**MachineKey**> 元素。

::: moniker-end

## <a name="how-to-configure-hostname-and-urlroot"></a><a name="SpecifyingVirtualServerName"></a> 如何配置 Hostname 和 UrlRoot

 若要在 NLB 群集上配置报表服务器扩展部署，必须定义单个虚拟服务器名称，以提供服务器群集的单访问点。 然后向您的环境中的域名服务器 (DNS) 注册此虚拟服务器名称。  
  
 在定义虚拟服务器名称之后，可以在 RSReportServer.config 文件中配置 **Hostname** 和 **UrlRoot** 属性，以在报表服务器 URL 中包括虚拟服务器名称。  
  
 在报表环境中使用通配符 URL 预留时，配置 **Hostname** 属性。 将 **Hostname** 属性指定为 NLB 服务器的虚拟服务器名称时，报表环境的网络流量将定向到 NLB 服务器。 然后，NLB 将在报表服务器节点之间分发请求。  
  
 此外，配置 **UrlRoot** 属性，以便报表链接处理的是已导出为静态报表的报表（例如 Excel 或 PDF 格式）或由订阅（例如电子邮件订阅）生成的报表。  
  
 如果将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 与 [!INCLUDE[winSPServ](../../includes/winspserv-md.md)] 3.0 或 [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007 集成到一起，或者在自定义 Web 应用程序中承载报表，则可能需要仅配置 **UrlRoot** 属性。 在这种情况下，将 **UrlRoot** 属性配置为 SharePoint 站点或 Web 应用程序的 URL。 这会将报表环境的网络流量定位到处理报表的应用程序，而不是报表服务器或 NLB 群集。  
  
 请不要修改 **ReportServerUrl**。 如果修改了此 URL，那么，在每次处理内部请求时，都将额外引入一个通过虚拟服务器的往返过程。 有关详细信息，请参阅[配置文件中的 URL（报表服务器配置管理器）](../../reporting-services/install-windows/urls-in-configuration-files-ssrs-configuration-manager.md)。 有关编辑配置文件的详细信息，请参阅[修改 Reporting Services 配置文件 &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)。  
  
1. 在文本编辑器中打开 RSReportServer.config。  
  
2. 查找 \<Service> 部分，并将以下信息添加到配置文件，将 Hostname 值替换为 NLB 服务器的虚拟服务器名 ：  
  
    ```xml
    <Hostname>virtual_server</Hostname>  
    ```  
  
3. 查找 **UrlRoot**。 此元素未在该配置文件中指定，但是所使用的默认值是采用 https:// 或 `https://<computername>/<reportserver>` 格式的 URL，其中 \<*reportserver*> 是报表服务器 Web 服务的虚拟目录名称。  
  
4. 为“UrlRoot”键入一个包括群集虚拟名称的值，其格式为： https:// 或 `https://<virtual_server>/<reportserver>`。  
  
5. 保存文件。  
  
6. 对于扩展部署中的每个报表服务器，在其相应的 RSReportServer.config 文件中重复上述步骤。  
  
## <a name="verify-report-server-access"></a><a name="Verify"></a> 验证报表服务器访问权限

 验证可通过虚拟服务器名称（例如，`https://MyVirtualServerName/reportserver` 和 `https://MyVirtualServerName/reports`）访问横向扩展部署。  
  
 通过查看报表服务器日志文件或检查 RS 执行日志（该执行日志表包含的 **InstanceName** 列可以显示处理特定请求的实例名称），可以检查哪个节点实际处理报表。 有关详细信息，请参阅 [Reporting Services 日志文件和源](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)。  
  
 如果不能连接到报表服务器上，请检查 NLB 以确保请求发送到报表服务器，并查看报表服务器 HTTP 日志，以确保服务器接收请求。  
  
### <a name="troubleshooting-failed-requests"></a>对失败的请求进行疑难解答

 如果请求未到达报表服务器实例，请检查 RSReportServer.config 文件，验证虚拟服务器名称是否指定为报表服务器 URL 的主机名：  
  
1. 在文本编辑器中打开 RSReportServer.config 文件。  
  
2. 查找 \<**Hostname**>、\<**ReportServerUrl**> 以及 \<**UrlRoot**>，并检查每个设置的主机名。 如果该值不是预期的主机名，请将其替换为正确的主机名。  
  
 如果在做出以下更改后启动 Reporting Services 配置工具，则该工具可能会将 \<**ReportServerUrl**> 设置更改为默认值。 请始终保留一份配置文件的备份副本，以备需要用包含要使用的设置的版本替换该配置文件时使用。  
  
## <a name="see-also"></a>另请参阅

 [配置 URL（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
 [配置本机模式报表服务器横向扩展部署（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)   
 [报表服务器配置管理器（本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [管理 Reporting Services 本机模式报表服务器](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)
