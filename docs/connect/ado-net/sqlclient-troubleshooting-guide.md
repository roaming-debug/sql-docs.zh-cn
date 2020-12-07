---
title: SqlClient 故障排除指南
description: 提供常见问题解决方案的页面。
ms.date: 11/27/2020
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 6cad6278eb6ac7b170ee108c1a3510db956ecb22
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96468080"
---
# <a name="sqlclient-troubleshooting-guide"></a>SqlClient 故障排除指南

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="exceptions-when-connecting-to-sql-server"></a>在连接到 SQL Server 时出现异常

连接建立失败的原因有多种。 下面是一些故障排除提示，可用作分析和解决许多问题的指南。

### <a name="unable-to-load-native-sni-server-name-indication-library"></a>无法加载本机 SNI（服务器名称指示）库

#### <a name="issues-in-net-framework-applications"></a>.NET Framework 应用程序中的问题

观察到 Stacktrace：

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x64.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x86.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

SNI 是在 Windows 上运行时 SqlClient 完成各种网络操作所依赖的本机 C++ 库。 在使用 MSBuild 项目 SDK 生成的 .NET Framework 应用程序中，本机 DLL 不是通过还原命令管理的。 因此，“Microsoft.Data.SqlClient.SNI”NuGet 包中将包含一个“.targets”文件，用于定义必需的“Copy”操作。

当直接依赖于“Microsoft.Data.SqlClient”库时，将自动引用包含的“.targets”文件。 在进行可传递（间接）引用的情况下，应手动引用此“.targets”文件，以确保在必要时可以执行“Copy”操作。

建议的解决方案：请确保应用程序的“.csproj”文件中引用了“.targets”文件，以确保执行“Copy”操作。

这些目标仅涵盖 Microsoft 的已知和常用目标。 如果外部工具或应用程序定义了用于复制二进制文件的自定义目标，则必须由工具维护人员定义新目标，以确保将本机 SNI Dll 连同 Microsoft.Data.SqlClient.dll 二进制文件一起复制，并在执行客户端应用程序时可用。

#### <a name="issues-in-net-core-applications"></a>.NET Core 应用程序中的问题

观察到 Stacktrace：

```log
System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.TdsParser' threw an exception.
---> System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
---> System.DllNotFoundException: Unable to load shared library 'Microsoft.Data.SqlClient.SNI.dll' or one of its dependencies.
```

> [!NOTE]
> 此错误可能仅发生在 Windows 应用程序上。 如果在 Unix 环境中出现此错误，则必须确保构建的应用程序是针对 Unix 运行时，而不是针对 Windows。

SNI 是在 Windows 上运行时 SqlClient 完成各种网络操作所依赖的本机 C++ 库。 Microsoft.Data.SqlClient 不管理此库在 .NET Core 中的加载/卸载。

