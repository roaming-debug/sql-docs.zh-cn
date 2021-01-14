---
title: 数据库要求
description: 使用 Master Data Services 配置管理器创建和配置 Master Data Services 数据库，该数据库存储所有主数据。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: fe731839-c5c4-4884-bb6a-644eca28bb30
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 21224e52ae9109f5404fc6a6ee11ad59e525995b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171169"
---
# <a name="database-requirements-master-data-services"></a>数据库要求 (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  所有主数据都存储于 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库中。 承载此数据库的计算机必须运行的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。  
  
 使用 [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] 在本地或远程计算机上创建和配置 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库。 如果您将数据库从一个环境移到另一个环境，则可以通过将 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] Web 服务和 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 与其新位置中的数据库相关联，在新环境中维护这些信息。  
  
> [!NOTE]  
>  您安装 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 组件的任何计算机必须具有许可证。 有关详细信息，请参阅“最终用户许可协议 (EULA)”。  
  
## <a name="requirements"></a>要求  
 在创建 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库之前，请确保满足以下要求。  
  
### <a name="sql-server-edition"></a>SQL Server 版本  
 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库可以承载在以下版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]上：  
  
 
-   [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Enterprise（64 位）x64  
  
-   [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Developer（64 位）x64  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Business Intelligence（64 位）x64  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Enterprise（64 位）x64  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Developer（64 位）x64  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Business Intelligence（64 位）x64  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise（64 位）x64 - 仅可从 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Enterprise 升级  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Developer（64 位）x64  
  
-   Microsoft SQL Server 2008 R2 Enterprise（64 位）x64  
  
-   Microsoft SQL Server 2008 R2 Developer (64-bit) x64  
  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。 
  
### <a name="operating-system"></a>操作系统  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]支持的 Windows 操作系统和其它要求的信息，请参阅 [安装 SQL Server 2016 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。  
  
### <a name="accounts-and-permissions"></a>帐户和权限  
  
|类型|说明|  
|----------|-----------------|  
|用户帐户|在 [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]中，可以使用 Windows 帐户或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例以承载 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库。 用户帐户必须属于  实例上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. 有关 **sysadmin** 角色的详细信息，请参阅 [服务器级别角色](../../relational-databases/security/authentication-access/server-level-roles.md)。|  
|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 管理员帐户|创建 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库时，必须指定要作为 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 系统管理员的域用户帐户。 对于与此数据库关联的所有 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序，此用户可以更新所有模型以及所有功能区域中的所有数据。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../../master-data-services/administrators-master-data-services.md)。|  
  
### <a name="database-backup"></a>数据库备份  
 最佳做法是每天在活动较少时进行完整数据库备份，并根据环境需要更频繁地备份事务日志。 有关数据库备份的详细信息，请参阅[备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)。  
  
## <a name="see-also"></a>另请参阅  
 [安装 Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)   
 [创建 Master Data Services 数据库](../../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [Master Data Services 数据库](../../master-data-services/master-data-services-database.md)   
 ["连接到 Master Data Services 数据库" 对话框](../../master-data-services/connect-to-a-master-data-services-database-dialog-box.md)   
 [创建数据库向导（Master Data Services 配置管理器）](../../master-data-services/create-database-wizard-master-data-services-configuration-manager.md)  
  
