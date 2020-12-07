---
title: 连接字符串语法
description: 了解 Microsoft SqlClient Data Provider for SQL Server 中的连接字符串语法。 每个提供程序的语法记录在其 ConnectionString 属性中。
ms.date: 11/13/2020
ms.assetid: 0977aeee-04d1-4cce-bbed-750c77fce06e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: f61b867b70825595a012b2167d2c63b13409a8e2
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442814"
---
# <a name="connection-string-syntax"></a>连接字符串语法

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

<xref:Microsoft.Data.SqlClient> 具有从 <xref:System.Data.Common.DbConnection> 继承的 `Connection` 对象，以及特定于提供程序的 <xref:System.Data.Common.DbConnection.ConnectionString%2A> 属性。 SqlClient 提供程序的特定连接字符串语法记录在其 `ConnectionString` 属性中。 有关连接字符串语法的更多信息，请参见 <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>。

## <a name="connection-string-builders"></a>连接字符串生成器

 Microsoft SqlClient Data Provider for SQL Server 引入了以下连接字符串生成器。

- <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>

连接字符串生成器使你可以在运行时构造具有有效语法的连接字符串，从而不必在代码中手动串联连接字符串值。 有关详细信息，请参阅[连接字符串生成器](connection-string-builders.md)。

## <a name="windows-authentication"></a>Windows 身份验证

建议使用 Windows 身份验证（有时也称为“集成安全性”）连接到支持其的数据源。 下表显示了 Microsoft SqlClient Data Provider for SQL Server 使用的 Windows 身份验证语法。

|提供程序|语法|  
|--------------|------------|  
|`SqlClient`|`Integrated Security=true;`<br /><br /> `-- or --`<br /><br /> `Integrated Security=SSPI;`|  

## <a name="sqlclient-connection-strings"></a>SqlClient 连接字符串

<xref:Microsoft.Data.SqlClient.SqlConnection> 连接字符串的语法记录在 <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> 属性中。 您可以使用 <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> 属性来获取或设置 SQL Server 数据库的连接字符串。 连接字符串关键字还映射到 <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> 中的属性。

> [!IMPORTANT]
> `Persist Security Info` 关键字的默认设置为 `false`。 如果将其设置为 `true` 或 `yes`，则允许在打开连接后通过连接获取安全敏感信息（包括用户 ID 和密码）。 保持将 `Persist Security Info` 设置为 `false`，以确保不受信任的源不能访问敏感的连接字符串信息。

### <a name="windows-authentication-with-sqlclient"></a>使用 SqlClient 进行 Windows 身份验证

下列各个语法形式都将使用 Windows 身份验证连接到本地服务器上的 AdventureWorks 数据库。

```csharp  
"Persist Security Info=False;Integrated Security=true;  
    Initial Catalog=AdventureWorks;Server=MSSQL1"  
"Persist Security Info=False;Integrated Security=SSPI;  
    database=AdventureWorks;server=(local)"  
"Persist Security Info=False;Trusted_Connection=True;  
    database=AdventureWorks;server=(local)"  
```  

### <a name="sql-server-authentication-with-sqlclient"></a>使用 SqlClient 进行 SQL Server 身份验证

Windows 身份验证是用于连接到 SQL Server 的首选方法。 但是，如果需要 SQL Server 身份验证，请使用下列语法来指定用户名和密码。 在此示例中，星号用来表示有效用户名和密码。

```csharp  
"Persist Security Info=False;User ID=*****;Password=*****;Initial Catalog=AdventureWorks;Server=MySqlServer"  
```  

连接到 Azure SQL 数据库或 Azure Synapse Analytics 并提供格式为 `user@servername` 的登录名时，请确保登录名中的 `servername` 值与为 `Server=` 提供的值相匹配。

> [!NOTE]
> Windows 身份验证优先于 SQL Server 登录。 如果您同时指定 Integrated Security=true 以及用户名和密码，将忽略用户名和密码，而使用 Windows 身份验证。

