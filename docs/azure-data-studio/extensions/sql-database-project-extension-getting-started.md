---
title: SQL 数据库项目扩展入门
description: 开始使用 Azure Data Studio 的 SQL 数据库项目扩展
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 07/30/2020
ms.openlocfilehash: 397bdebe2bdea292009e0216c6b3004ac9475b18
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040311"
---
# <a name="getting-started-with-the-sql-database-projects-extension-preview"></a>SQL 数据库项目扩展（预览版）入门

本文介绍三种开始使用 SQL 数据库项目扩展的方法：

1.  在“资源管理器”下转到“项目”viewlet，或在命令面板中搜索“新建数据库项目”，以创建新的数据库项目。
2. 可以通过命令面板中的“打开数据库项目”来打开现有数据库项目。
3. 通过命令面板“导入新的数据库项目”来使用现有数据库。

    ![新 viewlet](media/sql-database-projects-extension/projects-viewlet.png)

## <a name="create-an-empty-database-project"></a>创建空数据库项目

  在“资源管理器”下的“项目”viewlet 中，选择“新建项目”按钮，然后在显示的文本输入中输入项目名称。  在出现的“选择文件夹”对话框中，选择项目文件夹的目录、.sqlproj 文件和要包含的其他内容。
空项目在“项目”viewlet 中打开并可见，可以进行编辑。

## <a name="open-an-existing-project"></a>打开现有项目

 在“项目”viewlet 中，选择“打开项目”按钮，然后从显示的文件选取器中打开现有 .sqlproj 文件。 现有项目可以来自 Azure Data Studio 或 [Visual Studio SQL Server Data Tools](../../ssdt/sql-server-data-tools.md)。

将打开现有项目，其内容在“项目”viewlet 中可见以供编辑。

## <a name="create-a-database-project-from-an-existing-database"></a>从现有数据库创建数据库项目

 在“项目”viewlet 中，选择“从数据库导入项目”按钮并连接到 SQL Server。  建立连接后，从可用数据库列表选择数据库，然后设置项目的名称。

最后，选择提取的目标结构。  将打开新项目，其中包含所选数据库内容的 SQL 脚本。

## <a name="build-and-publish"></a>生成并发布

通过将项目生成为[数据层应用程序文件](../../relational-databases/data-tier-applications/data-tier-applications.md) (DACPAC) 并发布到支持的平台，在 Azure Data Studio 的 SQL 数据库项目扩展中实现数据库项目部署。 有关此过程的详细信息，请参阅[生成并发布项目](sql-database-project-extension-build.md)。

## <a name="schema-compare"></a>架构比较

如果安装了[架构比较扩展](schema-compare-extension.md)，则 SQL 数据库项目扩展与之进行交互，将项目内容与 dacpac 或现有数据库进行比较。  生成的架构比较可用于查看和应用源与目标之间的差异。

## <a name="next-steps"></a>后续步骤

- [使用 Azure Data Studio 的 SQL 数据库项目扩展生成并发布项目](sql-database-project-extension-build.md)