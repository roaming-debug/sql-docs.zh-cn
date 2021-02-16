---
title: 使用 Scale Out Manager 添加 SSIS Scale Out Worker | Microsoft Docs
description: 本文介绍如何使用 Scale Out Manager 将 SSIS Scale Out Worker 添加到现有的 Scale Out 环境。
ms.custom: performance
ms.date: 12/19/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 961ea14fc7663206a4d1ad86c6ca611be0f863d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354890"
---
# <a name="add-a-scale-out-worker-with-scale-out-manager"></a>使用 Scale Out Manager 添加 Scale Out Worker

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Integration Services Scale Out Manager 可简化向现有 Scale Out 环境添加 Scale Out Worker 的过程。 

按照以下步骤，将 Scale Out Worker 添加到 Scale Out 拓扑：

## <a name="1-install-scale-out-worker"></a>1.安装 Scale Out Worker
在 SQL Server 安装向导中，在“功能选择”页上选择 Integration Services 和 Scale Out Worker  。 
![功能选择 Worker](media/feature-select-worker.PNG)

在“Integration Services Scale Out 配置 - 辅助节点”页上，单击“下一步”即可跳过此处的配置，并使用“Scale Out Manager”完成安装后的配置    。

完成安装向导。

## <a name="2-open-the-firewall-on-the-scale-out-master-computer"></a>2.打开 Scale Out Master 计算机上的防火墙
在 Scale Out Master 计算机上的 Windows 防火墙中，打开在 Scale Out Master 安装期间指定的端口（默认为 8391）和 SQL Server 端口（默认为 1433）。

## <a name="3-add-a-scale-out-worker-with-scale-out-manager"></a>3.使用 Scale Out Manager 添加 Scale Out Worker
以管理员身份运行 SQL Server Management Studio 并连接到 Scale Out Master 的 SQL Server 实例。

在对象资源管理器中，右键单击“SSISDB”，并选择“管理 Scale Out”   。 

![管理 Scale Out](media/manage-scale-out.PNG)

在“Scale Out Manager”对话框中，切换到“Worker 管理器”   。 选择“+”，并按照“连接 Worker”对话框中的说明进行操作   。 

## <a name="next-steps"></a>后续步骤
有关详细信息，请参阅 [Scale Out Manager](integration-services-ssis-scale-out-manager.md)。
