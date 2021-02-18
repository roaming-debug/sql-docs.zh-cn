---
description: 连接到服务器（数据库引擎）
title: 连接到服务器（数据库引擎）
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.connectoserverunknownservertype.f1
- sql13.swb.connection.login.sqlce.f1
- sql13.swb.connecttoce.f1
- SQL13.SWB.CONNECTION.LOGIN.SQLSERVER.F1
- sql13.swb.connection.login.sqlserver.f1
- sql13.swb.manageSS2k.f1
- connect-to-server-database-engine
ms.assetid: ee9017b4-8a19-4360-9003-9e6484082d41
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 04/07/2020
ms.openlocfilehash: 9f0eca39db873b277d0c698407c5c02afa520b35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342662"
---
# <a name="connect-to-server-database-engine"></a>连接到服务器（数据库引擎）

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
连接到 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)] 时，可以使用此对话框查看或指定选项。 大多数情况下，可以通过在“服务器名称”  框中输入数据库服务器的计算机名称并单击“连接”  来进行连接。 如果连接到命名实例，请使用计算机名称后跟反斜杠，然后是实例名。 例如，`mycomputer\myinstance` 。 如果要连接到 [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)]，请使用后面跟有 **\sqlexpress** 的计算机名称。
  
许多因素都会对您能否连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]产生影响。 有关详细信息，请参阅以下资源：

- [教程第 1 课：连接到数据库引擎](../../relational-databases/lesson-1-connecting-to-the-database-engine.md)  

- [排查连接到 SQL Server 数据库引擎时的问题](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)  

- [解决 SQL Server 连接错误](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)   
  
## <a name="options"></a>选项

**服务器类型**  
从对象资源管理器注册服务器时，请选择要连接到的服务器的类型： [!INCLUDE[ssDE](../../includes/ssde_md.md)]、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]或 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]。 对话框的其余部分只显示适用于所选服务器类型的选项。 从“已注册的服务器”注册某服务器时，“服务器类型”  框是只读的，并且与“已注册的服务器”组件中显示的服务器类型匹配。 若要注册其他类型的服务器，请在开始注册新服务器之前，从“已注册的服务器”工具栏中选择“ [!INCLUDE[ssDE](../../includes/ssde_md.md)]”、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]、 [!INCLUDE[ssEW](../../includes/ssew-md.md)]或 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 。  
  
**服务器名称**  
选择要连接到的服务器实例。 默认情况下，显示上次连接到的服务器实例。  
  
> [!NOTE]  
> 要连接到 [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)] 的活动用户实例，请使用指定管道名称的命名管道协议进行连接，例如 `np:\\.\pipe\3C3DF6B1-2262-47\tsql\query`。 有关详细信息，请参阅 [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)] 文档。  

> [!NOTE]  
> 连接通常保留在“最近使用”(MRU) 历史记录中。 若要从 MRU 中删除条目，只需单击“服务器名称”  组合框，选择要删除的服务器的名称，然后按 DEL  键。  

**身份验证**  
当前版本的 SSMS 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)] 的实例时提供五种身份验证模式。 如果你的“身份验证”对话框中与以下列表中的不匹配，请从 [下载 SQL Server Management Studio (SSMS)](../download-sql-server-management-studio-ssms.md) 下载最新版本的 SSMS。  

> **Windows 身份验证**  
> [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows 身份验证模式允许用户通过 Windows 用户帐户进行连接。  
> 
> **SQL Server 身份验证**  
> 当用户使用指定的登录名和密码从不可信连接进行连接时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将通过检查是否已设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户以及指定的密码是否与以前记录的密码匹配，自行进行身份验证。 如果未设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户，则身份验证失败，并且用户会收到一条错误消息。 如果可以，请使用 Windows 身份验证或 Active Directory 密码身份验证。  
> 
> **Azure Active Directory - 支持 MFA 的通用目录**  
> Active Directory - 含 MFA 支持的通用身份验证是交互式的工作流，支持 Azure 多重身份验证 (MFA)。 Azure MFA 可满足用户简单登录过程的需求，同时可帮助保护数据访问权限和应用程序。 通过一系列的简单验证（电话、短信、带有 PIN 的智能卡或移动应用通知）提供强身份验证，用户可选择喜欢的验证方式。 用户帐户配置 MFA 后，该交互式身份验证工作流需要通过弹出式对话框、智能卡等进行额外用户交互。用户帐户配置 MFA 后，用户必须选择 Azure 通用身份验证进行连接。 如果用户帐户不需要 MFA，用户仍可使用其他两个 Azure Active Directory 身份验证选项。 有关详细信息，请参阅 [SQL 数据库和 Azure Synapse Analytics 针对 Azure AD MFA 的 SSMS 支持](/azure/azure-sql/database/authentication-mfa-ssms-overview)。 如有必要，可通过单击“选项  ”，选择“连接属性  ”选项卡，并完成“AD 域名或租户 ID  ”框，更改对登录名进行身份验证的域。  
> 
> **Azure Active Directory - 密码**  
> Azure Active Directory 身份验证是一种使用 Azure Active Directory (Azure AD) 中的标识连接到 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 的机制。  如果是使用凭证从未与 Azure 联合的域登录到 Windows，或使用基于初始域或客户端域的 Azure AD 进行 Azure AD 身份验证，请使用此方法连接到 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]。 有关详细信息，请参阅[使用 Azure Active Directory 身份验证连接到 SQL 数据库](/azure/azure-sql/database/authentication-aad-overview)。  
> 
> **Active Directory - 集成**  
> Azure Active Directory 身份验证是一种使用 Azure Active Directory (Azure AD) 中的标识连接到 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 的机制。 如果使用 Azure Active Directory 凭证从联合域登录到 Windows，请使用此方法连接到 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]。 有关详细信息，请参阅[使用 Azure Active Directory 身份验证连接到 SQL 数据库](/azure/azure-sql/database/authentication-aad-overview)。  
  
**用户名**  
连接所使用的 Windows 用户名。 只有选择使用“Azure Active Directory - 密码”身份验证进行连接时，此选项才可用  。 选择“Windows 身份验证”或“Azure Active Directory - 集成”身份验证时，它是只读的   。  
  
**登录**  
输入连接所用的登录名。 只有选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证或 Active Directory 密码身份验证进行连接时，此选项才可用。  
  
> [!NOTE]  
> 连接通常保留在“最近使用”(MRU) 历史记录中。 若要从 MRU 中删除条目，只需单击“服务器名称”  组合框，选择要删除的服务器的名称，然后按 DEL  键。 这是随 SSMS 18.5 一起引入的。

**密码**  
输入登录名的密码。 只有选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证或 Active Directory - 密码身份验证进行连接时，才可编辑此选项。  

**“连接”**  
单击以连接到服务器。

**选项**  
单击以显示“连接属性”  和“其他连接参数”  选项卡。