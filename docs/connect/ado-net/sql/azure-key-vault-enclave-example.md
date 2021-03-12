---
description: 展示如何将 Azure Key Vault 提供程序与通过安全 enclave 启用的 Always Encrypted 结合使用的示例
title: 展示如何将 Azure Key Vault 提供程序与通过安全 enclave 启用的 Always Encrypted 结合使用的示例 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: df1b2fb3c99eac35b2a742254b75a8f06b3f37bf
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464908"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted-enabled-with-secure-enclaves"></a>展示如何将 Azure Key Vault 提供程序与通过安全 enclave 启用的 Always Encrypted 结合使用的示例

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

## <a name="azurekeyvaultprovider-v20"></a>AzureKeyVaultProvider v2.0+

[!code-csharp [Azure Key Vault Provider 2.0 with Enclave Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderWithEnclaveProviderExample_2_0.cs#1)]

## <a name="azurekeyvaultprovider-v1x"></a>AzureKeyVaultProvider v1.x

[!code-csharp [Azure Key Vault Provider with Enclave Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderWithEnclaveProviderExample.cs#1)]

> [!NOTE]
>
> - 若要使用带有 .NET Standard 应用程序的安全 Enclave 的 Always Encrypted，需要 Microsoft.Data.SqlClient 版本 2.1.0 或更高版本。 支持的 .NET Standard 版本为 2.1 或更高版本。
>
> - 若要在 Linux 和 macOS 上使用带有安全 Enclave 的 Always Encrypted，需要 Microsoft.Data.SqlClient 版本 2.1.0 或更高版本。

## <a name="see-also"></a>另请参阅

- [展示结合使用 Azure Key Vault 提供程序和 Always Encrypted 的示例](azure-key-vault-example.md)
- [教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [结合 Always Encrypted 和 Microsoft .NET Data Provider for SQL Server](sqlclient-support-always-encrypted.md)
