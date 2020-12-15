---
title: 安装 & 配置 DW WideWorldImporters 示例数据库
description: 按照以下说明下载、安装和配置具有 SQL Server Management Studio 的 WideWorldImportersDW 示例数据库。
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 08/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 5096ecd27f6ba9d5d06dd4efd322a42e3218a0bd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466598"
---
# <a name="wideworldimportersdw-installation-and-configuration"></a>WideWorldImportersDW 安装和配置
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
WideWorldImportersDW 数据库的安装和配置说明。

- [SQL Server 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) (或更高版本) 或 [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)。 若要使用完整版本的示例，请使用 SQL Server Evaluation/开发人员/企业版。
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。 为获得最佳结果，请使用2016年6月版或更高版本。

## <a name="download"></a>下载

该示例的最新版本：

[宽世界-导入程序-版本](https://go.microsoft.com/fwlink/?LinkID=800630)

下载与 SQL Server 或 Azure SQL 数据库的版本对应的示例 WideWorldImportersDW 数据库备份/bacpac。

可从以下位置获取用于重新创建示例数据库的源代码。 请注意，数据填充基于 OLTP 数据库中的 ETL (WideWorldImporters) ：

[宽世界导入程序-源](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

## <a name="install"></a>安装


### <a name="sql-server"></a>SQL Server

若要将备份还原到 SQL Server 实例，可以使用 Management Studio。

1. 打开 SQL Server Management Studio，然后连接到目标 SQL Server 实例。
2. 右键单击 " **数据库** " 节点，然后选择 " **还原数据库**"。
3. 选择 " **设备** "，并单击按钮 **...**
4. 在对话框中 **选择 "备份设备**"，单击 " **添加**"，导航到服务器文件系统中的数据库备份，并选择备份。 单击 **“确定”** 。
5. 如果需要，在 " **文件** " 窗格中更改数据文件和日志文件的目标位置。 请注意，最佳做法是将数据和日志文件放在不同的驱动器上。
6. 单击 **“确定”** 。 这将启动数据库还原。 完成后，会在 SQL Server 实例上安装数据库 WideWorldImporters。

### <a name="azure-sql-database"></a>Azure SQL Database

若要将 bacpac 导入新的 SQL 数据库，可以使用 Management Studio。

1.  (可选) 如果你还没有 Azure 中的 SQL Server，请导航到 [Azure 门户](https://portal.azure.com/) 并创建新的 SQL 数据库。 在创建数据库的过程中，您将创建一个服务器。 记下服务器。
   - 请参阅 [此教程](/azure/azure-sql/database/single-database-create-quickstart) ，在几分钟内创建数据库
2. 打开 SQL Server Management Studio 并连接到 Azure 中的服务器。
3. 右键单击 " **数据库** " 节点，并选择 " **导入" Data-Tier 应用程序**"。
4. 在 " **导入设置** " 中，选择 " **从本地磁盘导入** "，然后从文件系统中选择示例数据库的 bacpac。
5. 在 " **数据库设置** " 下，将 "数据库名称" 更改为 *WideWorldImportersDW* ，并选择要使用的目标版本和服务目标。
6. 单击 " **下一步** "，然后单击 " **完成** " 开始部署。 完成此部署可能需要几分钟时间。 如果指定的服务目标低于 S2，可能需要更长时间。

## <a name="configuration"></a>配置

[适用于 SQL Server 2016 (和更高版本) 开发人员/评估/企业版]

示例数据库可以利用 PolyBase 查询 Hadoop 或 Azure blob 存储中的文件。 但是，默认情况下不会随 SQL Server 安装该功能-您需要在 SQL Server 安装过程中将其选中。 因此，安装后步骤是必需的。

1. 在 SQL Server Management Studio 中，连接到 WideWorldImportersDW 数据库并打开一个新的查询窗口。
2. 运行以下 T-sql 命令以在数据库中启用 PolyBase：

   EXECUTE [应用程序]。[Configuration_ApplyPolyBase]