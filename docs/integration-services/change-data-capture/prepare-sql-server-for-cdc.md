---
description: 为 CDC 准备 SQL Server
title: 为 CDC 准备 SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- prepSqlSrv
ms.assetid: 20b51dbf-a545-4234-87ae-4228268a0fb2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9a98ff226c4e2e861b60d04cb50e8b201035c16a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88431079"
---
# <a name="prepare-sql-server-for-cdc"></a>为 CDC 准备 SQL Server

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Oracle CDC 服务要求所有目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例都包含 MSXDBCDC 数据库。 您可以在 CDC 服务配置控制台中使用“准备 SQL Server”操作创建此数据库。 这将创建一个特殊的脚本，运行此脚本可创建所需的表、存储过程以及此数据库的其他所需项目。 只能为每个目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例执行一次此任务。  
  
 有关 MSXDBCDC 数据库的详细信息，请参阅“MSXDBCDC 数据库”。  
  
 在 CDC 服务配置控制台中，单击“准备 SQL Server”。 如果此选项不可用，则请确保在该控制台的左侧窗格中选择了“本地 CDC 服务”。  
  
## <a name="options"></a>选项  
  
### <a name="server-name"></a>服务器名称  
 键入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所在的服务器的名称。  
  
### <a name="authentication"></a>身份验证  
 选择以下方案之一：  
  
-   **Windows 身份验证**  
  
-   **SQL Server 身份验证**：如果选择此选项，则必须在连接到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中为用户键入“用户名”和“密码”。  
  
 若要为 Oracle CDC 准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，该登录名必须对 MSXDBCDC 数据库具有写入权限。 输入对 MSXDBCDC 数据库具有写入权限的登录名的凭据，例如 `sysasmin` 角色的成员。  
  
### <a name="options"></a>选项  
 单击箭头可以查看要配置的可用选项。 您可以选择保留这些选项不变，使用其默认值。 可用选项是：  
  
-   **连接超时值**：键入一个时间（秒钟），未超过该时间，Oracle CDC 服务将等待与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的连接，超过该时间后即超时。默认值为 **15**。  
  
-   **执行超时值**：键入一个时间（秒钟），未超过该时间，Oracle CDC Windows 服务将等待命令执行，超过该时间后即超时。默认值为 **30**。  
  
-   **加密连接**：选择 **“加密连接”** 将使用加密连接进行 Oracle CDC 服务和目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间的通信。  
  
-   **高级**：根据需要键入任何其他连接属性。  
  
### <a name="view-script"></a>查看脚本  
 单击“查看脚本”可查看安装脚本的只读版本。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统管理员可以将此脚本复制到 SQL Server 管理控制台，以便根据需要进行编辑。 有关准备 SQL Server 脚本的详细信息，请参阅 [为 Oracle CDC 视图脚本准备 SQL Server](../../integration-services/change-data-capture/prepare-sql-server-for-oracle-cdc-view-script.md)。  
  
## <a name="see-also"></a>另请参阅  
 [如何使用 CDC 服务](../../integration-services/change-data-capture/how-to-work-with-cdc-services.md)   
 [如何为 CDC 准备 SQL Server](../../integration-services/change-data-capture/how-to-prepare-sql-server-for-cdc.md)  
  
  
