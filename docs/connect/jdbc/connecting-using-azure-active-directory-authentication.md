---
title: 使用 Azure Active Directory 身份验证进行连接
description: 了解如何开发结合使用 Azure Active Directory 身份验证功能与 Microsoft JDBC Driver for SQL Server 的 Java 应用程序。
ms.custom: ''
ms.date: 01/04/2020
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9c9d97be-de1d-412f-901d-5d9860c3df8c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 68d8b2a131fa6ab1c9e287f70cb584db3aeedacc
ms.sourcegitcommit: 6154ee7f20bccce9d458ac7f3b0a21b9613d1131
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97902635"
---
# <a name="connecting-using-azure-active-directory-authentication"></a>使用 Azure Active Directory 身份验证进行连接

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

本文介绍了如何开发结合使用 Azure Active Directory 身份验证功能与 Microsoft JDBC Driver for SQL Server 的 Java 应用程序。

可以使用 Azure Active Directory (Azure AD) 身份验证，这是一种使用 Azure Active Directory 标识连接到 Azure SQL 数据库 v12 的机制。 使用 Azure Active Directory 身份验证以集中管理数据库用户的标识且作为 SQL Server 身份验证的一种替代方法使用。 使用 JDBC 驱动程序，可以在 JDBC 连接字符串中指定连接到 Azure SQL 数据库所需的 Azure Active Directory 凭据。 若要了解如何配置 Azure Active Directory 身份验证，请访问[使用 Azure Active Directory 身份验证连接到 SQL 数据库](/azure/azure-sql/database/authentication-aad-overview)。 

Microsoft JDBC Driver for SQL Server 中支持 Azure Active Directory 身份验证的连接属性包括：
*   **authentication**：使用此属性，可以指明要对连接使用哪种 SQL 身份验证方法。 可能的值包括： 
    * **ActiveDirectoryMSI**
        * 自驱动程序版本 v7.2  起受支持，`authentication=ActiveDirectoryMSI` 可用于从已启用“标识”支持的 Azure 资源内部连接到 Azure SQL 数据库/数据仓库。 （可选）还可以在 Connection/DataSource 属性中指定 msiClientId（与此身份验证模式一起），其中必须包含用于获取建立连接所需的 accessToken 的托管标识的客户端 ID。
    * **ActiveDirectoryIntegrated**
        * 自驱动程序版本 v6.0  起受支持，`authentication=ActiveDirectoryIntegrated` 可用于使用集成身份验证连接到 Azure SQL 数据库/数据仓库。 必须将本地 Active Directory 联合身份验证服务 (ADFS) 与云中的 Azure Active Directory 联合，才能使用此身份验证模式。 设置后，连接方法有两种，一种是将本机库“mssql-jdbc_auth-\<version>-\<arch>.dll”添加到 Windows OS 上的应用程序类路径，另一种是设置用于提供跨平台身份验证支持的 Kerberos 票证。 登录域加入计算机后，可以访问 Azure SQL 数据库/Azure Synapse Analytics，而不会看到系统提示输入凭据。
    * **ActiveDirectoryPassword**
        * 自驱动程序版本 v6.0 起受支持，`authentication=ActiveDirectoryPassword` 可用于使用 Azure AD 用户名和密码连接到 Azure SQL 数据库/Data Warehouse。
    * **SqlPassword**
        * 借助 `authentication=SqlPassword`，可以使用 userName/user 和 password 属性连接到 SQL Server。
    * **NotSpecified**
        * 如果不需要这些身份验证方法，请使用 `authentication=NotSpecified` 或将它保留为默认值。

*   **accessToken**：借助此连接属性，可以使用访问令牌连接到 SQL 数据库。 accessToken 只能使用 DriverManager 类中 getConnection() 方法的 Properties 参数进行设置。 不能在连接 URL 中使用此属性。  

有关详细信息，请参阅[设置连接属性](setting-the-connection-properties.md)页上的 authentication 属性。  


