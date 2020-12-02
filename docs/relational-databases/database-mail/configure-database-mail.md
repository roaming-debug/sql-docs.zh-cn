---
description: 配置数据库邮件
title: 配置数据库邮件 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlimail.profileandaccountmanagement.f1
- sql13.swb.sqlimail.newaccount.f1
- sql13.swb.dbmail. manageprofilesecurity.profileview.f1
- sql13.swb.sqlimail.manageexistingprofile.f1
- sql13.swb.sqlimail.addaccounttoprofile.f1
- sql13.swb.dbmail.manageexistingaccount.f1
- sql13.swb.sqlimail.manageprofilesecurity.profileview.f1
- sql13.swb.sqlimail.welcome.f1
- sql13.swb.sqlimail.manageprofilesecurity.principalview.f1
- sql13.swb.sqlimail.newsqlimailaccount.f1
- sql13.swb.sqlimail.selectconfiguration.f1
- sql13.swb.dbmail.completewizard.f1
- sql13.swb.dbmail.sendtestemail.test.f1
- sql13.swb.sqlimail.newprofile.f1
- sql13.swb.dbmail.addaccounttoprofile.f1
- sql13.swb.dbmail.newprofile.f1
- sql13.swb.sqlimail.manageexistingaccount.f1
- sql13.swb.dbmail.welcome.f1
- sql13.swb.dbmail.newaccount.f1
- sql13.swb.dbmail.profileandaccountmanagement.f1
- sql13.swb.dbmail.selectconfiguration.f1
- sql13.swb.dbmail.sendtestemail.f1
- sql13.swb.sqlimail.completewizard.f1
- sql13.swb.dbmail.configuresystem.f1
- sql13.swb.sqlimail.configuresystem.f1
- sql13.swb.dbmail.newsqlimailaccount.f1
- sql13.swb.dbmail.manageexistingprofile.f1
- sql13.swb.dbmail.manageprofilesecurity.principalview.f1
ms.assetid: 7edc21d4-ccf3-42a9-84c0-3f70333efce6
author: stevestein
ms.author: sstein
ms.openlocfilehash: daf9915e1aff8cf800fbc68929e7efdd6f95f4a2
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126919"
---
# <a name="configure-database-mail"></a>配置数据库邮件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用数据库邮件配置向导启用和配置数据库邮件，以及使用模板创建数据库邮件配置脚本。  
  
