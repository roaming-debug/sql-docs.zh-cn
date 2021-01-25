---
title: 了解加密支持
description: 了解如何确保 JDBC 驱动程序使用 TLS 加密来保护到 SQL 数据库的连接。
ms.custom: ''
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: vanto
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 073f3b9e-8edd-4815-88ea-de0655d0325e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 62cf191ec41e71084dbe3ca85230108f9fc712b0
ms.sourcegitcommit: 0576ce6d7c9c5514306a90e27fa621ef25825186
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2021
ms.locfileid: "98575709"
---
# <a name="understanding-encryption-support"></a>了解加密支持

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

当连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，如果应用程序请求加密并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例已配置为支持 TLS 加密，则 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 将启动 TLS 握手。 握手允许服务器和客户端协商将用于保护数据的加密方式和加密算法。 在完成 TLS 握手之后，客户端和服务器可以安全地发送已加密的数据。 在 TLS 握手期间，服务器向客户端发送其公钥证书。 公钥证书的颁发者称为证书颁发机构 (CA)。 客户端负责验证证书颁发机构是否为客户端信任的证书颁发机构。  
  
如果应用程序未请求加密，则 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 将不强制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持 TLS 加密。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例未配置为强制 TLS 加密，将建立连接而不进行加密。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例已配置为强制 TLS 加密，则当驱动程序在经正确配置的 Java 虚拟机 (JVM) 上运行时，它将自动启用 TLS 加密，否则连接将终止并且驱动程序将报错。  
  
