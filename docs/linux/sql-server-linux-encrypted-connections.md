---
title: 加密与 Linux 上 SQL Server 的连接
description: Linux 上的 SQL Server 可以使用传输层安全性 (TLS) 对通过网络在客户端应用程序与 SQL Server 实例之间传输的数据进行加密。
ms.date: 06/29/2020
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, encrypted connections
ms.openlocfilehash: 83d89d99bf2acebf663309926ae298a64d64732c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348141"
---
# <a name="encrypting-connections-to-sql-server-on-linux"></a>加密与 Linux 上 SQL Server 的连接

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Linux 上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可以使用传输层安全性 (TLS) 对通过网络在客户端应用程序与 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例之间传输的数据进行加密。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 在 Windows 和 Linux 上支持相同的 TLS 协议：TLS 1.2、1.1 和 1.0。 但是，配置 TLS 的步骤特定于运行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的操作系统。  

## <a name="requirements-for-certificates"></a>证书要求 
在开始之前，需要确保证书符合以下要求：
- 当前系统时间必须晚于证书的“有效起始时间”属性，并早于证书的“有效截止时间”属性。
- 该证书必须用于服务器身份验证。 这就要求，证书的“增强型密钥使用”属性必须指定“服务器身份验证(1.3.6.1.5.5.7.3.1)”。
- 必须使用 AT_KEYEXCHANGE 的 KeySpec 选项创建证书。 证书的密钥使用属性 (KEY_USAGE) 通常还包括密钥加密 (CERT_KEY_ENCIPHERMENT_KEY_USAGE)。
- 证书的“使用者”属性必须指明，证书公用名称 (CN) 与服务器计算机的主机名或完全限定的域名 (FQDN) 相同。 注意：支持通配符证书。

## <a name="configuring-the-openssl-libraries-for-use-optional"></a>配置要使用的 OpenSSL 库（可选）
可以在 `/opt/mssql/lib/` 目录中创建符号链接，以引用应当用于加密的 `libcrypto.so` 和 `libssl.so` 库。 若要强制 SQL Server 使用特定的 OpenSSL 版本，而不是系统提供的默认版本，此方法非常有用。 如果未提供这些符号链接，SQL Server 将在系统上加载默认配置的 OpenSSL 库。

这些符号链接应命名为 `libcrypto.so` 和 `libssl.so`，并放在 `/opt/mssql/lib/` 目录中。

## <a name="overview"></a>概述
TLS 用于加密从客户端应用程序到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的连接。 正确配置后，TLS 可为客户端和服务器之间的通信提供隐私和数据完整性。  TLS 连接可以是客户端启动的或服务器启动的。 

## <a name="client-initiated-encryption"></a>客户端启动的加密 
- **生成证书**（/CN 应与 SQL Server 主机完全限定的域名匹配）

> [!NOTE]
> 在本示例中，我们使用自签名证书，此证书不应用于生产方案。 你应该使用 CA 证书。<br>
> 确保保存证书和私钥的文件夹可供 mssql 用户/组访问，并且权限设置为 700 (drwx-----)。 你可手动创建文件夹，该文件夹归 mssql 用户/组所有并且其权限设置为 700 (drwx------)，或该文件夹归其他用户所有并且其权限设置为 755(drwxr-xr-x)，但仍可供 mssql 用户组访问。 例如，可在路径“/var/opt/mssql/”下创建名为“sslcert”的文件夹，然后保存证书和私钥，并将文件的权限设置为 600，如下所示。 

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key 
# in this case we are saving the certificate to the certs folder under /etc/ssl/ which has the following permission 755(drwxr-xr-x)
sudo mv mssql.pem /etc/ssl/certs/ drwxr-xr-x 
# in this case we are saving the private key to the private folder under /etc/ssl/ with permissions set to 755(drwxr-xr-x)
sudo mv mssql.key /etc/ssl/private/ 
```

- **配置 SQL Server**

```bash
systemctl stop mssql-server 
cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 0 
```

- **在客户端计算机（Windows、Linux 或 macOS）上注册证书**

    -   如果使用 CA 签名证书，则必须将证书颁发机构 (CA) 证书而不是用户证书复制到客户端计算机。 
    -   如果使用自签名证书，只需根据分发版将 .pem 文件复制到以下文件夹中，然后执行命令启用它们 
        - **Ubuntu**：将证书复制到 `/usr/share/ca-certificates/`，将其扩展重命名为 .crt，并使用 `dpkg-reconfigure ca-certificates` 将其作为系统 CA 证书启用。 
        - **RHEL**：将证书复制到 `/etc/pki/ca-trust/source/anchors/`，并使用 `update-ca-trust` 将其作为系统 CA 证书启用。
        - **SUSE**：将证书复制到 `/usr/share/pki/trust/anchors/`，并使用 `update-ca-certificates` 将其作为系统 CA 证书启用。
        - Windows：在当前用户 -> 受信任的根证书颁发机构 -> 证书下将 .pem 文件作为证书导入
        - **macOS**： 
           - 将证书复制到 `/usr/local/etc/openssl/certs`
           - 运行以下命令获取哈希值：`/usr/local/Cellar/openssl/1.0.2l/openssl x509 -hash -in mssql.pem -noout`
           - 将证书重命名为值。 例如：`mv mssql.pem dc2dd900.0`。 确保 dc2dd900.0 在 `/usr/local/etc/openssl/certs` 中
    
-   **连接字符串示例** 

    - **[!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]**   
  ![“SSMS 连接”对话框](media/sql-server-linux-encrypted-connections/ssms-encrypt-connection.png "“SSMS 连接”对话框")  
  
    - **SQLCMD** 

        `sqlcmd  -S <sqlhostname> -N -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=True; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=Yes; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=true; trustServerCertificate=false;"`

## <a name="server-initiated-encryption"></a>服务器启动的加密 

- **生成证书**（/CN 应与 SQL Server 主机完全限定的域名匹配）

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key   
sudo mv mssql.pem /etc/ssl/certs/ 
sudo mv mssql.key /etc/ssl/private/ 
```

- **配置 SQL Server**

```bash
systemctl stop mssql-server 
cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 1 
```

-   **连接字符串示例** 

    - **SQLCMD**

        `sqlcmd  -S <sqlhostname> -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=False; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=no; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=false; trustServerCertificate=false;"`

> [!NOTE]
> 如果客户端无法连接到 CA 来验证证书的真实性，请将“TrustServerCertificate”设置为 True

## <a name="common-connection-errors"></a>常见的连接错误  

|错误消息 |Fix |
|--- |--- |
|证书链是由不受信任的颁发机构颁发的。  |如果客户端在 TLS 握手期间无法验证 SQL Server 提供的证书上的签名，则会发生此错误。 请确保客户端直接信任 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 证书，或者信任签署 SQL Server 证书的 CA。 |
|目标主体名称不正确。  |确保 SQL Server 证书上的“公用名”字段与客户端连接字符串中指定的服务器名称匹配。 |  
|远程主机强行关闭现有连接。 |如果客户端不支持 SQL Server 所需的 TLS 协议版本，则会发生此错误。 例如，如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置为需要 TLS 1.2，请确保客户端也支持 TLS 1.2 协议。 |
| | |   
