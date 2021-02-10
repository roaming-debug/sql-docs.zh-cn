---
title: 将 Oracle HR 架构迁移到 Linux 上的 SQL Server |Microsoft Docs
description: 将 Oracle 架构示例转换为 Linux 上的 SQL Server
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.custom: ''
ms.technology: ssma
ms.openlocfilehash: fb21fdc7da82d6dae2390ddcf0884ea1e8ac8a58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044387"
---
# <a name="migrate-an-oracle-schema-to-sql-server-2017-on-linux-with-the-sql-server-migration-assistant"></a>使用 SQL Server 迁移助手将 Oracle 架构迁移到 Linux 上的 SQL Server 2017

本教程使用适用于 Windows 上的 Oracle SQL Server 迁移助手 (SSMA) 将 Oracle 示例 **HR** 架构转换为 [Linux 上的 SQL Server 2017](../../linux/sql-server-linux-overview.md)。

> [!div class="checklist"]
> * 在 Windows 上下载并安装 SSMA
> * 创建 SSMA 项目以管理迁移
> * 连接到 Oracle
> * 运行迁移报告
> * 转换示例 HR 架构
> * 迁移数据

## <a name="prerequisites"></a>必备条件

- 安装了 **HR** 架构的 Oracle 12c (12.2.0.1.0) 的实例
- Linux 上的 SQL Server 的工作实例

> [!NOTE]
> 相同的步骤可用于在 Windows 上执行 SQL Server，但你必须在 " **迁移到** 项目" 设置中选择 "Windows"。

## <a name="download-and-install-ssma-for-oracle"></a>下载并安装 SSMA for Oracle

有几个版本的 SQL Server 迁移助手可用，具体取决于你的源数据库。  下载最新版本的 [Oracle SQL Server 迁移助手](https://aka.ms/ssmafororacle) ，并使用下载页面上的说明进行安装。

> [!NOTE]
> 目前，Linux 不支持 **SSMA For Oracle 扩展包** ，但本教程不需要这样做。

## <a name="create-and-set-up-project"></a>创建和设置项目

使用以下步骤创建新的 SSMA 项目：

1. 打开 SSMA for Oracle，并从 "**文件**" 菜单中选择 "**新建项目**"。

1. 为项目指定名称。

1. 在 " **迁移到** " 字段中选择 "SQL Server 2017 (Linux) -预览"。

默认情况下，SSMA for Oracle 不使用 Oracle 示例架构。 若要启用 HR 架构，请执行以下步骤：

1. 在 SSMA 中，选择 " **工具** " 菜单。

1. 选择 " **默认项目设置**"，然后选择 " **加载系统对象**"。

1. 请确保已选中 " **HR** "，然后选择 **"确定"**。

## <a name="connect-to-oracle"></a>连接到 Oracle

接下来，将 SSMA 连接到 Oracle。

1. 在工具栏上，单击 " **连接到 Oracle**"。

1. 输入服务器名称、端口、Oracle SID、用户名和密码。

   ![连接到 Oracle](./media/sql-server-linux-convert-from-oracle/ConnectToOracle.png)

1. 然后单击“连接”。 几分钟后，SSMA for Oracle 将连接到您的数据库并读取其元数据。

## <a name="create-a-report"></a>创建报表

使用以下步骤生成迁移报告。

1. 在 **Oracle 元数据资源管理器** 中，展开服务器的节点。

1. 展开 " **架构**"，右键单击 " **HR**"，然后选择 " **创建报表**"。

   ![Oracle 元数据资源管理器创建报表](./media/sql-server-linux-convert-from-oracle/CreateReport.png)

1. 此时将打开一个新的浏览器窗口，其中包含一个报表，其中列出了与该转换相关联的所有警告和错误。

   > [!NOTE]
   > 对于本教程，无需执行任何操作。 如果你为自己的 Oracle 数据库执行这些步骤，则应查看报表以解决数据库的任何重要转换问题。

   ![示例迁移报表](./media/sql-server-linux-convert-from-oracle/SSMAReport.png)

## <a name="connect-to-sql-server"></a>连接到 SQL Server

接下来，选择 " **连接到 SQL Server** "，然后输入相应的连接信息。  如果你使用的数据库名称尚不存在，则 SSMA for Oracle 将为你创建该数据库名称。

![连接到 SQL Server](./media/sql-server-linux-convert-from-oracle/ConnectToSQLServer.png)

## <a name="convert-schema"></a>转换架构

在 **Oracle 元数据资源管理器** 中右键单击 **HR** ，然后选择 "转换架构"。

![转换架构](./media/sql-server-linux-convert-from-oracle/ConvertSchema.png)

## <a name="synchronize-database"></a>同步数据库

接下来，同步您的数据库。

1. 转换完成后，请使用 **SQL Server 元数据资源管理器** 来前往你在上一步中创建的数据库。

1. 右键单击数据库，选择 " **与数据库同步**"，然后单击 "确定"。

   ![与数据库同步](./media/sql-server-linux-convert-from-oracle/SynchronizeWithDatabase.png)

## <a name="migrate-data"></a>迁移数据

最后一步是迁移数据。

1. 在 **Oracle 元数据资源管理器** 中，右键单击 **HR**，然后选择 " **迁移数据**"。

1. 数据迁移步骤要求您重新输入 Oracle 和 SQL Server 凭据。

1. 完成后，查看 "数据迁移" 报表，其外观应类似于以下屏幕截图：

   ![数据迁移报表](./media/sql-server-linux-convert-from-oracle/DataMigrationReport.png)

## <a name="next-steps"></a>后续步骤

对于更复杂的 Orcale 架构，转换过程涉及到更多的时间、测试以及对客户端应用程序的可能更改。 本教程的目的是介绍如何在整个迁移过程中使用 SSMA for Oracle。

在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
> * 在 Windows 上安装 SSMA
> * 创建新的 SSMA 项目
> * 评估并运行 Oracle 迁移

接下来，了解其他使用 SSMA 的方法：

> [!div class="nextstepaction"]
>[SQL Server 迁移助手文档](../sql-server-migration-assistant.md)
