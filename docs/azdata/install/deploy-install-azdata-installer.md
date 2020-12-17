---
title: 使用 Windows Installer 安装 Azure Data CLI (azdata)
titleSuffix: ''
description: 了解如何使用安装程序来安装 Azure Data CLI (azdata) 工具。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c5b190c50dbbeebef94cdd15314539e5ce501160
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489637"
---
# <a name="install-azure-data-cli-azdata-with-windows-installer"></a>使用 Windows Installer 安装 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

[!INCLUDE [azdata](../../includes/applies-to-version/azdata.md)]

本文介绍如何使用安装程序在 Windows 上安装 `azdata`。 使用 `azdata` 管理 SQL Server 大数据群集或已启用 Azure Arc 的数据服务。

## <a name="steps-to-install-azdata-with-the-microsoft-windows-installer"></a>使用 Microsoft Windows Installer 安装 `azdata` 的步骤

使用 Microsoft Windows Installer 安装 `azdata`：

1. 删除 `azdata`（如果已使用 `pip` 安装了它）。 如果已使用 Windows Installer 安装了 `azdata`，则继续执行下一步。
1. 使用 [Windows Installer](https://aka.ms/azdata-msi) 安装 `azdata`。

### <a name="uninstall-azdata-with-windows-installer"></a>使用 Windows Installer 卸载 `azdata`

若要使用 Windows Installer 卸载 `azdata`，请按照相应操作系统的说明进行操作。

| 平台      | 说明                                           |
| ------------- |--------------------------------------------------------|
| Windows 10| “开始”>“设置”>“应用”                                |
| Windows 8     | “开始”>“控制面板”>“程序”>“卸载程序” |

要卸载的程序名为 `Azure Data CLI`。 选择此应用程序，然后单击 `Uninstall` 按钮。

## <a name="next-steps"></a>后续步骤

有关大数据群集的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ver15.md)]？](../../big-data-cluster/big-data-cluster-overview.md)

将 `azdata` 用于[启用了 Azure Arc 的数据服务](/azure/azure-arc/data/)
