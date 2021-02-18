---
title: Distributed Replay 安全性
titleSuffix: SQL Server Distributed Replay
description: 本文介绍了 SQL Server Distributed Replay 的安全配置步骤，以及有关数据保护和删除步骤的重要注意事项。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 7e2e586d-947d-4fe2-86c5-f06200ebf139
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: d7ae823b6d5839ce4fdc966af48c0634a37b1e2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345943"
---
# <a name="distributed-replay-security"></a>分布式重播安全性

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

在安装和使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 功能之前，应查看本主题中的重要安全信息。 本主题介绍了您使用分布式重播之前所需的安装后安全配置步骤。 本主题还介绍了与数据保护相关的重要注意事项和重要删除步骤。  
  
## <a name="user-and-service-accounts"></a>用户帐户和服务帐户  
 下表介绍了用于分布式重播的帐户。 在安装分布式重播功能后，您必须分配控制器和客户端服务帐户将运行为的安全主体。 因此，建议您先配置适当的域用户帐户，然后再安装分布式重播功能。  
  
|用户帐户|要求|  
|------------------|------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式重播控制器服务帐户|可以是域用户帐户或本地用户帐户。 如果使用本地用户帐户，则管理工具、控制器和客户端都必须在同一台计算机上运行。<br /><br /> **\*\* 安全说明 \*\*** 建议不要将该帐户设置为 Windows 本地管理员组的成员。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式重播客户端服务帐户|可以是域用户帐户或本地用户帐户。 如果使用本地用户帐户，则控制器、客户端和目标 SQL Server 都必须在同一台计算机上运行。<br /><br /> **\*\* 安全说明 \*\*** 建议不要将该帐户设置为 Windows 本地管理员组的成员。|  
|用于运行分布式重播管理工具的交互式用户帐户|可以是本地用户帐户或域用户帐户。 若要使用本地用户帐户，管理工具和控制器必须在同一台计算机上运行。|  
  
 **重要说明**：在你配置 Distributed Replay 控制器时，可以指定将用于运行 Distributed Replay 客户端服务的一个或多个用户帐户。 下面是支持的帐户的列表：  
  
-   域用户帐户  
  
-   用户创建的本地用户帐户  
  
-   管理员  
  
-   虚拟帐户和 MSA（托管服务帐户）  
  
-   Network Services、Local Services 和 System  
  
 不接受组帐户（本地或域）和其他内置帐户（如 Everyone）。  
  
 在安装分布式重播功能之后，若要设置服务帐户或其密码，您可以使用 Windows 服务工具。 若要更改与分布式重播控制器或客户端服务关联的服务帐户，请按以下步骤执行操作：  
  
1.  请根据所用操作系统执行以下两项操作之一：  
  
    -   单击“开始”，在“搜索”框中键入 **services.msc**，然后按 Enter。  
  
    -   依次单击 **“开始”** 、 **“运行”** ，键入 **services.msc**，然后按 Enter。  
  
2.  在“服务”对话框中，右键单击要配置的服务，然后单击“属性”。  
  
3.  在 **“登录”** 选项卡上，单击 **“本帐户”** 。  
  
4.  配置要使用的用户帐户。  
  
## <a name="file-and-folder-permissions"></a>文件和文件夹权限  
 指定服务帐户后，您必须向这些服务帐户授予必要的文件和文件夹权限。 根据下表配置文件和文件夹权限：  
  
|帐户|文件夹权限|  
|-------------|------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式重播控制器服务帐户|`<Controller_Installation_Path>\DReplayController` （读取、写入、删除）<br /><br /> `DReplayServer.xml` 文件（读取、写入）|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式重播客户端服务帐户|`<Client_Installation_Path>\DReplayClient` （读取、写入、删除）<br /><br /> `DReplayClient.xml` 文件（读取、写入）<br /><br /> 工作目录和结果目录，在客户端配置文件中分别由 `WorkingDirectory` 元素和 `ResultDirectory` 元素指定。 （读取、写入）|  
  
## <a name="dcom-permissions"></a>DCOM 权限  
 DCOM 用于控制器和管理工具之间以及控制器和所有客户端之间的远程过程调用 (RPC) 通信。 在安装分布式重播功能之后，您必须在控制器上配置计算机范围的和应用程序特定的 DCOM 权限。  
  
 若要配置控制器 DCOM 权限，请按以下步骤执行操作：  
  
