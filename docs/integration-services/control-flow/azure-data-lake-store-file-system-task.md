---
description: Azure Data Lake Store 文件系统任务
title: Azure Data Lake Store 文件系统任务 | Microsoft Docs
ms.custom: ''
ms.date: 08/22/2017
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSTASK.F1
- SQL14.DTS.DESIGNER.AFPADLSTASK.F1
author: Lingxi-Li
ms.author: lingxl
ms.reviewer: maghan
ms.openlocfilehash: 27ee71393bfbad6c824de25bd95c9ca465cf2a51
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123647"
---
# <a name="azure-data-lake-store-file-system-task"></a>Azure Data Lake Store 文件系统任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Azure Data Lake Store 文件系统任务允许用户在 [Azure Data Lake Store (ADLS)](https://azure.microsoft.com/services/data-lake-store/) 上执行各种文件系统操作。

Azure Data Lake Store 文件系统任务是[适用于 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)组件。

## <a name="configure-the-azure-data-lake-store-file-system-task"></a>配置 Azure Data Lake Store 文件系统任务

若要将 Azure Data Lake Store 文件系统任务添加到某个包，请将它从 SSIS 工具箱拖到设计器画布中。 然后双击该任务，或右键单击该任务并选择“编辑”，以打开“Azure Data Lake Store 文件系统任务编辑器”对话框。

“操作”属性指定要执行的文件系统操作。 请选择以下操作之一：

- **CopyToADLS：** 将文件上载到 ADLS。
- **CopyFromADLS：** 从 ADLS 下载文件。

## <a name="configure-the-properties-for-the-operation"></a>配置操作属性
对任何操作都必须指定 Azure Data Lake 连接管理器。

以下是特定于每种操作的属性：

### <a name="copytoadls"></a>CopyToADLS
- **LocalDirectory：** 指定包含待上载文件的本地源目录。
- **FileNamePattern：** 指定源文件的文件名筛选器。 仅上载名称与指定模式匹配的文件。 支持 `*` 和 `?` 通配符。
- **SearchRecursively：** 指定是否在源目录中以递归方式搜索要上载的文件。
- **AzureDataLakeDirectory：** 指定要将文件上载到的 ADLS 目标目录。
- **FileExpiry：** 指定上载到 ADLS 的文件的到期日期和时间。 将此属性留空表示文件永不过期。

### <a name="copyfromadls"></a>CopyFromADLS
- **AzureDataLakeDirectory：** 指定包含待下载文件的 ADLS 源目录。
- **SearchRecursively：** 指定是否在源目录中以递归方式搜索要下载的文件。
- **LocalDirectory：** 指定要存储已下载文件的目标目录。