## <a name="client-setup-requirements"></a>客户端安装要求
对于 ActiveDirectoryMSI  身份验证，必须在客户端计算机上安装以下组件：
* Java 8 或更高版本
* Microsoft JDBC Driver 7.2（或更高版本）for SQL Server
* 客户端环境必须是 Azure 资源，且必须已启用“标识”功能支持。
* 目标数据库中必须有包含的数据库用户，此用户表示 Azure 资源的系统分配的托管标识或用户分配的托管标识，或表示托管标识所属的一个组，且必须拥有 CONNECT 权限。

对于其他身份验证模式，必须在客户端计算机上安装以下组件：
* Java 7 或更高版本
* Microsoft JDBC Driver 6.0（或更高版本）for SQL Server
* 如果使用的是基于访问令牌的身份验证模式，必须有 [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) 及其依赖项，才能运行本文中的示例。 有关详细信息，请参阅 **使用访问令牌进行连接** 部分。
* 如果使用的是 ActiveDirectoryPassword  身份验证模式，必须有 [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) 及其依赖项。 有关详细信息，请参阅 **使用 ActiveDirectoryPassword 身份验证模式进行连接** 部分。
* 如果使用的是 ActiveDirectoryIntegrated  模式，必须有 azure-activedirectory-library-for-java 及其依赖项。 有关详细信息，请参阅 **使用 ActiveDirectoryIntegrated 身份验证模式进行连接** 部分。

## <a name="connecting-using-activedirectorymsi-authentication-mode"></a>使用 ActiveDirectoryMSI 身份验证模式进行连接
下面的示例展示了如何使用 `authentication=ActiveDirectoryMSI` 模式。 从 Azure 资源（例如 Azure 虚拟机、应用服务或与 Azure Active Directory 联合的函数应用）内部运行此示例。

执行此示例前，先将以下行中的服务器/数据库名称替换为你的服务器/数据库名称：

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
//Optional
ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used
```

使用 ActiveDirectoryMSI 身份验证模式的示例：

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AAD_MSI {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryMSI");
        // Optional
        ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

在 Azure 虚拟机上运行此示例会从系统分配的托管标识  或用户分配的托管标识  （如果已指定 msiClientId  的话）中提取访问令牌，并使用提取的访问令牌建立连接。 如果连接已建立，应该会看到以下消息：

```bash
You have successfully logged on as: <your Managed Identity username>
```

## <a name="connecting-using-activedirectoryintegrated-authentication-mode"></a>使用 ActiveDirectoryIntegrated 身份验证模式进行连接
自版本 6.4 起，Microsoft JDBC 驱动程序开始支持在多个平台（Windows、Linux 和 macOS）上使用 Kerberos 票证进行 ActiveDirectoryIntegrated 身份验证。
有关详细信息，请参阅[在 Windows、Linux 和 macOS 上设置 Kerberos 票证](#set-kerberos-ticket-on-windows-linux-and-macos)。 或者，在 Windows 上，mssql-jdbc_auth-\<version>-\<arch>.dll 也可用于通过 JDBC 驱动程序进行 ActiveDirectoryIntegrated 身份验证。

> [!NOTE]
>  如果使用的是旧版驱动程序，请查看这一[链接](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md)，以了解使用此身份验证模式所必需的相应依赖项。 

下面的示例展示了如何使用 `authentication=ActiveDirectoryIntegrated` 模式。 在与 Azure Active Directory 联合的域加入计算机上运行此示例。 数据库中必须有包含的数据库用户，此用户表示你的 Azure AD 用户或你所属的一个组，且必须拥有 CONNECT 权限。 

在生成并运行此示例前，先在运行此示例的客户端计算机上下载 [azure-activedirectory-library-for-java 库](https://github.com/AzureAD/azure-activedirectory-library-for-java)及其依赖项，并将它们添加到 Java 生成路径中

执行此示例前，先将以下行中的服务器/数据库名称替换为你的服务器/数据库名称：

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
```

