---
title: 设置数据库镜像 (SQL Server) | Microsoft Docs
description: 了解在 SQL Server 中设置数据库镜像的先决条件、建议和步骤，其中包括数据库镜像会话的概述。
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
ms.assetid: da45efed-55eb-4c71-be34-ac2589dfce8d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b2166452c4b1c6f87b907c2329a897e9c9041b75
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643201"
---
# <a name="setting-up-database-mirroring-sql-server"></a>设置数据库镜像 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本节说明了设置数据库镜像的前提条件、建议和步骤。 有关数据库镜像的介绍，请参阅 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)。  
  
> [!IMPORTANT]  
>  我们建议您在非高峰时段配置数据库镜像，因为配置会影响性能。  
  
  
##  <a name="preparing-a-server-instance-to-host-a-mirror-server"></a><a name="PrepareInstances"></a> 准备服务器实例以承载镜像服务器  
 对于每个数据库镜像会话：  
  
1.  主体服务器、镜像服务器和见证服务器（如果有）都必须由位于单独的主机系统中的独立服务器实例承载。 每个服务器实例都需要数据库镜像端点。 如果您需要创建一个数据库镜像端点，请确保其他服务器实例无法访问该端点。  
  
     服务器实例对数据库镜像使用的验证形式是其数据库镜像端点的一种属性。 两种类型的传输安全性可用于数据库镜像：Windows 身份验证或基于证书的身份验证。 有关详细信息，请参阅 [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)。  
  
     网络访问要求是特定于身份验证形式的，如下所示：  
  
    -   **在使用 Windows 身份验证的情况下**  
  
         如果服务器实例使用不同的域用户帐户运行，则每个实例还需要在其他实例的 **master** 数据库中具有登录名。 如果登录名不存在，则必须先创建。 有关详细信息，请参阅 [允许使用 Windows 身份验证对数据库镜像终结点进行网络访问 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)。  
  
    -   **在使用证书的情况下**  
  
         若要在给定的服务器实例上启用数据库镜像的证书验证，系统管理员必须配置每个服务器实例，以在出站连接和进站连接中使用证书。 必须先配置出站连接。 有关详细信息，请参阅[使用数据库镜像终结点证书 (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)。  
  
2.  确保所有数据库用户在镜像服务器上都有登录名。 有关详细信息，请参阅 [设置数据库镜像或 AlwaysOn 可用性组的登录帐户 (SQL Server)](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)。  
  
3.  在将承载镜像数据库的服务器实例上，设置镜像数据库所需的环境的其余部分。 有关详细信息，请参阅 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)。  
  
##  <a name="overview-establishing-a-database-mirroring-session"></a><a name="EstablishUsingWinAuthentication"></a> 概述：建立数据库镜像会话  
 以下是建立镜像会话的基本步骤：  
  
1.  通过对每个还原操作使用 RESTORE WITH NORECOVERY 还原以下备份来创建镜像数据库：  
  
    1.  在确保主体数据库在执行备份时使用了完整恢复模式后，还原主体数据库的最新完整数据库备份。 镜像数据库的名称必须与主体数据库的名称相同。  
  
    2.  如果您自还原完整备份以来已对数据库执行任何差异备份，请还原最新的差异备份。  
  
    3.  还原自完整数据库备份或差异数据库备份以来进行的所有日志备份。  
  
     有关详细信息，请参阅 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)的各版本中均未提供见证服务器实例。  
  
    > [!IMPORTANT]  
    >  在进行主体数据库的备份后，尽快完成剩余设置步骤。 对伙伴开始镜像之前，应该创建原始数据库的当前日志备份并将其还原到将来的镜像数据库。  
  
2.  可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 或数据库镜像向导来设置镜像。 有关详细信息，请参阅下列内容之一：  
  
    -   [使用 Windows 身份验证建立数据库镜像会话 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
    -   [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
3.  默认情况下，会话设置为完整事务安全（SAFETY 设置为 FULL），此设置会在同步、不带自动故障转移功能的高安全性模式下启动会话。 您可以将会话重新配置为在具有自动故障转移功能的高安全性模式下运行，或者在异步、高性能模式下运行，如下所示：  
  
    -   **具有自动故障转移的高安全性模式**  
  
         如果希望高安全性模式会话支持自动故障转移，则请添加见证服务器实例。  
  
         **添加见证服务器**  
  
        -   [使用 Windows 身份验证添加数据库镜像见证服务器 (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
        -   [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
        > [!NOTE]  
        >  数据库所有者可以随时关闭数据库的见证服务器。 关闭见证服务器就等于没有见证服务器，因此不能进行自动故障转移。  
  
    -   **高性能模式**  
  
         另外，如果您不想进行自动故障转移，并且您对性能的追求超过了可用性，则请关闭事务安全。 有关详细信息，请参阅[更改数据库镜像会话中的事务安全 (Transact-SQL)](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md)。  
  
        > [!NOTE]  
        >  在高性能模式下，WITNESS 需设置为 OFF。 有关详细信息，请参阅[仲裁：见证服务器如何影响数据库可用性（数据库镜像）](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md)。  
  
> [!NOTE]  
>  有关使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 设置使用 Microsoft Windows 身份验证的数据库镜像的示例，请参阅[示例：使用 Windows 身份验证设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)。  
>   
>  有关使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 来设置使用基于证书的安全机制的数据库镜像的示例，请参阅[示例：使用证书设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)。  
  
  
##  <a name="in-this-section"></a><a name="InThisSection"></a> 本节内容  
 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
 概述了在恢复挂起的会话之前创建或准备镜像数据库的步骤。 同时还提供了指向操作指南主题的链接。  
  
 [指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
 说明了服务器网络地址的语法，此地址如何标识服务器实例的数据库镜像端点以及如何查找完全限定的系统域名。  
  
 [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
 说明了如何使用配置数据库镜像安全向导在数据库上启动数据库镜像。  
  
 [使用 Windows 身份验证建立数据库镜像会话 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
 说明了设置数据库镜像的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 步骤。  
  
 [示例：使用 Windows 身份验证设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)  
 包含使用 Windows 身份验证创建带有见证服务器的数据库镜像会话所需的所有阶段的示例。  
  
 [示例：使用证书设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
 包含使用基于证书的身份验证创建带有见证服务器的数据库镜像会话所需的所有阶段的示例。  
  
 [设置数据库镜像或 AlwaysOn 可用性组的登录帐户 (SQL Server)](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)  
 说明了创建使用本地服务器实例以外的帐户的远程服务器实例登录。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **SQL Server Management Studio**  
  
-   [启动配置数据库镜像安全向导 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
 **Transact-SQL**  
  
-   [允许使用 Windows 身份验证对数据库镜像终结点进行网络访问 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)  
  
-   [允许数据库镜像终结点使用证书进行出站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [允许数据库镜像终结点将证书用于入站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [使用 Windows 身份验证建立数据库镜像会话 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [使用 Windows 身份验证添加数据库镜像见证服务器 (Transact-SQL)](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [将镜像数据库设置为使用 Trustworthy 属性 (Transact-SQL)](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
 **Transact-SQL/SQL Server Management Studio**  
  
-   [Upgrading Mirrored Instances](../../database-engine/database-mirroring/upgrading-mirrored-instances.md)  
  
-   [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [数据库镜像配置故障排除 (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [数据库镜像：互操作性和共存 &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-interoperability-and-coexistence-sql-server.md)   
 [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
  
