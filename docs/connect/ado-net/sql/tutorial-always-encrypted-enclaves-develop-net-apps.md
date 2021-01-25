---
description: 教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序
title: 教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序 | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 177737bd2927583bdfda1c9b36904faf4ed6023d
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534696"
---
# <a name="tutorial-develop-a-net-application-using-always-encrypted-with-secure-enclaves"></a>教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-xxxx-md.md)]

本教程介绍如何开发应用程序，该应用程序可发出使用[具有安全 enclave 的 Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-enclaves.md) 的服务器端安全 enclave 的数据库查询。

> [!NOTE]
> 具有安全 Enclave 的 Always Encrypted 仅可在 Windows 上使用。

## <a name="prerequisites"></a>先决条件

执行本教程中的以下步骤之前，请确保已完成以下教程之一：

- [教程：在 SQL Server 中开始使用具有安全 enclave 的 Always Encrypted](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md)
- [教程：在 Azure SQL 数据库中开始使用具有安全 enclave 的 Always Encrypted](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

此外，还需要 Visual Studio（推荐使用 2019 版）- 可从 [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com) 下载。 应用程序开发环境必须使用 .NET Framework 4.6 或更高版本/.NET Core 2.1 或更高版本。

## <a name="step-1-set-up-your-visual-studio-project"></a>步骤 1：创建 Visual Studio 项目

若要在 .NET Framework 应用程序中使用具有安全 enclave 的 Always Encrypted，需要确保应用程序定目标到 .NET Framework 4.6 或更高版本。 若要在 .NET Core 应用程序中使用具有安全 enclave 的 Always Encrypted，需要确保应用程序定目标到 .NET Core 2.1 或更高版本。

此外，如果将列主密钥存储在 Azure Key Vault 中，还需要将应用程序与 [Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider NuGet](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider) 集成。

1. 打开 Visual Studio。

2. 新建 C\# 控制台应用程序 (.NET Framework/Core) 项目。

3. 请确保项目至少定目标到 .NET Framework 4.6 或 .NET Core 2.1。 在“解决方案资源管理器”中右键单击项目，选择“属性”，然后设置目标框架。

4. 通过转到“工具”（主菜单）>“NuGet 包管理器” > “包管理器控制台”安装以下 NuGet 包。 在“包管理器控制台”中运行以下代码。

   ```powershell
   Install-Package Microsoft.Data.SqlClient -Version 1.1.0
   ```

5. 如果使用 Azure Key Vault 来存储列主密钥，通过转到“工具”（主菜单）>“NuGet 包管理器” > “包管理器控制台”安装以下 NuGet 包。 在“包管理器控制台”中运行以下代码。

   ```powershell
   Install-Package Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider -Version 1.0.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

## <a name="step-2-implement-your-application-logic"></a>步骤 2：实现应用程序逻辑

应用程序将连接到[教程：通过 SSMS 开始使用具有安全 enclave 的 Always Encrypted](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) 或 - [教程：在 Azure SQL 数据库中开始使用具有安全 enclave 的 Always Encrypted](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) 中的 ContosoHR 数据库，它运行包含 SSN 列上 `LIKE` 谓词和 Salary 列上范围比较的查询。

1. 将 Program.cs 文件（由 Visual Studio 生成）的内容替换为以下代码。 

    ```cs
    using System;
    using Microsoft.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {

                // Connection string for SQL Server
                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                // Connection string for Azure SQL Database
                //string connectionString = "Data Source = myserver.database.windows.net; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = AAS; Enclave Attestation Url = https://myattestationprovider.uks.attest.azure.net/attest/SgxEnclave; User ID=user; Password=password";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [HR].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%9838";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 20000;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();

                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader[0] + ", " + reader[1] + ", " + reader[2] + ", " + reader[3]);
                    }
                    Console.ReadKey();
                }
            }
        }
    }
    ```

2. 更新数据库连接字符串。
    1. 设置有效的服务器名称和数据库身份验证设置。
    2. 将 `Attestation Protocol` 关键字的值设置为：
       - `HGS` - 如果使用的是 [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] 和主机保护者服务 (HGS)。
       - `AAS` - 如果使用的是 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 和 Microsoft Azure 证明。
    3. 将 `Enclave Attestation URL` 设置为你的环境的证明 URL。

3. 生成并运行应用程序。

## <a name="see-also"></a>另请参阅

- [结合使用 Always Encrypted 和 Microsoft .NET Data Provider for SQL Server](sqlclient-support-always-encrypted.md)
- [展示结合使用 Azure Key Vault 提供程序和 Always Encrypted 的示例](azure-key-vault-example.md)
- [展示如何结合使用 Azure Key Vault 提供程序和已启用安全 Enclave 的 Always Encrypted 的示例](azure-key-vault-enclave-example.md)