1.  **打开 dcomcnfg.exe、组件服务管理单元**：这是用于配置 DCOM 权限的工具。  
  
    1.  在控制器计算机上，单击 **“开始”** 。  
  
    2.  在“搜索”框中键入 **dcomcnfg.exe** 。   
  
    3.  按 Enter。  
  
2.  **配置计算机范围内的 DCOM 权限**：为下表中列出的每个帐户授予相应的计算机范围的 DCOM 权限。 有关如何设置计算机范围的权限的详细信息，请参阅[清单：管理 DCOM 应用程序](/windows/win32/com/setting-machine-wide-security-using-dcomcnfg)。  
  
3.  **配置应用程序特定的 DCOM 权限**：为下表中列出的每个帐户授予相应的应用程序特定的 DCOM 权限。 控制器服务的 DCOM 应用程序名称为 **DReplayController**。 有关如何设置应用程序特定的权限的详细信息，请参阅[清单：管理 DCOM 应用程序](/windows/win32/com/setting-processwide-security-using-dcomcnfg)。  
  
 下表介绍了管理工具交互式用户帐户和客户端服务帐户所需的 DCOM 权限：  
  
|Feature|帐户|控制器上所需的 DCOM 权限|  
|-------------|-------------|---------------------------------------------|  
|分布式重播管理工具|交互式用户帐户|本地访问<br /><br /> 远程访问<br /><br /> 本地启动<br /><br /> 远程启动<br /><br /> 本地激活<br /><br /> 远程激活|  
|Distributed Replay 客户端|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式重播客户端服务帐户|本地访问<br /><br /> 远程访问<br /><br /> 本地启动<br /><br /> 远程启动<br /><br /> 本地激活<br /><br /> 远程激活|  
  
> [!IMPORTANT]  
>  若要帮助防止恶意查询或拒绝服务攻击，请确保您仅对客户端服务帐户使用受信任的用户帐户。 此帐户将能够连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]目标实例并针对该实例重播工作负荷。  
  
## <a name="sql-server-permissions"></a>SQL Server 权限  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式重播客户端服务帐户用于连接工作负荷的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]目标实例。 这些连接仅支持 Windows 身份验证模式。  
  
 在一组计算机上安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式重播客户端服务之后，必须向用于这些服务帐户的安全主体授予你打算针对其重播跟踪工作负荷的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的 sysadmin 服务器角色。 在分布式重播安装期间，不会自动执行此步骤。  
  
## <a name="data-protection"></a>数据保护  
 在分布式重播环境中，将授予以下用户帐户对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的目标服务器实例、输入跟踪数据和结果跟踪文件的完全访问权：  
  
-   用于运行该管理工具的交互式用户帐户。  
  
-   控制器服务帐户。  
  
-   客户端服务帐户。  
  
-   控制器上的本地 Administrators 组的成员。  
  
-   客户端上的本地 Administrators 组的成员。  
  
    > [!IMPORTANT]  
    >  这些帐户具有对包含在分布式重播功能所使用的跟踪数据文件、中间数据文件、调度数据文件或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件中的任何个人身份信息 (PII) 或敏感信息的完全访问权。  
  
 建议您采取以下安全防范措施：  
  
-   将输入跟踪数据、输出跟踪结果和数据库文件存储在使用 NTFS 文件系统 (NTFS) 的位置，并应用适当的访问控制列表 (ACL)。 如果需要，可对存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机上的数据进行加密。 请注意，不将 ACL 应用于跟踪文件，也不会执行数据屏蔽或模糊处理。 您应在使用完这些文件后迅速将其删除。  
  
-   将适当的 ACL 和保留策略应用于分布式重播功能所生成的所有中间文件和调度文件。  
  
-   使用传输层安全性 (TLS)（以前称为“安全套接字层 (SSL)”）来保护网络传输。  
  
## <a name="important-removal-steps"></a>重要删除步骤  
 建议您仅在测试环境中使用分布式重播功能。 在您完成测试之后，并在您为其他任务设置这些计算机之前，请确保您已完成以下操作：  
  
-   卸载分布式重播功能，并从控制器和所有客户端中删除相关的配置文件。  
  
-   删除用于测试的任何跟踪数据库文件、中间数据库文件、调度数据库文件和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库文件。 中间文件和调度文件分别存储在控制器上的工作目录和客户端上的工作目录中。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 分布式重播](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [安装 Distributed Replay - 概述](../../tools/distributed-replay/install-distributed-replay-overview.md)  
  
