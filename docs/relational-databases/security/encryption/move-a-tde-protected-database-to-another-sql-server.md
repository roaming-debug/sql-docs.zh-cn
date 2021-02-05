---
title: 将受 TDE 保护的数据库移到其他 SQL Server
description: 本文介绍如何使用透明数据加密 (TDE) 保护数据库，然后再使用 SQL Server Management Studio (SSMS) 或 Transact-SQL (T-SQL) 将该数据库移动到其他 SQL Server 实例。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Transparent Data Encryption, moving
- TDE, moving a database
ms.assetid: fb420903-df54-4016-bab6-49e6dfbdedc7
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 1f9b85e3d2f14eb534956c218d38f98b634dc89c
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813287"
---
# <a name="move-a-tde-protected-database-to-another-sql-server"></a>将受 TDE 保护的数据库移到其他 SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 通过透明数据加密 (TDE) 来保护数据库，然后再将数据库移动到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的其他实例。 TDE 针对数据和日志文件执行实时 I/O 加密和解密。 加密使用数据库加密密钥 (DEK)，它存储在数据库引导记录中，可在恢复时使用。 DEK 是使用存储在服务器的 **master** 数据库中的证书保护的对称密钥，或者是由 EKM 模块保护的非对称密钥。   
   
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   在移动 TDE 保护的数据库时，您还必须移动用于打开 DEK 的证书或非对称密钥。 该证书或非对称密钥必须安装在目标服务器的 **master** 数据库中，以便 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可以访问数据库文件。 有关详细信息，请参阅[透明数据加密 (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md)。  
  
-   您必须保留证书文件和私钥文件的备份，以便还原证书。 用于私钥的密码不必与数据库主密钥密码相同。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认情况下，将此处创建的文件存储在 **C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA** 中。 您的文件名和位置可能会有所不同。  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
  
-   要求针对 **master** 数据库的 **CONTROL DATABASE** 权限以便创建数据库主密钥。  
  
-   要求针对 **master** 数据库的 **CREATE CERTIFICATE** 权限以便创建保护 DEK 的证书。  
  
-   需要拥有已加密数据库的 **CONTROL DATABASE** 权限和用于加密数据库加密密钥的证书或非对称密钥的 **VIEW DEFINITION** 权限。  
  
##  <a name="to-create-a-database-protected-by-transparent-data-encryption"></a><a name="SSMSProcedure"></a> 创建由透明数据加密保护的数据库  

以下过程演示了如何使用 SQL Server Management Studio 和 Transact-SQL 创建由 TDE 保护的数据库。
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSCreate"></a> 使用 SQL Server Management Studio  
  
1.  在 **master** 数据库中创建数据库主密钥和证书。 有关详细信息，请参阅下面的 **使用 Transact-SQL** 。  
  
2.  在 **master** 数据库中创建服务器证书的备份。 有关详细信息，请参阅下面的 **使用 Transact-SQL** 。  
  
3.  在对象资源管理器中，右键单击 **“数据库”** 文件夹，并选择 **“新建数据库”** 。  
  
4.  在 **“新建数据库”** 对话框的 **“数据库名称”** 框中，输入新数据库的名称。  
  
5.  在 **“所有者”** 框中，输入新数据库的所有者的名称。 或者，单击省略号 (…) 以打开“选择数据库所有者”对话框 。 有关创建新的数据库的详细信息，请参阅 [Create a Database](../../../relational-databases/databases/create-a-database.md)。  
  
6.  在对象资源管理器中，右键单击加号以展开 **“数据库”** 文件夹。  
  
7.  右键单击您所创建的数据库，指向 **“任务”** ，然后选择 **“管理数据库加密”** 。  
  
     **“管理数据库加密”** 对话框中提供了以下选项。  
  
     **加密算法**  
     显示或设置要用于数据库加密的算法。 **AES128** 为默认算法。 此字段不能为空。 有关加密算法的详细信息，请参阅 [Choose an Encryption Algorithm](../../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)。  
  
     **使用服务器证书**  
     将加密设置为由证书提供保护。 从列表中选择一个。 如果没有服务器证书的 **VIEW DEFINITION** 权限，此列表将为空。 如果选择使用证书进行加密，则此值不能为空。 有关证书的详细信息，请参阅 [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)。  
  
     **使用服务器非对称密钥**  
     将加密设置为由非对称密钥提供保护。 仅显示可用的非对称密钥。 只有受 EKM 模块保护的非对称密钥才可以使用 TDE 对数据库进行加密。  
  
     **将数据库加密设置为 ON**  
     将数据库更改为打开（选中）或关闭（取消选中）TDE。  
  
8.  完成后，单击 **“确定”** 。  

###  <a name="using-transact-sql"></a><a name="TsqlCreate"></a> 使用 Transact-SQL  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```sql  
    -- Create a database master key and a certificate in the master database.  
    USE master ;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1';  
    GO  
    CREATE CERTIFICATE TestSQLServerCert   
    WITH SUBJECT = 'Certificate to protect TDE key'  
    GO  
    -- Create a backup of the server certificate in the master database.  
    -- The following code stores the backup of the certificate and the private key file in the default data location for this instance of SQL Server   
    -- (C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA).  
  
    BACKUP CERTIFICATE TestSQLServerCert   
    TO FILE = 'TestSQLServerCert'  
    WITH PRIVATE KEY   
    (  
        FILE = 'SQLPrivateKeyFile',  
        ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1'  
    );  
    GO  
    -- Create a database to be protected by TDE.  
    CREATE DATABASE CustRecords ;  
    GO  
    -- Switch to the new database.  
    -- Create a database encryption key, that is protected by the server certificate in the master database.   
    -- Alter the new database to encrypt the database using TDE.  
    USE CustRecords;  
    GO  
    CREATE DATABASE ENCRYPTION KEY  
    WITH ALGORITHM = AES_128  
    ENCRYPTION BY SERVER CERTIFICATE TestSQLServerCert;  
    GO  
    ALTER DATABASE CustRecords  
    SET ENCRYPTION ON;  
    GO  
    ```  
  
 有关详细信息，请参阅：  
  
-   [CREATE MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE (Transact-SQL)](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [BACKUP CERTIFICATE (Transact-SQL)](../../../t-sql/statements/backup-certificate-transact-sql.md)  
  
-   [CREATE DATABASE (SQL Server Transact-SQL)](../../../t-sql/statements/create-database-transact-sql.md)  
  
-   [CREATE DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)  
  
-   [ALTER DATABASE (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql.md)  
  
##  <a name="to-move-a-database-protected-by-transparent-data-encryption"></a><a name="TsqlProcedure"></a> 移动由透明数据加密保护的数据库 

以下过程演示了如何使用 SQL Server Management Studio 和 Transact-SQL 移动由 TDE 保护的数据库。
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSMove"></a> 使用 SQL Server Management Studio  
  
1.  在对象资源管理器中，右键单击在前面已进行加密的数据库，指向“任务”，然后选择“分离…” 。  
  
     在 **“分离数据库”** 对话框中提供了以下选项。  
  
     **要分离的数据库**  
     列出要分离的数据库。  
  
     **Database Name**  
     显示要分离的数据库的名称。  
  
     **删除连接**  
     断开与指定数据库的连接。  
  
    > [!NOTE]  
    >  不能分离连接为活动状态的数据库。  
  
     **更新统计信息**  
     默认情况下，分离操作将在分离数据库时保留过期的优化统计信息；若要更新现有的优化统计信息，请单击此复选框。  
  
     **保留全文目录**  
     默认情况下，分离操作保留所有与数据库关联的全文目录。 若要删除全文目录，请清除 **“保留全文目录”** 复选框。 只有从 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]升级数据库时，才会显示此选项。  
  
     **Status**  
     显示以下状态之一：“就绪”或“未就绪” 。  
  
     **消息**  
     **“消息”** 列可显示关于数据库的如下信息：  
  
    -   当数据库进行了复制操作，则 **“状态”** 为 **“未就绪”** ， **“消息”** 列将显示 **“已复制数据库”** 。  
  
    -   当数据库有一个或多个活动连接时，则“状态”为“未就绪”，“消息”列显示“ _\<number\_of\_active\_connections\>_ 个活动连接”，例如：   “1 个活动连接”。 在分离数据库之前，需要通过选择 **“删除连接”** 断开所有活动连接。  
  
     若要获取有关消息的详细信息，请单击相应的超链接文本打开活动监视器。  
  
2.  单击“确定”。  
  
3.  使用 Window 资源管理器，将数据库文件从源服务器移动到或复制到目标服务器上的相同位置。  
  
4.  使用 Window 资源管理器，将服务器证书和私钥文件的备份从源服务器移动到或复制到目标服务器上的相同位置。  
  
5.  在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的目标实例上创建数据库主密钥。 有关详细信息，请参阅下面的 **使用 Transact-SQL** 。  
  
6.  通过使用原始服务器证书备份文件重新创建服务器证书。 有关详细信息，请参阅下面的 **使用 Transact-SQL** 。  
  
7.  在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 的对象资源管理器中，右键单击“数据库”文件夹，然后选择“分离…” 。  
  
8.  在 **“附加数据库”** 对话框中的 **“要附加的数据库”** 下，单击 **“添加”** 。  
  
9. 在“定位数据库文件 - server\_name”对话框中，选择要附加到新服务器的数据库文件，然后单击“确定”。  
  
     在 **“附加数据库”** 对话框中提供了以下选项。  
  
     **“要附加的数据库”**  
     显示所选数据库的有关信息。  
  
     \<no column header>  
     显示一个图标，用以指示附加操作的状态。 下面的 **“状态”** 说明中介绍可能的图标。  
  
     **MDF 文件位置**  
     显示选定 MDF 文件的路径和文件名。  
  
     **Database Name**  
     显示数据库的名称。  
  
     **附加为**  
     根据需要，可以指定要附加数据库的其他名称。  
  
     **所有者**  
     提供数据库可能所有者的下拉列表，您可以根据需要从其中选择其他所有者。  
  
     **Status**  
     显示下表中相应的数据库状态。  
  
    |图标|状态文本|说明|  
    |----------|-----------------|-----------------|  
    |（无图标）|（无文本）|此对象的附加操作尚未启动或者可能挂起。 这是打开该对话框时的默认值。|  
    |绿色的右向三角形|正在进行|已启动附加操作，但是该操作未完成。|  
    |绿色的选中标记|Success|已成功附加该对象。|  
    |包含白色十字形的红色圆圈|错误|附加操作遇到错误，未成功完成。|  
    |包含左、右两个黑色象限和上、下两个白色象限的圆圈|已停止|由于用户停止了附加操作，该操作未成功完成。|  
    |包含一个指向逆时针方向的曲线箭头的圆圈|已回滚|附加操作已成功，但已对其进行回滚，因为在附加其他对象的过程中出现了错误。|  
  
     **消息**  
     显示空消息或“找不到文件”超链接。  
  
     **添加**  
     查找必需的主数据库文件。 当用户选择 .mdf 文件时，就会在 **“要附加的数据库”** 网格的相应字段中自动填充合适的信息。  
  
     **删除**  
     从 **“要附加的数据库”** 网格中删除选定文件。  
  
     " <database_name> " 数据库详细信息  
     显示要附加的文件的名称。 若要验证或更改文件的路径名，请单击“浏览”按钮 (…) 。  
  
    > [!NOTE]  
    >  如果文件不存在，则 **“消息”** 列显示“找不到”。 如果找不到日志文件，则说明它位于其他目录中或者已被删除。 您需要更新 **“数据库详细信息”** 网格中该文件的路径使其指向正确的位置，或者从网格中删除该日志文件。 如果找不到 .ndf 数据文件，则需要更新网格中该文件的路径使其指向正确的位置。  
  
     **原始文件名**  
     显示属于数据库的已附加文件的名称。  
  
     **文件类型**  
     指示文件类型，即 **“数据”** 或 **“日志”** 。  
  
     **当前文件路径**  
     显示所选数据库文件的路径。 可以手动编辑该路径。  
  
     **消息**  
     显示空消息或 **“找不到文件”** 超链接。  
  
###  <a name="using-transact-sql"></a><a name="TsqlMove"></a> 使用 Transact-SQL  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```sql  
    -- Detach the TDE protected database from the source server.   
    USE master ;  
    GO  
    EXEC master.dbo.sp_detach_db @dbname = N'CustRecords';  
    GO  
    -- Move or copy the database files from the source server to the same location on the destination server.   
    -- Move or copy the backup of the server certificate and the private key file from the source server to the same location on the destination server.   
    -- Create a database master key on the destination instance of SQL Server.   
    USE master;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1';  
    GO  
    -- Recreate the server certificate by using the original server certificate backup file.   
    -- The password must be the same as the password that was used when the backup was created.  
  
    CREATE CERTIFICATE TestSQLServerCert   
    FROM FILE = 'TestSQLServerCert'  
    WITH PRIVATE KEY   
    (  
        FILE = 'SQLPrivateKeyFile',  
        DECRYPTION BY PASSWORD = '*rt@40(FL&dasl1'  
    );  
    GO  
    -- Attach the database that is being moved.   
    -- The path of the database files must be the location where you have stored the database files.  
    CREATE DATABASE [CustRecords] ON   
    ( FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\CustRecords.mdf' ),  
    ( FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\CustRecords_log.LDF' )  
    FOR ATTACH ;  
    GO  
    ```  
  
 有关详细信息，请参阅：  
  
-   [sp_detach_db (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)  
  
-   [CREATE MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE (Transact-SQL)](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE DATABASE (SQL Server Transact-SQL)](../../../t-sql/statements/create-database-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [数据库分离和附加 (SQL Server)](../../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [借助 Azure SQL 数据库实现透明数据加密](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
  
