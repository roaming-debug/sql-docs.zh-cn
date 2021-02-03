---
description: 数据层应用程序
title: 数据层应用程序 | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- designing DACs
- How to [DAC]
- data-tier application [SQL Server], designing
- wizard [DAC]
ms.assetid: a04a2aba-d07a-4423-ab8a-0a31658f6317
author: stevestein
ms.author: sstein
ms.openlocfilehash: 562769cc1f9ce3e895b59811a61026747c0a3724
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251085"
---
# <a name="data-tier-applications"></a>数据层应用程序
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  数据层应用程序 (DAC) 是一个逻辑数据库管理实体，用于定义与用户数据库关联的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象，如表、视图和实例对象（包括登录名）。 DAC 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库部署的一个自包含单元，它使数据层开发人员和数据库管理员能够将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象打包到一个名为“DAC 包”（也称作 DACPAC）的可移植项目中。  
  
 BACPAC 是一个封装数据库架构以及数据库中存储的数据的相关项目。  
  
## <a name="benefits-of-data-tier-applications"></a>数据层应用程序的优点  
 大部分数据库应用程序的生命周期涉及开发人员和 DBA 共享并交换有关应用程序更新和维护活动的脚本和临时集成说明。 尽管对于少量的数据库，这一点是可接受的，但当数据库的数量、大小和复杂性增大时，它很快会变得不可缩放。  
  
 DAC 是一种数据库生命周期管理和效率工具，可用于进行声明性数据库开发以简化部署和管理。 开发人员可以在 SQL Server Data Tools 数据库项目中创作一个数据库，然后将该数据库生成到 DACPAC 中以便提交给 DBA。 DBA 可使用 SQL Server Management Studio 将 DAC 部署到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] 的测试或生产实例中。 或者，DBA 可以使用 DACPAC 升级之前用 SQL Server Management Studio 部署的数据库。 若要完成生命周期，DBA 可将数据库提取到 DACPAC 中，再将其提交给开发人员以反映测试或生产调整，或进一步更改数据库设计以响应应用程序中的更改。  
  
 DAC 驱动的部署相对于脚本驱动的做法的优点是，该工具可帮助 DBA 标识和验证各个源和目标数据库中的行为。 在升级期间，该工具会在升级可能导致数据丢失时向 DBA 发出警告，并且还会提供升级计划。 DBA 可以评估此计划，然后利用该工具继续进行升级。  
  
 DAC 还支持版本控制以帮助开发人员和 DBA 在数据库的生命周期内维护和管理数据库沿袭。  
  
## <a name="dac-concepts"></a>DAC 概念  
 DAC 简化了支持应用程序的数据层元素的开发、部署和管理：  
  
-   数据层应用程序 (DAC) 是一个逻辑数据库管理实体，用于定义与用户数据库关联的所有 SQL Server 对象（如表、视图和实例对象）。 DAC 是 SQL Server 数据库部署的一个自包含单元，它使数据层开发人员和 DBA 能够将 SQL Server 对象打包到一个名为“DAC 包”的可移植项目或 .dacpac 文件中。  
  
-   对于要被视为 DAC 的 SQL Server 数据库，必须通过用户操作显式注册或通过某项 DAC 操作隐式注册该数据库。 在注册数据库时，DAC 版本和其他属性将记录为数据库的元数据的一部分。 反之，也可以注销数据库并删除其 DAC 属性。  
  
