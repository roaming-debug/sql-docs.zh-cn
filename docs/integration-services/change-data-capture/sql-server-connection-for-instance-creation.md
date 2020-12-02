---
description: 用于实例创建的 SQL Server 连接
title: 用于实例创建的 SQL Server 连接 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 81d0e7e2-d8f0-4bd9-9565-218ce996f28e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e1b856327d3e249cd58efe6ccad732f70f900a50
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88457611"
---
# <a name="sql-server-connection-for-instance-creation"></a>用于实例创建的 SQL Server 连接

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  创建 Oracle CDC 实例时首先要执行的步骤之一是在目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上创建 CDC 数据库。 将为 SQL Server CDC 启用该 CDC 数据库，并且此启用将要求作为 `sysadmin` 固定服务器角色的成员的登录名。  
  
 在启动“创建 Oracle CDC 实例”向导的用户不是 `sysadmin` 固定服务器角色的成员时，“连接到 SQL Server”对话框将打开，并且请求 `sysadmin` 角色成员的凭据以便执行“为 SQL Server CDC 启用数据库”任务。 在创建 CDC 数据库时， `sysadmin` 登录名将被放弃，并且使用在进入 Oracle 设计器控制台时使用的原始 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名继续工作。  
  
## <a name="task-list"></a>任务列表  
 在“连接到 SQL Server”对话框中输入以下信息。  
  
 **服务器名称**  
 键入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所在的服务器的名称。  
  
 **身份验证**  
 选择以下方案之一：  
  
-   **Windows 身份验证**  
  
-   **SQL Server 身份验证**：如果选择此选项，则必须在连接到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中为用户键入“登录名”和“密码”。  
  
 该登录名必须具有允许访问 MSXCDCDB 数据库的数据库角色。 建议该登录名还具有访问要使用的任何其他数据库的权限，否则，该用户将无法查看这些数据库中的数据。  
  
 **选项**  
 单击箭头可以查看要配置的可用选项。 您可以选择保留这些选项不变，使用其默认值。 可用选项是：  
  
-   **连接超时值**：键入一个时间（秒钟），未超过该时间，Oracle CDC 服务将等待与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的连接，超过该时间后即超时。默认值为 **15**。  
  
-   **执行超时值**：键入一个时间（秒钟），未超过该时间，Oracle CDC Windows 服务将等待命令执行，超过该时间后即超时。默认值为 **30**。  
  
-   **加密连接**：选择 **“加密连接”** 将使用加密连接进行 Oracle CDC 服务和目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间的通信。  
  
-   **高级**：单击“高级”并根据需要在“高级连接属性”对话框中键入任何附加的连接属性。  
  
     有关“高级连接属性”对话框的信息，请参阅 [高级连接属性](../../integration-services/change-data-capture/advanced-connection-properties.md)。  
  
## <a name="see-also"></a>另请参阅  
 [创建 SQL Server 更改数据库](../../integration-services/change-data-capture/create-the-sql-server-change-database.md)   
 [针对 CDC 设计器的 SQL Server 连接所需权限](../../integration-services/change-data-capture/sql-server-connection-required-permissions-for-the-cdc-designer.md)  
  
  
