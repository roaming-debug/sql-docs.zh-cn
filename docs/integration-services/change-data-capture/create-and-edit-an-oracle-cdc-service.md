---
description: 创建和编辑 Oracle CDC 服务
title: 创建和编辑 Oracle CDC 服务 | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- createSrv
ms.assetid: 10cd612e-d8f1-4af2-97d3-a0c22e1e2326
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bcccb89d1af55f990388b389087c16c003d12c39
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130657"
---
# <a name="create-and-edit-an-oracle-cdc-service"></a>创建和编辑 Oracle CDC 服务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  您从 CDC 服务配置控制台创建和编辑新的 Oracle CDC Windows 服务。  
  
 若要创建新的 Oracle CDC Windows 服务，请从左侧窗格中选择 **“本地 CDC 服务”** ，然后从 **“操作”** 窗格中单击 **“新建服务”** 。 还可以右键单击“本地 CDC 服务”，然后选择“新建服务”。 “新 Oracle CDC Windows 服务”对话框将打开。  
  
 **OR**  
  
 若要编辑 CDC 服务属性，请选择要编辑其属性的服务，然后从 **“操作”** 窗格中单击 **“属性”** 。 还可以右键单击要使用的服务，然后选择“属性”。 此时将打开“CDC 服务属性”对话框。  
  
 在“新 Oracle CDC Windows 服务”对话框或“CDC 服务属性”对话框中输入以下信息。  
  
**服务名称**  
 键入新的 Oracle CDC Windows 服务的名称。 如果可能，不应使用长名称。 在服务名称中不能使用字符 / 和 \。  
  
> [!NOTE]  
> 在编辑服务时，此选项不可用。 不能更改已存在的 Windows 服务的名称。  
  
 **说明**  
 键入有助于标识该服务的说明。  
  
 **服务帐户**  
 选择下列选项之一以便确定运行服务所基于的帐户：  
  
-   **Local System 帐户**  
  
     该帐户不是建议的帐户，因为对服务提供了过多的权限。  
  
-   **本帐户**  
  
     在 Windows Vista 或 Windows Server 2008 中，默认服务帐户是 NETWORK SERVICE 帐户。  
  
     在 Windows 7、Windows Server 2008 R2 和更高版本中，默认服务帐户是 NT Service\\<service-name>。  
  
     通过使用这些帐户，您可以在不使用密码的情况下工作，因为对于这些帐户不需要密码。 此外，这些帐户仅提供运行 Oracle CDC 服务所需的必要权限。  
  
     可以将本地或域 Windows 帐户用于该服务帐户。 在此情况下，必须为该帐户输入 **“密码”** 。 该帐户可以用于本地主机或域帐户。 请确保在 Windows 控制面板中使用本地服务更改帐户时更新密码。  
  
 **服务器名称**：选择要连接到的目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例（例如 \\\\<computer_name>\\<instance_name>）。 默认情况下，显示上次连接到的服务器实例。  
  
 **身份验证**  
 选择以下方案之一：  
  
-   **Windows 身份验证**：如果您选择此选项，则 Oracle CDC 服务将使用服务帐户标识连接到目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例正在不同的计算机上运行，则必须将 Windows 身份验证用于域帐户。  
  
-   **SQL Server 身份验证**：如果选择此选项，则必须为您要使用的 **登录名键入** “用户名” **和** “密码” [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 Oracle CDC 服务在连接到目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时使用这些凭据。  
  
 Oracle CDC 服务使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名仅需是公共固定服务器角色的成员，无需其他权限。 在添加了新的 Oracle CDC 实例后，该登录名将获取对关联的 **CDC 数据库的** db_owner [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问权限。  
  
 若要创建 Oracle CDC Windows 服务定义，程序需要对关联的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的 MSXDBCDC 数据库具有更新访问权限。 在单击 **“确定”** 后，将出现一个对话框，提示用户输入具有对 MSXDBCDC 数据库的更新访问权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 有关必须在“连接到 SQL Server”对话框中键入的数据的信息，请参阅 [Connection to SQL Server](../../integration-services/change-data-capture/connection-to-sql-server.md)。  
  
 **选项**  
 单击箭头可以查看要配置的可用选项。 您可以选择保留这些选项不变，使用其默认值。 可用选项是：  
  
-   **连接超时值**：键入一个时间（秒钟），未超过该时间，Oracle CDC 服务将等待与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的连接，超过该时间后即超时。默认值为 **15**。  
  
-   **执行超时值**：键入一个时间（秒钟），未超过该时间，Oracle CDC Windows 服务将等待命令执行，超过该时间后即超时。默认值为 **30**。  
  
-   **加密连接**：选择 **“加密连接”** 将使用加密连接进行 Oracle CDC 服务和目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间的通信。  
  
-   **高级**：根据需要键入任何其他连接属性。  
  
 **主密码**  
 输入 Oracle CDC Windows 服务用来保护 Oracle 日志挖掘凭据的密码。  
  
 在高可用性配置中，如果在群集中的其他节点上配置了相同服务的其他实例，则也必须使用相同的主密码。 如果您丢失或修改了该主密码，则必须使用 CDC 设计器控制台重新输入在 Oracle CDC 实例数据库中存储的所有日志挖掘密码。  
  
## <a name="see-also"></a>另请参阅  
 [如何创建和编辑 CDC 服务](../../integration-services/change-data-capture/how-to-create-and-edit-a-cdc-service.md)  
  
  
