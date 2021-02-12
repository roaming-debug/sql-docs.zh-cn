---
title: SSDT 中的 Azure Active Directory
description: 了解 SQL Server Data Tools (SSDT) 为 Azure SQL 数据库和 Azure Synapse Analytics 提供的 Azure Active Directory 身份验证方法。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
author: markingmyname
ms.author: maghan
reviewer: ''
ms.custom: seo-lt-2019
ms.date: 10/28/2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 84e1c427d01eef2bc7df4bdf7af0e28e0c469844
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079719"
---
# <a name="azure-active-directory-support-in-sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT) 中的 Azure Active Directory 支持

[!INCLUDE[appliesto-xx-asdb-asdb-xxx-md.md](../includes/appliesto-xx-asdb-asdw-xxx-md.md)]

SQL Server Data Tools (SSDT) 提供了多种 [Azure Active Directory (Azure AD)](/azure/active-directory/active-directory-whatis) 身份验证方法。

在 Visual Studio 中，打开“SQL Server 对象资源管理器”（在“视图”菜单中），然后选择“添加 SQL Server”  ：

![SSDT 连接对话框](media/azure-active-directory/interactive.png)

#### <a name="which-azure-sql-products"></a>哪些 Azure SQL 产品？

本文介绍 [Azure 云](https://azure.microsoft.com/)中以下 Azure SQL 产品列表的 Azure AD：

- Azure SQL 数据库
- Azure Synapse Analytics

## <a name="active-directory-password-authentication"></a>Active Directory 密码身份验证

Active Directory 密码验证是一种连接到之前列出的 Azure SQL 产品的机制。 该机制使用 Azure Active Directory (Azure AD) 中的标识。 以下情况下使用此方法进行连接：

- 使用来自未与 Azure 联合的域的凭据登录到 Windows，或者
- 通过 Azure AD 使用 Azure AD 身份验证，并且它基于初始域或客户端域。

有关详细信息，请参阅[使用 Azure Active Directory 身份验证连接到 SQL 数据库](/azure/sql-database/sql-database-aad-authentication)。  

## <a name="active-directory-integrated-authentication"></a>Active Directory 集成身份验证

Active Directory 集成身份验证是一种使用 Azure Active Directory (Azure AD) 中的标识连接到列出的 Azure SQL 产品的机制。 如果使用 Azure Active Directory 凭据从联盟域登录 Windows，请使用这种方法进行连接。 有关详细信息，请参阅[使用 Azure Active Directory 身份验证连接到 SQL 数据库](/azure/sql-database/sql-database-aad-authentication)。

## <a name="active-directory-interactive-authentication"></a>Active Directory 交互式身份验证

使用 SSDT 连接到列出的 Azure SQL 产品时，Active Directory 交互式身份验证可用，但仅用于 [.NET Framework 4.7.2](/dotnet/api/?view=netframework-4.7.2&preserve-view=true) 或更高版本。

- [下载并安装任何版本的 .NET Framework](https://www.microsoft.com/net/download/all)。
- [Visual Studio 2017 版本 15.6](/visualstudio/releasenotes/vs2017-relnotes) 或更高版本。

#### <a name="multi-factor-authentication-mfa"></a>多重身份验证 (MFA)

Active Directory 交互式身份验证支持交互式身份验证，以便能够使用 Azure Active Directory (AD) 多重身份验证 (MFA) 向列出的 Azure SQL 产品进行身份验证。 此方法支持本机和联合的 Azure AD 用户，以及来自其他帐户的来宾用户。 其他类型的帐户包括：

- 企业对企业 (Azure AD B2B) 用户。
- Microsoft 帐户，如 @outlook.com、@hotmail.com、@live.com。
- 非 Microsoft 帐户，如 @gmail.com。

如果指定 MFA 方法，必须指定“用户名”，“密码”字段已禁用 。 

#### <a name="password-entry"></a>密码输入

使用 Active Directory 交互式身份验证进行验证时，将看到打开的身份验证窗口，其中提示用户手动输入密码。

![登录对话框](media/azure-active-directory/sign-in.png)

Azure AD 通过此附加 MFA 弹出窗口强制执行 MFA。

> [!NOTE]
> 使用 Active Directory 交互式身份验证将阻止自动化工作流。 必须存在以手动输入密码的形式与身份验证过程进行交互的人员。

## <a name="known-issues-and-limitations"></a>已知问题和限制

- 仅当连接到本文开头列出的 Azure SQL 产品时，才支持Active Directory 交互式身份验证。 SQL Server（本地或 VM 上）不支持它。
- 服务器资源管理器中的连接对话框不支持 Active Directory 交互式身份验证 。 必须结合使用 SSDT 和 SQL Server 对象资源管理器进行连接。
- SSDT 不支持将单一登录与当前登录 Visual Studio 的帐户集成。
- 在 Visual Studio 安装期间安装到扩展目录的 SQLPackage.exe 并不是指从此位置进行使用。 若要结合使用 SQLPackage.exe 和 Azure AD，请转到[数据层应用程序框架](https://www.microsoft.com/download/details.aspx?id=55088) 
- Azure AD 身份验证不支持 SSDT 数据比较。  


## <a name="see-also"></a>另请参阅  

[多重身份验证](/azure/sql-database/sql-database-ssms-mfa-authentication)  
[向 SQL 数据库进行Azure Active Directory 身份验证](/azure/sql-database/sql-database-aad-authentication-configure)  
[SSDT MSDN 论坛](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=ssdt)  
[SSDT 团队博客](/archive/blogs/ssdt/)  
[下载 SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
