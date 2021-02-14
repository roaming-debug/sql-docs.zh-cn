---
title: WideWorldImportersDW-ETL 工作流 |Microsoft Docs
description: 将 ETL 包与 SQL Server Integration Services (SSIS) 一起定期将 WideWorldImporters 数据库中的数据迁移到 WideWorldImportersDW。
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ce9cd8ee5a4153cb1d744d505a26b400497b460b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354048"
---
# <a name="wideworldimportersdw-etl-workflow"></a>WideWorldImportersDW ETL 工作流
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
当数据更改时，使用 *WWI_Integration* ETL 包将数据从 WideWorldImporters 数据库迁移到 WideWorldImportersDW 数据库。 包定期运行 (通常) 。

包通过使用 SQL Server Integration Services 来协调大容量 T-sql (操作，而不是 Integration Services) 中的单独转换来确保高性能。

首先加载维度，然后加载事实数据表。 在发生故障后，可以随时重新运行包。

工作流如下所示：

 ![WideWorldImporters ETL 工作流](media/wide-world-importers/wideworldimporters-etl-workflow.png)

工作流以确定适当截止时间的表达式任务开头。 截止时间是当前时间减去几分钟。  (此方法比向当前时间请求数据更可靠。 ) 从该时间开始截断任何毫秒。

主处理首先填充 "日期" 维度表。 处理可确保已在表中填充了当前年份的所有日期。

接下来，一系列数据流任务加载每个维度。 然后，它们会加载每个事实。

## <a name="prerequisites"></a>先决条件

- SQL Server 2016 (或更高版本) ，其中的 WideWorldImporters 和 WideWorldImportersDW 数据库 (在 SQL Server 的相同或不同实例中) 
- SQL Server Management Studio
- SQL Server 2016 Integration Services
  - 确保创建 Integration Services 目录。 若要创建 Integration Services 目录，请在 SQL Server Management Studio 对象资源管理器中右键单击 **Integration Services**，然后选择 " **添加目录**"。 保留默认选项。 系统将提示您启用 SQLCLR 并提供密码。


## <a name="download"></a>下载

有关该示例的最新版本，请参阅 [宽-全球导入程序-版本](https://go.microsoft.com/fwlink/?LinkID=800630)。 下载 *.ispac* Integration Services 包文件。

若要使源代码重新创建示例数据库，请参阅 [宽世界导入程序](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/wwi-ssis)。

## <a name="install"></a>安装

1. 部署 Integration Services 包：
   1. 在 Windows 资源管理器中，打开 *.ispac* 包。 这将启动 SQL Server Integration Services 部署向导。
   2. 在 " **选择源**" 下，按项目部署的默认值，并将路径指向 *每日 .ispac* 包。
   3. 在 " **选择目标**" 下，输入承载 Integration Services 目录的服务器的名称。
   4. 选择 Integration Services 目录下的路径，例如，在名为 *WideWorldImporters* 的新文件夹中。
   5. 选择 " **部署** " 以完成向导。

2. 为 ETL 过程创建 SQL Server 代理作业：
   1. 在 Management Studio 中，右键单击 **SQL Server 代理**，然后选择 "**新建**  >  **作业**"。
   2. 输入名称，例如， *WIDEWORLDIMPORTERS ETL*。
   3. 添加 **SQL Server Integration Services 包** 类型的 **作业步骤**。
   4. 选择具有 "Integration Services" 目录的服务器，然后选择 " *每日 ETL* 包"。
   5. 在 "**配置**  >  **连接管理器**" 下，确保已正确配置与源和目标的连接。 默认值为连接到本地实例。
   6. 选择 **"确定"** 以创建作业。

3. 执行或计划作业。