使用 ActiveDirectoryIntegrated 身份验证模式的示例：
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADIntegrated {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryIntegrated");

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

在客户端计算机上运行此示例会自动使用 Kerberos 票证，不需要密码。 如果连接已建立，应该会看到以下消息：

```
You have successfully logged on as: <your domain user name>
```

### <a name="set-kerberos-ticket-on-windows-linux-and-macos"></a>在 Windows、Linux 和 macOS 上设置 Kerberos 票证

必须设置 Kerberos 票证，将当前用户关联到 Windows 域帐户。 下面概述了几个关键步骤。

#### <a name="windows"></a>Windows
JDK 附带 `kinit`，在与 Azure Active Directory 联合的域加入计算机上，它可用于从密钥发行中心 (KDC) 获取 TGT。

##### <a name="step-1-ticket-granting-ticket-retrieval"></a>步骤 1：票证授予票证检索
- **运行位置**：Windows
- **操作**：
  - 使用命令 `kinit username@DOMAIN.COMPANY.COM` 从 KDC 获取 TGT，然后会看到输入域密码的提示。
  - 使用 `klist` 查看可用票证。 如果 kinit 成功，应该会看到来自 krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM 的票证。

> [!NOTE]
>  可能需要指定包含 `-Djava.security.krb5.conf` 的 `.ini` 文件，以便应用程序能够找到 KDC。

#### <a name="linux-and-macos"></a>Linux 和 macOS

##### <a name="requirements"></a>要求
访问 Windows 域加入计算机，以查询 Kerberos 域控制器。

##### <a name="step-1-find-kerberos-kdc"></a>步骤 1：查找 Kerberos KDC
- **运行位置**：Windows 命令行
- **操作**：`nltest /dsgetdc:DOMAIN.COMPANY.COM`（其中“DOMAIN.COMPANY.COM”映射到域名）
- **示例输出**
  ```
  DC: \\co1-red-dc-33.domain.company.com
  Address: \\2111:4444:2111:33:1111:ecff:ffff:3333
  ...
  The command completed successfully
  ```
- **要提取的信息**：DC 名称（在此示例中为 `co1-red-dc-33.domain.company.com`）

##### <a name="step-2-configuring-kdc-in-krb5conf"></a>步骤 2：在 krb5.conf 中配置 KDC
- **运行位置**：Linux/macOS
- **操作**：在选定编辑器中编辑 /etc/krb5.conf。 配置下列密钥
  ```
  [libdefaults]
    default_realm = DOMAIN.COMPANY.COM
   
  [realms]
  DOMAIN.COMPANY.COM = {
     kdc = co1-red-dc-28.domain.company.com
  }
  ```
  然后保存 krb5.conf 文件并退出

> [!NOTE]
>  域必须全部大写。

##### <a name="step-3-testing-the-ticket-granting-ticket-retrieval"></a>步骤 3：测试票证授予票证检索
- **运行位置**：Linux/macOS
- **操作**：
  - 使用命令 `kinit username@DOMAIN.COMPANY.COM` 从 KDC 获取 TGT，然后会看到输入域密码的提示。
  - 使用 `klist` 查看可用票证。 如果 kinit 成功，应该会看到来自 krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM 的票证。

## <a name="connecting-using-activedirectorypassword-authentication-mode"></a>使用 ActiveDirectoryPassword 身份验证模式进行连接
下面的示例展示了如何使用 `authentication=ActiveDirectoryPassword` 模式。

在生成并运行此示例前：
1.  在运行此示例的客户端计算机上下载 [azure-activedirectory-library-for-java 库](https://github.com/AzureAD/azure-activedirectory-library-for-java)及其依赖项，并将它们添加到 Java 生成路径中
2.  找到以下代码行，并将服务器/数据库名称替换为你的服务器/数据库名称。
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  找到以下代码行，并将用户名替换为要以其身份连接的 Azure AD 用户的用户名。
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ds.setPassword("password");     // replace with your password
    ```

使用 ActiveDirectoryPassword 身份验证模式的示例：
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADUserPassword {
    
    public static void main(String[] args) throws Exception{
        
        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        ds.setPassword("password"); // Replace with your password
        ds.setAuthentication("ActiveDirectoryPassword");
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
如果连接已建立，应该会看到以下消息输出：
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> 必须有包含的用户数据库，且数据库中必须有包含的数据库用户，此用户表示指定 Azure AD 用户或其所属的一个组，且必须拥有 CONNECT 权限。（Azure Active Directory 服务器管理员或组除外）

## <a name="connecting-using-access-token"></a>使用访问令牌进行连接
应用程序/服务可以从 Azure Active Directory 检索访问令牌，并使用它连接到 Azure SQL 数据库/数据仓库。

> [!NOTE] 
> accessToken  只能使用 DriverManager 类中 getConnection() 方法的 Properties 参数进行设置。 不能在连接字符串中使用此属性。

下面的示例展示了简单的 Java 应用程序，它使用基于访问令牌的身份验证连接到 Azure SQL 数据库/数据仓库。 在生成并运行此示例前，请先按照以下步骤操作：
1.  在 Azure Active Directory 中为服务创建应用程序帐户。
    1. 登录到 Azure 门户。
    2. 在左侧导航栏中，单击“Azure Active Directory”。
    3. 单击“应用注册”选项卡。
    4. 在抽屉中，单击“新建应用程序注册”。
    5. 输入 mytokentest 作为应用程序的易记名称，然后选择“Web 应用/API”。
    6. 不需要登录 URL。 只需提供任何内容: "https://mytokentest" 。
    7. 单击底部的“创建”。
    9. 在 Azure 门户中，单击应用程序的“设置”选项卡，然后打开“属性”选项卡。
    10. 查找“应用程序 ID”（亦称为“客户端 ID”）值，并将它复制到一边，稍后在配置应用程序时需要用到此值（例如 1846943b-ad04-4808-aa13-4702d908b5c1）。 请参阅以下快照。
    11. 在“密钥”部分下，创建密钥，具体方法为填写“名称”字段，选择密钥期限，然后保存配置（将“值”字段留空）。 保存后，“值”字段应该会自动填充，复制生成的值。 这是客户端密码。
    12. 在左侧面板中，单击“Azure Active Directory”。 在“应用注册”下，查找“终结点”选项卡。复制“OATH 2.0 令牌终结点”下的 URL（此为 STS URL）。
    
    ![Azure 门户应用注册终结点 - STS URL](media/jdbc_aad_token.png)  
2. 以 Azure Active Directory 管理员身份登录 Azure SQL Server 的用户数据库，并使用 T-SQL 命令为应用程序主体预配包含的数据库用户。 若要详细了解如何创建 Azure Active Directory 管理员和包含的数据库用户，请参阅[使用 Azure Active Directory 身份验证连接到 SQL 数据库或 Azure Synapse Analytics](/azure/azure-sql/database/authentication-aad-overview)。

    ```
    CREATE USER [mytokentest] FROM EXTERNAL PROVIDER
    ```

3.  在运行此示例的客户端计算机上下载 [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) 库及其依赖项，并将它们添加到 Java 生成路径中。 请注意，只需要 azure-activedirectory-library-for-java 即可运行此特定示例。 此示例使用这个库中的 API 从 Azure AD 检索访问令牌。 如果已有访问令牌，可以跳过这一步。 请注意，还需要删除此示例中检索访问令牌的部分。

在下面的示例中，将 STS URL、客户端 ID、客户端密码、服务器和数据库名称替换为你的值。

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

// The azure-activedirectory-library-for-java is needed to retrieve the access token from the AD.
import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;
import com.microsoft.aad.adal4j.ClientCredential;

public class AADTokenBased {

    public static void main(String[] args) throws Exception {

        // Retrieve the access token from the AD.
        String spn = "https://database.windows.net/";
        String stsurl = "https://login.microsoftonline.com/..."; // Replace with your STS URL.
        String clientId = "1846943b-ad04-4808-aa13-4702d908b5c1"; // Replace with your client ID.
        String clientSecret = "..."; // Replace with your client secret.

        AuthenticationContext context = new AuthenticationContext(stsurl, false, Executors.newFixedThreadPool(1));
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        Future<AuthenticationResult> future = context.acquireToken(spn, cred, null);
        String accessToken = future.get().getAccessToken();

        System.out.println("Access Token: " + accessToken);

        // Connect with the access token.
        SQLServerDataSource ds = new SQLServerDataSource();

        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name.
        ds.setDatabaseName("demo"); // Replace with your database name.
        ds.setAccessToken(accessToken);

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
``` 

如果连接成功，应该会看到以下消息输出：

```bash
Access Token: <your access token>
You have successfully logged on as: <your client ID>    
```
