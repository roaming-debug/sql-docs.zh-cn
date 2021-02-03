---
description: 数据收集器的安全性
title: 数据收集器的安全性 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- data collection [SQL Server]
- security [data collector]
- data collector [SQL Server], security
ms.assetid: e75d6975-641e-440a-a642-cb39a583359a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5e968978dd641f78dfb17d515ab07e3b7e27485
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250131"
---
# <a name="data-collector-security"></a>数据收集器的安全性
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  数据收集器使用由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理实现的基于角色的安全模式。 在此模式下，数据库管理员能够在只拥有执行相应任务所需的权限的安全上下文中运行各种数据收集器任务。 执行涉及内部表的操作时也采用这种方法，内部表只能通过存储过程或视图进行访问。 不会向内部表授予任何权限。 但是，还要对使用存储过程或视图访问表的用户进行权限检查。  
  
> [!IMPORTANT]  
>  此安全模式的另一个关键点是同心权限。 在同心权限下，特权较高的角色继承特权较低的角色对对象（包括警报、运算符、作业、计划和代理）的权限。 有关详细信息，请参阅 [SQL Server Agent Fixed Database Roles](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 以下各部分对数据收集安全性作了一般性介绍，还介绍了为使用户能够配置和使用数据收集器并执行与管理数据仓库关联的任务而必须授予他们的角色。  
  
## <a name="general-security"></a>一般安全性  
 数据收集器的安装应依照指定用于 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]的明文标准进行。  
  
### <a name="network-security"></a>网络安全  
 可以在目标实例、与配置服务器关联的关系实例、正在运行的收集组以及承载管理数据仓库的服务器之间传递敏感信息。  
  
 为保护通过网络传输的所有数据，实施了标准安全机制，如针对 [!INCLUDE[tsql](../../includes/tsql-md.md)]的协议加密。  
  
## <a name="permissions-for-configuring-and-using-the-data-collector"></a>用于配置和使用数据收集器的权限  
 用户必须是为数据收集器提供的一个或多个固定数据库角色的成员，具体的角色取决于任务。 按照从最高特权访问到最低特权访问的顺序，角色如下所示：  
  
-   **dc_admin**  
  
-   **dc_operator**  
  
-   **dc_proxy**  
  
 这些角色存储在 msdb 数据库中。 默认情况下，任何用户都不是这些数据库角色的成员。 这些角色的用户成员资格必须显式授予。  
  
 作为 **sysadmin** 固定服务器角色成员的用户具有对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理对象和数据收集器视图的完全访问权限。 但是，需要将这些用户显式添加到数据收集器角色中。  
  
> [!IMPORTANT]  
>  db_ssisadmin 角色和 dc_admin 角色的成员可以将其特权提升为 sysadmin。 因为这些角色可以修改 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包，而 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的 sysadmin 安全上下文可以执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包，所以可以实现特权提升。 若要在运行维护计划、数据收集组和其他 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包时防止此权限提升，请将运行包的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业配置为使用具有有限权限的代理帐户，或只将 sysadmin 成员添加到 db_ssisadmin 和 dc_admin 角色。  
  
### <a name="dc_admin-role"></a>dc_admin 角色  
 分配给 **dc_admin** 角色的用户对服务器实例上的数据收集器配置拥有全部管理员访问权限（创建、读取、更新和删除）。 此角色的成员可执行以下操作：  
  
-   设置收集器级别的属性。  
  
-   添加新收集组。  
  
-   安装新的收集类型。  
  
-   执行 **dc_operator** 角色有权执行的所有操作。  
  
 **dc_admin** 角色是以下角色的成员：  
  
-   **SQLAgentUserRole**。 创建计划和运行作业时需要此角色。  
  
    > [!NOTE]  
    >  为数据收集器创建的代理必须授予对 **dc_admin** 的访问权限，才能在需要代理的任何作业步骤中创建和使用这些代理。  
  
