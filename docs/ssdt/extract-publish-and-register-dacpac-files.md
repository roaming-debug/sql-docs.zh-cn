---
title: 提取、发布和注册 .dacpac 文件
description: 了解可对数据层应用程序 (DAC) 执行的操作。 示例包括提取、发布和注册快照 (.dacpac) 文件。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.DacTableChooser
- sql.data.tools.DacPublishDialog
- sql.data.tools.DacPropertiesDialog
- sql.data.tools.publishdacproject
- sql.data.tools.DacExtractDialog
ms.assetid: ed900f93-d3df-40f5-8e62-4d722595e041
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: f7d63d6b26d8f5f07b6a4344e84f3d490012c91f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077050"
---
# <a name="extract-publish-and-register-dacpac-files"></a>提取、发布和注册 .dacpac 文件

本主题介绍可以通过在 SQL Server 对象资源管理器中右键单击连接的数据库来执行的四个过程：  
  
-   发布数据层应用程序  
  
-   提取数据层应用程序  
  
-   注册数据层应用程序  
  
-   取消注册数据层应用程序  
  
## <a name="publish-data-tier-application"></a>发布数据层应用程序  
您可以将 .dacpac 发布到数据库。 该发布操作增量更新数据库架构以便匹配源 .dacpac 文件的架构。 如果该数据库在服务器上不存在，则发布操作将创建它。  
  
通过选择“数据库”节点也可以使用该操作。  
  
选择 .dacpac 文件。 在指定某个 .dacpac 文件后，“DAC 属性”按钮将启用。 “DAC 属性”对话框显示有关 .dacpac 文件的信息。  
  
指定指向数据库服务器的连接字符串，然后指定数据库名称（如果数据库名称不在连接字符串中）。  
  
“发布”和“生成脚本”按钮现在已启用。 如果您生成一个脚本，它将在文档窗口中打开并且可以根据需要进行保存。 如果你选择直接发布到数据库，则可以从“数据工具操作”窗口查看更新以及所使用的实际脚本的汇总。  
  
选中后，“作为数据层应用程序注册”复选框将导致最终生成的数据库在服务器元数据中作为数据层应用程序注册。 如果注册您发布到的数据库，则在数据库架构不同于其当前注册的 dacpac 的情况下，可能会导致发布失败。  
  
在“高级发布设置”对话框中提供其他发布配置，可以通过单击“高级”按钮访问该对话框。  
  
## <a name="extract-data-tier-application"></a>提取数据层应用程序  
您可以从数据库提取 .dacpac。 从一个活动 SQL Server 或 Azure SQL 数据库提取数据库快照文件 (.dacpac)，该文件除了包含数据库架构之外还可以包含用户表的数据。  
  
指定要创建的 .dacpac 文件。 “DAC 属性”按钮显示“DAC 属性”对话框，可用于指定 .dacpac 文件的属性。  
  
根据需要修改提取进程的配置。 在“提取设置”部分中，可以选择仅提取架构还是提取架构并包括表数据。 如果您选择提取架构和数据，则可以选择要提取其数据的表。  
  
## <a name="register-data-tier-application"></a>注册数据层应用程序  
您可以将数据库注册为实例内的数据层应用程序 (DAC)。 这将存储系统元数据中数据库架构的当前状态的表示形式。  
  
在“注册数据层应用程序”对话框中，指定已注册 DAC 的属性。  
  
## <a name="unregister-data-tier-application"></a>取消注册数据层应用程序  
通过取消注册，您可以从实例中删除已注册数据层应用程序的元数据。 取消注册并不删除已注册的数据库。  
  
## <a name="see-also"></a>另请参阅  
[连接的数据库开发](../ssdt/connected-database-development.md)  
  
