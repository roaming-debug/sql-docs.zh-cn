---
title: 在单台计算机上开始使用 SSIS Scale Out | Microsoft Docs
description: 本文介绍在单台计算机上开始使用 SSIS Scale Out 所需了解的一切内容
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.reviewer: maghan
ms.openlocfilehash: 1cf9077a9b500c9e5052b48aab13d325809bb297
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347378"
---
# <a name="get-started-with-integration-services-ssis-scale-out-on-a-single-computer"></a>在单台计算机上开始使用 Integration Services (SSIS) Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


本部分指导如何在单一计算机环境中使用默认设置来设置 Integration Services Scale Out。

## <a name="1-install-sql-server-features"></a>1.安装 SQL Server 功能
在 SQL Server 安装向导的“功能选择”页中，选择以下各项： 
-   数据库引擎服务
-   Integration Services
    -   Scale Out 主要角色
    -   Scale Out 辅助角色

![功能选择列表前半部分](media/feature-select-onebox1.PNG)

![功能选择列表后半部分](media/feature-select-onebox2.PNG)

在“服务器配置”页上，单击“下一步”，以接受默认服务帐户和启动类型   。

在“数据库引擎配置”页上，选择“混合模式”并单击“添加当前用户”    。 

![引擎配置](media/engine-config.PNG)

在“Integration Services Scale Out 配置 - 主节点”和“Integration Services Scale Out 配置 - 辅助节点”页上，单击“下一步”接受端口和证书的默认设置    。

完成 SQL Server 安装向导。

## <a name="2-install-sql-server-management-studio"></a>2.安装 SQL Server Management Studio

下载和安装 [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)。

## <a name="3-enable-scale-out"></a>3.启用 Scale Out
打开 SSMS 并连接到本地 SQL Server 实例。
在对象资源管理器中，右键单击“Integration Services 目录”，并选择“创建目录”   。

在“创建目录”对话框中，“启用此服务器作为 SSIS Scale Out Master”选项默认处于选中状态   。

## <a name="4-enable-a-scale-out-worker"></a>4.启用 Scale Out Worker
在 SSMS 中，右键单击“SSISDB”并选择“管理 Scale Out”   。 

![管理 Scale Out](media/manage-scale-out.PNG)

随即打开 Integration Services Scale Out Manager 应用。 有关详细信息，请参阅 [Scale Out Manager](integration-services-ssis-scale-out-manager.md)。

要启用 Scale Out Worker，请切换到“Worker 管理器”，选择要启用的 Worker  。 默认情况下禁用 Worker。 单击“启用 Worker”启用所选 Worker  。

## <a name="5-run-packages-in-scale-out"></a>5.在 Scale Out 中运行包
现在可以在 Scale Out 中运行 SSIS 包。有关详细信息，请参阅[在 Integration Services (SSIS) Scale Out 中运行包](run-packages-in-integration-services-ssis-scale-out.md)。

## <a name="next-steps"></a>后续步骤
-   [使用 Scale Out Manager 添加 Scale Out Worker](add-scale-out-worker.md)。
