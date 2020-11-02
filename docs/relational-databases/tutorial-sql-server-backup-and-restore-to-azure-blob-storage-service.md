---
title: 快速入门：备份和还原到 Azure blob 存储服务
description: 快速入门：了解如何将备份写入 Azure Blob 存储服务以及如何从中还原。 创建 Azure Blob 容器，编写备份，然后还原。
ms.custom: seo-dt-2019
ms.date: 04/09/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: quickstart
ms.assetid: 9e1d94ce-2c93-45d1-ae2a-2a7d1fa094c4
author: rothja
ms.author: jroth
ms.openlocfilehash: c1f79050a4bbabcfc8729ccdc270d47fe9055c29
ms.sourcegitcommit: 67befbf7435f256e766bbce6c1de57799e1db9ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524062"
---
# <a name="quickstart-sql-backup-and-restore-to-azure-blob-storage-service"></a>快速入门：将 SQL 备份和还原到 Azure Blob 存储服务
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)]
本快速入门可帮助你了解如何将备份写入 Azure Blob 存储服务以及如何从中还原。  本文介绍如何创建 Azure Blob 容器，将备份写入 Blob 服务，然后执行还原。
  
## <a name="prerequisites"></a>先决条件  
要完成本快速入门，必须熟悉 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 备份和还原概念以及 T-SQL 语法。  需要 Azure 存储帐户、SQL Server Management Studio (SSMS)，以及对运行 SQL Server 的服务器或 Azure SQL 托管实例的访问权限。 此外，用于发出 BACKUP 和 RESTORE 命令的帐户应属于具有“更改任意凭据”权限的 db_backupoperator数据库角色。 

