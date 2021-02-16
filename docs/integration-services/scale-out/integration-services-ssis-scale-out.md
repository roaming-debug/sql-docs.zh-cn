---
title: SQL Server Integration Services (SSIS) Scale Out | Microsoft Docs
description: 本文概述了 SQL Server Integration Services (SSIS) Scale Out 功能，它提供对 SSIS 包的高性能执行
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: dcfbd1c5-c001-4fb7-b9ae-916e49ab6a96
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9e75be10cff0f34d132ecfe57ddff10538091a77
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338360"
---
# <a name="integration-services-ssis-scale-out"></a>Integration Services (SSIS) Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


SQL Server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (SSIS) Scale Out 通过在多台计算机上分发包执行，提供高性能 SSIS 包执行。 设置 Scale Out 后，可从 SQL Server Management Studio (SSMS) 以 Scale Out 模式并行运行多个包执行。

## <a name="components"></a>组件
[!INCLUDE[ssIS_md](../../includes/ssis-md.md)] Scale Out 包括一个 [!INCLUDE[ssIS_md](../../includes/ssis-md.md)] Scale Out Master 和一个或多个 [!INCLUDE[ssIS_md](../../includes/ssis-md.md)] Scale Out Worker。

-   Scale Out Master 负责 Scale Out 管理和接收来自用户的包执行请求。 有关详细信息，请参阅 [Scale Out Master](integration-services-ssis-scale-out-master.md)。

-   Scale Out Worker 从 Scale Out Master 拉取执行任务并运行包。 有关详细信息，请参阅 [Scale Out Worker](integration-services-ssis-scale-out-worker.md)。

## <a name="configuration-options"></a>配置选项
可以按下列配置安装 Scale Out：

-   在一台计算机上，其中 Scale Out Master 与 Scale Out Worker 在同一计算机上并排运行  。

-   在多台计算机上，其中每个 Scale Out Worker 位于不同的计算机上  。

## <a name="what-you-can-do"></a>可执行的操作
设置 Scale Out 后，可执行以下操作：

-   并行运行部署到 SSISDB 目录的多个包。 有关详细信息，请参阅 [在 Scale Out 中运行包](run-packages-in-integration-services-ssis-scale-out.md)。

-   在 Scale Out Manager 应用中管理 Scale Out 拓扑。 有关详细信息，请参阅 [Integration Services Scale Out Manager](integration-services-ssis-scale-out-manager.md)。

## <a name="next-steps"></a>后续步骤
-   [在单台计算机上开始使用 Integration Services (SSIS) Scale Out](get-started-with-ssis-scale-out-onebox.md)

-   [演练：设置 Integration Services Scale Out](walkthrough-set-up-integration-services-scale-out.md)
