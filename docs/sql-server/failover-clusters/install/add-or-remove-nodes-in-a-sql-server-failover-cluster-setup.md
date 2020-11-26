---
title: 添加、删除节点故障转移群集实例
description: 本文介绍如何在现有 SQL Server Always On 故障转移群集实例中添加或删除节点。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- adding nodes
- nodes [Faillover Clustering], removing
- nodes [Faillover Clustering], adding
- failover clustering [SQL Server], nodes
- deleting nodes
- cluster maintenance [SQL Server]
- removing nodes
ms.assetid: fe20dca9-a4c1-4d32-813d-42f1782dfdd3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5c65c099af7ffc6346aaf0e73a26c5ee7e16f7ce
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96121312"
---
# <a name="add-or-remove-nodes-in-a-failover-cluster-instance-setup"></a>在故障转移群集实例中添加或删除节点（安装程序）

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

 使用此过程可以管理现有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的节点。  
  
 若要更新或删除 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI，你必须是本地管理员，且具有作为服务登录到基础 Windows Server 故障转移群集 (WSFC) 的所有节点的权限。 对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。  
  
 若要向现有的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 添加节点，则必须在要添加至 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的节点上运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序。 不要在活动节点上运行安装程序。  
  
 若要从现有的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 删除节点，则必须在要从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例删除的节点上运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序。  
  
 若要查看添加或删除节点的过程步骤，请选择下列操作之一：  
  
-   [向现有 Always On 故障转移群集实例添加节点](#Add)  
  
-   [从现有 Always On 故障转移群集实例删除节点](#Remove)  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装位置的操作系统驱动器号在添加到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的所有节点上必须匹配。  
  
##  <a name="add-node"></a><a name="Add"></a> 添加节点  
  
#### <a name="to-add-a-node-to-an-existing-ssnoversion-failover-cluster-instance"></a>向现有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例添加节点  
  
1.  插入 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装介质，然后双击根文件夹中的 Setup.exe。 若要从网络共享进行安装，请导航到共享中的根文件夹，然后双击 Setup.exe。  
  
2.  安装向导将启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装中心。 若要向现有故障转移群集实例中添加节点，请单击左窗格中的“安装”  。 然后，选择“向 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集添加节点”。  
  
3.  系统配置检查器将在您的计算机上运行发现操作。 若要继续， [!INCLUDE[clickOK](../../../includes/clickok-md.md)]。  
  
4.  如果您在已本地化的操作系统上进行安装，并且安装介质包括针对英语以及与操作系统相对应的语言的语言包，则您可以在“语言选择”页上为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例指定语言。 有关跨语言支持和安装注意事项的详细信息，请参阅 [SQL Server 中的本地语言版本](../../../sql-server/install/local-language-versions-in-sql-server.md)。  
  
     若要继续，请单击 **“下一步”** 。  
  
5.  在“产品密钥”页上指定产品的生产版本的 PID 密钥。 请注意，您必须针对活动节点上安装的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的版本为此安装输入产品密钥。  
  
6.  在“许可条款”页上阅读许可协议，然后选中相应的复选框以接受许可条款和条件。 为了帮助改进 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，您还可以启用功能使用情况选项并将报告发送给 [!INCLUDE[msCoName](../../../includes/msconame-md.md)]。 若要继续，请单击 **“下一步”** 。 若要结束安装程序，请单击 **“取消”** 。  
  
7.  系统配置检查器将在安装继续之前检验计算机的系统状态。 检查完成后，请单击 **“下一步”** 继续。  
  
8.  在“群集节点配置”页上，使用下拉框指定要在此安装操作期间修改的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的名称。  
  
9. 在“服务器配置 - 服务帐户”页上指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务的登录帐户。 此页上配置的实际服务取决于您选择安装的功能。 对于故障转移群集实例安装，将基于为活动节点提供的设置，在此页上预填充帐户名和启动类型信息。 必须为每个帐户提供密码。 有关详细信息，请参阅 [服务器配置 - 服务帐户](../../../database-engine/install-windows/install-sql-server.md) 和 [配置 Windows 服务帐户和权限](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。  
  
     **安全说明** [!INCLUDE[ssNoteStrongPass](../../../includes/ssnotestrongpass-md.md)]  
  
     为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务指定登录信息后，请单击 **“下一步”** 。  
  
10. 在“报告”页上，指定要发送到 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 以改进 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的信息。 默认情况下，将启用用于错误报告的选项。  
  
11. 系统配置检查器将再运行一组规则来针对您指定的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能验证您的计算机配置。  
  
12. “准备添加节点”页显示您在安装过程中指定的安装选项的树视图。  
  
13. “添加节点进度”页会提供相应的状态，因此您可以在安装过程中监视安装进度。  
  
14. 安装完成后，“完成”页会提供指向安装摘要日志文件以及其他重要说明的链接。 若要完成 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装过程，请单击 **“关闭”** 。  
  
15. 如果安装程序指示您重新启动计算机，请立即重新启动。 安装完成后，请务必阅读来自安装向导的消息。 有关安装程序日志文件的详细信息，请参阅 [查看和阅读 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  
  
##  <a name="remove-node"></a><a name="Remove"></a> 删除节点  
  
#### <a name="to-remove-a-node-from-an-existing-ssnoversion-failover-cluster-instance"></a>从现有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例中删除节点  
  
1.  插入 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装介质， 然后双击根文件夹中的 setup.exe。 若要从网络共享进行安装，请导航到共享中的根文件夹，然后双击 Setup.exe。  
  
2.  安装向导将启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装中心。 若要从现有的故障转移群集实例中删除节点，请单击左窗格中的“维护”，然后选择“从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集中删除节点”。  
  
3.  系统配置检查器将在您的计算机上运行发现操作。 若要继续， [!INCLUDE[clickOK](../../../includes/clickok-md.md)]。  
  
4.  在“安装程序支持文件”页上单击“安装”后，系统配置检查器在安装程序继续前验证您的计算机的系统状态。 检查完成后，请单击 **“下一步”** 继续。  
  
5.  在“群集节点配置”页上，使用下拉框指定要在此安装操作期间修改的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的名称。 **“此节点的名称”** 字段中将列出要删除的节点。  
  
6.  “准备删除节点”页显示您在安装期间指定的选项的树视图。 若要继续，请单击 **“删除”** 。  
  
7.  在删除操作期间，“删除节点进度”页会提供删除状态。  
  
8.  “完成”页会提供指向删除节点操作摘要日志文件以及其他重要说明的链接。 若要完成 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 删除节点操作，请单击 **“关闭”** 。 有关安装程序日志文件的详细信息，请参阅 [查看和阅读 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  
  
## <a name="see-also"></a>另请参阅  
 [查看和读取 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
