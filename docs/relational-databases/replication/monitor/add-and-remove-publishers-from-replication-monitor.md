---
title: 从复制监视器添加和删除发布服务器 (SSMS)
description: 了解如何从 SQL Server Management Studio (SSMS) 中的复制监视器添加和删除发布服务器。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Replication Monitor, adding and removing Publishers
ms.assetid: fa36c4b4-bfa5-494e-92e3-07a02d7332c3
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 7c6121b21be6a0a0d49581ebfcc4d637e7fd2f27
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481018"
---
# <a name="add-and-remove-publishers-from-replication-monitor"></a>从复制监视器中添加和删除发布服务器
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  如果从中启动复制监视器的服务器是发布服务器，则会自动将其添加到监视器中。 其他发布服务器可以通过 **“添加发布服务器”** 对话框来添加。 添加发布服务器后，该服务器便会显示在监视器左窗格中的某个组中。 默认情况下，包括 **“我的发布服务器”** 组，但还可以创建新组来管理一个或多个复制拓扑。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
### <a name="to-add-a-sql-server-publisher"></a>添加 SQL Server 发布服务器  
  
1.  在左窗格中右键单击 **“复制监视器”** 节点或发布服务器组节点，再单击 **“添加发布服务器”** 。  
  
2.  在 **“添加发布服务器”** 对话框中单击 **“添加”** ，再单击 **“添加 SQL Server 发布服务器”** 。  
  
3.  在 **“连接到服务器”** 对话框中输入该发布服务器的名称，然后选择身份验证类型。 如果选择 **“SQL Server 身份验证”** ，请输入登录名和密码。 您所指定的凭据由复制监视器进行保存，以便将来连接到此服务器时使用。 指定的 Windows 帐户或 SQL Server 登录名必须为 **sysadmin** 固定服务器角色的成员或分发数据库中 **replmonitor** 固定数据库角色的成员。  
  
4.  单击“连接”  。 如果发布服务器使用远程分发服务器，系统将在 **“连接到服务器”** 对话框中提示您连接到分发服务器。 您所指定的凭据由复制监视器进行保存，以便将来连接到此服务器时使用。 指定的 Windows 帐户或 SQL Server 登录名必须为 **sysadmin** 固定服务器角色的成员或分发数据库中 **replmonitor** 固定数据库角色的成员。  
  
5.  发布服务器和分发服务器的名称显示在 **“开始监视下列发布服务器”** 网格中。  
  
6.  若要为发布服务器指定刷新和连接选项，请在该网格中选择发布服务器，并根据需要修改选项。 有关刷新选项的详细信息，请参阅 [Caching, Refresh, and Replication Monitor Performance](../../../relational-databases/replication/monitor/caching-refresh-and-replication-monitor-performance.md)。  
  
7.  选择复制监视器中放置发布服务器的组。 若要创建新组，请单击 **“新建组”** 选项，然后输入组名称；在 **“在以下组中显示此发布服务器”** 列表中选择该组。  
  
8.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-add-an-oracle-publisher"></a>添加 Oracle 发布服务器  
  
1.  在左窗格中右键单击 **“复制监视器”** 节点或发布服务器组节点，再单击 **“添加发布服务器”** 。  
  
2.  在 **“添加发布服务器”** 对话框中，单击 **“添加”** ，然后单击 **“添加 Oracle 发布服务器”** 。  
  
