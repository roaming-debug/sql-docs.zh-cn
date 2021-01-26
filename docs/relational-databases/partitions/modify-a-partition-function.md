---
description: 修改分区函数
title: 修改分区函数 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: ae5bfc09-f27a-4ea9-9518-485278b11674
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b200814dedac25809d1295146ff71d8908bce4da
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766115"
---
# <a name="modify-a-partition-function"></a>修改分区函数
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 在已分区的表或已分区的索引的分区函数中增加或减少指定的分区数（加 1 或减 1），可以更改 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中的表或索引的分区方式。 增加分区的方法是将某个现有的分区“拆分”为两个分区并重新定义新分区的边界。 减少分区的方法是将两个分区的边界“合并”成一个。 减少分区操作将重新填充一个分区而不对另一个分区进行分配。  
  
> [!CAUTION]  
>  多个表或索引可以使用同一分区函数。 在修改分区函数时，将影响单个事务中的所有表或索引。 在修改分区函数之前，先检查其依赖关系。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要修改分区函数，可使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   ALTER PARTITION FUNCTION 只能用于将一个分区拆分为两个，或将两个分区合并为一个。 若要更改表或索引的分区方式（例如，从 10 个分区变为 5 个分区），可以使用下列选项之一：  
  
    -   使用所需的分区函数创建一个新的已分区表，然后使用 INSERT INTO ...SELECT FROM [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或者使用 **中的** “管理分区向导” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]将旧表中的数据插入新表。  
  
    -   为堆创建分区聚集索引。  
  
        > [!NOTE]  
        >  删除已分区的聚集索引将产生分区堆。  
  
    -   通过将 [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE INDEX 语句与 DROP EXISTING = ON 子句一起使用来删除并重新生成现有的已分区索引。  
  
    -   执行一系列 ALTER PARTITION FUNCTION 语句。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不对修改分区函数提供复制支持。 如果要对发布数据库中的分区函数进行更改，必须在订阅数据库中手动执行此操作。  
  
-   ALTER PARTITION FUNCTION 所影响的全部文件组都必须处于联机状态。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 可以使用以下任意权限执行 ALTER PARTITION FUNCTION：  
  
-   ALTER ANY DATASPACE 权限。 默认情况下，此权限授予 **sysadmin** 固定服务器角色和 **db_owner** 及 **db_ddladmin** 固定数据库角色的成员。  
  
-   对创建分区函数时所在数据库的 CONTROL 或 ALTER 权限。  
  
-   对包含创建分区函数所在的数据库的服务器具有 CONTROL SERVER 或 ALTER ANY DATABASE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **修改分区函数：**  
  
 无法使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]执行此特定操作。 若要修改某个分区函数，您必须先删除该函数，然后通过创建分区向导使用所需属性创建一个新函数。 有关详细信息，请参阅  
  
#### <a name="to-delete-a-partition-function"></a>删除分区函数  
  
1.  展开要从中删除分区函数的数据库，然后展开 **“存储”** 文件夹。  
  
2.  展开 **“分区函数”** 文件夹。  
  
3.  右键单击要删除的分区函数，然后选择“删除”。  
  
4.  在 **“删除对象”** 对话框中，确保已选择正确的分区函数，然后单击 **“确定”**。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-split-a-single-partition-into-two-partitions"></a>将一个分区拆分为两个分区  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Look for a previous version of the partition function "myRangePF1" and deletes it if it is found.  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
        DROP PARTITION FUNCTION myRangePF1;  
    GO  
    -- Create a new partition function called "myRangePF1" that partitions a table into four partitions.  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    --Split the partition between boundary_values 100 and 1000  
    --to create two partitions between boundary_values 100 and 500  
    --and between boundary_values 500 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    SPLIT RANGE (500);  
    ```  
  
#### <a name="to-merge-two-partitions-into-one-partition"></a>将两个分区合并为一个分区  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Look for a previous version of the partition function "myRangePF1" and deletes it if it is found.  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
        DROP PARTITION FUNCTION myRangePF1;  
    GO  
    -- Create a new partition function called "myRangePF1" that partitions a table into four partitions.  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    --Merge the partitions between boundary_values 1 and 100  
    --and between boundary_values 100 and 1000 to create one partition  
    --between boundary_values 1 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    MERGE RANGE (100);  
    ```  
  
 有关详细信息，请参阅 [ALTER PARTITION FUNCTION (Transact SQL)](../../t-sql/statements/alter-partition-function-transact-sql.md)。  
  
  
