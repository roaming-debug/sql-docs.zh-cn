---
description: 注册服务器
title: 注册服务器
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlserverregisteredserver.dhelp
helpviewer_keywords:
- connections [SQL Server], registered servers
- registering servers
- servers [SQL Server], registering
- server management [SQL Server], registering servers
- server registration [SQL Server]
ms.assetid: c2a2513e-fa09-419c-99e7-a12d57c5a0db
author: markingmyname
ms.author: maghan
ms.manageR: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 51c90ad84344c98ff0e144bd611ab243715e1787
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341062"
---
# <a name="register-servers"></a>注册服务器

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中注册服务器使您可以存储服务器连接信息，以供将来连接时使用。有三种方法可以在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中注册服务器。  
  
1.  在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之后首次启动它时，将自动注册 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 的本地实例。  
  
2.  也可以随时启动自动注册过程来还原本地服务器实例的注册。  
  
3.  最后，还可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]的“已注册的服务器”工具注册服务器。  
  
## <a name="benefits-of-registered-servers"></a>已注册服务器的优点  
 使用已注册的服务器，您可以执行下列操作：  
  
-   注册服务器以保留连接信息。  
  
-   确定已注册的服务器是否正在运行。  
  
-   将对象资源管理器和查询编辑器轻松连接到已注册的服务器。  
  
-   编辑或删除已注册服务器的注册信息。  
  
-   创建服务器组。  
  
-   通过在“已注册的服务器名称”框中提供与“服务器名称”列表中不同的值，为已注册的服务器提供用户友好名称。  
  
-   提供已注册服务器的详细说明。  
  
-   提供已注册服务器组的详细说明。  
  
-   导出已注册的服务器组。  
  
-   导入已注册的服务器组。  
  
-   查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机或脱机实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]日志文件。  
  
## <a name="related-tasks"></a>Related Tasks  
 参考以下主题可以开始使用注册服务器：  
  
|**说明**|**主题**|  
|---------------------|---------------|  
|注册本地服务器实例|[注册连接的服务器 (SQL Server Management Studio)](./register-a-connected-server-sql-server-management-studio.md)|  
|注册服务器|[创建新的已注册的服务器 (SQL Server Management Studio)](./create-a-new-registered-server-sql-server-management-studio.md)|  
|查看已注册的服务器|[查看 SQL Server Management Studio 中已注册的服务器](./view-registered-servers-in-sql-server-management-studio.md)|  
|删除注册服务器|[删除已注册的服务器 (SQL Server Management Studio)](./remove-a-registered-server-sql-server-management-studio.md)|  
|更改服务器注册|[更改服务器的注册信息 (SQL Server Management Studio)](./change-a-server-s-registration-sql-server-management-studio.md)|  
|连接到注册服务器|[连接到已注册的服务器 (SQL Server Management Studio)](./connect-to-a-registered-server-sql-server-management-studio.md)|  
|从已注册的服务器断开连接|[断开与已注册服务器的连接 (SQL Server Management Studio)](./disconnect-from-a-registered-server-sql-server-management-studio.md)|  
|移动已注册的服务器或服务器组|[移动已注册的服务器或已注册的服务器组 (SQL Server Management Studio)](./move-a-registered-server-or-registered-server-group.md)|  
|更改已注册的服务器或服务器组的名称|[更改已注册的服务器或已注册的服务器组的名称 (SQL Server Management Studio)](./change-the-name-of-registered-server-or-registered-server-group.md)|  
|创建或编辑服务器组|[创建或编辑服务器组 (SQL Server Management Studio)](./create-or-edit-a-server-group-sql-server-management-studio.md)|  
|删除服务器组|[删除服务器组 (SQL Server Management Studio)](./remove-a-server-group-sql-server-management-studio.md)|  
|导出已注册的服务器信息|[导出已注册服务器信息 (SQL Server Management Studio)](./export-registered-server-information-sql-server-management-studio.md)|  
|导入已注册的服务器信息|[导入已注册的服务器信息 (SQL Server Management Studio)](./import-registered-server-information-sql-server-management-studio.md)|  
|创建中央管理服务器和服务器组|[创建中央管理服务器和服务器组 (SQL Server Management Studio)](./create-a-central-management-server-and-server-group.md)|  
|同时对多个服务器执行语句|[同时对多个服务器执行语句 (SQL Server Management Studio)](./execute-statements-against-multiple-servers-simultaneously.md)|  
  
## <a name="see-also"></a>另请参阅  
 [远程服务器](../../database-engine/configure-windows/remote-servers.md)  
  
