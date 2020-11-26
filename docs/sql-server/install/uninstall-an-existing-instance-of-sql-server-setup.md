---
title: 卸载现有实例
description: 本文介绍如何卸载 SQL Server 的独立实例，这也是系统准备，以便可以重新安装 SQL Server。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- removing instances of SQL Server
- uninstalling instances of SQL Server
- removing SQL Server
- instances of SQL Server, uninstalling
- uninstalling SQL Server
ms.assetid: 3c64b29d-61d7-4b86-961c-0de62261c6a1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 16522114fb7e02517ec7385b6b7c73aa90b4b6b0
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127495"
---
# <a name="uninstall-an-existing-instance-of-sql-server-setup"></a>卸载现有 SQL Server 实例（安装程序）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  本文介绍如何卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的独立实例。 执行本文中提供的步骤，还可以准备系统以便重新安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
 > [!NOTE]
 > 若要卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集，请使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序提供的删除节点功能分别删除每个节点。 有关详细信息，请参阅[在 SQL Server 故障转移群集中添加或删除节点（安装程序）](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)  

## <a name="considerations"></a>注意事项

- 若要卸载 SQL Server，你必须是拥有“作为服务登录”权限的本地管理员。 
- 如果计算机具有所需的 *最小* 物理内存，请将页面文件的大小增至物理内存的两倍。 虚拟内存不足会导致无法完全删除 SQL Server。 
- 在具有多个 SQL Server 实例的系统上，仅在删除 SQL Server 的最后一个实例后，才会卸载 SQL Server Browser 服务。 可从“控制面板”的“程序和功能”中手动删除 SQL Server Browser 服务 。 
- 卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会删除在安装过程期间添加的 tempdb 数据文件。 如果带有 tempdb_mssql_*.ndf 名称模式的文件存在于系统数据库目录中，它们会被删除。 
  

  
## <a name="prepare"></a>准备  
  
1.  **备份您的数据。** 创建所有数据库（包括系统数据库）的[完整备份](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)，或手动将 .mdf 和 .ldf 文件复制到单独的位置。 **master** 数据库包含服务器的所有系统级别信息，例如登录名和架构。 **msdb** 数据库包含作业信息，例如 SQL Server 代理作业、备份历史记录和维护计划。 有关系统数据库的详细信息，请参阅[系统数据库](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)。 
  
    必须保存的文件包括以下数据库文件：  

    * master.mdf
    * msdbdata.mdf
    * Tempdb.mdf
    * mastlog.ldf
    * msdblog.ldf
    * Templog.ldf
    * model.mdf
    * Mssqlsystemresource.mdf
    * ReportServer[$InstanceName]
    * modellog.ldf
    * Mssqlsustemresource.ldf
    * ReportServer[$InstanceName]TempDB

    > [!NOTE]
    > SQL Server Reporting Services 附带 ReportServer 数据库。   

 
1.  停止所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。 建议先停止所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务，然后再卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件的本地安全组。 活动的连接可能会使卸载过程无法成功完成。  
  
1.  **使用具有适当权限的帐户。** 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户或具有同等权限的帐户登录到服务器。 例如，可以使用本地 Administrators 组的成员帐户登录到服务器。  
  
## <a name="uninstall"></a>卸载 

# <a name="windows-10--2016-"></a>[Windows 10/2016 +](#tab/Windows10)

若要从 Windows 10、Windows Server 2016、Windows Server 2019 及更高版本中卸载 SQL Server，请执行以下步骤： 

1. 若要开始删除过程，请从“开始”菜单导航至“设置”，然后选择“应用” 。 
1. 在搜索框中搜索 `sql`。 
1. 选择“Microsoft SQL Server（版本）（位）”。 例如，`Microsoft SQL Server 2017 (64-bit)`。
1. 选择“卸载”。
 
    ![卸载 SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-10.png)

1. 在 SQL Server 对话框弹出窗口中选择“删除”以启动 Microsoft SQL Server 安装向导。 

    ![删除 SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2017.png)
  
1.  在“选择实例”页上，使用下拉框指定要删除的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，或者指定与仅删除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 共享功能和管理工具相对应的选项。 若要继续操作，请选择“下一步”。  
  
1.  在“选择功能”页上，指定要从指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中删除的功能。  
  
1.  在 **“准备删除”** 页上查看要卸载的组件和功能的列表。 单击 **“删除”** 开始卸载  
 
1. 刷新“应用和功能”窗口，以验证是否已成功删除 SQL Server 实例，并确定哪些 SQL Server 组件仍然存在（如有）。 如果需要，也可以通过此窗口删除这些组件。 

# <a name="windows-2008---2012-r2"></a>[Windows 2008 - 2012 R2](#tab/windows2012)

若要从 Windows Server 2008、Windows Server 2012 和 Windows 2012 R2 卸载 SQL Server，请执行以下步骤： 

1. 若要开始删除过程，请导航至“控制面板”，然后选择“程序和功能” 。
1. 右键单击“Microsoft SQL Server（版本）（位）”，然后选择“卸载” 。 例如，`Microsoft SQL Server 2012 (64-bit)`。  
  
    ![卸载 SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-2012.png)

1. 在 SQL Server 对话框弹出窗口中选择“删除”以启动 Microsoft SQL Server 安装向导。 

    ![删除 SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2012.png)
  
1.  在“选择实例”页上，使用下拉框指定要删除的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，或者指定与仅删除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 共享功能和管理工具相对应的选项。 若要继续操作，请选择“下一步”。  
  
1.  在“选择功能”页上，指定要从指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中删除的功能。  
  
1.  在 **“准备删除”** 页上查看要卸载的组件和功能的列表。 单击 **“删除”** 开始卸载  
 
1. 刷新“程序和功能”窗口，以验证是否已成功删除 SQL Server 实例，并确定哪些 SQL Server 组件仍然存在（如有）。 如果需要，也可以通过此窗口删除这些组件。 

---

  
## <a name="in-the-event-of-failure"></a>发生故障时  

如果删除过程失败，请查看 [SQL Server 安装日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)以确定根本原因。 

知识库文章[如何在安装日志文件中识别 SQL Server 安装问题](https://support.microsoft.com/kb/955396/en-us)可帮助进行调查。 尽管它针对 SQL Server 2008，但所述方法适用于 SQL Server 的每个版本。 

  
## <a name="see-also"></a>另请参阅  
 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  
