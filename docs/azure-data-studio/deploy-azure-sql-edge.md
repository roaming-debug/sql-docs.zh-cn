---
title: 通过 Azure Data Studio 部署 Azure SQL Edge
description: 如何在 Azure Data Studio 中部署 Azure SQL Edge 实例
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: b0dd541ce4be57dd9b065b93413ea567c7f121b7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052218"
---
# <a name="deploy-azure-sql-edge-with-azure-data-studio-preview"></a>通过 Azure Data Studio 部署 Azure SQL Edge（预览版）

[Azure SQL Edge](/azure/azure-sql-edge/overview) 是已优化的关系数据库引擎，更适合 IoT 和 Azure IoT Edge 部署。 它提供为 IoT 应用程序和解决方案创建高性能数据存储和处理层的功能。 本文演示如何通过 Azure Data Studio 部署 Azure SQL Edge 实例，以及部署向导支持的部署方案。  

可通过 Azure Data Studio 中的部署向导实现以下方案：

- 本地容器实例
- 远程容器实例
- 新的 Azure IoT 中心和 VM
- Azure IoT 中心的现有设备
- Azure IoT 中心的多个设备

| 必需工具 | `Docker` | `Azure CLI` |
| ------------- | :---: | :---: |
| 本地容器实例 | x | |
| 远程容器实例 | | |
| 新的 Azure IoT 中心和 VM | | x |
| Azure IoT 中心的现有设备 |  | x |
| Azure IoT 中心的多个设备 |   |  x |

> [!NOTE]
> 虽然这些功能处于预览阶段，但可以通过扩展“Azure SQL Edge 部署扩展”实现 Azure SQL Edge 部署（预览版）。 要能够使用这些功能，请在 Azure Data Studio 中安装该扩展。

若要在 Azure Data Studio 中开始部署，请在“连接”视图中打开菜单，然后选择“新部署…”选项。   对于所有 Azure SQL Edge 方案，请在以下屏幕上选择“Azure SQL Edge”，并从“部署目标”下拉菜单中选择所需方案。  部署向导将生成一个 TSQL 笔记本，可以执行该笔记本来完成部署。 应注意的是，默认情况下，笔记本中包含连接信息（包括 SQL 管理员密码）。

![部署概述](media/deploy-azure-sql-edge/deploy-overview.png)

## <a name="local-container-instance"></a>本地容器实例

可以通过指定容器名称、`sa` 用户密码和用于 Azure SQL Edge 连接的主机端口，将 Azure SQL Edge 部署到 localhost 上的 Docker 容器。  完成部署后，笔记本底部会显示几个链接，指向进一步操作：

- **选择此处以连接到 Azure SQL Edge 实例**：在 Azure Data Studio 中创建指向新 Azure SQL Edge 实例的连接
- **打开终端窗口**：在 Azure Data Studio 中打开（现有或新的）终端
- **停止容器**：将命令复制到在用户执行时停止容器的终端
- **删除容器**：将命令复制到在用户执行时删除容器的终端

## <a name="remote-container-instance"></a>远程容器实例

通过指定容器信息和目标/主机计算机信息，可以将 Azure SQL Edge 部署到安装了 Docker 的远程主机上的 Docker 容器。  完成部署后，笔记本底部会显示一个连接链接。  由于远程容器主机环境，若要停止或删除容器，必须将命令复制到远程 shell 中执行。

## <a name="new-azure-iot-hub-and-vm"></a>新的 Azure IoT 中心和 VM

Azure SQL Edge 部署向导可创建多项 Azure 资源，以允许部署连接到 Azure IoT 中心的支持边缘的虚拟机 (VM)。 需要一个有效的 Azure 订阅。 此部署将创建以下内容：

- 资源组（如果未选择当前资源组）
- 网络安全组
- 虚拟机
- 静态公共 IP 地址

在该过程中，可以选择将 dacpac 文件压缩到文件夹中，并将其部署到新的 Azure SQL Edge 实例。  如果提供了 dacpac 文件，则会在同一资源组中创建 Azure Blob 存储帐户。

## <a name="existing-device-of-an-azure-iot-hub"></a>Azure IoT 中心的现有设备

如果你已有 IoT 中心和已连接的设备，可以根据资源组、IoT 中心名称和设备 ID 将 Azure SQL Edge 部署到设备。
使用部署向导期间提供的 IP 地址，可在笔记本底部生成快速连接链接。

在该过程中，可以选择将 dacpac 文件压缩到文件夹中，并将其部署到新的 Azure SQL Edge 实例。  如果提供了 dacpac 文件，则会在同一资源组中创建 Azure Blob 存储帐户。

## <a name="multiple-devices-of-an-azure-iot-hub"></a>Azure IoT 中心的多个设备

如果你已有 IoT 中心和已连接的设备，可以根据资源组、IoT 中心名称和用于选择设备的[目标条件](/azure/iot-edge/module-deployment-monitoring#target-condition)将 Azure SQL Edge 部署到设备。
使用部署向导期间提供的 IP 地址，可在笔记本底部生成快速连接链接。

在该过程中，可以选择将 dacpac 文件压缩到文件夹中，并将其部署到新的 Azure SQL Edge 实例。  如果提供了 dacpac 文件，则会在同一资源组中创建 Azure Blob 存储帐户。

## <a name="next-steps"></a>后续步骤

- [详细了解 Azure SQL Edge](/azure/azure-sql-edge/)