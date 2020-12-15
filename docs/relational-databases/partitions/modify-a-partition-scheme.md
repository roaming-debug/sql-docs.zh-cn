---
description: 修改分区方案
title: 修改分区方案 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 515de63f-dfc5-434d-9adb-f3b5992f745a
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 78426696f05d1983e57238a969c232872ac859ae
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97406531"
---
# <a name="modify-a-partition-scheme"></a>修改分区方案
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  通过使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 指定用于保存添加到已分区表的下一个分区的文件组，可以修改 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的分区方案。 可通过将 NEXT USED 属性分配给文件组来执行此操作。 您可以将 NEXT USED 属性分配给空文件组或已存有分区的文件组。 也就是说，一个文件组可以保存多个分区。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **创建已分区表或索引，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 受 ALTER PARTITION SCHEME 影响的所有文件组都必须处于联机状态。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 以下权限可用于执行 ALTER PARTITION SCHEME：  
  
-   ALTER ANY DATASPACE 权限。 默认情况下，此权限授予 **sysadmin** 固定服务器角色和 **db_owner** 及 **db_ddladmin** 固定数据库角色的成员。  
  
-   对创建分区方案时所在数据库的 CONTROL 或 ALTER 权限。  
  
-   对承载了创建分区方案时所在数据库的服务器的 CONTROL SERVER 或 ALTER ANY DATABASE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **修改分区方案：**  
  
 无法使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]执行此特定操作。 若要修改某个分区方案，您必须先删除该方案，然后通过创建分区向导使用所需属性创建一个新方案。 有关详细信息，请参阅 [“创建已分区表和已分区索引”](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)[下的](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md#SSMSProcedure) Create Partitioned Tables and Indexes **Using SQL Server Management Studio**。  
  
#### <a name="to-delete-a-partition-scheme"></a>删除分区方案  
  
1.  单击加号以便展开要删除分区方案的数据库。  
  
2.  单击加号以便展开 **“存储”** 文件夹。  
  
3.  单击加号以便展开 **“分区方案”** 文件夹。  
  
4.  右键单击要删除的分区方案，然后选择“删除”。  
  
5.  在 **“删除对象”** 对话框中，确保已选择正确的分区方案，然后单击 **“确定”**。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-modify-a-partition-scheme"></a>修改分区方案  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- add five new filegroups to the AdventureWorks2012 database  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test1fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test2fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test3fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test4fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test5fg;  
    GO  
    -- if the "myRangePF1" partition function and the "myRangePS1" partition scheme exist,  
    -- drop them from the AdventureWorks2012 database  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
    DROP PARTITION FUNCTION myRangePF1;  
    GO  
    IF EXISTS (SELECT * FROM sys.partition_schemes  
        WHERE name = 'myRangePS1')  
    DROP PARTITION SCHEME myRangePS1;  
    GO  
    -- create the new partition function "myRangePF1" with four partition groups  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    -- create the new partition scheme "myRangePS1"that will use   
    -- the "myRangePF1" partition function with five file groups.  
    -- The last filegroup, "test5fg," will be kept empty but marked  
    -- as the next used filegroup in the partition scheme.  
    CREATE PARTITION SCHEME myRangePS1  
    AS PARTITION myRangePF1  
    TO (test1fg, test2fg, test3fg, test4fg, test5fg);  
    GO  
    --Split "myRangePS1" between boundary_values 100 and 1000  
    --to create two partitions between boundary_values 100 and 500  
    --and between boundary_values 500 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    SPLIT RANGE (500);  
    GO  
    -- Allow the "myRangePS1" partition scheme to use the filegroup "test5fg"  
    -- for the partition with boundary_values of 100 and 500  
    ALTER PARTITION SCHEME myRangePS1  
    NEXT USED test5fg;  
    GO  
    ```  
  
 有关详细信息，请参阅 [ALTER PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/alter-partition-scheme-transact-sql.md)。  
  
  