-   **dc_operator**。 **dc_admin** 的成员继承授予 **dc_operator** 的权限。  
  
### <a name="dc_operator-role"></a>dc_operator 角色  
 **dc_operator** 角色的成员拥有读取和更新访问权限。 此角色支持与运行和配置收集组相关的操作任务。 此角色的成员可执行以下操作：  
  
-   启动或停止收集组。  
  
-   枚举现有的收集组。  
  
-   查看与收集组关联的详细信息（例如收集项和收集频率）。  
  
-   更改现有收集组的上载频率。  
  
-   更改属于现有收集组的收集项的收集频率。  
  
 **dc_operator** 角色是枚举和查看数据收集器包所需的以下角色的成员：  
  
-   **db_ssisltduser**  
  
-   **db_ssisoperator**  
  
 有关详细信息，请参阅 [Integration Services Roles（SSIS 服务）](../../integration-services/security/integration-services-roles-ssis-service.md)。  
  
### <a name="dc_proxy-role"></a>dc_proxy 角色  
 **dc_proxy** 角色的成员对数据收集器收集组和收集器级别的属性拥有读取访问权限。 此角色的成员还可以执行它们所拥有的作业和创建以现有代理帐户运行的作业步骤。  
  
 此角色的成员可执行以下操作：  
  
-   查看收集组配置信息（例如收集项的输入参数以及这些项的收集频率）。  
  
-   获取只能由已签名的存储过程访问的内部加密信息（例如用于上载数据的数据仓库连接信息）。  
  
-   记录收集组的运行时事件。  
  
 **dc_proxy** 角色是枚举和查看数据收集器包所需的以下角色的成员：  
  
-   **db_ssisltduser**。  
  
-   **db_ssisoperator**  
  
 有关详细信息，请参阅 [Integration Services Roles（SSIS 服务）](../../integration-services/security/integration-services-roles-ssis-service.md)。  
  
## <a name="permissions-for-configuring-and-using-the-management-data-warehouse"></a>用于配置和使用管理数据仓库的权限  
 用户必须是为访问管理数据仓库而提供的一个或多个固定数据库角色的成员，具体的角色取决于任务。 按照从最高特权访问到最低特权访问的顺序，角色如下所示：  
  
-   **mdw_admin**  
  
-   **mdw_writer**  
  
-   **mdw_reader**  
  
 这些角色存储在 msdb 数据库中。 默认情况下，任何用户都不是这些数据库角色的成员。 这些角色的用户成员资格必须显式授予。  
  
 作为 **sysadmin** 固定服务器角色成员的用户拥有对数据收集器视图的完全访问权限。 但是，需要将这些用户显式添加到数据库角色以执行其他操作。  
  
### <a name="mdw_admin-role"></a>mdw_admin 角色  
 **mdw_admin** 角色的成员对管理数据仓库拥有读取、写入、更新和删除访问权限。  
  
 此角色的成员可执行以下操作：  
  
-   在需要时更改管理数据仓库的架构（例如在安装新的收集类型时添加新表）。  
  
    > [!NOTE]  
    >  如果架构发生更改，用户还必须是 **dc_admin** 角色的成员才能安装新的收集器类型，因为执行此操作需要拥有更新 msdb 中数据收集器配置的权限。  
  
-   对管理数据仓库运行维护作业（例如存档或清除）。  
  
### <a name="mdw_writer-role"></a>mdw_writer 角色  
 **mdw_writer** 角色的成员可向管理数据仓库上载和写入数据。 将数据存储到管理数据仓库中的任何数据收集器都必须是此角色的成员。  
  
### <a name="mdw_reader-role"></a>mdw_reader 角色  
 **mdw_reader** 角色的成员对管理数据仓库拥有读取访问权限。 由于此角色的用途在于通过提供对历史数据的访问来支持故障排除，因此该角色的成员无法查看管理数据仓库架构的其他元素。  
  
## <a name="see-also"></a>另请参阅  
 [实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)  
  
  
