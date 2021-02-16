---
title: SQL Server Integration Services Scale Out Manager | Microsoft Docs
description: 本文介绍 Scale Out Manager 工具，它可用于管理 SSIS Scale Out
ms.custom: performance
ms.date: 12/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: a56792633259e2f8d224696f94c7095a0ec1ae96
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347423"
---
# <a name="integration-services-scale-out-manager"></a>Integration Services Scale Out Manager

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Scale Out Manager 是一种管理工具，可用于在单一应用中管理完整的 SSIS Scale Out 拓扑。 它消除了在多台计算机上执行管理任务或运行 Transact-SQL 命令的负担。

## <a name="open-scale-out-manager"></a>打开 Scale Out Manager

可通过两种方式打开 Scale Out Manager。

### <a name="1-open-scale-out-manager-from-sql-server-management-studio"></a>1.从 SQL Server Management Studio 打开 Scale Out Manager
打开 SQL Server Management Studio (SSMS) 并连接到 Scale Out Master 的 SQL Server 实例。

在对象资源管理器中，右键单击“SSISDB”，并选择“管理 Scale Out”   。

![管理 Scale Out](media/manage-scale-out.PNG)

> [!NOTE]
> 建议以管理员身份运行 SSMS，因为某些 Scale Out 管理操作（例如添加 Scale Out Worker）需要管理权限。

### <a name="2-open-scale-out-manager-by-running-managementtoolexe"></a>2.通过运行 ManagementTool.exe 打开 Scale Out Manager

在 `%SystemDrive%\Program Files (x86)\Microsoft SQL Server\150\DTS\Binn\Management` 下找到 `ManagementTool.exe`。 右键单击“ManagementTool.exe”并选择“以管理员身份运行”   。 

打开 Scale Out Manager 后，输入 Scale Out Master 的 SQL Server 实例名称并与其建立连接，以便管理 Scale Out 环境。

![门户连接](media/portal-connect-new.png)

## <a name="tasks-available-in-scale-out-manager"></a>Scale Out Manager 中支持的任务
在 Scale Out Manager 中，可执行以下操作：

### <a name="enable-scale-out"></a>启用 Scale Out
连接到 SQL Server 后，如果未启用 Scale Out，可以选择“启用”来启用它  。

![在门户中启用 Scale Out](media/portal-enable-scale-out-new.PNG) 

### <a name="view-scale-out-master-status"></a>查看 Scale Out Master 状态
Scale Out Master 的状态显示在“仪表板”页上  。

![门户仪表板](media/portal-dashboard-new.PNG)

### <a name="view-scale-out-worker-status"></a>查看 Scale Out Worker 状态
Scale Out Master 的状态显示在“Worker 管理器”页上  。 可选择每个辅助角色来查看单个状态。

![门户中的 Worker 管理器](media/portal-worker-manager-new.PNG)

### <a name="add-a-scale-out-worker"></a>添加 Scale Out Worker
要添加 Scale Out Worker，请选择 Scale Out Worker 列表底部的“+”  。 

输入要添加的 Scale Out Worker 的计算机名，然后单击“验证”  。 Scale Out Manager 会检查当前用户是否有权访问 Scale Out Master 和 Scale Out Worker 计算机上的证书存储

![连接 Worker](media/connect-worker-new.PNG)

如果验证成功，Scale Out Manager 会尝试读取辅助角色服务器配置文件并获取辅助角色的证书指纹。 有关详细信息，请参阅 [Scale Out Worker](integration-services-ssis-scale-out-worker.md)。 如果 Scale Out Manager 无法读取辅助角色服务配置文件，可通过两种替代方式提供辅助角色证书。 

- 可直接输入辅助角色证书的指纹。

    ![辅助角色证书 1](media/portal-cert1-new.PNG)

- 也可提供证书文件。

    ![辅助角色证书 2](media/portal-cert2-new.PNG)

收集信息后，Scale Out Manager 会描述要执行的操作。 通常，这些操作包括安装证书、更新辅助角色服务配置文件和重启辅助角色服务。

![门户中的添加确认 1](media/portal-add-confirm1-new.PNG)

如果无法访问辅助角色设置，必须手动更新并重启辅助角色服务。

![门户中的添加确认 2](media/portal-add-confirm2-new.PNG)

选择“确认”复选框，然后选择“确定”开始添加 Scale Out Worker   。

### <a name="delete-a-scale-out-worker"></a>删除 Scale Out Worker
要删除 Scale Out Worker，请选择 Scale Out Worker 并选择 Scale Out Worker 列表底部的“-”  。

### <a name="enable-or-disable-a-scale-out-worker"></a>启用或禁用 Scale Out Worker
要启用或禁用 Scale Out Worker，请选择 Scale Out Worker 并选择“启用 Worker”或“禁用 Worker”   。 如果辅助角色不在脱机状态，则 Scale Out Manager 中显示的辅助角色状态会发生相应更改。

## <a name="edit-a-scale-out-worker-description"></a>编辑 Scale Out Worker 描述
要编辑 Scale Out Worker 的描述，请选择 Scale Out Worker 并选择“编辑”  。 完成编辑描述后，请选择“保存”  。

![在门户中保存 Worker](media/portal-save-worker-new.PNG)

## <a name="next-steps"></a>后续步骤
有关详细信息，请参阅以下文章：
-   [Integration Services (SSIS) Scale Out Master](integration-services-ssis-scale-out-master.md)
-   [Integration Services (SSIS) Scale Out Worker](integration-services-ssis-scale-out-worker.md)