- 获取免费的 [Azure 帐户](https://azure.microsoft.com/offers/ms-azr-0044p/)。
- 创建 [Azure 存储帐户](/azure/storage/common/storage-quickstart-create-account?tabs=portal)。
- 安装 [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。
- 使用通过 [Azure SQL 虚拟机](/azure/sql-database/sql-database-managed-instance-configure-vm)或[点到站点](/azure/sql-database/sql-database-managed-instance-configure-p2s)建立的连接安装 [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads) 或部署 [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance-get-started)。
- 将用户帐户分配到 [db_backupoperator](./security/authentication-access/database-level-roles.md) 角色，并授予[更改任意凭据](../t-sql/statements/alter-credential-transact-sql.md)权限。 

## <a name="create-azure-blob-container"></a>创建 Azure Blob 容器
容器对 Blob 集进行分组。 所有 Blob 必须都在一个容器中。 一个存储帐户可含有无限数量的容器，但必须至少有一个容器。 一个容器可以存储无限数量的 Blob。 

若要创建容器，请执行下列步骤：

1. 打开 Azure 门户。 
1. 导航到你的存储帐户。 
1. 选择该存储帐户，向下滚动到“Blob 服务”。
1. 选择“Blob”，然后选择“+ 容器”以添加一个新容器。 
1. 输入容器名称并记下指定的容器名称。 此信息在本快速入门稍后部分的 T-SQL 语句的 URL（备份文件的路径）中使用。 
1. 选择“确定”。 
    
    ![新建容器](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/new-container.png)

  > [!NOTE]
  > 即使选择创建公共容器，SQL Server 备份和还原仍需要对存储帐户进行身份验证。 还可以使用 REST API 以编程方式创建容器。 有关详细信息，请参阅[创建容器](/rest/api/storageservices/Create-Container)

## <a name="create-a-test-database"></a>创建测试数据库 
在此步骤中，使用 SQL Server Management Studio (SSMS) 创建测试数据库。 

1. 启动 [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 并连接到 SQL Server 实例。
1. 打开“新建查询”窗口。 
1. 运行以下 TRANSACT-SQL (T-SQL) 代码来创建测试数据库。 刷新对象资源管理器中的“数据库”节点，查看新数据库 。 SQL 托管实例上新建的数据库会自动启用 TDE，因此需要禁用它才能继续操作。 

```sql
USE [master]
GO

-- Create database
CREATE DATABASE [SQLTestDB]
GO

-- Create table in database
USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO

-- Populate table 
USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO

-- Disable TDE for newly-created databases on SQL Managed Instance 
USE [SQLTestDB];
GO
ALTER DATABASE [SQLTestDB] SET ENCRYPTION OFF;
GO
```

## <a name="create-credential"></a>创建凭据

通过执行以下步骤，在 SQL Server Management Studio 中使用 GUI 创建凭据。 或者，也可以[以编程方式](tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature)创建凭据。 

1. 展开 [SQL Server Management Studio(SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 的对象资源管理器中的“数据库”节点 。
1. 右键单击新的 `SQLTestDB` 数据库，将鼠标悬停在“任务”上，然后选择“备份...”以启动“备份数据库”向导  。 
1. 从“备份到”目标下拉列表中选择“URL”，然后选择“添加”以启动“选择备份目标”对话框   。 

   ![备份到 URL](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/back-up-to-url.png)

1. 选择“选择备份目标”对话框上的“新建容器”以启动“连接到 Microsoft 订阅”窗口  。 

   ![“选择备份目标”对话框窗口的屏幕截图，其中突出显示了“新建容器”选项。](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-backup-destination.png)

1. 通过选择“登录...”登录到 Azure 门户，然后完成登录过程。 
1. 从下拉列表中选择你的订阅。 
1. 从下拉列表中选择你的存储帐户。 
1. 从下拉列表中选择以前创建的容器。 
1. 选择“创建凭据”以生成共享访问签名 (SAS)。  **保存此值以供还原使用。**

   ![创建凭据](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/create-credential.png)

1. 选择“确定”以关闭“连接到 Microsoft 订阅”窗口 。 这会填充“选择备份目标”对话框上的“Azure 存储容器”值。 选择“确定”以选择所选存储容器，然后关闭对话框。 
1. 此时，可以跳到下一部分中的步骤 4 以执行数据库的备份，如果要继续使用 Transact-SQL 备份数据库，则可以关闭“备份数据库”向导。 


## <a name="back-up-database"></a>备份数据库
在此步骤中，使用 SQL Server Management Studio 中的 GUI 或 Transact-SQL (T-SQL) 将数据库 `SQLTestDB` 备份到 Azure Blob 存储帐户。 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. 如果“备份数据库”向导尚未打开，则展开 [SQL Server Management Studio(SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 的对象资源管理器中的“数据库”节点  。
1. 右键单击新的 `SQLTestDB` 数据库，将鼠标悬停在“任务”上，然后选择“备份...”以启动“备份数据库”向导  。 
1. 从“备份到”下拉列表中选择“URL”，然后选择“添加”以启动“选择备份目标”对话框   。 

   ![备份到 URL](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/back-up-to-url.png)

1. 在“Azure 存储容器”下拉列表中选择上一步中创建的容器。 

   ![Azure 存储容器](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/azure-storage-container.png)

1. 选择“备份数据库”向导上的“确定”以备份数据库 。 
1. 成功备份数据库后，选择“确定”以关闭所有与备份相关的窗口。 

   > [!TIP]
   > 可以通过选择“备份数据库”向导顶部的“脚本”来编写此命令后面的 Transact-SQL 的脚本 ：![脚本命令](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/script-backup-command.png)


# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

通过运行以下命令，使用 Transact-SQL 备份数据库： 


```sql
USE [master]

BACKUP DATABASE [SQLTestDB] 
TO  URL = N'https://msftutorialstorage.blob.core.windows.net/sql-backup/sqltestdb_backup_2020_01_01_000001.bak' 
WITH  COPY_ONLY, CHECKSUM
GO
```

---

## <a name="delete-database"></a>删除数据库
在此步骤中，先删除数据库再执行还原。 此步骤仅适用于本教程，但不太可能用于普通的数据库管理过程。 可以跳过此步骤，但之后将需要在托管实例上的还原过程中更改数据库的名称，或运行还原命令 `WITH REPLACE` 以便在本地成功还原数据库。 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. 展开对象资源管理器中的“数据库”节点，右键单击 `SQLTestDB` 数据库，然后选择“删除”以启动“删除对象”向导  。 
1. 在托管实例上，选择“确定”以删除数据库。 在本地，选中“关闭现有连接”旁边的复选框，然后选择“确定”以删除数据库 。 

# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

通过运行以下 Transact-SQL 命令，删除数据库：

```sql
USE [master]
GO
DROP DATABASE [SQLTestDB]
GO

-- If connections currently exist on-premises, you'll need to set the database into single user mode first
USE [master]
GO
ALTER DATABASE [SQLTestDB] SET  SINGLE_USER WITH ROLLBACK IMMEDIATE
GO
USE [master]
GO
DROP DATABASE [SQLTestDB]
GO
```

---


## <a name="restore-database"></a>对话框的 
在此步骤中，使用 SQL Server Management Studio 中的 GUI 或 Transact-SQL 还原数据库。 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. 在 SQL Server Management Studio 的对象资源管理器中右键单击“数据库”节点，然后选择“还原数据库”  。 
1. 选择“设备”，然后选择省略号 (...) 以选择设备。 

   ![选择还原设备](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-restore-device.png)

1. 从“备份介质类型”下拉列表中选择“URL”，然后选择“添加”以添加设备  。 

   ![添加备份设备](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/add-backup-device.png)

1. 从下拉列表中选择容器，然后将其粘贴在创建凭据时保存的共享访问签名 (SAS) 中。 

   ![“选择备份文件位置”对话框的屏幕截图，其中已填充“共享访问签名”字段。](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/restore-from-container.png)

1. 选择“确定”以选择备份文件位置。 
1. 展开“容器”，然后选择备份文件所在的容器。 
1. 选择要还原的备份文件，然后选择“确定”。 如果未显示任何文件，则可能是使用了错误的 SAS 密钥。 可以按照上述步骤重新生成 SAS 密钥，以便添加容器。 

   ![选择还原文件](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-restore-file.png)

1. 选择“确定”，关闭“选择备份设备”对话框 。 
1. 选择“确定”以还原数据库。 

# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

若要从 Azure Blob 存储还原本地数据库，请修改以下 Transact-SQL 命令以使用自己的存储帐户，然后在新的查询窗口中运行该命令。 

```sql
USE [master]
RESTORE DATABASE [SQLTestDB] FROM 
URL = N'https://msftutorialstorage.blob.core.windows.net/sql-backup/sqltestdb_backup_2020_01_01_000001.bak'
```

---


## <a name="see-also"></a>另请参阅 
以下是一些建议阅读的主题，便于了解概念以及在将 Azure Blob 存储服务用于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 备份时的最佳做法。  
  
-   [使用 Microsoft Azure Blob 存储服务进行 SQL Server 备份和还原](../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)   
-   [从 SQL Server 备份到 URL 的最佳做法和故障排除](../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)  
