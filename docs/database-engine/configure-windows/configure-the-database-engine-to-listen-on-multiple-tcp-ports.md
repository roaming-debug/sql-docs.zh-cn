---
title: 将数据库引擎配置为侦听多个 TCP 端口 | Microsoft Docs
description: 熟悉表格格式数据流 (TDS) 端点。 了解如何使用它们将 SQL Server 数据库引擎配置为侦听多个 TCP 端口。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], multiple
- TDS
- listen on multiple ports
- endpoints [SQL Server], TDS
- adding ports
- tabular data stream
- multiple ports
ms.assetid: 8e955033-06ef-403f-b813-3d8241b62f1f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 28c73233d8a7b38ec2d14fa92c40f69d9ae0af05
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783274"
---
# <a name="configure-the-database-engine-to-listen-on-multiple-tcp-ports"></a>将数据库引擎配置为侦听多个 TCP 端口
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 SQL Server 配置管理器在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中将 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 配置为侦听多个 TCP 端口。 为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]启用 TCP/IP 后， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将侦听连接点上是否有传入的连接（由 IP 地址和 TCP 端口号组成）。下列过程将创建一个表格格式数据流 (TDS) 端点，以便 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 侦听其他 TCP 端口。  
  
 创建第二个 TDS 端点的可能原因包括：  
  
-   通过将防火墙配置为限制访问特定子网上的本地客户机的默认端点，提高安全性。 通过创建防火墙对 Internet 公开的新端点并限制服务器支持组对此端点的连接权限，维护支持组对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Internet 访问。  
  
-   使用非一致内存访问 (NUMA) 时，将连接与特定处理器关联。  
  
 配置 TDS 端点的步骤如下，但可以不按顺序进行：  
  
-   为 TCP 端口创建 TDS 端点，恢复对默认端点的访问权限（如果适用）。  
  
-   对所需的服务器主体授予对端点的访问权限。  
  
-   指定所选 IP 地址的 TCP 端口号。  
  
 有关默认 Windows 防火墙设置的详细信息以及有关影响数据库引擎、Analysis Services、Reporting Services 和 Integration Services 的 TCP 端口的说明，请参阅 [配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)。  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-create-a-tds-endpoint"></a>创建 TDS 端点  
  
-   发出以下语句，为服务器上所有可用的 TCP 地址的端口 1500 创建名为 **CustomConnection** 的端点。  
  
    ```  
    USE master;  
    GO  
    CREATE ENDPOINT [CustomConnection]  
    STATE = STARTED  
    AS TCP  
       (LISTENER_PORT = 1500, LISTENER_IP =ALL)  
    FOR TSQL() ;  
    GO  
    ```  
  
 创建新的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 端点时，将撤消默认 TDS 端点的 **public** 连接权限。 如果默认端点需要访问 **public** 组，请使用 `GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] to [public];` 语句重新应用此权限。  
  
#### <a name="to-grant-access-to-the-endpoint"></a>授予对端点的访问权限  
  
-   发出以下语句，对 corp 域中的 SQLSupport 组授予对 **CustomConnection** 端点的访问权限。  
  
    ```  
    GRANT CONNECT ON ENDPOINT::[CustomConnection] to [corp\SQLSupport] ;  
    GO  
    ```  
  
#### <a name="to-configure-the-sql-server-database-engine-to-listen-on-an-additional-tcp-port"></a>将 SQL Server 数据库引擎配置为侦听其他 TCP 端口  
  
1.  在 SQL Server 配置管理器中，展开“SQL Server 网络配置”，然后单击 _<instance_name>_ 的“协议”。  
  
2.  展开 _<instance_name>_ 的“协议”，然后单击“TCP/IP”。  
  
3.  在右窗格中，右键单击要启用的每个禁用的 IP 地址，再单击“启用”。  
  
4.  右键单击“IPAll”，再单击“属性”。  
  
5.  在 **“TCP 端口”** 框中，键入要 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 侦听的端口（用逗号分隔）。 在本示例中，如果列出了默认端口 1433，则键入 **,1500** ，随后框中将显示 **1433,1500**，然后单击“确定” 。  
  
    > [!NOTE]  
    >  如果不想对所有 IP 地址启用端口，则在属性框中只为所需地址配置其他端口。 然后，在控制台窗格中，右键单击“TCP/IP”，单击“属性”，然后在“全部侦听”框中选择“否”。  
  
6.  在左窗格中，单击 **“SQL Server 服务”** 。  
  
7.  在右侧窗格中，右键单击“SQL Server  _<instance_name>_ ”，然后单击“重新启动”。  
  
     [!INCLUDE[ssDE](../../includes/ssde-md.md)]重新启动后，错误日志将列出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要侦听的端口。  
  
#### <a name="to-connect-to-the-new-endpoint"></a>连接到新端点  
  
-   发出以下语句，以使用可信连接并假定用户为 [corp\SQLSupport] 组的成员，连接到名为 ACCT 的服务器上的 SQL Server 默认实例的 **CustomConnection** 端点。  
  
    ```  
    sqlcmd -SACCT,1500  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [DROP ENDPOINT (Transact SQL)](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [GRANT 终结点权限 (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [将 TCP IP 端口映射到 NUMA 节点 (SQL Server)](../../database-engine/configure-windows/map-tcp-ip-ports-to-numa-nodes-sql-server.md)  
  
  
