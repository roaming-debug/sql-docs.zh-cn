---
title: 在 Azure 虚拟机上安装
description: 在 Azure 云中的虚拟机上，使用 SQL Server 机器学习服务运行 Python 和 R 数据科学以及机器学习解决方案。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/02/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: e8eb32776aa72095c03305d4eb31cf5bb220ebe4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471238"
---
# <a name="install-sql-server-machine-learning-services-with-python-and-r-on-an-azure-virtual-machine"></a>在 Azure 虚拟机上安装带有 Python 和 R 的 SQL Server 机器学习服务
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

了解如何在 Azure 虚拟机上安装带有 SQL Server 机器学习服务的 Python 和 R。 通过此操作，无需再执行机器学习服务的安装和配置任务。

执行以下步骤:

1. 在 Azure 中预配 SQL Server 虚拟机
1. 取消阻止防火墙
1. 启用远程客户端的 ODBC 回调
1. 添加网络协议

## <a name="provision-sql-server-virtual-machine-in-azure"></a>在 Azure 中预配 SQL Server 虚拟机

有关分步说明，请参阅[如何在 Azure 门户中配置 Windows SQL Server 虚拟机](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision)。 

在[配置 SQL Server 设置](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#3-configure-sql-server-settings)步骤中，将机器学习服务添加到实例。

<a name="firewall"></a>

## <a name="unblock-the-firewall"></a>取消阻止防火墙

默认情况下，Azure 虚拟机上的防火墙包含一项规则，该规则阻止本地用户帐户的网络访问。

必须禁用该规则以确保可从远程数据科学客户端访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  否则，机器学习代码将无法在使用虚拟机工作区的计算上下文中执行。

若要从远程数据科学客户端进行访问，请执行以下操作：

1. 在虚拟机上打开“高级安全 Windows 防火墙”。
2. 选择“出站规则”。 
3. 禁用以下规则：
  
     `Block network access for R local user accounts in SQL Server instance MSSQLSERVER`
  
## <a name="enable-odbc-callbacks-for-remote-clients"></a>启用远程客户端的 ODBC 回调

如果预期调用服务器的客户端需要发出 ODBC 查询作为其机器学习解决方案的一部分，必须确保 Launchpad 能够代表远程客户端发出 ODBC 调用。 

为此，必须允许 Launchpad 使用的 SQL 辅助角色帐户登录到实例。 有关详细信息，请参阅[将 SQLRUserGroup 添加为数据库用户](../security/create-a-login-for-sqlrusergroup.md)。

<a name="network"></a>

## <a name="add-network-protocols"></a>添加网络协议

+ 启用命名管道
  
  [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 对客户端与服务器计算机之间的连接以及某些内部连接使用命名管道协议。 如果未启用命名管道，必须同时在 Azure 虚拟机以及连接到服务器的任何数据科学客户端上安装并启用命名管道。
  
+ 启用 TCP/IP

  建立环回连接需要 TCP/IP。 如果出现错误“DBNETLIB；SQL Server 不存在或访问被拒绝”，请在支持实例的虚拟机上启用 TCP/IP。