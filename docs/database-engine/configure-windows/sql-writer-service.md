---
title: SQL 编写器服务 | Microsoft Docs
description: 了解 SQL 编写器服务。 了解它如何通过卷影复制服务框架在 SQL Server 中提供附加的备份和还原功能。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- VDI [SQL Server]
- restoring [SQL Server], SQL Writer Service
- backups [SQL Server], while SQL Server running
- Volume Shadow Copy Service
- volume backups while running [SQL Server]
- Virtual Backup Device Interface [SQL Server]
- SQL Writer Service
- services [SQL Server], SQL Writer
- MSDE Writer
- VSS
ms.assetid: 0f299867-f499-4c2a-ad6f-b2ef1869381d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8bd9e1cacb36ce4906e30c2b8030b5e6b0b09787
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170909"
---
# <a name="sql-writer-service"></a>SQL 编写器服务
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  SQL 编写器服务通过卷影复制服务框架，提供了用来备份和还原 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的附加功能。  
  
 SQL 编写器服务是自动安装的。 在卷影复制服务 (VSS) 应用程序请求备份或还原时，必须运行该服务。 若要配置该服务，请使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 服务小程序。 SQL 编写器服务可安装在所有操作系统上。  
  
## <a name="purpose"></a>目的  
 在运行时， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 会锁定数据文件并具有独占访问权限。 如果 SQL 编写器服务没有运行，Windows 中运行的备份程序将不能访问数据文件，而且必须使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 才能进行备份。  
  
 使用 SQL 编写器服务，可以使 Windows 备份程序在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 运行时复制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件。  
  
## <a name="volume-shadow-copy-service"></a>卷影复制服务  
 VSS 是实现某一框架的一组 COM API，使得在系统中的应用程序连续写入卷的同时，能够进行卷备份。 VSS 具有一致的接口，使得更新磁盘数据的用户应用程序（编写器）和备份应用程序的用户应用程序（请求程序）之间能够协同工作。  
  
 VSS 可捕获和复制正在运行的系统（尤其是服务器）的稳定映像以进行备份，而且不会过度降低它们所提供服务的性能和稳定性。 有关 VSS 的详细信息，请参阅 Windows 文档。  

> [!NOTE]
> 若要使用 VSS 来备份托管基本可用性组的虚拟机，且虚拟机当前托管的数据库处于次要状态，那么自 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 CU2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU9 起，这些数据库 *不会* 与虚拟机一起备份。  这是因为，基本可用性组不支持在次要副本上备份数据库。  如果低于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的这些版本，不仅无法备份，还会看到错误消息。
  
## <a name="virtual-backup-device-interface-vdi"></a>虚拟备份设备接口 (VDI)  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供称为虚拟备份设备接口 (VDI) 的 API，使独立软件供应商能够将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 集成到他们的产品中来支持备份和还原操作。 这些 API 能够提供非常高的可靠性和极佳的性能，并支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的所有备份与还原功能，包括所有的热备份和快照备份功能。 如果第三方供应商应用程序请求快照 (VSS) 备份，则 SQL 编写器服务将调用 VDI API 函数以执行实际备份。 请注意，VDI API 独立于 VSS，并经常用于不使用 VSS API 的软件解决方案中。
  
## <a name="permissions"></a>权限  
 SQL 编写器服务必须以 **Local System** 帐户运行。 SQL 编写器服务使用 **NT Service\SQLWriter** 登录以连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 使用 **NT Service\SQLWriter** 登录可使 SQL 编写器进程以较低的特权等级（指定为 **no login** 的帐户）运行，以提高安全性。 如果禁用 SQL 编写器服务，则依赖 VSS 快照的所有实用程序（如系统中心数据保护管理器，及某些其他的第三方产品）可能会出错，甚至更糟 — 获得的可能是不一致的数据库备份。 如果运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的系统及主机系统（在运行虚拟机的情况下）均无需使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 以外的任何备份，则可安全地禁用 SQL 编写器服务并移除其登录项。  注意：SQL 编写器服务可能会被系统或卷级备份（无论该备份是否直接基于快照）调用。 某些系统备份产品使用 VSS 来避免被已打开或已锁定的文件阻止访问。 需要在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中提升 SQL 编写器服务的权限，这是因为，在活动期间，它会简单地冻结 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的所有 I/O。  
  
## <a name="features"></a>功能  
 SQL 编写器支持：  
  
-   完整数据库备份和还原，包括全文目录  
  
-   差异备份和还原  
  
-   移动式还原  
  
-   数据库重命名  
  
-   仅复制备份  
  
-   自动恢复数据库快照  
  
 SQL 编写器不支持：  
  
-   日志备份  
  
-   文件和文件组备份  
  
-   页面还原  
  
## <a name="remarks"></a>备注
SQL 编写器服务是独立于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 引擎的服务，由不同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和同一台服务器上的不同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例共同使用。  SQL 编写器服务文件随附在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装包中，并标有与所随附 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 引擎相同的版本号。  如果服务器上安装了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新实例或现有实例已升级，且安装或升级的实例的版本号高于服务器上现有 SQL 编写器服务的版本号，这个文件就会替换安装包中的现有文件。  请注意，如果 SQL 编写器服务已通过 Service Pack 或累积更新进行更新，且安装的是 RTM 版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，便可将新版 SQL 编写器服务替换为旧版服务，前提是安装文件的主版本号更高。  例如，SQL 编写器服务在 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 CU2 中进行了更新。  如果此实例升级到 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] RTM，那么已更新的 SQL 编写器服务就会替换为旧版服务。  在此示例中，必须将最新 CU 应用于新实例，才能获取新版 SQL 编写器服务。