### <a name="connect-to-a-named-instance-of-sql-server"></a>连接到 SQL Server 的命名实例

若要连接到 SQL Server 的命名实例，请使用“服务器名\实例名”语法。

```csharp  
"Data Source=MySqlServer\MSSQL1;"  
```  

在生成连接字符串时，您还可以将 <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A> 的 `SqlConnectionStringBuilder` 属性设置为实例名。 <xref:Microsoft.Data.SqlClient.SqlConnection.DataSource%2A> 对象的 <xref:Microsoft.Data.SqlClient.SqlConnection> 属性是只读的。

### <a name="type-system-version-changes"></a>类型系统版本更改

<xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> 中的 `Type System Version` 关键字指定 SQL Server 类型的客户端表示形式。 有关 <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> 关键字的更多信息，请参见 `Type System Version`。

## <a name="connecting-and-attaching-to-sql-server-express-user-instances"></a>连接并附加到 SQL Server Express 用户实例

用户实例是 SQL Server Express 中的一个功能。 它们允许以最低权限的本地 Windows 帐户运行的用户附加并运行 SQL Server 数据库，而无需具有管理权限。 使用用户 Windows 凭据执行用户实例，而不是作为服务执行用户实例。

有关使用用户实例的详细信息，请参阅 [SQL Server Express 用户实例](./sql/sql-server-express-user-instances.md)。

## <a name="using-trustservercertificate"></a>使用 TrustServerCertificate

`TrustServerCertificate` 关键字仅在连接到具有有效证书的 SQL Server 实例时有效。 将 `TrustServerCertificate` 设置为 `true` 时，传输层使用 TLS/SSL 将通道加密，并跳过遍历证书链以验证信任的过程。

```csharp  
"TrustServerCertificate=true;"
```  

> [!NOTE]
> 如果 `TrustServerCertificate` 设置为 `true` 并已启动加密，将使用对服务器指定的加密级别，即使 `Encrypt` 在连接字符串中被设置为 `false`。 否则连接将会失败。

### <a name="enabling-encryption"></a>启用加密

如果要在未向服务器预配证书的情况下启用加密，必须在 SQL Server 配置管理器中设置“强制协议加密”和“信任服务器证书”选项。 在这种情况下，如果服务器上未设置可验证的证书，加密将使用不带验证的自签名服务器证书。

应用程序设置无法降低在 SQL Server 中配置的安全级别，但可以增强安全级别。 应用程序可以通过将 `TrustServerCertificate` 和 `Encrypt` 关键字设置为 `true` 来请求加密，以保证在未预配服务器证书和未对客户端配置“强制协议加密”的情况下仍能够进行加密。 但是，如果未在客户端配置中启用 `TrustServerCertificate`，则仍需要提供服务器证书。

下表描述了各种情况。

|“强制协议加密”客户端设置|“信任服务器证书”客户端设置|“密加/使用数据加密”连接字符串/属性|“信任服务器证书”连接字符串/特性|结果|  
|----------------------------------------------|---------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------|------------|  
|否|空值|否（默认值）|忽略|无加密。|  
|否|不适用|是|否（默认值）|仅当存在可验证的服务器证书时才加密，否则连接尝试将失败。|  
|否|不适用|是|是|始终加密，但可能使用自签名的服务器证书。|  
|是|否|忽略|忽略|仅当存在可验证的服务器证书时才加密，否则连接尝试将失败。|  
|是|是|否（默认值）|忽略|始终加密，但可能使用自签名的服务器证书。|  
|是|是|是|否（默认值）|仅当存在可验证的服务器证书时才加密，否则连接尝试将失败。|  
|是|是|是|是|始终加密，但可能使用自签名的服务器证书。|  

有关详细信息，请参阅[使用加密但不验证](/sql/relational-databases/native-client/features/using-encryption-without-validation)。

## <a name="see-also"></a>请参阅

- [连接字符串](connection-strings.md)
- [连接到数据源](connecting-to-data-source.md)
