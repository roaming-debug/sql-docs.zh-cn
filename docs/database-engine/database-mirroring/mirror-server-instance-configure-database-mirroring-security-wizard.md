---
title: 配置安全向导：镜像服务器实例
description: 介绍 SQL Server Management Studio 中“配置数据库镜像安全向导”的“镜像服务器实例”页面。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.mirrorsrvr.f1
ms.assetid: 53223432-615e-440f-904d-925d33ec2144
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 783e7eabf5af38b22c4438e92909a5f70f8c4a4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343309"
---
# <a name="configure-database-mirrroing-security-wizard-mirror-server-instance"></a>配置数据库镜像安全向导：镜像服务器实例
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用此页可以指定有关具有镜像数据库的服务器实例的信息。  
  
> [!IMPORTANT]  
>  镜像服务器实例必须与主体服务器实例运行相同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（Standard 或 Enterprise）。 此外，极力建议这些服务器实例在可以处理相同工作负荷的类似系统上运行。  
  
 **使用 SQL Server Management Studio 配置数据库镜像**  
  
-   [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [启动配置数据库镜像安全向导 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>选项  
 **镜像服务器实例**  
 如果已指定镜像服务器实例（在“数据库属性”对话框的“镜像”页中），则将显示该实例；有关详细信息，请参阅[数据库属性（“镜像”页）](../../relational-databases/databases/database-properties-mirroring-page.md)   。  
  
 否则，请输入镜像服务器实例的名称。 注意，镜像服务器实例不能与主体服务器实例相同。  
  
 **“连接”**  
 如果尚未指定镜像服务器实例，请单击“连接”  。 这将显示 **“连接到服务器”** 对话框，在其中可以指定服务器实例并建立连接。  
  
 如果已经指定实例，但向导缺少一个具有足够权限检查端点存在性的连接，请单击 **“连接”** 。 这将显示“连接到服务器”  对话框，其中列出了你预先选择的服务器实例，而且此时你已无法更换该实例。 指定具有足够权限的域帐户，并连接到服务器实例。  
  
> [!NOTE]  
>  与服务器实例建立连接时，配置数据库镜像安全向导将使用 **“连接到服务器”** 对话框中提供的凭据。 这些凭据与镜像会话的凭据不同，镜像会话使用启动帐户（其中服务器实例作为服务运行）的凭据。  
  
 **侦听程序端口**  
 此选项的行为取决于此服务器实例是否存在镜像端点，如下所示：  
  
-   如果该服务器实例不存在侦听器端口，则端口号 5022 将显示在 **“端口”** 文本框中。 可以使用任何可用的端口号，例如 7022。  
  
-   如果镜像端点已经存在，则会显示该端点的端口号。 如果需要更改端口，请使用 ALTER ENDPOINT 命令。 有关详细信息，请参阅 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)。  
  
    > [!NOTE]  
    >  端口号是必需的。  
  
 **端点名称**  
 如果此服务器实例存在镜像端点，则端点名称将显示在此处。 如果端点不存在，则可以指定端点的名称。  
  
 **加密通过此端点发送的数据**  
 默认情况下，将启用加密。 如果启用，则要求（而不仅仅是支持）进行加密，并且所有加密选项都将使用默认值。 有关详细信息，请参阅 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)的信息。  
  
 若要禁用加密，请清除此复选框。 若要重新启用加密，请选中此复选框。  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像终结点 (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [数据库属性（“镜像”页）](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
