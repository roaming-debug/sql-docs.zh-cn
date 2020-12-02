---
description: 如何为 CDC 准备 SQL Server
title: 如何为 CDC 准备 SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: a327fa18-58f4-4e69-bb87-44faf47e20ef
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6f445a638f07ff6f524fc624f63cbc9bd6b29750
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88496195"
---
# <a name="how-to-prepare-sql-server-for-cdc"></a>如何为 CDC 准备 SQL Server

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Oracle CDC 服务要求所有目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例都包含 MSXDBCDC 数据库。 您可以在 CDC 服务配置控制台中使用“准备 SQL Server”操作创建此数据库。只能为每个目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例执行一次此任务。  
  
 下面的内容介绍如何使用 CDC 服务配置控制台为 Oracle 变更数据捕获准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。 此过程创建 MSXDBCDC 数据库，并且定义所需的表、存储过程和其他所需项目。  
  
 为 Oracle CDC 准备 SQL Server 是由 Oracle CDC 服务管理员进行的。 有关 CDC 服务管理员角色的详细信息，请参阅 [User Roles](../../integration-services/change-data-capture/user-roles.md)。  
  
### <a name="to-enable-sql-server-for-cdc"></a>为 CDC 启用 SQL Server  
  
1.  从 **“开始”** 菜单上，选择 **“Oracle CDC 服务配置”**。  
  
2.  从左侧的窗格中选择 **“本地 CDC 服务”** ，然后从 **“操作”** 窗格中单击 **“准备 SQL Server”**。  
  
     还可以右键单击“本地 CDC 服务”，然后选择“准备 SQL Server”。  
  
3.  在“为 Oracle CDC 准备 SQL Server 实例”对话框中输入所需信息。 有关如何在此对话框中输入所需信息的信息，请参阅 [Prepare SQL Server for CDC](../../integration-services/change-data-capture/prepare-sql-server-for-cdc.md)。  
  
     若要为 Oracle CDC 准备 SQL Server 实例，该登录名必须对 MSXDBCDC 数据库具有写入权限。 输入对 MSXDBCDC 数据库具有写入权限的登录名的凭据，例如 `sysasmin` 角色的成员。  
  
 **注意**：可以单击“查看脚本”来查看安装脚本的只读版本。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统管理员可以将此脚本复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理控制台，以便根据需要编辑和执行该脚本。  
  
## <a name="see-also"></a>另请参阅  
 [为 CDC 准备 SQL Server](../../integration-services/change-data-capture/prepare-sql-server-for-cdc.md)  
  
  