3.  在“连接到服务器”对话框中，输入与 Oracle 发布服务器关联的   分发服务器的名称，然后选择身份验证类型[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 如果选择 **“SQL Server 身份验证”** ，请输入登录名和密码。 您所指定的凭据由复制监视器进行保存，以便将来连接到此服务器时使用。 指定的 Windows 帐户或 SQL Server 登录名必须为 **sysadmin** 固定服务器角色的成员或分发数据库中 **replmonitor** 固定数据库角色的成员。  
  
4.  单击“连接”  。  
  
5.  发布服务器和分发服务器的名称显示在 **“开始监视下列发布服务器”** 网格中。  
  
6.  若要为发布服务器指定刷新和连接选项，请在该网格中选择发布服务器，并根据需要修改选项。 有关刷新选项的详细信息，请参阅 [Caching, Refresh, and Replication Monitor Performance](../../../relational-databases/replication/monitor/caching-refresh-and-replication-monitor-performance.md)。  
  
7.  选择复制监视器中放置发布服务器的组。 若要创建新组，请单击 **“新建组”** 选项，然后输入组名称；在 **“在以下组中显示此发布服务器”** 列表中选择该组。  
  
8.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-add-one-or-more-publishers-that-use-the-same-distributor"></a>添加一个或多个使用相同分发服务器的发布服务器  
  
1.  在左窗格中右键单击 **“复制监视器”** 节点或发布服务器组节点，再单击 **“添加发布服务器”** 。  
  
2.  在 **“添加发布服务器”** 对话框中，单击 **“添加”** ，然后单击 **“指定分发服务器并添加其发布服务器”** 。  
  
3.  在 **“连接到服务器”** 对话框中，输入该分发服务器的名称，然后选择身份验证类型。 如果选择 **“SQL Server 身份验证”** ，请输入登录名和密码。 您所指定的凭据由复制监视器进行保存，以便将来连接到此服务器时使用。 指定的 Windows 帐户或 SQL Server 登录名必须为 **sysadmin** 固定服务器角色的成员或分发数据库中 **replmonitor** 固定数据库角色的成员。  
  
4.  单击“连接”  。  
  
5.  分发服务器和每个发布服务器的名称显示在 **“开始监视下列发布服务器”** 网格中。 如果一个发布服务器已经添加到复制监视器中，则该网格中不显示其名称。  
  
6.  若要为发布服务器指定刷新和连接选项，请在该网格中选择发布服务器，并根据需要修改选项。 有关刷新选项的详细信息，请参阅 [Caching, Refresh, and Replication Monitor Performance](../../../relational-databases/replication/monitor/caching-refresh-and-replication-monitor-performance.md)。  
  
7.  选择复制监视器中放置发布服务器的组。 若要创建新组，请单击 **“新建组”** 选项，然后输入组名称；在 **“在以下组中显示此发布服务器”** 列表中选择该组。  
  
8.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-modify-settings-for-the-publisher-and-publisher-groups"></a>修改发布服务器和发布服务器组的设置  
  
1.  在左窗格中右键单击发布服务器，再单击 **“发布服务器设置”** 。  
  
2.  在 **“发布服务器设置”** 对话框中进行任何更改：  
  
    -   若要更改复制监视器用来连接到服务器的凭据，请单击 **“发布服务器连接”** 或 **“分发服务器连接”** ，然后在 **“连接到服务器”** 对话框中输入凭据。  
  
    -   若要将发布服务器从一个组移动到另一个组，请在 **“开始监视下列发布服务器”** 网格中选择发布服务器，然后在 **“在以下组中显示此发布服务器”** 列表中选择新组。  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-remove-a-publisher-from-replication-monitor"></a>从复制监视器删除发布服务器  
  
1.  右键单击左窗格中的某发布服务器。  
  
2.  单击 **“删除”** 。  
  
### <a name="to-add-a-publisher-group-to-replication-monitor"></a>向复制监视器添加发布服务器组  
  
1.  只有在添加发布服务器或修改发布服务器设置时才能创建发布服务器组。 有关详细信息，请参阅有关添加发布服务器的操作过程。  
  
### <a name="to-remove-a-publisher-group-from-replication-monitor"></a>从复制监视器删除发布服务器组  
  
1.  从复制监视器将所有发布服务器移动到其他组或删除它们。 有关详细信息，请参阅本主题前面的操作过程。  
  
2.  右键单击发布服务器组，再单击 **“删除”** 。  
  
## <a name="see-also"></a>另请参阅  
 [“配置分发”](../../../relational-databases/replication/configure-distribution.md)   
 [监视复制](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