建议的解决方案：确保在 .NET Core 进程中加载本机运行时库的 filesystem 上授予“执行”权限。 如果这不能解决问题，则可以在 [dotnet/runtime](https://github.com/dotnet/runtime) 存储库中提交问题，以获取进一步支持。

#### <a name="native-sni-pdb-not-found-errors"></a>本机 SNI（找不到 pdb）错误

观察到 Stacktrace：

```log
An assembly specified in the application dependencies manifest (sql2csv.deps.json) was not found:
  package: 'Microsoft.Data.SqlClient.SNI.runtime', version: '2.0.0'
  path: 'runtimes/win-x64/native/Microsoft.Data.SqlClient.SNI.pdb'
```

建议的解决方案：确保客户端应用程序引用 Microsoft.Data.SqlClient 包的最小版本 [v2.1.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient/2.1.0)。 使用 EF Core 时，请直接添加对此包版本的 Microsoft.Data.SqlClient 的引用以覆盖依赖项。

### <a name="hostname-resolution-errors"></a>主机名解析错误

观察到 Stacktrace：

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 0 - No such host is known.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
 ---> System.Net.Internals.SocketExceptionFactory+ExtendedSocketException (00000005, 0xFFFDFFFF): Name does not resolve
```

#### <a name="possible-reasons"></a>可能的原因

- 未在 SQL Server 上启用 TCP/命名管道协议

  建议的解决方案：在 SQL Server 配置管理器控制台的 SQL Server 实例上启用 TCP/命名管道协议。

- 主机名未知

  建议的解决方案：确保主机名从启动连接的客户端解析为服务器的 IP 地址。


### <a name="login-phase-errors"></a>登录阶段错误

观察到 Stacktrace：

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A connection was successfully established with the server, but then an error occurred during the pre-login handshake.
(provider: SSL Provider, error: 31 - Encryption(ssl/tls) handshake failed)
System.IO.EndOfStreamException: End of stream reached
```

```log
A connection was successfully established with the server, but then an error occurred during the login process.
(provider: SSL Provider, error: 0 - The target principal name is incorrect.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections.
The duration spent while attempting to connect to this server was - [Pre-Login] initialization=837; handshake=394; [Login] initialization=3; authentication=15; [Post-Login] complete=1027;
---> System.ComponentModel.Win32Exception (258): Unknown error 258
at Microsoft.Data.SqlClient.SqlInternalConnection.OnError(SqlException exception, Boolean breakConnection, Action1 wrapCloseInAction)
```

#### <a name="possible-reasons-and-solutions"></a>可能的原因和解决方法

- SQL Server 不支持 TLS 1.2

  此错误通常发生在 docker 映像容器、Unix 客户端或 Windows 客户端等客户端环境中，其中 TLS 1.2 是受支持的最低的 TLS 协议版本。

  建议的解决方案：在支持的 SQL Server<sup>1</sup> 版本上安装最新更新，并确保在服务器上启用 TLS 1.2 协议。

  <sup>1</sup> 查看 [SqlClient 驱动程序支持生命周期](sqlclient-driver-support-lifecycle.md)，获取包含不同版本的 Microsoft.Data.SqlClient 的受支持 SQL Server 版本列表。

  不安全的解决方案：在 docker 映像/客户端环境中配置 TLS/SSL 设置，以便与 TLS 1.0 连接。

  ```docker
  MinProtocol = TLSv1
  CipherString = DEFAULT@SECLEVEL=1
  ```

  > [!NOTE]
  > 当使用 TLS 1.0 或 TLS 1.1 从 Windows/Linux 环境连接到 SqlClient v2.0 以上版本时，如果目标 SQL Server 和客户端在建立连接时无法协商最低的 TLS 版本 1.2，则会引发安全警告消息：`Security Warning: The negotiated <TLS1.0 | TLS1.1> is an insecure protocol and is supported for backward compatibility only. The recommended protocol version is TLS 1.2 and later.`

- SQL Server 强制加密

  如果目标服务器是 Azure SQL 实例或启用了“强制加密”属性的本地 SQL Server，则将建立加密连接，为此，客户端必须与服务器建立信任关系。

  建议的解决方案：以下两个可用选项可修复此问题：

    1. 在客户端环境中安装目标 SQL Server 的 TLS/SSL 证书。 如果需要加密，将对其进行验证。
    2. 在连接字符串中设置“信任服务器证书 = true”属性。

  不安全的解决方案：禁用 SQL Server 上的“强制加密”设置。

- 没有使用 SHA-256 或更高版本签名的 TLS/SSL 证书。

  建议的解决方案：为服务器生成新的 TLS/SSL 证书，其哈希至少使用 SHA-256 哈希算法进行签名。

### <a name="connection-pool-exhaustion-errors"></a>连接池耗尽错误

观察到 Stacktrace：

```log
System.InvalidOperationException: Timeout expired. The timeout period elapsed prior to obtaining a connection from the pool.
This may have occurred because all pooled connections were in use and max pool size was reached.
```

#### <a name="possible-reasons-and-solutions"></a>可能的原因和解决方法

客户端应用程序打开的连接数超过了连接池在给定时间可以保持活动状态的数量。

建议的解决方案：将“最大池大小”连接属性配置为较高值并及时关闭未使用的连接。

## <a name="contact-support"></a>联系支持人员

如果本指南未解决连接问题，你可以查看 [dotnet/sqlclient](https://github.com/dotnet/SqlClient) 存储库中的现有问题，并在需要时提交新问题。
