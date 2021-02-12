---
title: 面向项目的脱机数据库开发
description: 查看面向项目的脱机数据库开发任务的可用资源，例如将对象导入到数据库中，以及使用序列对象。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbprojectwizard.general
- sql.data.tools.dbprojectwizard.summary
ms.assetid: e61e830d-9fcd-45e7-b7b4-93a42155dd56
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: e36876d1b30b12dbae1dd4c7d3c72230070c9ef8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077049"
---
# <a name="project-oriented-offline-database-development"></a>面向项目的脱机数据库开发

本节介绍 SQL Server Data Tools (SSDT) 提供的用于创作、生成、调试、发布和部署数据库项目的功能。  
  
使用 SSDT，你可以创建一个脱机数据库项目，并且可以在该项目中通过添加、修改或删除对象的定义（由脚本表示）实施架构更改，而无需连接到某一服务器实例。 这些都可以通过使用表设计器或 Transact\-SQL 编辑器来实现。 你还可以在同一个项目中编写和调试 Transact\-SQL 和 CLR 对象。 您可以使用架构比较来确保您的项目与生产数据库保持同步，并且出于比较目的在开发周期的每个阶段中为您的项目创建快照。 当您在基于团队的环境中处理您的数据库项目时，可以将版本控制用于所有文件。 在开发、测试和调试了您的数据库项目后，可以将您的项目提交给授权人士，以便发布到生产环境。  
  
> [!NOTE]  
> 本节中的操作指南主题包含一系列可按顺序完成的任务。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|描述|  
|---------|---------------|  
|[导入到数据库项目](../ssdt/import-into-a-database-project.md)|介绍从活动的数据库、.dacpac 或脚本导入对象。|  
|[“添加数据库引用”对话框](../ssdt/add-database-reference-dialog-box.md)|介绍添加数据库引用的各种方式。|  
|[“检查更新”对话框](../ssdt/check-for-updates-dialog-box.md)|说明 SQL Server Data Tools 如何检查产品更新。|  
|[数据库项目设置](../ssdt/database-project-settings.md)|介绍各种项目设置来控制数据库和生成配置的各个方面。|  
|[如何：在 SQL Server 数据库项目中浏览对象](../ssdt/how-to-browse-objects-in-a-sql-server-database-project.md)|Visual Studio 中的 SQL Server 对象资源管理器现在包含一个已不推荐使用的“项目”节点，在该节点下，你的解决方案中的所有 SQL Server 数据库项目都在类似 SQL Server Management Studio 的层次结构下进行分组。|  
|[“数据工具操作”窗口](../ssdt/data-tools-operations-window.md)|介绍 **“数据工具操作”** 窗口，该窗口会显示某些操作的进度并告知您任何错误。|  
|[Transact-SQL 编辑器选项](../ssdt/transact-sql-editor-options.md)|说明 Transact\-SQL 选项。|  
|[如何：创建新的数据库项目](../ssdt/how-to-create-a-new-database-project.md)|创建一个数据库项目并导入现有数据库架构。|  
|[如何：使用架构比较来比较不同数据库定义](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)|比较数据库和项目的架构并保持同步。|  
|[如何：生成并部署到本地数据库](../ssdt/how-to-build-and-deploy-to-a-local-database.md)|使用本地的按需运行的 SQL Server 实例，该实例是在你调试数据库项目时激活的。|  
|[如何：更改目标平台并发布数据库项目](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)|将项目的目标 SQL Server 平台更改为 SQL Server 的任何支持的实例并验证语法。|  
|[如何：创建项目快照](../ssdt/how-to-create-a-snapshot-of-a-project.md)|创建数据库架构的只读代理，当源项目出现了无用的修改，可将源项目还原。|  
|[如何：在项目中使用 Microsoft SQL Server 2012 对象](../ssdt/how-to-use-microsoft-sql-server-2012-objects-in-your-project.md)|向您的项目添加一个新的序列对象。|  
|[如何：处理 CLR 数据库对象](../ssdt/how-to-work-with-clr-database-objects.md)|在 SQL Server Data Tools 数据库项目中创建和发布 CLR 对象。|  
|[如何：将 Visual Studio 2010 数据库项目转换为 SQL Server 数据库项目并重新定位到其他平台](../ssdt/how-to-convert-visual-studio-2010-database-projects-to-ssql-server-projects.md)|将在 Visual Studio 2010 中创建的现有 SQL Server 数据库、CLR 对象和数据层应用程序项目转换为 SQL Server Data Tools 数据库项目。|  
|[如何：指定预先部署或后期部署脚本](../ssdt/how-to-specify-predeployment-or-postdeployment-scripts.md)|论述如何使用在部署您的数据库之前或之后您要运行的脚本。|  
  
## <a name="related-sections"></a>相关章节

[管理表、关系和修复错误](../ssdt/manage-tables-relationships-and-fix-errors.md)
