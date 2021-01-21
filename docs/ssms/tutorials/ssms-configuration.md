---
title: SSMS 组件和配置
description: 介绍 SQL Server Management Studio 环境的组件和基本配置选项的教程。
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
keywords: SQL Server, SSMS, SQL Server Management Studio
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.date: 03/16/2018
ms.openlocfilehash: 1688998d5a3f6b9033808701c33ff141cf8cc8c3
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597191"
---
# <a name="sql-server-management-studio-components-and-configuration"></a>SQL Server Management Studio 组件和配置

本教程介绍 SQL Server Management Studio (SSMS) 中的多种窗口组件和一些适用于工作区的基本配置选项。 在本文中，学习如何： 

> [!div class="checklist"]
> * 识别构成 SSMS 环境的组件
> * 更改环境布局，并将其重置为默认值
> * 最大化查询编辑器
> * 更改字体
> * 配置启动选项
> * 将配置重置为默认值

## <a name="prerequisites"></a>必备条件

若要完成本教程，需要 SQL Server Management Studio。  

* 安装 [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md)。

## <a name="sql-server-management-studio-components"></a>SQL Server Management Studio 组件

本节介绍工作区中可用的各种窗口组件及其使用方法。

* 要关闭窗口，请选择标题栏右侧的“X”  。
* 要重新打开窗口，请在“视图”菜单中选择窗口  。

    ![“视图”菜单](media/ssms-configuration/viewmenu.png)

* **对象资源管理器** (F8)：对象资源管理器是服务器中所有数据库对象的树状视图。 此视图包括 SQL Server 数据库引擎、SQL Server Analysis Services、SQL Server Reporting Services 和 SQL Server Integration Services 的数据库。 对象资源管理器包括连接到它的所有服务器的信息。 

    ![“对象资源管理器”](media/ssms-configuration/objectexplorer.png)
* **查询窗口** (Ctrl+N)：选择“新建查询”后，在此窗口中输入 Transact-SQL (T-SQL) 查询  。 查询结果也显示在此处。

    ![“新建查询”窗口](media/ssms-configuration/newquery.png)

* **属性** (F4)：当查询窗口打开时，可看到“属性”视图。 该视图显示查询的基本属性。 例如，它显示查询开始的时间，返回的行数及连接详细信息。  

    ![配置属性](media/ssms-configuration/properties.png)

* **模板浏览器** (Ctrl+Alt+T)：模板浏览器中有各种预构建的 T-SQL 模板。 可使用这些模板执行各种功能，如创建或备份数据库。 

    ![模板浏览器](media/ssms-configuration/templates.png)

* **对象资源管理器详细信息** (F7)：此视图比对象资源管理器中的视图更精细。 可使用对象资源管理器详细信息同时操纵多个对象。 例如，在此窗口中，可选择多个数据库，然后删除它们或同时编写其脚本。 

    ![对象资源管理器详细信息](media/ssms-configuration/objectexplorerdetails.PNG) 

## <a name="change-the-environment-layout"></a>更改环境布局 

本节介绍如何更改环境布局，例如如何移动各种窗口。 

* 要移动窗口，请按住标题，然后拖动窗口。 
* 要固定或取消固定窗口，请在标题栏中选择图钉图标：

    ![固定对象](media/ssms-configuration/pushpin.png)

* 每个窗口组件都有一个下拉菜单，可以各种方式使用该菜单来操纵窗口： 

    ![窗口选项](media/ssms-configuration/windowoptions.png)

* 打开两个或更多查询窗口时，可以垂直或水平标记这些窗口，以便看到这两个查询窗口。 要查看已标记的窗口，请右键单击查询的标题，然后选择所需的已标记选项：

    ![“查询”选项卡选项](media/ssms-configuration/querytabbedoptions.png)

    * 以下是水平选项卡组：

      ![水平选项卡组示例](media/ssms-configuration/horizontaltab.png)

    * 这是垂直选项卡组：

      ![垂直选项卡组示例](media/ssms-configuration/verticaltabgroup.png)

    * 要合并选项卡，请右键单击查询标题并选择“移到上一选项卡组”或“移到下一选项卡组”   ：

      ![合并查询选项卡](media/ssms-configuration/mergetabgroups.png)

* 要还原默认环境布局，请在“窗口”菜单中选择“重置窗口布局”   ：

    ![还原窗口布局](media/ssms-configuration/resetwindowlayout.png)

## <a name="maximize-query-editor"></a>最大化查询编辑器

可将查询编辑器最大化为全屏模式：

1. 单击“查询编辑器”窗口中的任意位置。

2. 按 Shift+Alt+Enter，在全屏模式和常规模式之间进行切换。 

这种键盘快捷键适用于任何文档窗口。 

## <a name="change-basic-settings"></a>更改基本设置

本节介绍如何从“工具”菜单修改 SSMS 中的一些基本设置  。

  ![“工具”菜单](media/ssms-configuration/tools.png)

* 要修改突出显示的工具栏，请选择“工具” > “自定义”：

    ![自定义工具栏](media/ssms-configuration/toolbar.png)

### <a name="change-the-font"></a>更改字体

* 要更改字体，请选择“工具” > “选项” > “字体和颜色”：

     ![更改字体和颜色](media/ssms-configuration/fontsandcolors.png)

### <a name="change-startup-options"></a>更改启动选项

* 启动选项决定了首次打开 SSMS 时工作区的外观。 要更改启动选项，请选择“工具” > “选项” > “启动”：

    ![更改启动选项](media/ssms-configuration/startup.png)

### <a name="reset-settings-to-the-default"></a>将设置重置为默认值

* 可从菜单导出和导入所有这些设置。 要导入或导出设置或恢复默认设置，请选择“工具” > “导入和导出设置” 

    ![导入和导出设置](media/ssms-configuration/settings.png)

## <a name="next-steps"></a>后续步骤

熟悉 SSMS 的最好方式是进行实践演练。 这些教程  和操作说明  文章可帮助你使用 SSMS 的各种功能。  这些文章教你如何管理 SSMS 组件，以及如何查找常用功能。

* [连接到实例和查询实例](../quickstarts/ssms-connect-query-sql-server.md)
* [脚本](scripting-ssms.md)
* [在 SSMS 中使用模板](../template/templates-ssms.md)
* [使用 SSMS 的其他提示和技巧](ssms-tricks.md)