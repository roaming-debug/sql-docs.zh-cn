---
title: 连接到 Azure SQL 数据库
description: 本文介绍了在使用 Microsoft JDBC Driver for SQL Server 连接到 Azure SQL 数据库时遇到的问题。
ms.custom: ''
ms.date: 12/18/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 49645b1f-39b1-4757-bda1-c51ebc375c34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03768a309ac10fc16fd1a743660df6fe74b088e7
ms.sourcegitcommit: bc8474fa200ef0de7498dbb103bc76e3e3a4def4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/21/2020
ms.locfileid: "97709666"
---
# <a name="connecting-to-an-azure-sql-database"></a>连接到 Azure SQL 数据库

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

本文介绍了在使用 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 连接到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 时遇到的问题。 若要详细了解如何连接到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]，请参阅：  
  
- [Azure SQL 数据库](/azure/sql-database/sql-database-technical-overview)  
  
- [如何：使用 JDBC 连接到 Azure SQL](/azure/sql-database/sql-database-connect-query-java)  

- [使用 Azure Active Directory 身份验证连接](connecting-using-azure-active-directory-authentication.md)  
  
## <a name="details"></a>详细信息

连接到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 后，应连接到 master 数据库，以调用 SQLServerDatabaseMetaData.getCatalogs  。  
[!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 不支持从用户数据库中返回整个目录集。 SQLServerDatabaseMetaData.getCatalogs 使用 sys.databases 视图获取目录  。 请参阅 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 中有关权限的讨论，以了解 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 上的 SQLServerDatabaseMetaData.getCatalogs 行为。  
  
## <a name="connections-dropped"></a>删除的连接

连接到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 后，空闲连接可能在一段时间不活动后被网络组件（如防火墙）终止。 在此上下文中，有两种类型的空闲连接：  

- TCP 层出现空闲，在此情形下，任意数量的网络设备都可以删除连接。  

- Azure SQL 网关出现空闲，在此情形下，可能会发生 TCP keepalive 消息（使连接从 TCP 的角度看不再空闲），但在 30 分钟内没有活动的查询。 在这种情况下，网关将确定 TDS 连接处于空闲状态已有 30 分钟，因此将中止连接。  
  
若要解决第二个问题并避免网关终止空闲连接，可以执行以下操作：

* 配置 Azure SQL 数据源时，请使用重定向[连接策略](/azure/azure-sql/database/connectivity-architecture#connection-policy)。

* 通过轻量级活动保持连接。 不建议使用此方法，如果没有其他可能的选项，则应使用此方法。

为了解决第一个问题并避免网络组件删除空闲连接，应在加载驱动程序的操作系统上设置以下注册表设置（或非 Windows 的等效设置）：  
  
|注册表设置|建议的值|  
|----------------------|-----------------------|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ KeepAliveTime|30000|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ KeepAliveInterval|1000|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ TcpMaxDataRetransmissions|10|  
  
重启计算机，注册表设置才能生效。  

KeepAliveTime 和 KeepAliveInterval 值以毫秒为单位。 这些设置的效果是在 10 到 40 秒内断开无响应连接。 在发送保持连接数据包后，如果没有收到响应，则每秒会重试一次，最多可重试 10 次。 如果在该时间段内未收到响应，则客户端套接字会断开连接。 根据你的环境，你可能想要增加 KeepAliveInterval 以适应已知中断（例如虚拟机迁移），这可能会导致服务器无响应时间超过 10 秒。

> [!NOTE]
> TcpMaxDataRetransmissions 在 Windows Vista 或 Windows 2008 及更高版本上无法控制。

为了在 Azure 中运行时执行此配置，请创建一个启动任务来添加注册表项。  例如，将以下启动任务添加到服务定义文件：  


```xml
<Startup>  
    <Task commandLine="AddKeepAlive.cmd" executionContext="elevated" taskType="simple">  
    </Task>  
</Startup>  
```

然后将 AddKeepAlive.cmd 文件添加到您的项目。 将“复制到输出目录”设置为“始终复制”。 以下脚本是一个 AddKeepAlive.cmd 文件示例：  

```bat
if exist keepalive.txt goto done  
time /t > keepalive.txt  
REM Workaround for JDBC keep alive on Azure SQL  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveTime /t REG_DWORD /d 30000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveInterval /t REG_DWORD /d 1000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v TcpMaxDataRetransmissions /t REG_DWORD /d 10 >> keepalive.txt  
shutdown /r /t 1  
:done  
```

## <a name="appending-the-server-name-to-the-userid-in-the-connection-string"></a>将服务器名称追加到连接字符串中的 userId  

在 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 4.0 之前的版本中，连接到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 时，要求将服务器名称追加到连接字符串中的 UserId。 例如，user@servername 。 从 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 4.0 版本开始，不再要求将 @@servername 追加到连接字符串中的 UserId。  

## <a name="using-encryption-requires-setting-hostnameincertificate"></a>使用加密需要设置 hostNameInCertificate

在 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 的 7.2 版本之前，当连接到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 时，如果指定 encrypt=true  ，则应指定 hostNameInCertificate  （如果连接字符串中的服务器名称为 shortName.domainName   ，则将 hostNameInCertificate  属性设置为 \*.domainName  。）。 此属性是驱动程序版本 7.2 的可选属性。

例如：

```java
jdbc:sqlserver://abcd.int.mscds.com;databaseName=myDatabase;user=myName;password=myPassword;encrypt=true;hostNameInCertificate=*.int.mscds.com;
```

## <a name="see-also"></a>另请参阅

[通过 JDBC 驱动程序连接到 SQL Server](connecting-to-sql-server-with-the-jdbc-driver.md)
