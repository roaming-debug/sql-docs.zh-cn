---
description: 展示如何结合使用 Azure Key Vault 提供程序和已启用安全 Enclave 的 Always Encrypted 的示例
title: 展示如何结合使用 Azure Key Vault 提供程序和已启用安全 Enclave 的 Always Encrypted 的示例 | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2020
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 39d03c4ddcf7edc7d0df1176c9aed1b92f9af9d5
ms.sourcegitcommit: 4c3949f620d09529658a2172d00bfe37aeb1a387
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2020
ms.locfileid: "96123973"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted-enabled-with-secure-enclaves"></a>展示如何结合使用 Azure Key Vault 提供程序和已启用安全 Enclave 的 Always Encrypted 的示例

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

此示例展示了如何在访问加密列时使用 Azure Key Vault 提供程序。

[!code-csharp [Azure Key Vault Provider with Enclave Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderWithEnclaveProviderExample.cs#1)]

> [!NOTE]
> - 若要使用带有 .NET Standard 应用程序的安全 Enclave 的 Always Encrypted，需要 Microsoft.Data.SqlClient 版本 2.1.0 或更高版本。 支持的 .NET Standard 版本为 2.1 或更高版本。 
>
> - 若要在 Linux 和 macOS 上使用带有安全 Enclave 的 Always Encrypted，需要 Microsoft.Data.SqlClient 版本 2.1.0 或更高版本。

## <a name="see-also"></a>另请参阅

- [展示结合使用 Azure Key Vault 提供程序和 Always Encrypted 的示例](azure-key-vault-example.md)
- [教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [结合 Always Encrypted 和 Microsoft .NET Data Provider for SQL Server](sqlclient-support-always-encrypted.md)
