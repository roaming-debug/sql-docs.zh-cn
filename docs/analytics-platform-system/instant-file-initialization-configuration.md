---
title: 配置即时文件初始化
description: 配置分析平台系统上的即时文件初始化。 即时文件初始化是一项 SQL Server 功能，使数据文件操作能够更快地运行。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 6843efa430fcd43149d048c9d21c5120954ab896
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637813"
---
# <a name="instant-file-initialization-configuration"></a>即时文件初始化配置
即时文件初始化是一项 SQL Server 功能，使数据文件操作能够更快地运行。 选中此复选框可打开的即时文件初始化，将提高 SQL Server PDW 的性能。 但是，如果这会给您的企业带来安全风险，则不要选中该框。  
  
> [!IMPORTANT]  
> 启用即时文件初始化时，SQL Server 不会覆盖包含零的已删除位。  如果未经授权的用户获取对已删除数据的访问权限，则此行为可能会产生安全漏洞。 但是，通过确保 SQL Server 数据库和备份文件始终连接到 SQL Server 的实例，SQL Server PDW 可降低这种风险;只有 SQL Server 的服务帐户和本地管理员才能访问 SQL Server PDW 上已删除的数据。  
  
当启用 TDE 时，即时文件初始化功能不可用。  
  
## <a name="add-permission-for-the-backup-account"></a>添加备份帐户权限  
备份过程需要 (可以访问备份存储位置的 Windows 用户帐户) 的网络凭据。 通过使用 [sp_pdw_add_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md) 过程授权 PDW 使用帐户。 有关整个备份过程，请参阅 [备份数据库](../t-sql/statements/backup-transact-sql.md) 。 若要使用即时文件初始化，必须向备份帐户授予 `Perform volume maintenance tasks` 权限。  
  
1.  在备份服务器上，打开 **本地安全策略** 应用程序 (`secpol.msc`) 。  
  
2.  在左侧窗格中，展开“本地策略” ，然后单击“用户权限指派” 。  
  
3.  在右侧窗格中，双击“执行卷维护任务”。  
  
4.  单击“添加用户或组”  ，添加用于备份的任何用户帐户。  
  
5.  单击“应用” ，然后关闭所有“本地安全策略”  对话框。  
  
## <a name="to-turn-instant-file-initialization-on-or-off"></a>启用或禁用即时文件初始化  
  
1.  启动 Configuration Manager。 有关详细信息，请参阅 [启动 Configuration Manager &#40;Analytics 平台系统&#41;](launch-the-configuration-manager.md)。  
  
2.  在 Configuration Manager 的左窗格中，单击 " **即时文件初始化**"。  
  
3.  若要启用即时文件初始化，请选中 " **启用所有节点上的即时文件初始化**" 旁边的框。 若要禁用即时文件初始化，请清除 " **启用所有节点上的即时文件初始化**" 旁边的复选框。  
  
    > [!WARNING]  
    > 关闭即时文件初始化时，上述功能的安全注意事项可能仍适用于启用即时文件初始化时删除的文件。  
  
4.  单击“应用”  。 此更改将在下一次重新启动设备服务时，传播 SQL Server PDW 上的 SQL Server 实例。 若要立即重新启动设备服务，请参阅 [PDW 服务状态 &#40;分析平台系统&#41;](pdw-services-status.md)。  
  
5.  你可能需要重复上述步骤以 **添加备份帐户的权限** ，以删除 " **执行卷维护任务** " 权限。  
  
![DWConfig 工具 PDW 即时文件初始化](./media/instant-file-initialization-configuration/SQL_Server_PDW_DWConfig_ApplPDWInstant.png "SQL_Server_PDW_DWConfig_ApplPDWInstant")  
  
有关即时文件初始化的详细信息，请参阅 [即时文件初始化](/previous-versions/sql/sql-server-2008-r2/ms175935(v=sql.105))。  
