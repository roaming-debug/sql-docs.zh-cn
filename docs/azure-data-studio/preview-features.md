---
title: Azure Data Studio 中的预览功能
description: 了解 Azure Data Studio 预览功能以及如何启用和使用这些功能的详细信息。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: hannahqin
ms.author: hanqin
ms.reviewer: maghan
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: bfb4ea0b745fa61ea6d7c688ab90eaf369b2a5bb
ms.sourcegitcommit: 9c6130d498f1cfe11cde9f2e65c306af2fa8378d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035994"
---
# <a name="preview-features-in-azure-data-studio"></a>Azure Data Studio 中的预览功能

在 Azure Data Studio 中，新增功能和改进功能通常先作为预览功能发布，然后再正式发布 (GA)。 功能保留在预览中的时间取决于用户反馈、质量检查和长期路线图。 通过启用预览功能，你可以完全访问 Azure Data Studio，并有机会提供早期反馈。

## <a name="how-do-i-enable-preview-features"></a>如何启用预览功能？

### <a name="on-first-launch"></a>首次启动时

如果你是新用户，则可以在首次启动 Azure Data Studio 时选择加入预览功能。 启动时，屏幕右下角将显示 toast 通知，你可以选择启用或禁用预览功能。 选择“是(推荐)”，以启用预览功能。

![在首次启动时预览 toast 通知](./media/getting-started/preview-toast-notification.png)

### <a name="in-settings"></a>在“设置”中

你可以在“设置”中随时启用或禁用预览功能。

1. 选择左下角的“齿轮”图标，然后从上下文菜单中选择“设置” 。 “设置”选项卡将打开。

   ![ADS 中用于访问“设置”的齿轮图标](./media/settings/open-settings-menu.png)

2. 在搜索栏中键入“启用预览功能”。

3. 若要启用预览功能，请选中“Workbench：启用预览功能”下的“启用未发布预览功能” 复选框。 若要禁用预览功能，请清除该复选框。

   ![ADS 中的“启用预览功能”设置](./media/settings/preview-features-settings.png)

## <a name="list-of-preview-features-in-azure-data-studio"></a>Azure Data Studio 中预览功能的列表

### <a name="general-features-in-preview"></a>预览中的常规功能

* Azure 门户集成
* 备份/还原
* 部署
    * SQL Edge
    * SQL Server 大数据群集
    * SQL Server 容器映像
    * Windows 上的 SQL Server
* 功能导览
*  SQLCMD 模式
* 新的欢迎页

### <a name="notebook-features-in-preview"></a>笔记本功能（预览）

* Dotnet 交互式支持
* Markdown 工具栏
*  新建笔记本工具栏
* 笔记本内核
    * Kusto
    * PowerShell
    * PySpark
    * Python
    * Spark | Scala
    * Spark | R
    * SQL
* 通过浏览器打开笔记本
* 固定笔记本
* Python 依赖项向导

### <a name="first-party-extensions-in-preview"></a>第一方扩展（预览）

* SQL Server 的管理员包
* Azure SQL 数据仓库见解
* 中央管理服务器
* 适用于 Windows 的数据库管理工具扩展
* Kusto
* 语言包
* PostgreSQL
* PowerShell
* 查询历史记录
* 适用于 Azure Data Studio 的 SandDance
* 服务器报表
* SQL 评估
* SQL Server 代理
* SQL Server Profiler
* 机器学习
* 托管实例仪表板
* Visual Studio IntelliCode
* whoisactive

## <a name="next-steps"></a>后续步骤

* [Azure Data Studio](what-is-azure-data-studio.md)
