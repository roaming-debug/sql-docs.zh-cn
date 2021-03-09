---
title: 从 Linux 或 macOS 连接
description: 了解如何使用 Microsoft ODBC Driver for SQL Server 建立从 Linux 或 macOS 到数据库的连接。
ms.custom: ''
ms.date: 09/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connect to linux
- configure odbc.ini
ms.assetid: f95cdbce-e7c2-4e56-a9f7-8fa3a920a125
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d1bdbcbb34be9cbfa075ead7e1cd03ec813a5d9d
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247301"
---
# <a name="connecting-from-linux-or-macos"></a>从 Linux 或 macOS 连接

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

本主题讨论如何创建与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库的连接。  
  
## <a name="connection-properties"></a>连接属性  

有关 Linux 和 macOS 上支持的所有连接字符串关键字和属性，请参阅 [DSN 和连接字符串关键字和属性](../dsn-connection-string-attribute.md)。

> [!IMPORTANT]  
> 当连接到使用数据库镜像（有一个故障转移伙伴）的数据库时，不要在连接字符串中指定数据库名称。 应发送 use database_name 命令连接到该数据库，然后再执行查询。  
  
传递给 Driver 关键字的值可以是下列值之一：  
  
- 安装该驱动程序时使用的名称。

- 已在用于安装驱动程序的模板 .ini 文件中指定的该驱动程序库的路径。  

DSN 是可选的。 你可以使用 DSN 在 `DSN` 名称下定义连接字符串关键字，然后可以在连接字符串中引用该关键字。 若要创建 DSN，请创建（如有必要）并编辑仅可供当前用户访问的面向用户 DSN 的文件 ~/.odbc.ini（主目录中的 `.odbc.ini`），或面向系统 DSN 的 `/etc/odbc.ini`（需要管理权限）。以下示例文件显示了 DSN 所需的最少条目：  

```ini
# [DSN name]
[MSSQLTest]  
Driver = ODBC Driver 17 for SQL Server  
# Server = [protocol:]server[,port]  
Server = tcp:localhost,1433
#
# Note:  
# Port is not a valid keyword in the odbc.ini file  
# for the Microsoft ODBC driver on Linux or macOS
#  
```  

若要在连接字符串中使用上述 DSN 进行连接，请指定 `DSN` 关键字，例如：`DSN=MSSQLTest;UID=my_username;PWD=my_password`  
上述连接字符串等效于指定一个不带 `DSN` 关键字的连接字符串，例如：`Driver=ODBC Driver 17 for SQL Server;Server=tcp:localhost,1433;UID=my_username;PWD=my_password`

你可以选择指定协议和端口来连接到服务器。 例如，**Server=tcp:** _servername_ **,12345**。 请注意，Linux 和 macOS 驱动程序支持的唯一协议是 `tcp`。

若要连接到静态端口上的命名实例，请使用 Server=servername,port_number<b></b>。 在 17.4 版之前，不支持连接到动态端口。

可以选择将 DSN 信息添加到模板文件并执行以下命令，以将其添加到 `~/.odbc.ini`：
 - **odbcinst -i -s -f** _template_file_  

