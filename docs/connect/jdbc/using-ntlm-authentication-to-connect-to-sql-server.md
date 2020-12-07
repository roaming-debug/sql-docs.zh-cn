---
title: 使用 NTLM 身份验证连接到 SQL Server
description: 了解如何通过 JDBC 驱动程序使用 NTLM 身份验证建立 SQL 数据库连接。
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: lilgreenbird
ms.author: v-susanh
manager: kenvh
ms.openlocfilehash: 31510c4fbe4291168753809c227650951592e1e6
ms.sourcegitcommit: 644223c40af7168f9d618526e9f4cd24e115d1db
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2020
ms.locfileid: "96328037"
---
# <a name="using-ntlm-authentication-to-connect-to-sql-server"></a>使用 NTLM 身份验证连接到 SQL Server

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 允许应用程序使用 authenticationScheme 连接属性，以指明要使用 NTLM v2 身份验证连接到数据库。 

以下属性也用于 NTLM 身份验证：

- **domain = domainName**（可选）
- **user = userName**
- **密码 = 密码**
- **integratedSecurity = true**

除了 domain 以外，其他属性都是必需的；使用 NTLM authenticationScheme 属性时，如果缺少任何必需属性，驱动程序都会抛出错误。 

若要详细了解连接属性，请参阅[设置连接属性](../../connect/jdbc/setting-the-connection-properties.md)。 若要详细了解 Microsoft NTLM 身份验证协议，请参阅 [Microsoft NTLM](/windows/desktop/SecAuthN/microsoft-ntlm)。

## <a name="remarks"></a>备注

请参阅[网络安全：LAN Manager 身份验证级别](/windows/security/threat-protection/security-policy-settings/network-security-lan-manager-authentication-level)，了解控制 NTLM 身份验证行为的 SQL Server 设置。 

## <a name="logging"></a>日志记录

新增了一个记录程序以支持 NTLM 身份验证：com.microsoft.sqlserver.jdbc.internals.NTLMAuthentication。 有关详细信息，请参阅[跟踪驱动程序操作](../../connect/jdbc/tracing-driver-operation.md)。

## <a name="datasource"></a>数据源

使用数据源创建连接时，可以使用 setAuthenticationScheme、setDomain 和 setServerSpn（可选）以编程方式设置 NTLM 属性。

```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName("<server>");
ds.setPortNumber(1433); // change if necessary
ds.setIntegratedSecurity(true);
ds.setAuthenticationScheme("NTLM");
ds.setDomain("<domainName>");
ds.setUser("<userName>");
ds.setPassword("<password>");
ds.setDatabaseName("<database>");
ds.setServerSpn("<serverSpn");

try (Connection c = ds.getConnection(); Statement s = c.createStatement();
        ResultSet rs = s.executeQuery("select auth_scheme from sys.dm_exec_connections where session_id=@@spid")) {
    while (rs.next()) {
        System.out.println("Authentication Scheme: " + rs.getString(1));
    }
}
```

## <a name="service-principal-names"></a>服务主体名称

服务主体名称 (SPN) 是客户端用来唯一标识服务实例的名称。

可以使用 serverSpn 连接属性指定 SPN 或让驱动程序生成它（默认）。 此属性采用“MSSQLSvc/fqdn:port\@REALM”的形式，其中 fqdn 是完全限定的域名，port 是端口号，REALM 是 SQL Server 的领域（采用大写形式）。 此属性的领域部分是可选的，因为默认领域与 SQL Server 领域相同。

例如，SPN 可能如下所示：“MSSQLSvc/some-server.zzz.corp.contoso.com:1433”

有关服务主体名称 (SPN) 的详细信息，请参阅：

- [客户端连接中的服务主体名称 (SPN) 支持](../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)

> [!NOTE]  
> serverSpn 连接属性仅受 Microsoft JDBC Driver 4.2 及更高版本支持。

> 在 JDBC 驱动程序版本 6.2 推出前，需要显式设置 serverSpn。 自版本 6.2 起，驱动程序可以默认生成 serverSpn，尽管也能显式使用 serverSpn。

## <a name="security-risks"></a>安全风险

NTLM 协议是一种老旧的身份验证协议，存在各种造成安全风险的漏洞。 它基于相对较弱的加密方案，容易受到各种攻击。 取而代之的是 Kerberos，它更安全，值得推荐。 NTLM 身份验证应仅在安全的受信任环境中使用，或在无法使用 Kerberos 时使用。

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 只支持 NTLM v2，它比原来的 v1 协议有一些安全改进。 还建议启用扩展保护，或使用 SSL 加密来提高安全性。 

若要详细了解如何启用扩展保护，请参阅：

- [使用扩展保护连接到数据库引擎](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)

若要详细了解如何使用 SSL 加密进行连接，请参阅：

- [使用 SSL 加密进行连接](../../connect/jdbc/connecting-with-ssl-encryption.md)

> [!NOTE]
> 对于版本 7.4，不支持同时启用扩展保护和加密。

## <a name="see-also"></a>另请参阅

[通过 JDBC 驱动程序连接到 SQL Server](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)
