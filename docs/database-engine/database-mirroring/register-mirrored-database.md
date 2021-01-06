---
title: 注册镜像数据库 | Microsoft Docs
description: 了解如何在服务器实例上注册镜像数据库，方法是将其添加到数据库镜像监视器，这将缓存有关数据库的信息。
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.registermirroreddb.f1
ms.assetid: 6acd02b9-2311-49b0-a5f8-3852beecb4b0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5dfb3293071982720908c61368a475376db24dd3
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641300"
---
# <a name="register-mirrored-database"></a>注册镜像数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用此对话框，通过向数据库镜像监视器添加一个或多个数据库，可在给定的服务器实例中注册一个或多个镜像数据库。 添加数据库时，数据库镜像监视器会在本地缓存有关数据库及其伙伴的信息，以及如何将数据库连接到伙伴的信息。  
  
> [!IMPORTANT]  
>  如果是主体服务器实例而不是镜像服务器实例上的 **sysadmin** 固定服务器角色的成员，那么只能查看主体服务器实例上的状态。  
  
 **使用 SQL Server Management Studio 监视数据库镜像**  
  
-   [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>选项  
 **服务器实例**  
 从列表中选择一个服务器实例（数据库镜像监视器已为该列表包含的服务器实例存储了一个连接），或单击“连接”。 若要为列出的服务器实例指定新凭据，请单击 **“连接”** 并使用新凭据进行连接。  
  
> [!NOTE]  
>  若要在多个服务器实例上注册数据库，请在针对某一服务器实例完成所需数据库的检查之后，单击“应用”，然后再选择另一个服务器实例。  
  
 **“连接”**  
 若要为服务器实例指定新凭据，请单击“连接”并使用新凭据进行连接。 当连接到服务器实例时，数据库镜像监视器显示 **“等待数据”** 。  
  
 **镜像数据库**  
 “镜像数据库”网格可列出服务器实例中的镜像数据库。  
  
 该网格包含以下列：  
  
|列名称|说明|  
|-----------------|-----------------|  
|**注册**|检查您要注册的每个数据库。 如果正在监视数据库，则该数据库的复选框会被选中且禁用。<br /><br /> 注意：若要撤消注册某一数据库，请关闭“已注册镜像数据库”对话框，在导航树中选择该数据库，并从“操作”菜单中选择“撤消注册”  。|  
|**Database**|选定服务器实例上的镜像数据库名称。|  
|**当前角色**|选定服务器实例上的数据库的当前镜像角色，即主体或镜像。|  
|**伙伴(连接为)**|数据库的故障转移伙伴的名称。 括号中显示“控制台用户的 Windows 身份验证”或“登录名\<login name>’的 SQL Server 身份验证” 。 如果之前已添加实例，则此内容为当前使用的身份验证信息；如果尚未将实例添加到监视器中，则此内容为将要使用的身份验证信息。|  
  
 **当单击“确定”后，显示“管理服务器连接”对话框。**  
 在默认情况下，对于以前未给定凭据的伙伴服务器实例，数据库镜像监视器将使用 Windows 身份验证凭据。 启用该选项，可在完成数据库注册后为一个或多个服务器实例更改凭据。  
  
 如果启用该选项，则单击 **“确定”** 时将打开 **“管理服务器连接”** 对话框。 在此，您可以选择要指定凭据的服务器实例，以便监视器在连接到给定的故障转移伙伴时使用该凭据。  
  
 若要编辑伙伴的凭据，请在 **“服务器实例”** 网格中找到对应条目，并在该行中单击 **“编辑”** 。 此时将为该服务器实例名称打开 **“连接到服务器”** 对话框，同时将凭据控制初始化为当前的缓存值。 根据需要更改凭据，并单击 **“连接”** 。 如果凭据有足够的权限，则会以新凭据更新 **“连接方式”** 列。  
  
 **应用**  
 单击该按钮，在保存对话框打开的状态下注册选定的数据库（并为伙伴服务器实例保存凭据）。  
  
## <a name="see-also"></a>另请参阅  
 [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [启动配置数据库镜像安全向导 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
