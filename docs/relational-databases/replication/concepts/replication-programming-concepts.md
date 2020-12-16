---
description: 复制编程概念
title: 复制编程概念 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- replication [SQL Server], planning
- programming [SQL Server replication], planning
- programming [SQL Server replication]
ms.assetid: 2cd846e7-5bf3-4144-8772-703c4f439a2a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 4b80e66ee6f550939599d6811db15839fdbe2287
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477898"
---
# <a name="replication-programming-concepts"></a>复制编程概念
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  在开发使用复制功能的应用程序之前，应该先完成以下常规计划步骤：  
  
1.  定义复制拓扑。  
  
2.  定义应用程序功能。  
  
3.  规划安全性。  
  
4.  选择开发环境。  
  
5.  选择合适的复制编程接口。  
  
 下面，本主题将对这些步骤进行更为详细地说明。 为了阐明计划的过程，本主题还提供了一个示例。  
  
## <a name="defining-the-replication-topology"></a>定义复制拓扑  
 复制编程的第一步是为应用程序定义复制拓扑。 如果您要编写使用现有复制拓扑的应用程序，如访问现有订阅服务器上数据的客户端应用程序，则可以直接跳到下一步。  
  
> [!NOTE]  
>  在某些情况下，部署复制拓扑是应用程序的唯一目的。  
  
 您要定义的复制拓扑取决于多个因素，其中包括：  
  
-   复制的数据是否需要更新以及由谁更新。  
  
-   有关一致性、自主性和延迟性的数据分布需求。  
  
-   复制环境，包括业务用户、技术基础结构、网络和安全性，以及数据特征。  
  
-   复制类型和复制选项。  
  
-   复制拓扑及其如何与复制类型相符。  
  
 如果不熟悉 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制，请参阅[复制类型](../../../relational-databases/replication/types-of-replication.md)。  
  
## <a name="defining-application-functionality"></a>定义应用程序功能  
 定义完复制拓扑后，应确定应用程序提供的功能。 应用程序的功能范围广泛，既可以是对订阅进行同步的脚本，也可以是具有用于配置复制的用户界面的应用程序。 复制支持下列常规编程任务：  
  
-   设置复制。  
  
-   同步订阅服务器。  
  
-   维护复制拓扑。  
  
-   监视复制拓扑。  
  
-   排除复制故障。  
  
 将复制功能与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 提供的其他功能相结合来扩展应用程序也是很常见的。 下表列出了一些您可以在您的复制应用程序中提供的扩展功能。  
  
|功能|示例|  
|-------------------|-------------|  
|使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理对象 (SMO) 进行服务器管理|允许管理员在复制拓扑中附加一个数据库并将其配置为发布服务器的应用程序。|  
|使用 ADO.NET 进行数据访问|用户能够以编程方式脱机访问并更改本地订阅服务器数据库中的复制的销售数据且随后单击按钮即可连接并同步请求订阅的应用程序。|  
  
## <a name="planning-for-security"></a>规划安全性  
 对于任何应用程序，安全性都是非常重要的，应当在编写代码前，就完成安全性规划。 应用程序安全性可划分为三个主要部分：保护数据库、保护复制和编写安全的代码。  
  
 以下主题提供安全性方面的信息：  
  
-   [查看和修改复制安全设置](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
-   [SQL Server 数据库引擎和 Azure SQL Database 的安全中心](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
## <a name="choosing-a-development-environment"></a>选择开发环境  
 开发复制应用程序时，有三种基本开发环境可供选择。 除了一些例外情况，每种开发环境都可以开发相同的功能。 复制应用程序可在以下任一环境中开发：  
  
-   **托管代码**  
  
     利用 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 编程和 .NET 公共语言运行时 (CLR) 的优点的面向对象开发环境。 对于 .NET 开发和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 应用程序，编程环境推荐使用托管代码。 使用托管复制接口可以在无须了解 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 的情况下以面向对象的方式进行复制管理编程，而且它还可在运行复制代理时提供某些脚本所不具备的回调功能。 托管代码是开发可重用组件和用户界面应用程序的最佳环境。  
  
-   **脚本**  
  
     简单应用程序，执行一系列命令，如 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本中的复制系统存储过程或批处理文件中的命令。 您可以使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 进程内托管提供程序在托管环境中执行脚本，使用托管复制接口可以实现同样的功能，而且该接口还提供回调功能。 脚本是执行运行次数少且无需回调功能的任务的最佳环境，如安装复制服务器。  
  
-   **本机代码**  
  
     面向对象的开发环境，直接访问系统或 COM 对象，因而不由 CLR 托管代码。 本机代码复制接口是不推荐使用或已停止使用的。 有关详细信息，请参阅 [SQL Server 复制中不推荐使用的功能](../../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)或[复制的向后兼容性](../../../relational-databases/replication/replication-backward-compatibility.md)。  
  
## <a name="choose-the-appropriate-replication-programming-interface"></a>选择合适的复制编程接口  
 计划步骤的最后一步是为所选开发环境选择可实现所需复制功能的合适复制编程接口。 下表显示了可用的复制编程接口。  
  
|接口|环境|使用|  
|---------------|-----------------|----------|  
|[复制管理对象概念](../../../relational-databases/replication/concepts/replication-management-objects-concepts.md)|托管代码|管理、监视和同步。|  
|<xref:Microsoft.SqlServer.Replication>|托管代码|同步。|  
|<xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>|托管代码|创建业务逻辑处理程序，以集成自定义逻辑与合并同步进程。|  
|[复制存储过程 (Transact-SQL)](../../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)|脚本编写|管理和监视。|  
|[Replication Agent Executables Concepts](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)|脚本编写|同步。|  
  
## <a name="example"></a>示例  
 在 [!INCLUDE[ssSampleDBCoShort](../../../includes/sssampledbcoshort-md.md)] 中，需要为全球 200 位销售代表发布数据。 销售代表经常出差，需要使用便携式计算机或个人数字助理 (PDA) 更改客户数据以及添加新订单。 这些更改需要在销售代表将便携式计算机连接到网络时，与发布服务器进行同步。  
  
 对于此应用程序，计划步骤可如下：  
  
1.  此应用程序的复制拓扑已经存在。 但必须在客户端创建一个新的请求订阅。 应使用参数化的筛选器进行发布，以便为每位销售代表复制一组唯一数据。  
  
2.  除了销售应用程序所需的典型数据访问外，此应用程序还应允许销售人员通过单击按钮来按需同步请求订阅。 由于销售代表要安装并运行该应用程序，因此该应用程序还要能在客户端配置订阅并应用初始快照。 作为可选功能，该应用程序还可以使用 Windows 提供的用于检测无线网络连接的基础结构，在检测到无线连接时自动同步订阅。  
  
3.  遵守所有复制安全指南，包括连接发布服务器时使用 Windows 身份验证和虚拟专用网 (VPN)。 若要实现 Web 同步，请使用传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”）连接。 有关详细信息，请参阅 [Configure Web Synchronization](../../../relational-databases/replication/configure-web-synchronization.md)。  
  
4.  为了利用 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 的功能，应使用托管代码语言开发应用程序。  
  
5.  对于这些要求，复制管理对象 (RMO) 托管接口可提供此应用程序所需的所有复制功能。  
  
 在可下载的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的 AdventureWorks 示例应用程序中已实现此示例方案。  
  
  
