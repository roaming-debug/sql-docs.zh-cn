---
description: 演示将 Azure Key Vault 提供程序与 Always Encrypted 配合使用的示例
title: 演示将 Azure Key Vault 提供程序与 Always Encrypted 配合使用的示例 | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2020
ms.reviewer: v-daenge
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 3ac1790e6e41d4024ef7d092378f6d548ed41c4f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835644"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted"></a>演示将 Azure Key Vault 提供程序与 Always Encrypted 配合使用的示例

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

此示例展示了如何在访问加密列时使用 Azure Key Vault 提供程序。

[!code-csharp [AKVProvider Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample.cs#1)]

> [!NOTE]
> - 若要使用不带 .NET Standard 应用程序的安全 Enclave 的 Always Encrypted 功能，需要 Microsoft.Data.SqlClient 版本 2.1.0 或更高版本。 支持的 .NET Standard 版本为 2.0 或更高版本。 
>
> - 若要在 Linux 和 macOS 上使用 Always Encrypted 功能，需要 Microsoft.Data.SqlClient 版本 2.1.0 或更高版本。

## <a name="see-also"></a>另请参阅

- [展示如何结合使用 Azure Key Vault 提供程序和已启用安全 Enclave 的 Always Encrypted 的示例](azure-key-vault-enclave-example.md)
- [教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [结合 Always Encrypted 和 Microsoft .NET Data Provider for SQL Server](sqlclient-support-always-encrypted.md)