-   **开始之前：** [限制和局限](#Restrictions)、[安全性](#Security)  
  
-   **To configure Database Mail, using:**  [Database Mail Configuration Wizard](#DBWizard), [Using Templates](#Template)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 使用 **DatabaseMail XPs** 选项可以在此服务器上启用数据库邮件。 有关详细信息，请参阅主题 [Database Mail XPs Server 配置选项](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) 。  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 在任何数据库中启用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Broker 都需要数据库锁。 如果在 **msdb** 中停用了 Service Broker，则若要启用数据库邮件，应首先停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理，以使 Service Broker 可以获取所需的锁。  
  
###  <a name="security"></a><a name="Security"></a> Security  
 要配置数据库邮件，您必须是 **sysadmin** 固定服务器角色的成员。 若要发送数据库邮件，您必须是 **msdb** 数据库中的 **DatabaseMailUserRole** 数据库角色的成员。  
  
##  <a name="using-database-mail-configuration-wizard"></a><a name="DBWizard"></a> 使用数据库邮件配置向导  
 **使用向导配置数据库邮件**  
  
1.  在对象资源管理器中，展开要配置数据库邮件的实例所在节点。  
  
2.  展开 **“管理”** 节点。  
  
3.  右键单击“数据库邮件”然后单击“配置数据库邮件”。  
  
4.  完成向导对话框  

    -   [“欢迎”页](#Welcome)  
  
    -   [“选择配置任务”页](#ConfigTask)  
  
    -   [“新建帐户”页](#NewAccount)  
  
    -   [“管理现有帐户”页](#ExistingAccount)  
  
    -   [“新建配置文件”页](#NewProfile)  
  
    -   [“管理现有配置文件”页](#ExistingProfile)  
  
    -   [“将帐户添加到配置文件”页](#AddAccount)  
  
    -   [“管理帐户和配置文件”页](#AccountsProfiles)  
  
    -   [“管理配置文件安全性”，“公共”选项卡](#ProfileSecurityPublic)  
  
    -   [“管理配置文件安全性”，“专用”选项卡](#ProfileSecurityPrivate)  
  
    -   [“配置系统参数”页](#SystemParameters)  
  
    -   [“完成向导”页](#CompleteWizard)  
  
    -   [“发送测试电子邮件”页](#TestEmail)  
  
###  <a name="welcome-page"></a><a name="Welcome"></a> “欢迎”页  
 此页说明配置数据库邮件的步骤。  
  
 **不再显示此页** - 选中它，可在以后的显示中跳过此“欢迎”页。  
  
 **下一步** - 继续到“选择配置任务”页。  
  
 **取消** - 终止向导而无需配置数据库邮件  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="select-configuration-task"></a><a name="ConfigTask"></a> 选择配置任务  
 使用 **“选择配置任务”** 页可以指示每次使用此向导时要完成的任务。 如果您在完成向导前改变了主意，请使用 **“上一步”** 按钮返回此页并选择其他任务。  
  
> [!NOTE]  
>  如果数据库邮件尚未启用，您将收到以下消息： **“数据库邮件功能不可用。是否要启用此功能？** 回答“是”相当于使用 **sp_configure** 系统存储过程的 [Mail XPs 选项](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md)启用数据库邮件。  
  
 **通过执行以下任务来设置数据库邮件**  
 执行第一次设置数据库邮件所需的所有任务。 此选项包含所有其他三个选项。  
  
 **管理数据库邮件帐户和配置文件**  
 创建新的数据库邮件帐户和配置文件，或者查看、更改或删除现有数据库邮件帐户和配置文件。  
  
 **管理配置文件安全性**  
 配置对数据库邮件配置文件具有访问权限的用户。  
  
 **查看或更改系统参数**  
 配置数据库邮件系统参数，例如附件的最大文件大小。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="new-account-page"></a><a name="NewAccount"></a> “新建帐户”页  
 使用此页可以创建新的数据库邮件帐户。 数据库邮件帐户包含向 SMTP 服务器发送电子邮件所需的信息。  
  
 数据库邮件帐户包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 向 SMTP 服务器发送电子邮件所需的信息。 每个帐户均包含一个电子邮件服务器的信息。  
  
 数据库邮件帐户仅用于数据库邮件。 数据库邮件帐户与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户或 Microsoft Windows 帐户之间不相互对应。 可以利用 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的凭据或您提供的其他凭据发送数据库邮件，也可以用匿名方式发送。 使用基本身份验证时，数据库邮件帐户中的用户名和密码仅用于在电子邮件服务器中进行身份验证。 帐户无需与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户或运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的计算机上的用户对应。  
  
 **帐户名称**  
 键入新帐户的名称。  
  
 **说明**  
 键入帐户的说明。 说明是可选的。  
  
 **电子邮件地址**  
 键入帐户电子邮件地址的名称。 这是发送电子邮件的电子邮件地址。 例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的帐户可能会通过地址 SqlAgent@Adventure-Works.com 发送电子邮件。  
  
 **显示名称**  
 键入由此帐户发送的电子邮件上显示的名称。 显示名称为可选项。 这是由此帐户发送的邮件上显示的名称。 例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的帐户可能会在电子邮件上显示名称“SQL Server 代理自动发件人”。  
  
 **答复电子邮件**  
 键入电子邮件地址，该地址是答复由此帐户发送的电子邮件所用到的地址。 答复电子邮件为可选项。 例如，给 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的帐户的回信可能会发送给数据库管理员 danw@Adventure-Works.com。  
  
 服务器名称  
 键入此帐户发送电子邮件所用的 SMTP 服务器的名称或 IP 地址。 通常此格式类似于 **smtp.** _<your_company>_ **.com**。 如需相关帮助，请询问您的邮件管理员。  
  
 **端口号**  
 键入此帐户的 SMTP 服务器的端口号。 大多数 SMTP 服务器使用端口 25。  
  
 **此服务器要求安全连接(SSL)**  
 使用安全套接字层加密通信。  
  
 **使用数据库引擎服务凭据的 Windows 身份验证**  
 使用为 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 服务配置的凭据生成指向 SMTP 服务器的连接。  
  
 **基本身份验证**  
 指定 SMTP 服务器要求的用户名和密码。  
  
 **用户名**  
 键入数据库邮件登录 SMTP 服务器所用的用户名。 如果 SMTP 服务器要求基本身份验证，则需要提供用户名。  
  
 **密码**  
 键入数据库邮件登录 SMTP 服务器所用的密码。 如果 SMTP 服务器要求基本身份验证，则需要提供密码。  
  
 **确认密码**  
 再次输入密码以进行确认。 如果 SMTP 服务器要求基本身份验证，则需要提供密码。  
  
 **匿名身份验证**  
 在没有登录凭据的情况下，将邮件发送至 SMTP 服务器。 在 SMTP 服务器不要求进行身份验证时可以使用此选项。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="manage-existing-account-page"></a><a name="ExistingAccount"></a> “管理现有帐户”页  
 使用此页可以管理现有数据库邮件帐户。  
  
 **帐户名称**  
 选择要查看、更新或删除的帐户。  
  
 **删除**  
 删除选定的帐户。 必须从关联的配置文件中删除此帐户，或者删除此类配置文件，才能够删除所选帐户。  
  
 **说明**  
 查看或更新帐户的说明。 说明是可选的。  
  
 **电子邮件地址**  
 查看或更新帐户电子邮件地址的名称。 这是发送电子邮件的电子邮件地址。 例如，Microsoft SQL Server 代理的帐户可能会通过地址“SqlAgent\@Adventure-Works.com”发送电子邮件。  
  
 **显示名称**  
 查看或更新由此帐户发送的电子邮件上显示的名称。 显示名称为可选项。 这是由此帐户发送的邮件上显示的名称。 例如，SQL Server 代理的帐户可能会在电子邮件上显示名称 **SQL Server Agent Automated Mailer** 。  
  
 **答复电子邮件**  
 查看或更新电子邮件地址，该地址是答复由此帐户发送的电子邮件所用到的地址。 答复电子邮件为可选项。 例如，给 SQL Server 代理的帐户的回信可能会发送给数据库管理员“danw\@Adventure-Works.com”。  
  
 服务器名称  
 查看或更新该帐户发送电子邮件所用的 SMTP 服务器的名称。 通常此格式类似于 **smtp.<your_company>.com**。 如需相关帮助，请询问您的邮件管理员。  
  
 **端口号**  
 查看或更新此帐户的 SMTP 服务器的端口号。 大多数 SMTP 服务器使用端口 25。  
  
 **此服务器要求安全连接(SSL)**  
 使用安全套接字层加密通信。  
  
 **使用数据库引擎服务凭据的 Windows 身份验证**  
 使用为 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 服务配置的凭据生成指向 SMTP 服务器的连接。  
  
 **基本身份验证**  
 指定 SMTP 服务器要求的用户名和密码。  
  
 **用户名**  
 查看或更新数据库邮件登录 SMTP 服务器所用的用户名。 如果 SMTP 服务器要求基本身份验证，则需要提供用户名。  
  
 **密码**  
 更改数据库邮件登录 SMTP 服务器所用的密码。 如果 SMTP 服务器要求基本身份验证，则需要提供密码。  
  
 **确认密码**  
 再次输入密码以进行确认。 如果 SMTP 服务器要求基本身份验证，则需要提供密码。  
  
 **匿名身份验证**  
 在没有登录凭据的情况下，将邮件发送至 SMTP 服务器。 在 SMTP 服务器不要求进行身份验证时可以使用此选项。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="new-profile-page"></a><a name="NewProfile"></a> “新建配置文件”页  
 使用此页可以创建数据库邮件配置文件。 数据库邮件配置文件是数据库邮件帐户的集合。 在无法访问电子邮件服务器时，配置文件通过提供其他的数据库邮件帐户来提高可靠性。 至少需要一个数据库邮件帐户。 有关在配置文件中设置数据库邮件帐户的优先级的详细信息，请参阅 [Create a Database Mail Profile](../../relational-databases/database-mail/create-a-database-mail-profile.md)。  
  
 使用 **“上移”** 和 **“下移”** 按钮可以更改数据库邮件帐户的使用顺序。 此顺序由一个名为序列号的值来确定。 **“上移”** 减小序列号， **“下移”** 增大序列号。 序列号可以确定数据库邮件使用配置文件中帐户的顺序。 对于新的电子邮件，数据库邮件将从序列号最小的帐户开始。 如果该帐户失败，数据库邮件就使用具有下一个序列号较大的帐户，依此类推，直到数据库邮件成功发送邮件，或者序列号最大的帐户也失败为止。 如果具有最高序列号的帐户失败，则数据库邮件将在一段时间内（该时间在数据库邮件的 **AccountRetryDelay** 参数中配置）暂停发送邮件，之后从最低序列号开始重新尝试发送邮件。 使用数据库邮件的 **AccountRetryAttempts** 参数，可以配置外部邮件进程使用指定配置文件中的每个帐户尝试发送电子邮件的次数。 可以在数据库邮件配置向导的 **“配置系统参数”** 页上配置 **AccountRetryDelay** 和 **AccountRetryAttempts** 参数。  
  
 **配置文件名称**  
 键入新配置文件的名称。 将使用此名称创建配置文件。 不要使用现有配置文件的名称。  
  
 **说明**  
 键入配置文件的说明。 说明是可选的。  
  
 **SMTP 帐户**  
 为配置文件选择一个或多个帐户。 优先级设置数据库邮件使用帐户的顺序。 如果没有列出任何帐户，必须单击 **“添加”** 继续，并添加新的 SMTP 帐户。  
  
 **添加**  
 向配置文件中添加帐户。  
  
 **删除**  
 从配置文件中删除所选帐户。  
  
 **上移**  
 提高选定帐户的优先级。  
  
 **“下移”**  
 降低选定帐户的优先级。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="manage-existing-profile-page"></a><a name="ExistingProfile"></a> “管理现有配置文件”页  
 使用此页可以管理现有的数据库邮件配置文件。 数据库邮件配置文件是数据库邮件帐户的集合。 在无法访问电子邮件服务器时，配置文件通过提供其他的数据库邮件帐户来提高可靠性。 至少需要一个数据库邮件帐户。 有关在配置文件中设置数据库邮件帐户的优先级的详细信息，请参阅 [Create a Database Mail Profile](../../relational-databases/database-mail/create-a-database-mail-profile.md)。  
  
 使用 **“上移”** 和 **“下移”** 按钮可以更改数据库邮件帐户的使用顺序。 此顺序由一个名为序列号的值来确定。 **“上移”** 减小序列号， **“下移”** 增大序列号。 序列号可以确定数据库邮件使用配置文件中帐户的顺序。 对于新的电子邮件，数据库邮件将从序列号最小的帐户开始。 如果该帐户失败，数据库邮件就使用具有下一个序列号较大的帐户，依此类推，直到数据库邮件成功发送邮件，或者序列号最大的帐户也失败为止。 如果具有最高序列号的帐户失败，则数据库邮件将在一段时间内（该时间在数据库邮件的 **AccountRetryDelay** 参数中配置）暂停发送邮件，之后从最低序列号开始重新尝试发送邮件。 使用数据库邮件的 **AccountRetryAttempts** 参数，可以配置外部邮件进程使用指定配置文件中的每个帐户尝试发送电子邮件的次数。 可以在数据库邮件配置向导的 **“配置系统参数”** 页上配置 **AccountRetryDelay** 和 **AccountRetryAttempts** 参数。  
  
 **配置文件名称**  
 选择要管理的配置文件的名称。  
  
 **删除**  
 删除所选配置文件。 系统将提示您选择 **“是”** 以删除所选配置文件并舍弃任何未发送的邮件，或者选择 **“否”** 以仅在没有未发送邮件时删除所选配置文件。  
  
 **说明**  
 查看或更改所选配置文件的说明。 说明是可选的。  
  
 **SMTP 帐户**  
 为配置文件选择一个或多个帐户。 故障转移优先级设置了在进行故障转移时数据库邮件使用帐户的顺序。  
  
 **添加**  
 向配置文件中添加帐户。  
  
 **删除**  
 从配置文件中删除所选帐户。  
  
 **上移**  
 提高所选帐户的故障转移优先级。  
  
 **“下移”**  
 降低所选帐户的故障转移优先级。  
  
 **Priority**  
 查看帐户的当前故障转移优先级。  
  
 **帐户名称**  
 查看帐户的名称。  
  
 **E-mail Address**  
 查看帐户的电子邮件地址。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="add-account-to-profile-page"></a><a name="AddAccount"></a> Add Account to Profile Page  
 使用此页可选择要添加到配置文件的帐户。 请从 **“帐户名”** 框中选择现有帐户，或单击 **“新建帐户”**。  
  
 **帐户名称**  
 选择要添加到配置文件的帐户的名称。  
  
 **电子邮件地址**  
 查看所选帐户的电子邮件地址。 您不能在此页中更改电子邮件地址。 若要更改该帐户的电子邮件地址，请返回到该向导的主页，再选择“管理数据库邮件帐户和配置文件”选项。  
  
 服务器名称  
 查看所选帐户的邮件服务器名称。 您不能在此页中更改服务器名称。 若要更改该帐户的服务器名称，请返回到该向导的主页，再选择 **“管理数据库邮件帐户和配置文件”** 选项。  
  
 **“新建帐户”**  
 创建新帐户。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="manage-accounts-and-profiles-page"></a><a name="AccountsProfiles"></a> “管理帐户和配置文件”页  
 使用此页可选择用于管理配置文件或帐户的任务。  
  
 **创建新帐户**  
 创建新帐户。  
  
 **查看、更改或删除现有帐户**  
 管理或删除现有帐户。  
  
 **创建新配置文件**  
 创建新的配置文件。  
  
 **查看、更改或删除现有配置文件。还可以管理与该配置文件关联的帐户。**  
 更新或删除现有配置文件。 使用此选项，您还可以管理与该配置文件关联的帐户。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="manage-profile-security-public-tab"></a><a name="ProfileSecurityPublic"></a> “管理配置文件安全性”，“公共”选项卡  
 使用此页可配置公共配置文件。  
  
 配置文件可以为公共配置文件或专用配置文件。 只有特定用户或角色才能访问专用配置文件。 公共配置文件允许所有用户或角色访问邮件主机数据库 (**msdb**)，以使用该配置文件发送电子邮件。  
  
 配置文件可以是默认的配置文件。 在这种情况下，用户或角色可以使用该配置文件发送电子邮件，而无需显式指定配置文件。 如果发送电子邮件的用户或角色具有默认的专用配置文件，则数据库邮件将使用该配置文件。 如果用户或角色没有默认的专用配置文件，则 **sp_send_dbmail** 将使用 **msdb** 数据库的默认公共配置文件。 如果用户或角色没有默认的专用配置文件，且该数据库也没有默认的公共配置文件，则 **sp_send_dbmail** 将返回错误。 仅可以将一个配置文件标记为默认配置文件。  
  
 **公共**  
 选择此选项可将指定的配置文件转为公共配置文件。  
  
 **配置文件名**  
 显示配置文件的名称。  
  
 **默认配置文件**  
 选择此选项可将指定的配置文件转为默认配置文件。  
  
 **仅显示现有的公共配置文件**  
 选择此选项将仅显示指定数据库中的公共配置文件。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="manage-profile-security-private-tab"></a><a name="ProfileSecurityPrivate"></a> “管理配置文件安全性”，“专用”选项卡  
 使用此页可配置专用配置文件。  
  
 配置文件可以为公共配置文件或专用配置文件。 只有特定用户或角色才能访问专用配置文件。 公共配置文件允许所有用户或角色访问邮件主机数据库 (**msdb**)，以使用该配置文件发送电子邮件。  
  
 配置文件可以是默认的配置文件。 在这种情况下，用户或角色可以使用该配置文件发送电子邮件，而无需显式指定配置文件。 如果发送电子邮件的用户或角色具有默认的专用配置文件，则数据库邮件将使用该配置文件。 如果用户或角色没有默认的专用配置文件，则 **sp_send_dbmail** 将使用 **msdb** 数据库的默认公共配置文件。 如果用户或角色没有默认的专用配置文件，且该数据库也没有默认的公共配置文件，则 **sp_send_dbmail** 将返回错误。  
  
 **用户名**  
 在 **msdb** 数据库中选择用户或角色的名称。  
  
 **访问**  
 选择该用户或角色是否可以访问指定的配置文件。  
  
 **配置文件名称**  
 查看配置文件的名称。  
  
 **是默认配置文件**  
 选择该配置文件是否为该用户或角色的默认配置文件。 每个用户或角色只能有一个默认配置文件。  
  
 **仅显示此用户的现有专用配置文件**  
 选择此选项将仅显示指定的用户或角色可以访问的配置文件。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="configure-system-parameters"></a><a name="SystemParameters"></a> AccountRetryAttempts  
 使用此页可以指定数据库邮件系统参数。 查看系统参数和每个参数的当前值。 选择某个参数可以在信息窗格中查看其简短说明。  
  
 **帐户重试次数**  
 外部电子邮件进程尝试使用指定配置文件中的每个帐户发送电子邮件的次数。  
  
 **帐户重试延迟时间(秒)**  
 在尝试使用配置文件中的所有帐户传递消息后，外部邮件进程在再次尝试使用所有帐户之前等待的时间（秒）。  
  
 **最大文件大小(字节)**  
 附件的最大大小（以字节为单位）。  
  
 **禁止的附件文件扩展名**  
 一组以逗号分隔的扩展名，具有这些扩展名的文件不能作为电子邮件附件发送。 单击浏览按钮 (**...**) 以添加其他扩展名。  
  
 **数据库邮件可执行文件的最短生存期(秒)**  
 外部邮件进程保持活动状态的最少时间（以秒为单位）。 只要数据库邮件队列中有电子邮件，该进程就会保持活动状态。 此参数指定了在没有要处理的消息时该进程保持活动状态的时间。  
  
 **日志记录级别**  
 指定数据库邮件日志中要记录的消息。 可能的值包括：  
  
-   普通 - 仅记录错误  
  
-   扩展 - 记录错误、警告和信息性消息  
  
-   详细 - 记录错误、警告、信息性消息、成功消息和其他内部消息。 使用详细日志记录可进行故障排除。  
  
 默认值为“扩展”。  
  
 **全部重置**  
 选择此选项可将该页上的值重置为默认值。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="complete-the-wizard-page"></a><a name="CompleteWizard"></a> “完成向导”页  
 使用此页可以查看 **“数据库邮件配置向导”** 将要执行的操作。 在完成该向导之前，不会进行任何更改。  
  
 [数据库邮件配置向导](#DBWizard)  
  
###  <a name="send-test-e-mail-page"></a><a name="TestEmail"></a> Send Test E-Mail Page  
 使用 **从 _<instance_name>_ 发送测试电子邮件** 页，可以使用指定的数据库邮件配置文件发送电子邮件。 只有 **sysadmin** 固定服务器角色的成员才可以使用此页发送测试电子邮件。  
  
 **数据库邮件配置文件**  
 从列表中选择数据库邮件配置文件。 这是必填字段。 如果没有显示配置文件，则没有配置文件或您不具有选择配置文件的权限。 使用 **数据库邮件配置向导** 可以创建和配置配置文件。 如果没有列出配置文件，请使用数据库邮件配置向导来创建要使用的配置文件。  
  
 **收件人**  
 电子邮件收件人的地址。 至少需要一个收件人。  
  
 **主题**  
 测试电子邮件的主题行。 更改默认主题，以更好地标识电子邮件以进行故障排除。  
  
 **正文**  
 测试电子邮件的正文。 更改默认主题，以更好地标识电子邮件以进行故障排除。  
  
 “数据库邮件测试电子邮件”对话框确认数据库邮件尝试发送的测试消息，并为测试电子邮件提供 **mailitem_id**。 请与收件人核实以确定该电子邮件是否已到达。 通常几分钟后即可接收到电子邮件，但是由于网络速度较慢、邮件服务器上的邮件积压或服务器暂时不可用，该电子邮件可能会延迟。 使用 **mailitem_id** 以进行故障排除。  
  
 **发送电子邮件**  
 测试电子邮件的 **mailitem_id** 。  
  
 **故障排除**  
 单击可打开联机丛书的 [对数据库邮件进行故障排除](/previous-versions/sql/sql-server-2008-r2/ms188663(v=sql.105))主题。  
  
 [数据库邮件配置向导](#DBWizard)  
  
##  <a name="using-templates"></a><a name="Template"></a> 模板  
 **创建数据库邮件配置脚本**  
  
1.  在 **“视图”** 菜单上，选择 **“模板资源管理器”**。  
  
2.  在 **“模板资源管理器”** 窗口中，展开 **“数据库邮件”** 文件夹。  
  
3.  双击“简单数据库邮件配置”。 模板将在新的查询窗口中打开。  
  
4.  在 **“查询”** 菜单上，选择 **“指定模板参数的值”**。 将打开 **“替换模板参数”** 窗口。  
  
5.  为 **profile_name**、 **account_name**、 **SMTP_servername**、 **email_address** 和 **display_name** 键入值。 SQL Server Management Studio 将使用您提供的值填充模板。  
  
6.  执行脚本来创建配置。  
  
7.  该脚本不授予任何数据库用户对配置文件的访问权限。 因此，默认情况下，只有 **sysadmin** 固定安全角色的成员才可以使用该配置文件。 有关对配置文件授予访问权限的详细信息，请参阅 [sysmail_add_principalprofile_sp (Transact SQL)](../../relational-databases/system-stored-procedures/sysmail-add-principalprofile-sp-transact-sql.md)  
  