-   通常，DAC 工具可以读取由早期版本的 SQL Server 中的 DAC 工具生成的 DACPAC 文件，并且可以将 DACPAC 部署到早期版本的 SQL Server。 但是，早期版本的 DAC 工具无法读取由更高版本的 DAC 工具生成的 DACPAC 文件。 具体来说：  
  
    -   SQL Server 2008 R2 中引入了 DAC 操作。 除了 SQL Server 2008 R2 数据库以外，这些工具还支持从 SQL Server 2008、SQL Server 2005 和 SQL Server 2000 数据库生成 DACPAC 文件。  
  
    -   除了 SQL Server 2016 数据库以外，SQL Server 2016 附带的这些工具可读取由 SQL Server 2008 R2 或 SQL Server 2012 附带的 DAC 工具生成的 DACPAC 文件。 这包括 SQL Server 2014、2012、2008 R2、2008 和 2005 中的数据库，但 **不** 包括 SQL Server 2000 中的数据库。  
  
    -   SQL Server 2008 R2 中的 DAC 工具无法读取由 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 或  [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中的工具生成的 DACPAC 文件。  
  
-   DACPAC 是一个扩展名为 .dacpac 的 Windows 文件。 该文件支持由表示 DACPAC 源的详细信息的多个 XML 部分、数据库中的对象和其他特征构成的开放格式。 高级用户可使用产品附带的 DacUnpack.exe 实用工具来解压缩该文件，以便更仔细地检查每个部分。  
  
-   用户必须是 dbmanager 角色的成员或分配了 CREATE DATABASE 权限才能创建数据库，包括通过部署 DAC 包来创建数据库   。 用户必须是 dbmanager 角色的成员或分配了 DROP DATABASE 权限才能删除数据库   。  
  
## <a name="dac-tools"></a>DAC 工具  
 可以在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]附带的多个工具中无缝使用 DACPAC。 这些工具可满足将 DACPAC 用作互操作性单元的各个用户角色的需求。  
  
-   应用程序开发人员：  
  
    -   可以使用 SQL Server Data Tools 数据库项目来设计数据库。 成功生成此项目会导致生成 .dacpac 文件中包含的 DACPAC。  
  
    -   可以将 DACPAC 导入到数据库项目中，并继续设计数据库。  
  
        SQL Server Data Tools 还支持本地 DB 以进行未连接的客户端数据库应用程序开发。 开发人员可以拍摄此本地数据库的快照以创建 .dacpac 文件中包含的 DACPAC。  
  
    -   开发人员可单独将一个数据库项目直接发布到数据库，甚至无需生成 DACPAC。 发布操作的行为与使用其他工具执行的部署操作的行为类似。  
  
-   数据库管理员：  
  
    -   可以使用 SQL Server Management Studio 从现有数据库中提取 DACPAC，并且可以执行其他 DAC 操作。  
  
    -   此外，[!INCLUDE[ssSDS](../../includes/sssds-md.md)] 的 DBA 可使用 Azure 门户进行 DAC 操作。  
  
-   独立软件供应商：  
  
    -   托管服务和 SQL Server 的其他数据管理产品可以使用 DACFx API 来进行 DAC 操作。  
  
-   IT 管理员：  
  
    -   IT 系统集成者和管理员可以使用 SqlPackage.exe 命令行工具来进行 DAC 操作。  
  
## <a name="dac-operations"></a>DAC 操作  
 DAC 支持以下操作：  
  
-   **EXTRACT** - 用户可将数据库提取到 DACPAC 中。  
  
-   **DEPLOY** - 用户可将 DACPAC 部署到主机服务器。 在通过可管理性工具（如 SQL Server Management Studio 或 Azure 门户）进行部署时，主机服务器中生成的数据库将隐式注册为数据层应用程序。  
  
-   **REGISTER** - 用户可以将数据库注册为数据层应用程序。  
  
-   **UNREGISTER** - 可以注销之前注册为 DAC 的数据库。  
  
-   **UPGRADE** - 可以使用 DACPAC 升级数据库。 甚至在之前未注册为数据层应用程序的数据库上也支持升级操作，但升级操作的结果是，将隐式注册数据库。  
  
