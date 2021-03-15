---
title: SqlPackage for Azure Synapse Analytics
description: 在 Azure Synapse Analytics 方案中使用 SqlPackage 的提示
ms.custom: tools|sos
ms.date: 03/10/2021
ms.prod: sql
ms.reviewer: llali; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 661230edf4deea3d62ceef7d8b400cdb951330a5
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622904"
---
# <a name="sqlpackage-for-azure-synapse-analytics"></a>SqlPackage for Azure Synapse Analytics

本文重点介绍针对 Azure Synapse Analytics 数据库的 SqlPackage.exe 功能。

## <a name="extract"></a>Extract
若要将架构[提取](sqlpackage-extract.md)到 Azure Blob 存储，需要以下属性：
- /p:AzureStorageBlobEndpoint
- /p:AzureStorageContainer
- /p:AzureStorageKey

提取访问权限通过存储帐户密钥进行授权。  附加参数是可选的，该参数可在容器中设置存储根路径：
- /p:AzureStorageRootPath

如果没有此属性，路径将默认为 `servername/databasename/timestamp/`。

## <a name="publish"></a>发布
若要从 Azure Blob 存储中的 dacpac [发布](sqlpackage-publish.md)架构，需要以下属性：
- /p:AzureStorageBlobEndpoint
- /p:AzureStorageContainer
- /p:AzureStorageRootPath
- /p:AzureStorageKey or /p:AzureSharedAccessSignatureToken

可以通过存储帐户密钥或共享访问签名 (SAS) 令牌来授权发布访问权限。

## <a name="next-steps"></a>后续步骤
- 详细了解[提取](sqlpackage-extract.md)
- 详细了解[发布](sqlpackage-publish.md)
- 了解有关 [Azure Blob 存储](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)的详细信息
- 详细了解 [Azure 存储帐户密钥](https://docs.microsoft.com/azure/storage/common/storage-account-keys-manage)