有关 ini 文件和 `odbcinst` 的完整文档，请参阅 [unixODBC 文档](http://www.unixodbc.org/odbcinst.html)。 有关 `odbc.ini` 文件中特定于 ODBC Driver for SQL Server 的条目，请参阅 [DSN 和连接字符串关键字和属性](../dsn-connection-string-attribute.md)，以了解 Linux 和 macOS 中支持的关键字和属性。

可以使用 `isql` 测试连接来验证驱动程序是否正在运行，也可以使用以下命令：
 - **bcp master.INFORMATION_SCHEMA.TABLES out OutFile.dat -S <server> -U <name> -P <password>**  

## <a name="using-tlsssl"></a>使用 TLS/SSL  

你可以使用传输层安全性 (TLS)（以前称为安全套接字层 (SSL)）来加密与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的连接。 TLS 通过网络保护 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 用户名和密码。 TLS 还验证服务器的身份以防止中间人 (MITM) 攻击。  

启用加密可提高安全性，但会降低性能。

有关更多信息，请参阅[加密连接到 SQL Server](/previous-versions/sql/sql-server-2008-r2/ms189067(v=sql.105)) 和[使用未经验证的加密](../../../relational-databases/native-client/features/using-encryption-without-validation.md)。

无论 **Encrypt** 和 **TrustServerCertificate** 的设置如何，服务器登录凭据（用户名和密码）都始终处于加密状态。 下表显示了 **Encrypt** 和 **TrustServerCertificate** 设置的效果。  

||**TrustServerCertificate=no**|**TrustServerCertificate=yes**|  
|-|-------------------------------------|------------------------------------|  
|**Encrypt=no**|不检查服务器证书。<br /><br />在客户端和服务器之间发送的数据没有加密。|不检查服务器证书。<br /><br />在客户端和服务器之间发送的数据没有加密。|  
|**Encrypt=yes**|检查服务器证书。<br /><br />在客户端和服务器之间发送的数据已加密。<br /><br />[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] TLS/SSL 证书中的使用者公用名 (CN) 或使用者可选名称 (SAN) 中的名称（或 IP 地址）应与连接字符串中指定的服务器名称（或 IP 地址）完全匹配。|不检查服务器证书。<br /><br />在客户端和服务器之间发送的数据已加密。|  

默认情况下，加密的连接会始终验证服务器的证书。 但是，如果你连接到具有自签名证书的服务器，还要添加 `TrustServerCertificate` 选项，以绕过针对受信任的证书颁发机构列表的证书检查：  

```
Driver={ODBC Driver 17 for SQL Server};Server=ServerNameHere;Encrypt=YES;TrustServerCertificate=YES  
```  
  
TLS 使用 OpenSSL 库。 下表显示了 OpenSSL 的受支持的最低版本和每个平台的默认证书信任存储位置：

|平台|最低的 OpenSSL 版本|默认证书信任存储位置|  
|------------|---------------------------|--------------------------------------------|
|Debian 10|1.1.1|/etc/ssl/certs|
|Debian 9|1.1.0|/etc/ssl/certs|
|Debian 8.71|1.0.1|/etc/ssl/certs|
|OS X 10.11、macOS 10.12-10.15|1.0.2|/usr/local/etc/openssl/certs|
|Red Hat Enterprise Linux 8|1.1.1|/etc/pki/tls/cert.pem|
|Red Hat Enterprise Linux 7|1.0.1|/etc/pki/tls/cert.pem|
|Red Hat Enterprise Linux 6|1.0.0-10|/etc/pki/tls/cert.pem|
|SUSE Linux Enterprise 15|1.1.0|/etc/ssl/certs|
|SUSE Linux Enterprise 11、12|1.0.1|/etc/ssl/certs|
|Ubuntu 18.10、19.04、19.10、20.04|1.1.1|/etc/ssl/certs|
|Ubuntu 18.04|1.1.0|/etc/ssl/certs|
|Ubuntu 16.04、16.10、17.10|1.0.2|/etc/ssl/certs|
|Ubuntu 14.04|1.0.1|/etc/ssl/certs|

使用 SQLDriverConnect 进行连接时，还可以使用 `Encrypt` 选项在连接字符串中指定加密。

## <a name="adjusting-the-tcp-keep-alive-settings"></a>调节 TCP Keep-Alive 设置

从 ODBC Driver 17.4 开始，如果没有收到响应，驱动程序发送 keep-alive 数据包并将其重新发送的频率是可配置的。
若要进行配置，请将以下设置添加到 `odbcinst.ini` 的驱动程序部分，或者添加到 `odbc.ini` 的 DSN 部分。 当连接到 DSN 时，驱动程序将使用 DSN 部分中的设置（如果存在）；否则，如果只连接一个连接字符串，它将使用 `odbcinst.ini` 的驱动程序部分中的设置。 如果这两个位置中不存在该设置，则驱动程序将使用默认值。

- `KeepAlive=<integer>` 控制 TCP 通过发送 keep-alive 数据包尝试验证空闲连接是否仍保持原样的频率。 默认值为 30 秒。

- `KeepAliveInterval=<integer>` 确定在收到响应之前分隔 keep-alive 重新传输的时间间隔。  默认值为 **1** 秒。

## <a name="see-also"></a>另请参阅

- [安装 Linux 上的 Microsoft ODBC Driver for SQL Server](installing-the-microsoft-odbc-driver-for-sql-server.md)
- [安装 macOS 上的 Microsoft ODBC Driver for SQL Server](install-microsoft-odbc-driver-sql-server-macos.md)
- [编程指南](programming-guidelines.md)
