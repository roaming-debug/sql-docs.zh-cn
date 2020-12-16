---
description: 教程：开发使用具有安全 enclave 的 Always Encrypted 的 .NET Framework 应用程序
title: 教程：开发使用具有安全 enclave 的 Always Encrypted 的 .NET Framework 应用程序 | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 84ab8334a2a34552d0aa301d7fd92d04dd9ce214
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463078"
---
# <a name="tutorial-develop-a-net-framework-application-using-always-encrypted-with-secure-enclaves"></a>教程：开发使用具有安全 enclave 的 Always Encrypted 的 .NET Framework 应用程序
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

本教程介绍如何开发简单的应用程序，该应用程序可发出使用[具有安全 enclave 的 Always Encrypted](encryption/always-encrypted-enclaves.md) 的服务器端安全 enclave 的数据库查询。 

## <a name="prerequisites"></a>先决条件
本教程是[教程：通过 SSMS 开始使用含安全 enclave 的 Always Encrypted](./tutorial-getting-started-with-always-encrypted-enclaves.md) 的延续。 请务必先完成上一教程，再执行以下步骤。

此外，还需要 Visual Studio（推荐使用 2019 版）- 可从 [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com) 下载。 应用程序开发计算机必须运行 .NET Framework 4.7.2 或更高版本。

## <a name="step-1-set-up-your-visual-studio-project"></a>步骤 1：创建 Visual Studio 项目

若要在 .NET Framework 应用程序中使用具有安全 enclave 的 Always Encrypted，需要确保应用程序是针对 .NET Framework 4.7.2 生成的并与 [Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders NuGet](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders) 集成。 此外，如果将列主密钥中存储在 Azure Key Vault 中，还需要将应用程序与 [Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider NuGet](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider) 2.2.0 或更高版本集成。 

1. 打开 Visual Studio。

2. 创建新的 C\# 控制台应用 (.NET Framework) 项目。

3. 请确保项目至少面向 .NET Framework 4.7.2。 右键单击“解决方案资源管理器”中的项目，选择“属性”，将目标框架设置为“.NET Framework 4.7.2”。

4. 通过转到“工具”（主菜单）>“NuGet 包管理器” > “包管理器控制台”安装以下 NuGet 包。 在“包管理器控制台”中运行以下代码。

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders -IncludePrerelease
   ```

5. 如果使用 Azure Key Vault 来存储列主密钥，通过转到“工具”（主菜单）>“NuGet 包管理器” > “包管理器控制台”安装以下 NuGet 包。 在“包管理器控制台”中运行以下代码。

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider -IncludePrerelease -Version 2.2.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

7. 打开项目的 App.config 文件。

8. 找到 \<configuration\> 部分，然后添加或更新 \<configSections\> 部分。

   a. 如果 \<configuration\> 部分不包含 \<configSections\> 部分，请直接在 \<configuration\> 下添加以下内容。
   
      ```xml
      <configSections>
         <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
      </configSections>
      ```
   b. 如果 \<configruation\> 部分已包含 \<configSections\> 部分，请在 \<configSections\> 中添加以下行：

   ```xml
   <section name="SqlColumnEncryptionEnclaveProviders"  type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data,  Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" /\>
   ```

9. 在配置部分的 \<configSections\> 下添加以下部分，它指定一个 enclave 提供程序，用来证明并与 VBS enclave 进行交互：

   ```xml
   <SqlColumnEncryptionEnclaveProviders>
       <providers>
           <add name="VBS"  type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider,  Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,    Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/>
       </providers>
   </SqlColumnEncryptionEnclaveProviders>
   ```

下面是一个简单控制台应用程序的 app.config 文件的完整示例。
```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
  <SqlColumnEncryptionEnclaveProviders>
    <providers>
      <add name="VBS"  type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider,  Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,    Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/>
    </providers>
  </SqlColumnEncryptionEnclaveProviders>
  <startup> 
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
  </startup>
</configuration>
```
## <a name="step-2-implement-your-application-logic"></a>步骤 2：实现应用程序逻辑
应用程序将连接到[教程：通过 SSMS 开始使用具有安全 enclave 的 Always Encrypted](tutorial-getting-started-with-always-encrypted-enclaves.md) 中的 ContosoHR 数据库，它将运行一个包含“SSN”列上的 `LIKE` 谓词的查询和对“Salary”列运行范围比较 。

1. 将 Program.cs 文件（由 Visual Studio 生成）的内容替换为以下代码。 使用你的服务器名称和环境的 enclave 证明 URL 更新数据库连接字符串。 此外，也可以更新数据库身份验证设置。

    ```cs
    using System;
    using System.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {
    
                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [dbo].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%1111";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 900;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();
    
                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader);
                        Console.WriteLine(reader[0] + ", " + reader[1] + ", " + reader[2] + ", " + reader[3]);
                    }   
                    Console.ReadKey();
                }
            }
        }
    }
    ```
2. 生成并运行应用程序。  

## <a name="see-also"></a>另请参阅
- [对用于 SQL Server 的 .NET Framework 数据提供程序使用 Always Encrypted](encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)
