---
title: 设置登录帐户（镜像和可用性组）
description: 配置登录帐户以访问数据库镜像或 Always On 可用性组的数据库镜像终结点。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- logins [SQL Server], database mirroring
ms.assetid: e9f5287b-1325-4cda-88a6-19eaaa52a652
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ea62f1f934527ce40e2b5fe2f03b8b6c1b8e49bc
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641973"
---
# <a name="set-up-login-accounts---database-mirroring-always-on-availability"></a>设置登录帐户 - 数据库镜像 AlwaysOn 可用性
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为了使两个服务器实例能够连接到彼此的[数据库镜像端点](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)，每个实例的登录帐户都要具有访问另一实例的权限。 每个登录帐户还要具有可以连接到另一实例的数据库镜像端点的权限。  
  
 此要求的影响取决于服务器实例是否使用相同的域用户帐户运行：  
  
-    如果服务器实例使用相同的域用户帐户运行，则正确的用户登录名将自动存在于全部两个 master 数据库中。 这样可简化数据库镜像和 AlwaysOn 可用性组的安全性配置。  
  
-   如果服务器实例使用不同的用户帐户运行，则在承载镜像服务器的服务器实例上或承载辅助副本的每个服务器实例上都必须手动复制承载主体服务器或主要副本的服务器实例上的用户登录名。 有关详细信息，请参阅本主题后面的[为不同帐户创建登录名](#CreateLogin)和[授予连接权限](#GrantConnect)。  
  
    > [!IMPORTANT]  
    >  要创建更安全的环境，请考虑为每个服务器实例使用单独的域帐户。  
  
##  <a name="create-a-login-for-a-different-account"></a><a name="CreateLogin"></a> 为不同帐户创建登录名  
 如果两个服务器实例作为不同的帐户运行，则系统管理员必须使用 CREATE LOGIN [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句针对每个服务器实例为远程实例的启动服务帐户创建一个登录名。 有关详细信息，请参阅 [CREATE LOGIN (Transact-SQL)](../../t-sql/statements/create-login-transact-sql.md)。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果使用非域帐户运行 ，则必须使用证书。 有关详细信息，请参阅[使用数据库镜像终结点证书 (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)。  
  
 例如，若要使 loginA 下运行的服务器实例 sqlA 连接到在 loginB 下运行的服务器实例 sqlB，则 sqlB 上必须存在 loginA 且 sqlA 上必须存在 loginB。 此外，对于包含见证服务器实例 (sqlC) 且三个服务器实例运行在不同域帐户下的数据库镜像会话，必须创建下列登录帐户：  
  
|所在实例...|创建登录帐户并将连接权限授予...|  
|--------------------|--------------------------------------------------------------|  
|sqlA|sqlB 和 sqlC|  
|sqlB|sqlA 和 sqlC|  
|sqlC|sqlA 和 sqlB|  
  
> [!NOTE]  
>  可以使用计算机帐户（而不是域用户）连接网络服务帐户。 如果使用的是计算机帐户，则必须将其作为用户添加到其他服务器实例上。  
  
##  <a name="grant-connect-permission"></a><a name="GrantConnect"></a> 为不同帐户创建登录名  
 在服务器实例上创建登录帐户后，必须授予登录帐户连接到服务器实例的数据库镜像端点的权限。 系统管理员使用 GRANT [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句授予连接权限。 有关详细信息，请参阅 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)的信息。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [创建登录名](../../relational-databases/security/authentication-access/create-a-login.md)  
  
-   [允许使用 Windows 身份验证对数据库镜像终结点进行网络访问 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)。  
  
-   [使用数据库镜像终结点证书 (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像终结点 (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [数据库镜像配置故障排除 (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [AlwaysOn 可用性组配置故障排除 (SQL Server)](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
  
