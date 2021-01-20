---
title: AdventureWorks 示例数据库
description: 按照以下说明下载 AdventureWorks 示例数据库，并使用 Transact-sql (T-sql) SQL Server，SQL Server Management Studio (SSMS) 或 Azure Data Studio 下载并安装 AdventureWorks 示例数据库。
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 06/16/2020
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: ec38d7eeed89d03bab4ebd92446517542e2e51a0
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597006"
---
# <a name="adventureworks-sample-databases"></a>AdventureWorks 示例数据库
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本文提供了下载 AdventureWorks 示例数据库的直接链接，以及用于将它们还原到 SQL Server 和 Azure SQL 数据库的说明。 

有关示例的详细信息，请参阅 [示例 GitHub 存储库](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases)。 

## <a name="prerequisites"></a>先决条件

- [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019) 或 [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) 或 [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)


## <a name="download-backup-files"></a>下载备份文件 

使用以下链接下载适用于你的方案的相应示例数据库。 

- **OLTP** 数据适用于最典型的联机事务处理工作负荷。 
- 数据 **仓库 (DW)** 数据用于数据仓库工作负荷。 
- **轻型 (LT)** 数据是 **OLTP** 示例的轻型 pared 版本。 

如果你不确定需要的内容，请从与你的 SQL Server 版本相匹配的 OLTP 版本开始。 

|**OLTP** |**数据仓库** |**轻型**|
|---------|---------|---------|
|[AdventureWorks2019](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak)|[AdventureWorksDW2019](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2019.bak)|[AdventureWorksLT2019](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2019.bak)|
|[AdventureWorks2017](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2017.bak)|[AdventureWorksDW2017](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2017.bak)|[AdventureWorksLT2017](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2017.bak)|
|[AdventureWorks2016](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016.bak)|[AdventureWorksDW2016](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016.bak)|[AdventureWorksLT2016](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2016.bak)|
|[AdventureWorks2016_EXT .bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016_EXT.bak)|[AdventureWorksDW2016_EXT .bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016_EXT.bak)| 不适用 |
|[AdventureWorks2014](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2014.bak)|[AdventureWorksDW2014](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2014.bak)|[AdventureWorksLT2014](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2014.bak)|
|[AdventureWorks2012](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2012.bak)|[AdventureWorksDW2012](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2012.bak)|[AdventureWorksLT2012](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2012.bak)|
|[AdventureWorks2008R2](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-oltp.bak)| [AdventureWorksDW2008R2](https://github.com/microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-dw.bak) | 不适用 |

其他文件可直接在 GitHub 上找到： 

- [SQL Server 2014-2019](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)
- [SQL Server 2012](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2012)
- [SQL Server 2008 和2008R2](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2008r2)


## <a name="restore-to-sql-server"></a>还原到 SQL Server 

您可以使用该 `.bak` 文件将您的示例数据库还原到您的 SQL Server 实例中。 您可以使用 [RESTORE (transact-sql) ](../t-sql/statements/restore-statements-transact-sql.md) 命令或使用 [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) 或 [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)中 (GUI) 的图形界面来执行此操作。

# <a name="sql-server-management-studio-ssms"></a>[SQL Server Management Studio (SSMS)](#tab/ssms)

如果不熟悉如何使用 SQL Server Management Studio (SSMS) ，可以参阅 [连接 & 查询](../ssms/quickstarts/ssms-connect-query-sql-server.md) 开始使用。 

若要在 SQL Server Management Studio 中还原数据库，请执行以下步骤：

1. `.bak`从 "[下载备份文件](#download-backup-files)" 部分中提供的链接之一下载相应的文件。
2. 将该 `.bak` 文件移动到 SQL Server 的备份位置。 这不同于安装位置、实例名称和 SQL Server 版本。 例如，SQL Server 2019 的默认实例的默认位置是：

   `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`. 

3. 打开 SQL Server Management Studio (SSMS) 并连接到中的 SQL Server。 
4. 右键单击 **对象资源管理器**  >  **还原数据库 ...** "中的" 数据库 "以启动 **还原数据库** 向导。 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-ssms.png" alt-text="此屏幕截图显示了如何选择通过右键单击对象资源管理器中的 &quot;数据库&quot;，然后选择 &quot;还原数据库&quot; 来还原数据库。":::


1. 选择 " **设备** "，然后选择省略号 **( ... )** 选择一个设备。 
1. 选择 " **添加** "，然后选择 `.bak` 最近移动到此位置的文件。 如果你将文件移到此位置但无法在向导中看到它，则这通常表示权限问题 SQL Server 或登录 SQL Server 的用户无权在此文件夹中使用此文件。 
1. 选择 **"确定"** 以确认您的数据库备份选择，并关闭 " **选择备份设备** " 窗口。 
1. 检查 " **文件** " 选项卡以确认 **还原为** "位置"，文件名与 " **还原数据库** 向导" 中的预期位置和文件名匹配。 
1. 选择“确定”以还原数据库。 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-wizard-ssms.png" alt-text="显示 &quot;还原数据库&quot; 窗口的屏幕截图，其中突出显示了要还原的备份集和 &quot;确定&quot; 选项。":::

有关还原 SQL Server 数据库的详细信息，请参阅 [使用 SSMS 还原数据库备份](../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)。

# <a name="transact-sql-t-sql"></a>[Transact-SQL (T-SQL)](#tab/tsql)

您可以使用 Transact-sql (T-sql) 还原示例数据库。 下面提供了还原 AdventureWorks2019 的示例，但数据库名称和安装文件路径可能因环境而异。 

若要将 AdventureWorks2019 还原到 **Windows**，请根据环境修改相应的值，然后运行以下 Transact-sql (t-sql) 命令：


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019] 
FROM  DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\AdventureWorks2019.bak' 
WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO

