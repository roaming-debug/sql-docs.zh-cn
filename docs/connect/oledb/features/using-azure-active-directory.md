---
title: 使用 Azure Active Directory
description: 了解可用于连接到 Azure SQL 数据库的 Microsoft OLE DB Driver for SQL Server 中可用的 Azure Active Directory 身份验证方法。
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 71f95203e006141649db7b884b56d085f562974b
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504677"
---
# <a name="using-azure-active-directory"></a>使用 Azure Active Directory
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="purpose"></a>目的


从版本 [18.2.1](../release-notes-for-oledb-driver-for-sql-server.md#1821) 开始，Microsoft OLE DB Driver for SQL Server 允许 OLE DB 应用程序使用联合身份连接到 Azure SQL 数据库的实例。 新的身份验证方法包括：
- Azure Active Directory 登录 ID 和密码
- Azure Active Directory 访问令牌
- Azure Active Directory 集成身份验证
- SQL 登录 ID 和密码


版本 [18.3.0](../release-notes-for-oledb-driver-for-sql-server.md#1830) 添加了对以下身份验证方法的支持：
- Azure Active Directory 交互式身份验证
- Azure Active Directory 托管标识身份验证

版本 [18.5.0](../release-notes-for-oledb-driver-for-sql-server.md#1850) 添加了对以下身份验证方法的支持：
- Azure Active Directory 服务主体身份验证

> [!NOTE]
> 不支持使用以下将 `DataTypeCompatibility`（或其对应的属性）设置为 `80` 的身份验证模式：
> - 使用登录 ID 和密码进行 Azure Active Directory 身份验证
> - 使用访问令牌进行 Azure Active Directory 身份验证
> - Azure Active Directory 集成身份验证
> - Azure Active Directory 交互式身份验证
> - Azure Active Directory 托管标识身份验证
> - Azure Active Directory 服务主体身份验证

## <a name="connection-string-keywords-and-properties"></a>连接字符串关键字和属性
引入了以下连接字符串关键字，以支持 Azure Active Directory 身份验证：

|连接字符串关键字|连接属性|说明|
|---               |---                |---        |
|访问令牌|SSPROP_AUTH_ACCESS_TOKEN|指定对 Azure Active Directory 进行身份验证的访问令牌。 |
|身份验证|SSPROP_AUTH_MODE|指定要使用的身份验证方法。|

有关新关键字/属性的详细信息，请参阅以下页面：
- [将连接字符串关键字与适用于 SQL Server 的 OLE DB 驱动程序结合使用](../applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)
- [初始化和授权属性](../ole-db-data-source-objects/initialization-and-authorization-properties.md)

## <a name="encryption-and-certificate-validation"></a>加密和证书验证
本部分讨论加密和证书验证行为的更改。 仅当使用新的身份验证或访问令牌连接字符串关键字（或其对应的属性）时，这些更改才有效。

### <a name="encryption"></a>加密
为了提高安全性，当使用新的连接属性/关键字时，驱动程序会通过将默认加密值设置为 `yes` 来重写该值。 重写在数据源对象初始化时发生。 如果在通过任何方法初始化之前设置加密，则会考虑该值，而不会进行重写。

> [!NOTE]   
> 在 ADO 应用程序以及通过 `IDataInitialize::GetDataSource` 获取 `IDBInitialize` 接口的应用程序中，实现接口的核心组件会将加密显式设置为其默认值 `no`。 因此，新的身份验证属性/关键字遵循此设置，而不会重写加密值。 因此，建议这些应用程序显式设置 `Use Encryption for Data=true` 来重写默认值。

### <a name="certificate-validation"></a>证书验证
为了提高安全性，新的连接属性/关键字遵循 `TrustServerCertificate` 设置（及其相应的连接字符串关键字/属性），独立于客户端加密设置。 因此，默认情况下会验证服务器证书。

> [!NOTE]   
> 还可以通过 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SNI18.0\GeneralFlags\Flag2` 注册表项的 `Value` 字段来控制证书验证。 有效值为 `0` or `1`进行求值的基于 SQL 语言的筛选器表达式。 OLE DB 驱动程序在注册表和连接属性/关键字设置之间选择最安全的选项。 也就是说，只要至少有一个注册表/连接设置启用服务器证书验证，驱动程序就会验证服务器证书。

## <a name="gui-additions"></a>GUI 添加件
驱动程序图形用户界面已增强，以允许 Azure Active Directory 身份验证。 有关详细信息，请参阅：
- [“SQL Server 登录”对话框](../help-topics/sql-server-login-dialog.md)
- [通用数据链接 (UDL) 配置](../help-topics/data-link-pages.md)

## <a name="example-connection-strings"></a>连接字符串示例
本节展示将与 `IDataInitialize::GetDataSource` 和 `DBPROP_INIT_PROVIDERSTRING` 属性一起使用的新连接字符串关键字和现有连接字符串关键字的示例。

### <a name="sql-authentication"></a>SQL 身份验证
- 使用 `IDataInitialize::GetDataSource`：
    - 新建:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=SqlPassword**;User ID=[username];Password=[password];Use Encryption for Data=true
    - 已弃用：
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];User ID=[username];Password=[password];Use Encryption for Data=true
- 使用 `DBPROP_INIT_PROVIDERSTRING`：
    - 新建:
        > Server=[server];Database=[database];**Authentication=SqlPassword**;UID=[username];PWD=[password];Encrypt=yes
    - 已弃用：
        > Server=[server];Database=[database];UID=[username];PWD=[password];Encrypt=yes

### <a name="integrated-windows-authentication-using-security-support-provider-interface-sspi"></a>使用安全支持提供程序接口 (SSPI) 进行集成 Windows 身份验证

- 使用 `IDataInitialize::GetDataSource`：
    - 新建:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryIntegrated**;Use Encryption for Data=true
    - 已弃用：
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Integrated Security=SSPI**;Use Encryption for Data=true
- 使用 `DBPROP_INIT_PROVIDERSTRING`：
    - 新建:
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryIntegrated**;Encrypt=yes
    - 已弃用：
        > Server=[server];Database=[database];**Trusted_Connection=yes**;Encrypt=yes

### <a name="azure-active-directory-username-and-password-authentication"></a>Azure Active Directory 用户名和密码身份验证

- 使用 `IDataInitialize::GetDataSource`：
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryPassword**;User ID=[username];Password=[password];Use Encryption for Data=true
- 使用 `DBPROP_INIT_PROVIDERSTRING`：
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryPassword**;UID=[username];PWD=[password];Encrypt=yes

### <a name="azure-active-directory-integrated-authentication"></a>Azure Active Directory 集成身份验证

- 使用 `IDataInitialize::GetDataSource`：
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryIntegrated**;Use Encryption for Data=true
- 使用 `DBPROP_INIT_PROVIDERSTRING`：
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryIntegrated**;Encrypt=yes

### <a name="azure-active-directory-authentication-using-an-access-token"></a>使用访问令牌进行 Azure Active Directory 身份验证

- 使用 `IDataInitialize::GetDataSource`：
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Access Token=[access token]**;Use Encryption for Data=true
- 使用 `DBPROP_INIT_PROVIDERSTRING`：
    > 不支持通过 `DBPROP_INIT_PROVIDERSTRING` 提供访问令牌

### <a name="azure-active-directory-interactive-authentication"></a>Azure Active Directory 交互式身份验证

- 使用 `IDataInitialize::GetDataSource`：
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryInteractive**;User ID=[username];Use Encryption for Data=true
- 使用 `DBPROP_INIT_PROVIDERSTRING`：
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryInteractive**;UID=[username];Encrypt=yes

### <a name="azure-active-directory-managed-identity-authentication"></a>Azure Active Directory 托管标识身份验证

- 使用 `IDataInitialize::GetDataSource`：
    - 用户分配的托管标识：
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryMSI**;User ID=[Object ID];Use Encryption for Data=true
    - 系统分配的托管标识：
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryMSI**;Use Encryption for Data=true
- 使用 `DBPROP_INIT_PROVIDERSTRING`：
    - 用户分配的托管标识：
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryMSI**;UID=[Object ID];Encrypt=yes
    - 系统分配的托管标识：
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryMSI**;Encrypt=yes

### <a name="azure-active-directory-service-principal-authentication"></a>Azure Active Directory 服务主体身份验证

- 使用 `IDataInitialize::GetDataSource`：
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];Authentication=ActiveDirectoryServicePrincipal;User ID=[Application (client) ID];Password=[Application (client) secret];Use Encryption for Data=true
- 使用 `DBPROP_INIT_PROVIDERSTRING`：
    > Server=[server];Database=[database];Authentication=ActiveDirectoryServicePrincipal;UID=[Application (client) ID];PWD=[Application (client) secret];Encrypt=yes

## <a name="code-samples"></a>代码示例

以下示例显示了使用连接关键字连接到 Azure Active Directory 所需的代码。 

### <a name="access-token"></a>访问令牌
```cpp
#include <string>
#include <iostream>
#include <msdasc.h>

int main()
{
    wchar_t azureServer[] = L"server";
    wchar_t azureDatabase[] = L"mydatabase";
    wchar_t accessToken[] = L"eyJ0eXAiOi...";
    IDBInitialize *pIDBInitialize = nullptr;
    IDataInitialize* pIDataInitialize = nullptr;
    HRESULT hr = S_OK;

    CoInitialize(nullptr);

    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(azureServer) + L";Initial Catalog=" + 
                              std::wstring(azureDatabase) + L";Access Token=" + accessToken + L";Use Encryption for Data=true;";
    hr = CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, 
                          IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to create an IDataInitialize instance." << std::endl;
        goto Cleanup;
    }
    hr = pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), 
                                         IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to get data source object." << std::endl;
        goto Cleanup;
    }
    hr = pIDBInitialize->Initialize();
    if (FAILED(hr))
    {
        std::cout << "Failed to establish connection." << std::endl;
        goto Cleanup;
    }

Cleanup:
    if (pIDBInitialize)
    {
        pIDBInitialize->Uninitialize();
        pIDBInitialize->Release();
    }
    if (pIDataInitialize)
    {
        pIDataInitialize->Release();
    }

    CoUninitialize();
}
```
### <a name="active-directory-integrated"></a>Active Directory 已集成
```cpp
#include <string>
#include <iostream>
#include <msdasc.h>

