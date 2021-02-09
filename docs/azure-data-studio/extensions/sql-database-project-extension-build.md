---
title: 生成和发布项目
description: 使用 SQL Server 数据库项目扩展进行生成和发布
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 06/25/2020
ms.openlocfilehash: 06021fc598165982156093c12c26434f06cbc422
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495705"
---
# <a name="build-and-publish-a-project"></a>生成和发布项目

通过 Azure Data Studio 的 SQL 数据库项目扩展（预览版）中的生成过程，可在 Windows、macOS 和 Linux 环境中创建 dacpac。 可以通过发布过程将项目部署到本地或云环境。

## <a name="prerequisites"></a>先决条件

- 安装并配置 [Azure Data Studio 的 SQL 数据库项目扩展](sql-database-project-extension.md)。

## <a name="build-a-database-project"></a>生成数据库项目

 在“项目”viewlet 中“资源管理器”下，右键单击 .sqlproj 根节点的，然后选择“生成”。 

 将自动显示包含生成过程输出的输出窗格。  成功生成后，将显示以下消息： 

 ``` ... exited with code: 0 ```

## <a name="publish-a-database-project"></a>发布数据库项目

通过生成过程成功编译项目后，可将数据库发布到 SQL Server 实例。 若要发布数据库项目，请在“项目”viewlet 中“资源管理器”下，右键单击 .sqlproj 根节点的，然后选择“发布”。 

在显示的“发布数据库”对话框中，指定服务器连接和要创建的数据库名称。

## <a name="next-steps"></a>后续步骤

- [Azure Data Studio 的 SQL 数据库项目扩展](sql-database-project-extension.md)
- [从命令行生成 SQL 数据库项目](sql-database-project-extension-build-from-command-line.md)