```

若要将 AdventureWorks2019 还原到 **linux**，请将 Windows filesystem 路径更改为 linux，然后运行以下 Transact-sql (t-sql) 命令： 


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019]
FROM DISK = '/var/opt/mssql/backup/AdventureWorks2019.bak'
WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf',
MOVE 'AdventureWorks2017_log' TO '/var/opt/mssql/data/AdventureWorks2019_log.ldf',
FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

# <a name="azure-data-studio"></a>[Azure Data Studio](#tab/data-studio)

如果不熟悉如何使用 [Azure Data Studio Studio](../azure-data-studio/download-azure-data-studio.md)，可以查看 [connect & query](../azure-data-studio/quickstart-sql-server.md) 开始使用

若要在 Azure Data Studio 中还原数据库，请执行以下步骤：

1. `.bak`从 "[下载备份文件](#download-backup-files)" 部分中提供的链接之一下载相应的文件。
1. 将该 `.bak` 文件移动到 SQL Server 的备份位置。 这不同于安装位置、实例名称和 SQL Server 版本。 例如，SQL Server 2019 的默认实例的默认位置是：

    `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`.

1. 打开 Azure Data Studio Studio 并连接到 SQL Server 实例。
1. 右键单击服务器，然后选择 " **管理**"。

   :::image type="content" source="media/adventureworks-install-configure/ads-manage.png" alt-text="显示 Azure Data Studio，其中突出显示了 &quot;管理&quot; 选项并调用了。":::

1. 选择 **还原**

   :::image type="content" source="media/adventureworks-install-configure/ads-restore-database.png" alt-text="从顶部菜单中选择 &quot;还原&quot; 以还原数据库。":::

1. 在 " **常规** " 选项卡上，填写 " **源**" 下列出的值。
    1. 在 " **还原自**" 下，选择 " *备份文件*"。
    1. 在 " **备份文件路径**" 下，选择存储 .bak 文件的位置。 
    
   :::image type="content" source="media/adventureworks-install-configure/ads-source.png" alt-text="选择备份文件路径":::
    
    这会自动填充其他字段，例如 **数据库**、 **目标数据库** 和 **还原到**。 

   :::image type="content" source="media/adventureworks-install-configure/ads-destination-restore-plan.png" alt-text="选择备份文件路径后，字段的其余部分自动填充数据":::

1. 选择 " **还原** " 以还原数据库。 

   :::image type="content" source="media/adventureworks-install-configure/ads-restore.png" alt-text="准备就绪后，选择 &quot;还原&quot; 以还原数据库。":::

---

## <a name="deploy-to-azure-sql-database"></a>部署到 Azure SQL 数据库

可以通过两个选项查看示例 Azure SQL 数据库数据。 创建新数据库时，可以使用示例，或者可以使用 SQL Server Management Studio (SSMS) 将数据库从 SQL Server 直接部署到 Azure。

若要获取 Azure SQL 托管实例的示例数据，请参阅 [将全球范围内的导入程序还原到 SQL 托管实例](/azure/azure-sql/managed-instance/restore-sample-database-quickstart)。 

### <a name="deploy-new-sample-database"></a>部署新的示例数据库

在 Azure SQL 数据库中创建新数据库时，可以选择创建空白数据库，也可以选择创建示例数据库。 

请按照以下步骤使用示例数据库创建新数据库： 

1. 连接到 Azure 门户。
1. 选择导航窗格左上角的 " **创建资源** "。 
1. 选择 " **数据库** "，然后选择 " **SQL 数据库**"。 
1. 填写所需信息以创建数据库。 
1. 在 " **其他设置** " 选项卡上，选择 " **示例** " 作为 " **数据源**" 下的现有数据： 

   :::image type="content" source="media/adventureworks-install-configure/deploy-sample-to-azure.png" alt-text="创建 Azure SQL 数据库时，请在 &quot;Azure 门户中的&quot; 其他设置 &quot;选项卡上选择&quot; 示例 &quot;作为数据源":::

1. 选择 " **创建** " 以创建新的 SQL 数据库，这是 AdventureWorksLT 数据库的还原副本。 


### <a name="deploy-database-from-sql-server"></a>从 SQL Server 部署数据库

SQL Server Management Studio 提供直接将数据库部署到 Azure SQL 数据库的功能。 此方法当前不提供数据验证，因此用于开发和测试，不应用于生产。 

若要将示例数据库从 SQL Server 部署到 Azure SQL 数据库，请执行以下步骤：

1. 连接到 SQL Server Management Studio 中的 SQL Server。 
1. 如果尚未这样做，请将 [示例数据库还原到 SQL Server](#restore-to-sql-server)。 
1. 右键单击还原的数据库 **对象资源管理器**  >  **任务**"  >  **将数据库部署到 Microsoft Azure SQL 数据库 ...**"。 

   :::image type="content" source="media/adventureworks-install-configure/deploy-db-to-azure.png" alt-text="选择将数据库部署到 Microsoft Azure SQL 数据库，方法是右键单击数据库，然后选择 &quot;任务&quot;":::

1. 按照向导连接到 Azure SQL 数据库并部署数据库。 


## <a name="creation-scripts"></a>创建脚本

无需还原数据库，您也可以使用脚本来创建 AdventureWorks 数据库，而无需考虑版本。 

以下脚本可用于创建整个 AdventureWorks 数据库： 

- [AdventureWorks OLTP 脚本 Zip](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks-oltp-install-script.zip)
- [AdventureWorks DW 脚本 Zip](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW-data-warehouse-install-script.zip)

有关使用脚本的其他信息可在 [GitHub](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)上找到。 

## <a name="next-steps"></a>后续步骤

恢复示例数据库后，请使用以下教程来开始使用 SQL Server： 


[SQL Server 数据库引擎教程](../relational-databases/database-engine-tutorials.md)   
[与 SQL Server Management Studio (SSMS) 进行连接和查询 ](../ssms/quickstarts/ssms-connect-query-sql-server.md)   
[通过 Azure Data Studio 进行连接和查询](../ssms/quickstarts/ssms-connect-query-sql-server.md)