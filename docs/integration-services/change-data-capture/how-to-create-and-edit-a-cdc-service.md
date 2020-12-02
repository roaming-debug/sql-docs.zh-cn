---
description: 如何创建和编辑 CDC 服务
title: 如何创建和编辑 CDC 服务 | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 1b3d47a5-dc89-482d-bbc7-fff04f194c43
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6c0ecefc040d07405e700ee879b2220ff820e0cf
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88484626"
---
# <a name="how-to-create-and-edit-a-cdc-service"></a>如何创建和编辑 CDC 服务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  这些过程介绍如何从 CDC 服务配置控制台创建和编辑新的 Oracle CDC Windows 服务。  
  
 此过程要求对配置了 Oracle CDC 服务的计算机具有管理员权限的 Windows 用户。  
  
### <a name="to-create-a-new-cdc-service"></a>创建新的 CDC 服务  
  
1.  从 **“开始”** 菜单上，选择 **“Oracle CDC 服务配置”**。  
  
2.  从左侧窗格中，右键单击“本地 CDC 服务”，然后选择“新建服务”。  
  
     也可以从 **“操作”** 窗格中选择 **“新建服务”** 。  
  
3.  在“新建 Oracle CDC 服务”对话框中键入或输入所需信息。 有关如何在“新建 Oracle CDC 服务”对话框中输入信息的信息，请参阅 [Create and Edit an Oracle CDC Service](../../integration-services/change-data-capture/create-and-edit-an-oracle-cdc-service.md) 。  
  
     在“新建 Oracle CDC 服务”对话框中提供的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名由 Oracle CDC 服务在该服务运行时使用。 该登录名仅需是公共固定服务器角色的成员，无需其他权限。 在添加了新的 Oracle CDC 实例后，该登录名将接收对关联的 **CDC 数据库的** db_owner [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问权限。  
  
4.  在完成输入所需的信息后，单击 **“确定”**。  
  
     若要创建 Oracle CDC Windows 服务定义，程序需要对关联的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的 MSXDBCDC 数据库具有更新访问权限。 在单击 **“确定”** 后，将出现一个对话框，提示用户输入具有对 MSXDBCDC 数据库的更新访问权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
     有关必须在“连接到 SQL Server”对话框中键入的数据的信息，请参阅 [Connection to SQL Server](../../integration-services/change-data-capture/connection-to-sql-server.md)。  
  
5.  单击 **“确定”** 关闭“新建 Oracle CDC 服务”对话框。  
  
### <a name="to-edit-a-cdc-service"></a>编辑 CDC 服务  
  
1.  从 **“开始”** 菜单上，选择 **“Oracle CDC 服务配置”**。  
  
2.  从左侧窗格中，选择“本地 CDC 服务”，然后右键单击要编辑的本地服务并且选择“属性”。  
  
     还可以在中部选择要使用的服务，然后从 **“操作”** 窗格中单击 **“属性”**。  
  
3.  在“CDC 服务属性”对话框中键入或输入所需信息。 有关如何在“CDC 服务属性”对话框中输入信息的信息，请参阅 [Create and Edit an Oracle CDC Service](../../integration-services/change-data-capture/create-and-edit-an-oracle-cdc-service.md) 。  
  
4.  在您输入完所需信息后，单击 **“确定”**，“连接到 SQL Server”对话框将打开。  
  
     在对 MSXDBDCDC 数据库没有写入权限的登录名尝试创建新的 Oracle CDC 实例时，将显示错误消息。 在该对话框中单击 **“确定”** 将显示“连接到 SQL Server”对话框。 在此对话框中，必须为对 MSXDBCDC 数据库具有写入权限的登录名（例如 **db_owner** 数据库角色）输入凭据。  
  
     有关必须在“连接到 SQL Server”对话框中键入的数据的信息，请参阅 [Connection to SQL Server](../../integration-services/change-data-capture/connection-to-sql-server.md)。  
  
5.  在“连接到 Oracle”对话框中单击 **“确定”** 。 这两个对话框将关闭，并且将更新并注册该服务。  
  
## <a name="see-also"></a>另请参阅  
 [Change Data Capture Designer for Oracle by Attunity](../../integration-services/change-data-capture/change-data-capture-designer-for-oracle-by-attunity.md)   
 [创建和编辑 Oracle CDC 服务](../../integration-services/change-data-capture/create-and-edit-an-oracle-cdc-service.md)  
  
  
