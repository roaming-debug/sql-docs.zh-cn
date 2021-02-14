---
title: 安装和配置 WideWorldImporters 示例数据库
description: 按照以下说明下载、安装和配置具有 SQL Server Management Studio 的 WideWorldImporters 示例数据库。
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 588597398c11b9b374bdecd272162fe82053cd8f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354080"
---
# <a name="installation-and-configuration"></a>安装和配置
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Wide World 导入 OLTP 数据库安装和配置说明。

## <a name="prerequisites"></a>先决条件

- [SQL Server 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) (或更高版本) 或 [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)。 有关该示例的完整版本，请使用 SQL Server Evaluation/开发人员/企业版。
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。 为获得最佳结果，请使用2016年6月版或更高版本。

## <a name="download"></a>下载

该示例的最新版本：

[宽世界-导入程序-版本](https://go.microsoft.com/fwlink/?LinkID=800630)

下载与 SQL Server 或 Azure SQL 数据库的版本对应的示例 WideWorldImporters 数据库备份/bacpac。

可从以下位置获取用于重新创建示例数据库的源代码。 请注意，重新创建示例将导致数据略有不同，因为数据生成中存在随机因素：

[宽世界-导入程序](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

## <a name="install"></a>安装


### <a name="sql-server"></a>SQL Server

若要将备份还原到 SQL Server 实例，可以使用 Management Studio。

1. 打开 SQL Server Management Studio，然后连接到目标 SQL Server 实例。
2. 右键单击 " **数据库** " 节点，然后选择 " **还原数据库**"。
3. 选择 " **设备** "，并单击按钮 **...**
4. 在对话框中 **选择 "备份设备**"，单击 " **添加**"，导航到服务器文件系统中的数据库备份，并选择备份。 单击“确定”。 
5. 如果需要，在 " **文件** " 窗格中更改数据文件和日志文件的目标位置。 请注意，最佳做法是将数据和日志文件放在不同的驱动器上。
6. 单击“确定”。  这将启动数据库还原。 完成后，会在 SQL Server 实例上安装数据库 WideWorldImporters。

### <a name="azure-sql-database"></a>Azure SQL 数据库

若要将 bacpac 导入新的 SQL 数据库，可以使用 Management Studio。

1.  (可选) 如果你还没有 Azure 中的 SQL Server，请导航到 [Azure 门户](https://portal.azure.com/) 并创建新的 SQL 数据库。 在创建数据库的过程中，您将创建一个服务器。 记下服务器。
   - 请参阅 [此教程](/azure/azure-sql/database/single-database-create-quickstart) ，在几分钟内创建数据库
2. 打开 SQL Server Management Studio 并连接到 Azure 中的服务器。
3. 右键单击 " **数据库** " 节点，并选择 " **导入" Data-Tier 应用程序**"。
4. 在 " **导入设置** " 中，选择 " **从本地磁盘导入** "，然后从文件系统中选择示例数据库的 bacpac。
5. 在 " **数据库设置** " 下，将 "数据库名称" 更改为 *WideWorldImporters* ，并选择要使用的目标版本和服务目标。
6. 单击 " **下一步** "，然后单击 " **完成** " 开始部署。 在 P1 上完成几分钟的时间。 如果需要较低的定价层，建议导入到新的 P1 数据库，然后将定价层更改为所需的级别。

## <a name="configuration"></a>Configuration

### <a name="full-text-indexing"></a>全文索引

示例数据库可以使用 Full-Text 的索引。 但是，默认情况下不会随 SQL Server 安装该功能-在 SQL Server 安装过程中，你需要选择它， (默认情况下，它在 Azure SQL 数据库) 中处于启用状态。 因此，安装后步骤是必需的。

1. 在 SQL Server Management Studio 中，连接到 WideWorldImporters 数据库并打开一个新的查询窗口。
2. 运行以下 T-sql 命令，以允许在数据库中使用 Full-Text 索引：  `EXECUTE Application.Configuration_ApplyFullTextIndexing`


### <a name="sql-server-audit"></a>SQL Server 审核

适用范围：SQL Server

在 SQL Server 中启用审核需要服务器配置。 若要为 WideWorldImporters 示例启用 SQL Server 审核，请在数据库中运行以下语句：

```sql
EXECUTE [Application].[Configuration_ApplyAuditing]
```

在 Azure SQL 数据库中，通过 [Azure 门户](https://portal.azure.com/)配置审核。

### <a name="row-level-security"></a>行级安全性

适用于： Azure SQL 数据库

默认情况下，WideWorldImporters 中未启用 Row-Level 安全性。 若要在数据库中启用 Row-Level 安全，请运行以下存储过程：

```sql
EXECUTE [Application].[Configuration_ApplyRowLevelSecurity]
```