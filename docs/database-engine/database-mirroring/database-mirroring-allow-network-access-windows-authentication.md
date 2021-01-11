---
title: 数据库镜像 - 允许网络访问 - Windows 身份验证 | Microsoft Docs
description: 了解如何使用 Windows 身份验证来连接两个 SQL Server 实例的数据库镜像终结点，这可能需要手动配置。
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 28c8fec5-5feb-4c84-8d72-f2bd1ae3b40d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: afd66ca781ca5f0a982fe5e932fd783c6c599a93
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642117"
---
# <a name="database-mirroring---allow-network-access---windows-authentication"></a>数据库镜像 - 允许网络访问 - Windows 身份验证
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  将 Windows 身份验证用于连接两个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的数据库镜像端点在以下条件下要求手动配置登录帐户：  
  
-   如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例基于不同的域帐户（在相同的域或受信任的域中）作为服务运行，则必须在每个远程服务器实例上的 **master** 中创建各帐户的登录名，并且必须授予该登录帐户对端点的 CONNECT 权限。  
  
-   如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例作为网络服务帐户运行，则必须在每个远程服务器实例上的 master 中创建各主机帐户 (*DomainName* **\\** ComputerName$) 的登录名，并且必须授予该登录帐户对端点的 CONNECT 权限。 其原因在于，基于网络服务帐户运行的服务器实例使用主机的域帐户进行身份验证。  
  
> [!NOTE]  
>  确保每个服务器实例都有一个端点。 有关详细信息，请参阅 [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)。  
  
### <a name="to-configure-logins-for-windows-authentication"></a>为 Windows 身份验证配置登录名  
  
1.  对于每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的用户帐户，在其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上创建一个登录名。 将 [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) 语句与 FROM WINDOWS 子句一起使用。  
  
     有关详细信息，请参阅 [创建登录名](../../relational-databases/security/authentication-access/create-a-login.md)。  
  
2.  另外，为了确保登录用户对端点具有访问权限，请使用 [GRANT](../../t-sql/statements/grant-transact-sql.md) 语句为登录授予对端点的连接权限。 注意，如果用户是管理员，则无需授予对端点的连接权限。  
  
     有关详细信息，请参阅 [Grant a Permission to a Principal](../../relational-databases/security/authentication-access/grant-a-permission-to-a-principal.md)。  
  
## <a name="example"></a>示例  
 下面的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 示例为属于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 域的 `Otheruser` 用户帐户创建了一个 `Adomain`登录名。 并授予了此用户对预先存在的 `Mirroring_Endpoint`数据库镜像端点的连接权限。  
  
```  
USE master;  
GO  
CREATE LOGIN [Adomain\Otheruser] FROM WINDOWS;  
GO  
GRANT CONNECT on ENDPOINT::Mirroring_Endpoint TO [Adomain\Otheruser];  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [数据库镜像终结点 (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  
