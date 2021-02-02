---
description: 通过分离和附加来移动数据库 (Transact-SQL)
title: 通过分离和附加来移动数据库 (Transact-SQL)
ms.date: 06/03/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- database attaching [SQL Server]
- moving databases [SQL Server]
- database detaching [SQL Server]
- relocating databases [SQL Server]
- detaching databases [SQL Server]
- attaching databases [SQL Server]
ms.assetid: 6732a431-cdef-4f1e-9262-4ac3b77c275e
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
ms.openlocfilehash: d57963f6203ab107c19ff1713f21a41378995725
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813483"
---
# <a name="move-a-database-using-detach-and-attach-transact-sql"></a>通过分离和附加来移动数据库 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中将分离的数据库移至其他位置，并将其重新附加到相同或不同的服务器实例。 但是，我们建议您使用 ALTER DATABASE 计划重定位过程（而不使用分离和附加操作）移动数据库。 有关详细信息，请参阅 [Move User Databases](../../relational-databases/databases/move-user-databases.md)。  
  
> [!IMPORTANT]  
>  建议您不要附加或还原来自未知或不可信源的数据库。 此类数据库可能包含恶意代码，这些代码可能会执行非预期的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码，或者通过修改架构或物理数据库结构导致错误。 使用来自未知源或不可信源的数据库前，请在非生产服务器上针对数据库运行 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) ，然后检查数据库中的代码，例如存储过程或其他用户定义代码。  
  
## <a name="procedure"></a>过程  
  
#### <a name="to-move-a-database-by-using-detach-and-attach"></a>使用分离和附加操作移动数据库  
  
1.  分离数据库。 有关详细信息，请参阅 [分离数据库](../../relational-databases/databases/detach-a-database.md)。  
  
2.  在 Windows 资源管理器或 Windows“命令提示符”窗口中，将分离的数据库文件和日志文件移至新位置。  
  
     即使打算创建新的日志文件，也应该移动日志文件。 在某些情况下，重新附加数据库需要使用其现有的日志文件。 因此，除非在不使用分离日志文件的情况下可以成功附加数据库，否则，请始终保留所有分离的日志文件。  
  
    > [!NOTE]  
    >  如果尝试在不指定日志文件的情况下附加数据库，则附加操作将会在日志文件的原始位置中查找文件。 如果原始位置还有一份日志，则附加该日志。 若要避免使用原始日志文件，请指定新日志文件的路径，或在日志文件复制到新位置之后，删除其原始副本。  
  
3.  附加复制的文件。 有关详细信息，请参阅 [Attach a Database](../../relational-databases/databases/attach-a-database.md)。  
  
## <a name="example"></a>示例  
 以下示例创建名为 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 的 `MyAdventureWorks`数据库副本。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在与该服务器实例（附加该数据库副本）连接的查询编辑器窗口中执行。  
  
1.  执行以下 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 语句以分离 [!INCLUDE[tsql](../../includes/tsql-md.md)] 数据库：  
  
    ```sql
    USE master;  
    GO  
    EXEC sp_detach_db @dbname = N'AdventureWorks2012';  
    GO  
    ```  
  
2.  使用您选择的方法，将数据库文件（AdventureWorks208R2_Data.mdf 和 AdventureWorks208R2_log）分别复制到：C:\MySQLServer\AdventureWorks208R2_Data.mdf 和 C:\MySQLServer\AdventureWorks208R2_Log.ldf。  
  
    > [!IMPORTANT]  
    >  对于生产数据库，请将数据库和事务日志存放在不同的磁盘上。  
  
     若要通过网络将文件复制到远程计算机的磁盘上，请使用远程位置的通用命名约定 (UNC) 名称。 UNC 名称采用以下格式： **\\\\** _服务器名称_ **\\** _共享名_ **\\** _路径_ **\\** _文件名_。 将文件写入本地硬盘时，必须对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例使用的用户帐户授予读写远程磁盘文件所需的相应权限。  
  
3.  通过执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句来附加移动的数据库及其日志（可选）：  
  
    ```sql
    USE master;  
    GO  
    CREATE DATABASE MyAdventureWorks   
        ON (FILENAME = 'C:\MySQLServer\AdventureWorks2012_Data.mdf'),  
        (FILENAME = 'C:\MySQLServer\AdventureWorks2012_Log.ldf')  
        FOR ATTACH;  
    GO  
    ```  
  
     在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，新附加的数据库在对象资源管理器中不是立即可见的。 若要查看数据库，请在对象资源管理器中，单击 **“查看”** ，再单击 **“刷新”**。 在对象资源管理器中展开 **“数据库”** 节点后，新附加的数据库即显示在数据库列表中。  
  
## <a name="see-also"></a>另请参阅  
 [数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)  
  
  
