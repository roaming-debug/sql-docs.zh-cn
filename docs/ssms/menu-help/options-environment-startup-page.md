---
title: " SQL Server 选项页 -“环境”-“启动”"
description: 选项（“环境”-“启动”页）
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.date: 11/05/2018
ms.openlocfilehash: 6db0e4b3cf24087b9441b9e3c6b83b59cbefe48f
ms.sourcegitcommit: af64e2b8d498af26b973e86db5c00f8d72991295
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2021
ms.locfileid: "98193044"
---
# <a name="options-environment---startup-page"></a>选项（“环境”-“启动”页）

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

使用“选项”  对话框可以配置 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 启动操作、常规窗口管理选项和其他常规设置。 在“工具”  菜单上，单击“选项”  ，展开“环境”  文件夹，再然后单击“启动”  。

## <a name="ui-element-list"></a>UI 元素列表

**启动时**

选择 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在启动时执行的操作。 选项包括：

- “打开对象资源管理器”  ，如果选择此选项，将在启动时提示输入连接信息，然后打开对象资源管理器。

- “打开新查询窗口”  ，如果选择此选项，将在启动时提示输入连接信息，然后打开 SQL 查询编辑器。

- “打开对象资源管理器和查询窗口”  ，如果选择此选项，将在启动时提示输入连接信息，然后利用该连接打开对象资源管理器和 SQL 查询编辑器。

- “打开对象资源管理器和活动监视器”

- “打开空环境”  ，如果选择此选项，将在启动时打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]，但不打开 SQL 查询编辑器窗口，并且不会将对象资源管理器连接到服务器。

**在对象资源管理器中隐藏系统对象**

如果选中此复选框，将从对象资源管理器内的树视图中删除系统数据库、系统表、系统视图和系统存储过程。 系统函数和系统数据类型将不隐藏。 此选项仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例，并且不会影响对象资源管理器中已连接的服务器。

## <a name="see-also"></a>另请参阅

- [选项对话框的 F1 帮助](options-dialog-boxes-f1-help.md)
- [选项（“环境”-“常规”页）](options-environment-general-page.md)
