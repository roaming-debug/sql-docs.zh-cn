---
title: SSRS 本机模式下的电子邮件设置（配置管理器）| Microsoft Docs
description: SQL Server Reporting Services 包括电子邮件传递扩展插件，便于用户通过电子邮件分发报表。
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
f1_keywords:
- SQL13.rsconfigtool.emailsettings.F1
helpviewer_keywords:
- SQL11.rsconfigtool.emailsettings.F1
ms.assetid: cdad1529-bfa6-41fb-9863-d9ff1b802577
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 31ab6390286c748d8f9cb8f6b98b80c9c53818a1
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596927"
---
# <a name="e-mail-settings---reporting-services-native-mode-configuration-manager"></a>电子邮件设置 - Reporting Services 本机模式（配置管理器）
SQL Server Reporting Services 包括电子邮件传递扩展插件，便于用户通过电子邮件分发报表。 根据定义电子邮件订阅的方式，传递可能由通知、链接、附件或嵌入报表组成。 电子邮件传递扩展插件可与现有的邮件服务器技术一起使用。 邮件服务器必须是 SMTP 服务器或转发器。 报表服务器通过操作系统提供的协作数据对象 (CDO) 库 (cdosys.dll) 连接到 SMTP 服务器。

默认情况下，未配置报表服务器电子邮件传递扩展插件。 必须使用报表服务器配置管理器对此扩展插件进行最低配置。 若要设置高级属性，必须编辑 RSReportServer.config 文件。 如果无法将报表服务器配置为使用此扩展插件，则可以将报表传递到共享文件夹。 有关详细信息，请参阅 Reporting Services 中的文件共享传递。

## <a name="configuration-requirements"></a>配置要求

- 报表服务器电子邮件传递在协作数据对象 (CDO) 上实现，且需要本地或远程的简单邮件传输协议 (SMTP) 服务器或 SMTP 转发器。 所有 Windows 操作系统都不支持 SMTP。 如果使用的是基于 Itanium 的 Windows Server 2008 版本，则也不支持 SMTP。 有关通过 CDO 提供的配置选项的详细信息，请参阅 MSDN 上的 [Configuration CoClass](/previous-versions/exchange-server/exchange-10/ms526939(v=exchg.10)) （配置 CoClass）。

配置的身份验证帐户必须对 SMTP 服务器拥有权限才能发送邮件。

- 电子邮件传递扩展插件在电子邮件附件中使用 UTF-8 编码。 您无法修改此编码；HTML 呈现扩展插件仅支持 UTF-8。

> [!NOTE] 
> 默认电子邮件传递扩展插件不支持给待发邮件进行数字签名或加密。

## <a name="setting-configuration-options-for-e-mail-delivery"></a>为电子邮件传递设置配置选项
必须先设置确定使用哪一个 SMTP 服务器的配置值，才能使用报表服务器电子邮件传递。

若要针对电子邮件传递配置报表服务器，请执行下列操作：

- 如果要仅指定一个 SMTP 服务器和一个具有发送电子邮件权限的用户帐户，请使用报表服务器配置管理器。 以下是配置报表服务器电子邮件传递扩展插件所需的最低设置。

