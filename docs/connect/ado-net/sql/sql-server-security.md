---
title: SQL Server 安全性
description: 简要介绍 SQL Server 安全功能，以及创建面向 SQL Server 的安全 ADO.NET 应用程序的应用程序方案。
ms.date: 09/26/2019
ms.assetid: 9053724d-a1fb-4f0f-b9dc-7f6dd893e8ff
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-kaywon
ms.openlocfilehash: e70f6da7755b28c400cc80a5ee6de79a004e4e42
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97038915"
---
# <a name="sql-server-security"></a>SQL Server 安全性

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

SQL Server 具有许多支持创建安全数据库应用程序的功能。  
  
无论要使用哪个版本的 SQL Server，通用安全注意事项（如数据盗窃和蓄意破坏）都适用。 数据完整性也应视为安全问题。 如果数据不受保护，且允许临时数据操作，只要数据被无意或恶意地修改为不正确的值或被完全删除，数据就可能变得毫无价值。 此外，通常还必须遵守法律要求，如正确存储机密信息。 存储某些类型的个人数据是完全禁止的，具体取决于适用于特定管辖权地的法律。  
  
与每个版本的 Windows 一样，每个版本的 SQL Server 都具有不同的安全功能，版本越高，功能越强。 请务必了解，单靠安全功能并不能保证数据库应用程序的安全。 每个数据库应用程序都有独特的要求、执行环境、部署模型、物理位置和用户群体。 某些本地范围内的应用程序可能只需要最低限度的安全措施，而其他本地应用程序或通过 Internet 部署的应用程序可能需要严格的安全措施以及持续监视和评估。  
  
SQL Server 数据库应用程序的安全要求应该在设计时就加以考虑，而不应事后再考虑。 在开发周期的早期评估威胁，让你有机会在检测到漏洞的任意位置缓解潜在损害。  
  
即使应用程序的初始设计是合理的，但随着系统不断发展，可能也会出现新的威胁。 通过在数据库周围布建多道防线，可以最大限度地减少安全漏洞造成的损害。 第一道防线是，通过永不授予超过绝对必要的权限来减少攻击面。  
  
本节中的主题简要说明了 SQL Server 中开发人员相关的安全功能，并提供了指向 SQL Server 联机丛书中相关主题以及提供更详细说明的其他资源的链接。  
  
## <a name="in-this-section"></a>在本节中  
[SQL Server 中的身份验证](authentication-sql-server.md)  
介绍了 SQL Server 登录和身份验证，并收录了其他资源的链接。 

[Azure Active Directory 身份验证](azure-active-directory-authentication.md)  
介绍如何通过 SqlClient 使用受支持的 Azure Active Directory 身份验证模式连接到 Azure SQL 数据源。
  
[SQL Server 中的应用程序安全方案](application-security-scenarios-sql-server.md)  
包含对 ADO.NET 和 SQL Server 应用程序的各种应用程序安全方案进行讨论的主题。  
  
[SQL Server Express 安全性](sql-server-express-security.md)  
介绍 SQL Server Express 的安全注意事项。  
  
## <a name="related-sections"></a>相关章节  
[SQL Server 数据库引擎和 Azure SQL Database 的安全中心](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
介绍了 SQL Server 和 Azure SQL 数据库的安全注意事项。

[安装 SQL Server 的安全注意事项](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
介绍了在安装 SQL Server 之前要注意的安全事项。

## <a name="next-steps"></a>后续步骤
- [SQL Server 和 ADO.NET](index.md)