> [!NOTE]  
> 确保传递给 serverName 的值与服务器证书的公用名 (CN) 或使用者替代名称 (SAN) 中的 DNS 名称完全匹配，以便成功建立 TLS 连接。  
>
> 有关如何为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置 TLS 的详细信息，请参阅[启用数据库引擎的加密连接](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
## <a name="remarks"></a>备注

为了允许应用程序使用 TLS 加密，从 1.2 版开始，[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 引入了以下连接属性：encrypt、trustServerCertificate、trustStore、trustStorePassword 和 hostNameInCertificate。 有关详细信息，请参阅[设置连接属性](../../connect/jdbc/setting-the-connection-properties.md)。  
  
 下表总结了此 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 版本对于可能的 TLS 连接方案的行为方式。 每种方案使用一组不同的 TLS 连接属性。 该表包含：  
  
- **blank**：“连接字符串中不存在此属性”  
  
- **value**：“连接字符串中存在此属性且属性的值有效”  
  
- **any**：“连接字符串中是否存在此属性或属性的值是否有效均无关紧要”  
  
> [!NOTE]  
> 同样的行为适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户身份验证和 Windows 集成身份验证。  
  
| encrypt        | trustServerCertificate | hostNameInCertificate | trustStore | trustStorePassword | 行为                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| -------------- | ---------------------- | --------------------- | ---------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| false 或 blank | any                    | any                   | any        | any                | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 将不强制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持 TLS 加密。 如果服务器具有自签名证书，驱动程序将启动 TLS 证书交换。 将不会验证 TLS 证书，而只加密登录数据包中的凭据。<br /><br /> 如果服务器要求客户端支持 TLS 加密，驱动程序将启动 TLS 证书交换。 将不验证 TLS 证书，但将加密整个通信。                                                                                                                                                                                    |
| true           | true                   | any                   | any        | any                | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 要求对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 TLS 加密。<br /><br /> 如果服务器要求客户端支持 TLS 加密，或者服务器支持加密，则驱动程序将启动 TLS 证书交换。 如果 trustServerCertificate 属性设置为“true”，驱动程序将不验证 TLS 证书。<br /><br /> 如果服务器未配置为支持加密，驱动程序将报错并终止连接。                                                                                                                                                                                          |
| true           | false 或 blank         | blank                 | blank      | blank              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 要求对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 TLS 加密。<br /><br /> 如果服务器要求客户端支持 TLS 加密，或者服务器支持加密，则驱动程序将启动 TLS 证书交换。<br /><br /> 驱动程序将使用在连接 URL 上指定的 serverName 属性以验证服务器 TLS 证书，并依赖于信任关系管理器工厂的查找规则以确定要使用哪一个证书存储区。<br /><br /> 如果服务器未配置为支持加密，驱动程序将报错并终止连接。                                                                             |
| true           | false 或 blank         | 值                 | blank      | blank              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 要求对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 TLS 加密。<br /><br /> 如果服务器要求客户端支持 TLS 加密，或者服务器支持加密，则驱动程序将启动 TLS 证书交换。<br /><br /> 驱动程序将使用为 hostNameInCertificate 属性指定的值验证 TLS 证书的 subject 值。<br /><br /> 如果服务器未配置为支持加密，驱动程序将报错并终止连接。                                                                                                                                                                 |
| true           | false 或 blank         | blank                 | 值      | 值              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 要求对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 TLS 加密。<br /><br /> 如果服务器要求客户端支持 TLS 加密，或者服务器支持加密，则驱动程序将启动 TLS 证书交换。<br /><br /> 驱动程序将使用 trustStore 属性值查找证书 trustStore 文件，并使用 trustStorePassword 属性值检查 trustStore 文件的完整性。<br /><br /> 如果服务器未配置为支持加密，驱动程序将报错并终止连接。                                                                                                                |
| true           | false 或 blank         | blank                 | blank      | 值              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 要求对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 TLS 加密。<br /><br /> 如果服务器要求客户端支持 TLS 加密，或者服务器支持加密，则驱动程序将启动 TLS 证书交换。<br /><br /> 驱动程序将使用 trustStorePassword 属性值检查默认 trustStore 文件的完整性。<br /><br /> 如果服务器未配置为支持加密，驱动程序将报错并终止连接。                                                                                                                                                                                  |
| true           | false 或 blank         | blank                 | 值      | blank              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 要求对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 TLS 加密。<br /><br /> 如果服务器要求客户端支持 TLS 加密，或者服务器支持加密，则驱动程序将启动 TLS 证书交换。<br /><br /> 驱动程序将使用 trustStore 属性值查找 trustStore 文件的位置。<br /><br /> 如果服务器未配置为支持加密，驱动程序将报错并终止连接。                                                                                                                                                                                                 |
| true           | false 或 blank         | 值                 | blank      | 值              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 要求对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 TLS 加密。<br /><br /> 如果服务器要求客户端支持 TLS 加密，或者服务器支持加密，则驱动程序将启动 TLS 证书交换。<br /><br /> 驱动程序将使用 trustStorePassword 属性值检查默认 trustStore 文件的完整性。 此外，驱动程序还将使用 hostNameInCertificate 属性值以验证 TLS 证书。<br /><br /> 如果服务器未配置为支持加密，驱动程序将报错并终止连接。                                                                   |
| true           | false 或 blank         | 值                 | 值      | blank              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 要求对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 TLS 加密。<br /><br /> 如果服务器要求客户端支持 TLS 加密，或者服务器支持加密，则驱动程序将启动 TLS 证书交换。<br /><br /> 驱动程序将使用 trustStore 属性值查找 trustStore 文件的位置。 此外，驱动程序还将使用 hostNameInCertificate 属性值以验证 TLS 证书。<br /><br /> 如果服务器未配置为支持加密，驱动程序将报错并终止连接。                                                                                  |
| true           | false 或 blank         | 值                 | 值      | 值              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 要求对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 TLS 加密。<br /><br /> 如果服务器要求客户端支持 TLS 加密，或者服务器支持加密，则驱动程序将启动 TLS 证书交换。<br /><br /> 驱动程序将使用 trustStore 属性值查找证书 trustStore 文件，并使用 trustStorePassword 属性值检查 trustStore 文件的完整性。 此外，驱动程序还将使用 hostNameInCertificate 属性值以验证 TLS 证书。<br /><br /> 如果服务器未配置为支持加密，驱动程序将报错并终止连接。 |
  
如果 encrypt 属性设置为 true，则 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 将使用 JVM 的默认 JSSE 安全提供程序与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 协商 TLS 加密  。 默认的安全提供程序可能不支持成功协商 TLS 加密所需的全部功能。 例如，默认的安全提供程序可能不支持在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] TLS 证书中使用的 RSA 公钥的大小。 在这种情况下，默认的安全提供程序可能报错，此错误将导致 JDBC 驱动程序终止连接。 为了解决此问题，可以使用以下选项之一：  
  
- 使用具有较小 RSA 公钥的服务器证书配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
- 在“\<java-home>/lib/security/java.security”安全属性文件中将 JVM 配置为使用其他 JSSE 安全提供程序  
  
- 使用其他 JVM  
  
## <a name="validating-server-tls-certificate"></a>验证服务器 TLS 证书  

在 TLS 握手期间，服务器向客户端发送其公钥证书。 JDBC 驱动程序或客户端必须验证服务器证书是由客户端信任的证书颁发机构颁发的。 驱动程序要求服务器证书必须满足以下条件：  
  
- 证书是由受信任的证书颁发机构颁发的。  
  
- 必须颁发证书才能进行服务器身份验证。  
  
- 证书未过期。  
  
- 证书使用者中的公用名 (CN) 或使用者替代名称 (SAN) 中的 DNS 名称与连接字符串中指定的 serverName 值完全匹配，或与 hostNameInCertificate 属性值（如果指定）完全匹配。  
  
- DNS 名称可包含通配符。 在版本 7.2 之前，[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 不支持通配符匹配。 也就是说，abc.com 与 \*.com 不匹配，但 \*.com 与 \*.匹配。 在版本 7.2 和更高版本中，支持标准证书通配符匹配。  
  
## <a name="see-also"></a>另请参阅

[使用加密](../../connect/jdbc/using-ssl-encryption.md)

[保护 JDBC 驱动程序应用程序](../../connect/jdbc/securing-jdbc-driver-applications.md)  
