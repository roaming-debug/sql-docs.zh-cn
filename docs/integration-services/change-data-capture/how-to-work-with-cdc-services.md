---
description: 如何使用 CDC 服务
title: 如何使用 CDC 服务 | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: db5c718a-6e7f-48ec-82a3-9d5b131716e5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b98acd138794028dcecce9d121295154358799b2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88496116"
---
# <a name="how-to-work-with-cdc-services"></a>如何使用 CDC 服务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  本过程介绍如何使用 CDC 服务配置控制台准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，以便使用 Oracle CDC 服务和创建新的 CDC 服务。  
  
### <a name="to-work-with-cdc-services"></a>使用 CDC 服务  
  
1.  从 **“开始”** 菜单上，选择 **“Oracle CDC 服务配置”**。  
  
2.  从左侧窗格中，选择“本地 CDC 服务”（根级别）。  
  
3.  您可以执行一项或两项下列任务：  
  
    -   **准备 SQL Server**  
  
         从 CDC 服务配置控制台右侧的 **“操作”** 窗格中选择此选项。  
  
         还可以右键单击“本地 CDC 服务”，然后选择“准备 SQL Server”。  
  
         “为 Oracle CDC 准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例”对话框随即将会打开。  
  
         若要为 Oracle CDC 服务准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，登录名必须具有含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 固定服务器角色的 `dbcreator` 登录名。 此登录名用于创建 MSXDBCDC 数据库，该数据库是添加 Oracle CDC 服务以及以后添加 Oracle CDC 实例所必需的。  
  
         有关如何使用此对话框的信息，请参阅 [Prepare SQL Server for CDC](../../integration-services/change-data-capture/prepare-sql-server-for-cdc.md)。 有关如何为 CDC 启用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的信息，请参阅 [如何为 CDC 准备 SQL Server](../../integration-services/change-data-capture/how-to-prepare-sql-server-for-cdc.md)。  
  
    -   **创建新的 CDC 服务**  
  
         从 CDC 服务配置控制台右侧的 **“操作”** 窗格中，单击 **“新建服务”** 。  
  
         还可以右键单击“本地 CDC 服务”，然后选择“新建服务”。   
  
         “新建 Oracle CDC 服务”对话框将打开。  
  
         有关如何使用此对话框的信息，请参阅 [创建和编辑 Oracle CDC 服务](../../integration-services/change-data-capture/create-and-edit-an-oracle-cdc-service.md)。 有关如何创建或编辑 CDC 服务的信息，请参阅 [如何创建和编辑 CDC 服务](../../integration-services/change-data-capture/how-to-create-and-edit-a-cdc-service.md)。  
  
         Oracle CDC 服务使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名仅需是 `public` 固定服务器角色的成员，无需其他权限。 但是，若要创建 Oracle CDC 服务，该登录名必须对 MSXDBCDC 数据库具有写入权限，例如，必须向该登录名分配 **db_owner** 数据库角色。 在对 MSXDBDCDC 数据库没有写入权限的登录名尝试创建新的 Oracle CDC 实例时，将显示错误消息。 在该对话框中单击 **“确定”** 将显示“连接到 SQL Server”对话框。  
  
         有关如何为对 MSXDBCDC 数据库具有写入权限的登录名（例如 **db_owner** 数据库角色）输入凭据的信息，请参阅 [创建和编辑 Oracle CDC 服务](../../integration-services/change-data-capture/create-and-edit-an-oracle-cdc-service.md) 和 [连接到 SQL Server](../../integration-services/change-data-capture/connection-to-sql-server.md)。  
  
## <a name="see-also"></a>另请参阅  
 [使用 CDC 服务](../../integration-services/change-data-capture/work-with-cdc-services.md)  
  
  