## <a name="bacpac"></a>BACPAC  
 BACPAC 是扩展名为 .bacpac 的 Windows 文件，用于封装数据库的架构和数据。 BACPAC 的主要用例是将数据库从一台服务器移到另一台服务器上（或[将数据库从本地服务器移到云中](/azure/azure-sql/database/migrate-to-database-from-sql-server)），同时以开放格式对现有数据库进行存档。  
 与 DACPAC 类似，BACPAC 文件是开放的 - BACPAC 的架构内容与 DACPAC 的架构内容相同。 BACPAC 中的数据是以 JSON 格式进行存储。  
  
 虽然 DACPAC 和 BACPAC 相似，但它们面向的方案不同。 DACPAC 侧重于捕获和部署架构，包括升级现有数据库。 DACPAC 的主要用例是将严密定义的架构依次部署到开发、测试和生产环境中。 用例还包括与此相反的情况：捕获生产的架构并将其应用回测试和开发环境。  
  
 相比之下，BACPAC 侧重于捕获架构和数据，支持以下两种主要操作：  
  
-   **EXPORT** - 用户可以将数据库的架构和数据导出到 BACPAC。  
  
-   **IMPORT** - 用户可以将架构和数据导入到主机服务器上的新数据库中。  
  
 数据库管理工具 SQL Server Management Studio、Azure 门户和 DACFx API 支持这两种功能。  
  
## <a name="permissions"></a>权限  
 用户必须是 **dbmanager** 角色的成员或分配了 **CREATE DATABASE** 权限才能创建数据库，包括通过部署 DAC 包来创建数据库。 用户必须是 **dbmanager** 角色的成员或分配了 **DROP DATABASE** 权限才能删除数据库。  
  
## <a name="data-tier-application-tasks"></a>数据层应用程序任务  
  
|任务|主题链接|  
|----------------------|-----------|  
|介绍如何使用 DAC 包文件创建一个新的 DAC 实例。|[部署数据层应用程序](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)|  
|介绍如何使用新的 DAC 包文件将某一实例升级到该 DAC 的新版本。|[升级数据层应用程序](../../relational-databases/data-tier-applications/upgrade-a-data-tier-application.md)|  
|介绍如何删除 DAC 实例。 您还可以选择是分离或删除关联数据库，还是保持数据库不变。|[删除数据层应用程序](../../relational-databases/data-tier-applications/delete-a-data-tier-application.md)|  
|介绍如何使用 SQL Server 实用工具查看当前部署的 DAC 的运行状况。|[监视数据层应用程序](../../relational-databases/data-tier-applications/monitor-data-tier-applications.md)|  
|介绍如何创建包含 DAC 中数据和元数据的存档的 .bacpac 文件。|[导出数据层应用程序](../../relational-databases/data-tier-applications/export-a-data-tier-application.md)|  
|介绍如何使用 DAC 存档文件 (.bacpac) 来执行 DAC 的逻辑还原，或者将 DAC 迁移到其他[!INCLUDE[ssDE](../../includes/ssde-md.md)]或 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 实例。|[导入 BACPAC 文件以创建新的用户数据库](../../relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database.md)|  
|介绍如何导入 BACPAC 文件来在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中创建新的用户数据库。|[从数据库中提取 DAC](../../relational-databases/data-tier-applications/extract-a-dac-from-a-database.md)|  
|介绍如何将现有数据库提升为 DAC 实例。 DAC 定义将生成并存储于系统数据库中。|[将数据库注册为 DAC](../../relational-databases/data-tier-applications/register-a-database-as-a-dac.md)|  
|介绍如何查看 DAC 包的内容以及在生产系统中使用该包之前 DAC 升级将执行的操作。|[验证 DAC 包](../../relational-databases/data-tier-applications/validate-a-dac-package.md)|  
|介绍如何将 DAC 包的内容放置于一个文件夹中，数据库管理员可以查看该文件夹，以便在将该 DAC 部署到生产服务器之前查看其内容。|[解压缩 DAC 包](../../relational-databases/data-tier-applications/unpack-a-dac-package.md)|  
|介绍如何使用向导部署现有数据库。 此向导使用 DAC 执行部署。|[使用 DAC 部署数据库](../../relational-databases/data-tier-applications/deploy-a-database-by-using-a-dac.md)|  
  
## <a name="see-also"></a>另请参阅  
 [对 SQL Server 对象和版本的 DAC 支持](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110))  
  