int main()
{
    wchar_t azureServer[] = L"server";
    wchar_t azureDatabase[] = L"mydatabase";
    IDBInitialize *pIDBInitialize = nullptr;
    IDataInitialize* pIDataInitialize = nullptr;
    HRESULT hr = S_OK;

    CoInitialize(nullptr);

    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(azureServer) + L";Initial Catalog=" + 
                              std::wstring(azureDatabase) + L";Authentication=ActiveDirectoryIntegrated;Use Encryption for Data=true;";

    hr = CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, 
                          IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize));
    if (FAILED(hr)) 
    {
        std::cout << "Failed to create an IDataInitialize instance." << std::endl;
        goto Cleanup;
    }
    hr = pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), 
                                         IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to get data source object." << std::endl;
        goto Cleanup;
    }
    hr = pIDBInitialize->Initialize();
    if (FAILED(hr))
    {
        std::cout << "Failed to establish connection." << std::endl;
        goto Cleanup;
    }

Cleanup:
    if (pIDBInitialize)
    {
        pIDBInitialize->Uninitialize();
        pIDBInitialize->Release();
    }
    if (pIDataInitialize)
    {
        pIDataInitialize->Release();
    }

    CoUninitialize();
}
```

## <a name="next-steps"></a>后续步骤
- [使用 OAuth 2.0 代码授权流来授权访问 Azure Active Directory Web 应用程序](/azure/active-directory/azuread-dev/v1-protocols-oauth-code)。

- 了解 SQL Server 的 [Azure Active Directory 身份验证](/azure/azure-sql/database/authentication-aad-overview)。

- 使用 OLE DB 驱动程序支持的[连接字符串关键字](../applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)配置驱动程序连接。