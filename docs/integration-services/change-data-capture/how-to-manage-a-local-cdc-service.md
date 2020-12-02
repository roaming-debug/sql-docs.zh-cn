---
description: 如何管理本地 CDC 服务
title: 如何管理本地 CDC 服务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 7f9be649-cd93-40c1-bc48-0480106f207c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d103c224088d2dfa22fe1d8dcb36306177aac54
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88496167"
---
# <a name="how-to-manage-a-local-cdc-service"></a>如何管理本地 CDC 服务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  本过程介绍如何使用 CDC 服务配置控制台来管理特定的 CDC 服务。  
  
### <a name="to-manage-a-specific-cdc-service"></a>管理特定的 CDC 服务  
  
1.  从 **“开始”** 菜单上，选择 **“Oracle CDC 服务配置”**。  
  
2.  从 CDC 服务配置控制台的左侧窗格中，展开 **“本地 CDC 服务”**。  
  
3.  选择要使用的 CDC 服务。  
  
     也可以右键单击要使用的 CDC 服务，然后选择所需操作。  
  
     **或者**  
  
     从 CDC 服务配置控制台的左侧窗格中选择 **“本地 CDC 服务”** ，然后从 CDC 服务配置控制台的中部选择要使用的服务。  
  
     也可以右键单击要使用的 CDC 服务，然后选择所需操作。  
  
4.  您可在使用 CDC 服务时执行以下任务。  
  
    -   **删除服务**  
  
         从 CDC 服务配置控制台右侧的 **“操作”** 窗格，单击 **“删除”** 以便删除服务。  
  
         也可以右键单击要删除的 CDC 服务，然后选择“删除”。  
  
         **注意**：如果在删除服务时该服务正在运行，则该服务将在被删除前停止。  
  
         若要删除 Oracle CDC Windows 服务定义，程序需要对关联的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的 MSXDBCDC 数据库具有更新访问权限。 在您单击 **“确定”** 删除该服务后，程序将尝试在 MSXDBCDC 数据库中删除 Oracle CDC 服务注册。 如果由于权限不足而失败，将显示一个对话框，提示用户输入具有对 MSXDBCDC 数据库的更新访问权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
         有关必须在“连接到 SQL Server”对话框中输入的数据的信息，请参阅 [Manage an Oracle CDC Service](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md) 和 [Connection to SQL Server for Delete](../../integration-services/change-data-capture/connection-to-sql-server-for-delete.md)。  
  
    -   **编辑 CDC 服务属性**  
  
         从 CDC 服务配置控制台右侧的 **“操作”** 窗格中，单击 **“属性”**。  
  
         也可以右键单击要编辑其属性的 CDC 服务，然后选择“属性”。  
  
## <a name="see-also"></a>另请参阅  
 [Manage an Oracle CDC Service](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md)  
  
  