- （可选）使用文本编辑器在 RSreportserver.config 文件中指定其他设置。 此文件包含报表服务器电子邮件传递的所有配置设置。 如果要使用本地 SMTP 服务器或将电子邮件限定传递到特定主机，则需要在这些文件中指定其他设置。 要详细了解如何查找和修改配置文件，请参阅[修改 Reporting Services 配置文件 (RSreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)。

> [!NOTE] 
> 报表服务器电子邮件设置都是基于 CDO。 若要了解有关特定设置的更多详细信息，可以参考 CDO 产品文档。

## <a name="configure-report-server-e-mail-using-the-report-server-configuration-manager"></a><a name="rsconfigman"/>使用报表服务器配置管理器配置报表服务器电子邮件

1. 启动报表服务器配置管理器，然后连接到报表服务器实例。

2. 在“发件人地址”中，输入要在所生成电子邮件的“发件人：”字段中使用的电子邮件地址   。 

     您必须指定一个有权从 SMTP 服务器中发送邮件的用户帐户。 为“发件人地址”键入的值保存在 rsreportserver.config 文件中的 `<From>` 字段中  。  

3.  在 **SMTP Server** 中，指定要使用的 SMTP 服务器或网关。 

     此值可以是 IP 地址、企业 Intranet 上计算机的 NetBIOS 名称或者完全限定域名。 键入的 **SMTP Server** 的值保存在 rsreportserver.config 文件中的 `<SMTPServer>` 字段中。

4. 使用“身份验证”下拉列表来指定如何对 SMTP 服务器进行身份验证  。 此 

     - **不进行身份验证** 意味着你将以匿名方式连接到指定的邮件服务器。
     
          选择此选项将在 rsreportserver.config 中设置 `<SendUsing>` 的值为 **2** 和 `<SMTPAuthenticate>` 的值为 **0** 。
     
     - **用户名和密码（基本）** 允许你指定用户名和密码以连接到邮件服务器。 还可以选择“使用安全连接”将此连接通过加密连接转至你的邮件服务器  。
     
          选择此选项将在 rsreportserver.config 中设置 `<SendUsing>` 的值为 **2** 和 `<SMTPAuthenticate>` 的值为 **1** 。选择“使用安全连接”会将 `SMTPUseSSL` 设置为“True”   。 将在 `<SendUserName>` 中将“用户名”设置为一个加密值  。 将在 `<SendPassword>` 中将“密码”设置为一个加密值  。
     
     - **报表服务器服务帐户 (NTLM)** 将使用你为报表服务器指定的服务帐户。 如果使用报表服务器服务帐户进行身份验证，请验证该服务帐户在 SMTP 服务器上是否具有“发送方式”  权限。
     
          选择此选项将在 rsreportserver.config 中设置 `<SendUsing>` 的值为 **2** 和 `<SMTPAuthenticate>` 的值为 **2** 。

5. 选择“应用”。 

6. 你可以在 rsreportserver.config 中对电子邮件配置选择性地调整附加字段。

## <a name="example-report-server-e-mail-configuration"></a>报表服务器电子邮件配置示例
下面的示例说明了远程 SMTP 服务器的 RSreportserver.config 文件中的设置： 要了解设置说明及有效值，请参阅 [Rsreportserver.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)。

```
<RSEmailDPConfiguration>
     <SMTPServer>mySMTPServer.Adventure-Works.com</SMTPServer>
     <SMTPServerPort></SMTPServerPort>
     <SMTPAccountName></SMTPAccountName>
     <SMTPConnectionTimeout></SMTPConnectionTimeout>
     <SMTPServerPickupDirectory></SMTPServerPickupDirectory>
     <SMTPUseSSL>False</SMTPUseSSL>
     <SendUsing>2</SendUsing>
     <SMTPAuthenticate>2</SMTPAuthenticate>
     <From>my-rs-email-account@Adventure-Works.com</From>
     <EmbeddedRenderFormats>
          <RenderingExtension>MHTML</RenderingExtension>
     </EmbeddedRenderFormats>
     <PrivilegedUserRenderFormats></PrivilegedUserRenderFormats>
     <ExcludedRenderFormats>
          <RenderingExtension>HTMLOWC</RenderingExtension>
          <RenderingExtension>NULL</RenderingExtension>
          <RenderingExtension>RGDI</RenderingExtension>
     </ExcludedRenderFormats>
     <SendEmailToUserAlias>True</SendEmailToUserAlias>
     <DefaultHostName></DefaultHostName>
     <PermittedHosts>
          <HostName>Adventure-Works.com</HostName>
          <HostName>hotmail.com</HostName>
     </PermittedHosts>
     <SendUserName></SendUserName>
     <SendPassword></SendPassword>
</RSEmailDPConfiguration>
```
## <a name="configuration-options-for-setting-the-to-field-in-a-message"></a>用于设置邮件中的“收件人:”字段的配置选项
根据“管理单独的订阅”任务授予的权限而创建的用户定义订阅包含基于域用户帐户的预设用户名。 用户创建订阅时，“收件人:”  字段中的收件人姓名会使用创建该订阅的人员的域用户帐户自行转换为地址。

如果您所用的 SMTP 服务器或转发器使用了不同于域用户帐户的电子邮件帐户，则 SMTP 服务器尝试将报表传递给该用户时，报表传递会失败。

若要解决该问题，可以修改允许用户在“收件人:”  字段中输入名称的配置设置：

1. 使用文本编辑器打开 RSReportServer.config。

2. 将 `<SendEmailToUserAlias>` 设置为 **False**。

3. 将 `<DefaultHostName>` 设置为 SMTP 服务器或转发器的域名系统 (DNS) 名称或 IP 地址。

4. 保存文件。

## <a name="configuration-options-for-remote-smtp-service"></a>远程 SMTP 服务的配置选项
报表服务器与 SMTP 服务器或转发器之间的连接是由下列配置设置决定的：

- `<SendUsing>` 指定发送消息的方法。 您可以选择网络 SMTP 服务或本地 SMTP 服务拾取目录。 若要使用远程 SMTP 服务，必须在 RSReportServer.config 文件中将此值设置为 **2** 。
- `<SMTPServer>` 指定远程 SMTP 服务器或转发器。 如果使用远程 SMTP 服务器或转发器，则必须指定此值。
- `<From>` 设置显示在电子邮件的“发件人：”行中的值  。 如果使用远程 SMTP 服务器或转发器，则必须指定此值。

其他用于远程 SMTP 服务的值包括以下值（请注意，除非您要覆盖默认值，否则无需指定这些值）：

- `<SMTPServerPort>` 默认情况下，为端口 25 进行配置。
- `<SMTPAuthenticate>` 指定如何将报表服务器连接到远程 SMTP 服务器。 默认值为 **0** （或不进行身份验证）。 这种情况下，将通过匿名访问创建连接。 报表服务器和 SMTP 服务器可能需要成为同一域的成员，这取决于域配置。
- 若要向受限制的通讯组列表发送电子邮件（例如，只接受经过身份验证的帐户发来的邮件的通讯组列表），则将 `<SMTPAuthenticate>` 设置为 **1** 或 **2**。 如果设置为 **1**，你将还需要设置 `<SendUserName>` 和 `<SendPassword>`。 建议通过报表服务器配置管理器执行此操作，因为它将加密 `<SendUserName>` 和 `<SendPassword>` 的值。

### <a name="to-configure-a-remote-smtp-service-for-the-report-server"></a>配置报表服务器的远程 SMTP 服务

> [!NOTE] 
> 建议通过报表服务器配置管理器配置邮件服务器。

1. 请验证报表服务器 Windows 服务是否对 SMTP 服务器拥有 **Send As** 权限。

2. 在文本编辑器中打开 RSReportServer.config 文件。

3. 请验证是否将 `<UrlRoot>` 设置为报表服务器 URL 地址。 此值是在您配置报表服务器时设置的，应该已经填写。 如果未设置此值，则请键入报表服务器 URL 地址。

4. 在“传递”部分中，查找 `<RSEmailDPConfiguration>`。
     
5. 在 `<SMTPServer>`中，键入 SMTP 服务器的名称。 此值可以是 IP 地址、企业 Intranet 上计算机的 UNC 名称或者完全限定域名。

6. 将 `<SendUsing>` 的值设置为 **2** 以使用报表服务器的服务帐户。 将 `<SendUsing>` 的值设置为 **1** 以进行基本身份验证。 如果设置为 **1**，将需要另外提供 `<SendUserName>` 和 `<SendPassword>`的值。 如果想要加密这些值，请在报表服务器配置管理器中设置身份验证。

7. 如果设置 `<SMTPAuthenticate>` 的值为 1 或 2，则将 **的值设置为** 1 `<SendUsing>` 。

7. 设置 `<From>`。 您必须指定一个有权从 SMTP 服务器中发送邮件的用户帐户。

8. 保存文件。

     报表服务器将自动使用新的设置；不需要重新启动该服务。 您可以指定其他 SMTP 设置，以进一步配置如何将 SMTP 服务器用于报表服务器电子邮件传递。

## <a name="configuration-options-for-local-smtp-service"></a>本地 SMTP 服务的配置选项
若要测试报表服务器电子邮件传递或解决所遇到的疑难问题，则配置本地 SMTP 服务会很有用。 默认情况下，不启用本地 SMTP 服务。

报表服务器与本地 SMTP 服务器或转发器之间的连接是由下列配置设置决定的：

- 将 **SendUsing** 设置为 **1**。
- 将 **SMTPServerPickupDirectory** 设置为本地驱动器中的文件夹。

  > [!NOTE] 
  > 如果正在使用本地 SMTP 服务器，请确保不要设置 SMTPServer。

- **“发件人”** 用于设置显示在电子邮件的 **“发件人:”** 行中的值。 此值是必需的。

### <a name="to-configure-a-local-smtp-service-for-the-report-server"></a>配置报表服务器的本地 SMTP 服务

1. 在控制面板中，选择“打开或关闭 Windows 功能”以启动“添加角色和功能向导”   。

2. 选择“基于角色或基于功能的安装”，然后选择“下一步”   。

3. 选择要在上面安装 Internet Information Server (IIS) 的服务器，然后选择“下一步”  。

4. 在“服务器角色”页上选择“下一步”   。
     
5. 在“功能”页上，选择“SMTP 服务器”，然后选择“下一步”    。

     如果系统提示你添加 SMTP 服务器所需的功能，请选择“添加功能”  。

6. 在“Web 服务器角色 (IIS)”页上，选择“下一步”   。

7. 在“角色服务”页上，选择“下一步”   。

8. 在“确认”页上，选择“安装”   。

9. 验证“简单邮件传输协议 (SMTP)”windows 服务是否正在服务控制台中运行  。

     若要配置本地 SMTP 服务器，你将需要使用管理工具下的 IIS 6.0 管理器。

10. 在文本编辑器中打开 RSReportServer.config 文件。

11. 请验证是否将 `<UrlRoot>` 设置为报表服务器 URL 地址。 此值是在您配置报表服务器时设置的，应该已经填写。 如果未设置，请键入你的报表服务器的 Web 服务 URL 地址。

12. 在“传递”部分中，查找 `<RSEmailDPConfiguration>`。
     
13. 请确保 `<SMTPServer>` 存在，但为空。
     
14. 将 `<SendUsing>` 设置为 1。
     
14. 将 `<SMTPAuthenticate>` 设置为 0。
     
15. 将 `<SMTPServerPickupDirectory>` 设置到 SMTP 服务拾取文件夹。
     
     默认位置将为 C:\inetpub\mailroot\Pickup  。
     
16. 设置 `<From>`。 这将设置电子邮件的“发件人：”行中显示的值  。
     
17. 保存文件。
  
## <a name="see-also"></a>另请参阅  
[报表服务器配置管理器（本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
[Modify a Reporting Services Configuration File (rsreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)  
[Rsreportserver.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)
  
