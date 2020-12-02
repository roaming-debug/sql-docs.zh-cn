---
title: 快速入门：备份和还原数据库
titleSuffix: SQL Server
description: 本文介绍了如何在 SQL Server 中创建新数据库、备份数据库以及还原备份。
author: cawrites
ms.author: chadam
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: backup-restore
ms.prod_service: backup-restore
ms.assetid: ''
ms.openlocfilehash: d658358fa8e09b967b921d162b8f443032a16ddc
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129198"
---
# <a name="quickstart-backup-and-restore-a-sql-server-database-on-premises"></a>快速入门：本地备份和还原 SQL Server 数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

在本快速入门中，你将新建数据库、将其简单备份，然后将其还原。 

有关更详细的操作方法，请参阅[创建完整数据库备份](create-a-full-database-backup-sql-server.md)和[使用 SSMS 还原备份](restore-a-database-backup-using-ssms.md)。

## <a name="prerequisites"></a>先决条件
要完成本快速入门，需要具备以下条件： 

- [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)
- [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)

## <a name="create-a-test-database"></a>创建测试数据库 

1. 启动 [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) 并连接到 SQL Server 实例。
1. 打开“新建查询”窗口。 
1. 运行以下 TRANSACT-SQL (T-SQL) 代码来创建测试数据库。 刷新对象资源管理器中的“数据库”节点，查看新数据库 。 

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO


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
```
 
## <a name="take-a-backup"></a>进行备份
要备份数据库，请执行以下操作： 

1. 启动 [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) 并连接到 SQL Server 实例。
1. 在对象资源管理器中，展开“数据库”节点 。  
1. 右键单击数据库，将鼠标悬停在“任务”上，然后选择“备份...” 。 
1. 在“目标”下，确认备份路径正确。 如需更改它，请选择“删除”以删除现有路径，然后选择“添加”来键入新路径 。 可通过省略号导航到特定文件。 
1. 选择“确定”以备份数据库。 

![执行 SQL 备份](media/quickstart-backup-restore-database/backup-db-ssms.png)

或者，可运行以下 Transact-SQL 命令来备份数据库： 

```sql
BACKUP DATABASE [SQLTestDB] 
TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Backup\SQLTestDB.bak' 
WITH NOFORMAT, NOINIT,  
NAME = N'SQLTestDB-Full Database Backup', SKIP, NOREWIND, NOUNLOAD,  STATS = 10
GO
```


## <a name="restore-a-backup"></a>还原备份
要还原数据库，请执行以下操作： 

1. 启动 [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) 并连接到 SQL Server 实例。
1. 在对象资源管理器中右键单击“数据库”节点，然后选择“还原数据库...”  。

    ![还原数据库](media/quickstart-backup-restore-database/restore-db-ssms1.png)

1. 选择“设备:”，然后选择省略号 (...) 来查找备份文件。 
1. 选择“添加”，然后导航到 `.bak` 文件所在的位置。 选择 `.bak` 文件，然后选择“确定”。 
1. 选择“确定”，关闭“选择备份设备”对话框 。 
1. 选择“确定”以还原数据库备份。 

    ![还原数据库](media/quickstart-backup-restore-database/restore-db-ssms2.png)

或者，可运行以下 Transact-SQL 脚本来还原数据库：

```sql
USE [master]
RESTORE DATABASE [SQLTestDB] 
FROM DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Backup\SQLTestDB.bak' WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

### <a name="clean-up-resources"></a>清理资源
运行以下 Transact-SQL 命令来删除所创建的数据库及其在 MSDB 数据库中的备份历史记录：

```sql
EXEC msdb.dbo.sp_delete_database_backuphistory @database_name = N'SQLTestDB'
GO

USE [master]
DROP DATABASE [SQLTestDB]
GO
```

## <a name="see-more"></a>查看详细信息
[备份和还原概述](back-up-and-restore-of-sql-server-databases.md)
[备份到 URL](sql-server-backup-to-url.md)
[创建完整备份](create-a-full-database-backup-sql-server.md)
[还原数据库备份](restore-a-database-backup-using-ssms.md)
