---
description: 创建已分区表和已分区索引
title: 创建已分区表和已分区索引 | Microsoft Docs
ms.custom: ''
ms.date: 1/5/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.createpartition.progress.f1
- sql13.swb.createpartition.partitioncolumn.f1
- sql13.swb.createpartition.createjob.f1
- sql13.swb.createpartition.finish.f1
- sql13.swb.createpartition.selectoutput.f1
- sql13.swb.createpartition.partitionfunction.f1
- sql13.swb.createpartition.partitionscheme.f1
- sql13.swb.createpartition.getstart.f1
- sql13.swb.createpartition.mappartition.f1
- sql13.swb.createpartition.summary.f1
helpviewer_keywords:
- partitioned indexes [SQL Server], creating
- partition schemes [SQL Server], creating
- partition functions [SQL Server], creating
- partitioned tables [SQL Server], creating
- partition functions [SQL Server]
- partition schemes [SQL Server]
ms.assetid: 7641df10-1921-42a7-ba6e-4cb03b3ba9c8
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 273e6f625b0a86be179da529e8b52e0d7ce991cc
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049139"
---
# <a name="create-partitioned-tables-and-indexes"></a>创建已分区表和已分区索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  可以使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建已分区表或索引。 已分区表和索引中的数据水平划分到可分散到数据库的多个文件组的单元中。 分区可以使大型表和索引更易于管理并且更灵活。  
  
 创建已分区表或索引通常包含四个操作：  
  
1.  创建将持有分区方案所指定的分区的文件组和相应的文件。  
  
2.  创建一个分区函数，该函数根据指定列的值将表或索引的各行映射到分区。  
  
3.  创建一个将已分区表或已分区索引的分区映射到新文件组的分区方案。  
  
4.  创建或修改表或索引，并指定分区方案作为存储位置。  
 
> [!NOTE]
> Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 中仅支持主文件组。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **创建已分区表或索引，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   分区函数和方案的作用域被限制为在其中创建它们的数据库。 在该数据库内，分区函数驻留在与其他函数的命名空间不同的一个单独命名空间内。  
  
-   如果分区函数中的任何行具有带 Null 值的分区列，会将这些行分配到最左侧分区。 但是，如果将 NULL 指定为边界值并指示 RIGHT，则最左侧的分区仍为空，NULL 值位于第二个分区。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 创建已分区表需要在数据库中具有 CREATE TABLE 权限，对在其中创建表的架构具有 ALTER 权限。 创建已分区索引需要对要创建索引的表或视图具有 ALTER 权限。 创建已分区表或索引需要以下附加权限之一：  
  
-   ALTER ANY DATASPACE 权限。 默认情况下，此权限授予 **sysadmin** 固定服务器角色和 **db_owner** 及 **db_ddladmin** 固定数据库角色的成员。  
  
-   对在其中创建分区函数和分区方案的数据库的 CONTROL 或 ALTER 权限。  
  
-   对在其中创建分区函数和分区方案的数据库所在服务器的 CONTROL SERVER 或 ALTER ANY DATABASE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 按此过程中的步骤创建一个或多个文件组、相应文件和一个表。 创建已分区表时，您将在下一个过程中引用这些对象。  
  
#### <a name="to-create-new-filegroups-for-a-partitioned-table"></a>创建已分区表的新文件组  
  
1.  在对象资源管理器中，右键单击要创建已分区表的数据库，然后选择“属性”。  
  
2.  在“数据库属性 - database_name”对话框中的“选择页面”下，选择“文件组” 。  
  
3.  在 **“行”** 下单击 **“添加”**。 在新行中，输入文件组名称。  
  
    > [!WARNING]  
    >  如果指定多个文件组，则在创建分区时，除了为边界值指定的文件组数外，还必须始终具有一个额外的文件组。  
  
4.  继续添加行，直到您创建了已分区表的所有文件组。  
  
5.  单击“确定”。   
  
6.  在 **“选择页”** 下，选择 **“文件”**。  
  
7.  在 **“行”** 下单击 **“添加”**。 在新行中，输入文件名并选择文件组。  
  
8.  继续添加行，直到为每个文件组至少创建了一个文件。  
  
9. 展开 **“表”** 文件夹，并按常规方式创建一个表。 有关详细信息，请参阅[创建表（数据库引擎）](../../relational-databases/tables/create-tables-database-engine.md)。 或者，您可以在下一过程中指定现有的表。  
  
#### <a name="to-create-a-partitioned-table"></a>创建已分区表  
  
1.  右键单击要分区的表，指向“存储”，然后单击“创建分区…”。  
  
2.  在 **“创建分区向导”** 的 **“欢迎使用创建分区向导”** 页上，单击 **“下一步”**。  
  
3.  在 **“选择分区列”** 页的 **“可用分区列”** 网格中，选择要对表分区的列。 **“可用分区列”** 网格只显示其数据类型可以用于对数据进行分区的列。 如果选择计算列作为分区列，必须将该列指定为持久化列。  
  
     可供选择的分区列和值范围主要由数据按逻辑方式可分组的范围所决定。 例如，您可以选择按一年的月份数或季度数将数据分成逻辑组。 您计划对数据进行的查询将确定此逻辑组对于管理表分区是否足够。 所有数据类型都可有效用作分区列，除了 **text**、 **ntext**、 **image**、 **xml**、 **timestamp**、 **varchar(max)**、 **nvarchar(max)**、 **varbinary(max)**、别名数据类型或 CLR 用户定义的数据类型。  
  
     此页还提供以下附加选项：  
  
     **将此表与选定的已分区表并置**  
     允许您选择一个已分区表，其中包含根据分区列要与此表相联接的相关数据。 对于其分区已按分区列联接的表，通常可以更有效地进行查询。  
  
     **将非唯一索引和唯一索引的存储空间调整为与索引分区列一致**  
     将已分区表的所有索引调整为与同一分区方案一致。 当表及其索引对齐时，由于数据是使用同一算法进行分区的，所以您可以更高效地在已分区表中移入和移出分区。  
  
     选择分区列和任意其他选项后，单击 **“下一步”**。  
  
4.  在 **“选择分区函数”** 页的 **“选择分区函数”** 下，单击 **“新建分区函数”** 或 **“现有分区函数”**。 如果选择 **“新建分区函数”**，则输入函数的名称。 如果选择“现有分区函数”，则从列表中选择要使用的函数名称。 如果数据库中没有其他分区函数， **“现有分区函数”** 选项将不可用。  
  
     完成此页后，单击 **“下一步”** 。  
  
5.  在 **“选择分区方案”** 页的 **“选择分区方案”** 下，单击 **“新建分区方案”** 或 **“现有分区方案”**。 如果选择 **“新建分区方案”**，则输入方案的名称。 如果选择“现有分区方案”，则从列表中选择要使用的方案名称。 如果数据库中没有其他分区方案， **“现有分区方案”** 选项将不可用。  
  
     完成此页后，单击 **“下一步”** 。  
  
6.  在 **“映射分区”** 页的 **“范围”** 下，选择 **“左边界”** 或 **“右边界”** 以指定在创建的每个文件组内是包括最高边界值还是最低边界值。 如果指定多个文件组，则在创建分区时，除了为边界值指定的文件组数外，还必须始终输入一个额外的文件组。  
  
     在 **“选择文件组并指定边界值”** 网格的 **“文件组”** 下，选择要对数据进行分区的文件组。 在 **“边界”** 下，输入每个文件组的边界值。 如果边界值为左侧空，则分区函数使用分区函数名称将整个表或索引映射到单个分区。  
  
     此页还提供以下附加选项：  
  
     **设置边界…**  
     打开 **“设置边界值”** 对话框可以为分区选择所需的边界值和日期范围。 仅在您选择了包含以下数据类型之一的分区列时，此选项才可用： **date**、 **datetime**、 **smalldatetime**、 **datetime2** 或 **datetimeoffset**。  
  
     **预计存储空间**  
     估计为分区指定的每个文件组的存储空间的行计数、所需空间和可用空间。 这些值将以只读值形式显示在网格中。  
  
     **“设置边界值”** 对话框允许包含以下附加选项：  
  
     **开始日期**  
     选择分区范围值的开始日期。  
  
     **结束日期**  
     选择分区范围值的结束日期。 如果在“映射分区”页上选择了“左边界”，此日期将是每个文件组/分区的最后一个值。 如果在“映射分区”页上选择了“右边界”，此日期将是倒数第二个文件组中的第一个值。  
  
     **日期范围**  
     为每个分区选择所需的日期粒度或范围值增量。  
  
     完成此页后，单击 **“下一步”** 。  
  
7.  在 **“选择输出选项”** 页上，指定要如何完成已分区表。 选择 **“创建脚本”** 可以基于向导中的前一页创建 SQL 脚本。 选择 **“立即运行”** 可以在完成向导中的其余页后创建新的已分区表。 选择 **“计划”** 可以在将来的预定时间创建新的已分区表。  
  
     如果选择 **“创建脚本”** ， **“脚本选项”** 下的以下选项将可用：  
  
     **将脚本保存到文件**  
     将脚本生成为 .sql 文件。 在 **“文件名”** 框中输入文件名和位置，或单击 **“浏览”** 以打开 **“脚本文件位置”** 对话框。 从 **“另存为”** 选择 **“Unicode 文本”** 或 **“ANSI 文本”** 。  
  
     **将脚本保存到剪贴板**  
     将脚本保存到剪贴板。  
  
     **将脚本保存到“新建查询”窗口**  
     将脚本生成到新的查询编辑器窗口。 这是默认选项。  
  
     如果选择 **“计划”** ，则单击 **“更改计划”** 。  
  
    1.  在“新建作业计划”对话框的“名称”框中，输入作业计划的名称 。  
  
    2.  在 **“计划类型”** 列表中选择计划类型：  
  
        -   **SQL Server 代理启动时自动启动**  
  
        -   **CPU 空闲时启动**  
  
        -   **重复执行**。 如果新的已分区表定期使用新信息更新，请选择此选项。  
  
        -   **执行一次**。 这是默认选项。  
  
    3.  选择或清除 **“已启用”** 复选框以启用或禁用计划。  
  
    4.  如果选择 **“重复执行”** ：  
  
        1.  在 **“频率”** 下的 **“执行”** 列表中，指定执行的频率：  
  
            -   如果选择 **“每天”** ，请在 **“执行间隔”** 框中输入重复作业计划的频率（天）。  
  
            -   如果选择 **“每周”** ，请在 **“执行间隔”** 框中输入重复作业计划的频率（周）。 选择运行作业计划的一周中的某天或某些天。  
  
            -   如果选择 **“每月”** ，可以选择 **“天”** 或 **“特定日期”** 。  
  
                -   如果选择 **“天”** ，请输入要运行作业计划的当月日期和作业计划的重复频率（月）。 例如，如果要每隔一个月在当月的 15 日运行计划作业，请选择“天”，在第一个框中输入“15”，在第二个框中输入“2”。 请注意，第二个框中允许的最大数是“99”。  
  
                -   如果选择 **“特定日期”** ，请选择要运行作业计划的当月内一周的特定一天和作业计划的重复频率（月）。 例如，如果要每隔一个月在当月的最后一个工作日运行作业计划，请选择“天”，从第一个列表中选择“最后一周”，从第二个列表中选择“工作日”，然后在最后一个框中输入“2”  。 还可以从前两个列表中选择“第一周”、“第二周”、“第三周”或“第四周”以及特定工作日（例如：星期日或星期三）。 请注意，最后一个框中允许的最大数是“99”。  
  
        2.  在 **“每天频率”** 下，指定作业计划运行的当天作业计划的重复频率。  
  
            -   如果选择 **“执行一次，时间为:”** ，请在 **“执行一次，时间为:”** 框中输入运行作业计划的当天的特定时间。 输入当天的小时、分钟和秒以及 AM 或 PM。  
  
            -   如果选择 **“执行间隔”** ，请在 **“频率”** 下指定所选日运行作业计划的频率。 例如，如果要在运行作业计划的当天每隔 2 小时重复一次，请选择“执行间隔”，在第一个框中输入“2”，然后从列表中选择“小时” 。 从此列表中还可以选择“分钟”和“秒”。 请注意，第一个框中允许的最大数是“100”。  
  
                 在 **“开始时间”** 框中，输入开始运行作业计划的时间。 在 **“结束时间”** 框中，输入停止重复作业计划的时间。 输入当天的小时、分钟和秒以及 AM 或 PM。  
  
        3.  在 **“持续时间”** 下的 **“开始日期”** 中，输入希望作业计划开始运行的日期。 选择 **“结束日期”** 或 **“无结束日期”** 以指示作业计划应在何时停止运行。 如果选择 **“结束日期”** ，输入希望作业计划停止运行的日期。  
  
    5.  如果选择“执行一次”，请在“执行一次”下的“日期”框中输入将运行作业计划的日期。 在 **“时间”** 框中，输入将运行作业计划的时间。 输入当天的小时、分钟和秒以及 AM 或 PM。  
  
    6.  在 **“摘要”** 下的 **“说明”** 中，验证所有作业计划设置均正确。  
  
    7.  单击“确定”。  
  
     完成此页后，单击 **“下一步”** 。  
  
8.  在 **“检查摘要”** 页的 **“检查所做选择”** 下，展开所有可用选项以验证所有分区设置是正确的。 如果一切正常，请单击 **“完成”** 。  
  
9. 在 **“创建分区向导进度”** 页上，监视有关创建分区向导的操作的状态信息。 根据在向导中选择的选项，“进度”页可能会包含一个操作或多个操作。 最上面的方框显示向导的总体状态和向导已接收到的状态、错误和警告消息数。  
  
     **“创建分区向导进度”** 页上提供以下选项：  
  
     **详细信息**  
     提供向导执行的操作所返回的操作、状态和所有消息。  
  
     **Action**  
     指定每个操作的类型和名称。  
  
     **Status**  
     指示向导操作作为一个整体返回的值是“成功”还是“失败”。  
  
     **消息**  
     提供从该进程中返回的任何错误或警告消息。  
  
     **Report**  
     创建包含创建分区向导结果的报告。 这些选项是 **“查看报告”** 、 **“将报告保存到文件”** 、 **“将报告复制到剪贴板”** 和 **“将报告作为电子邮件发送”** 。  
  
     **查看报告**  
     打开“查看报告”对话框，其中包含关于创建分区向导进度的文本报告。  
  
     **将报告保存到文件**  
     打开“将报告另存为”对话框。  
  
     **将报告复制到剪贴板**  
     将向导的进度报告结果复制到剪贴板。  
  
     **“将报告作为电子邮件发送”**  
     将向导的进度报告结果复制到电子邮件。  
  
     完成时，单击“关闭” 。  
  
 创建分区向导将创建分区函数和方案，然后将分区应用于指定的表。 若要验证表分区，请在对象资源管理器中，右键单击表，然后选择“属性”。 单击 **“存储”** 页。 该页面将显示分区函数和方案的名称以及分区数目之类的信息。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-partitioned-table"></a>创建已分区表  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 该示例将创建新的文件组、分区函数和分区方案。 将创建一个新表，该表具有指定为存储位置的分区方案。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Adds four new filegroups to the AdventureWorks2012 database  
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
  
    -- Adds one file for each filegroup.  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test1dat1,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat1.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test1fg;  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test2dat2,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t2dat2.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test2fg;  
    GO  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test3dat3,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t3dat3.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test3fg;  
    GO  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test4dat4,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t4dat4.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test4fg;  
    GO  
    -- Creates a partition function called myRangePF1 that will partition a table into four partitions  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
        AS RANGE LEFT FOR VALUES (1, 100, 1000) ;  
    GO  
    -- Creates a partition scheme called myRangePS1 that applies myRangePF1 to the four filegroups created above  
    CREATE PARTITION SCHEME myRangePS1  
        AS PARTITION myRangePF1  
        TO (test1fg, test2fg, test3fg, test4fg) ;  
    GO  
    -- Creates a partitioned table called PartitionTable that uses myRangePS1 to partition col1  
    CREATE TABLE PartitionTable (col1 int PRIMARY KEY, col2 char(10))  
        ON myRangePS1 (col1) ;  
    GO  
    ```  


#### <a name="to-create-a-partitioned-table-in-azure-sqldbesa"></a>若要在 Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 中创建已分区表

Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 中不支持添加文件和文件组，但支持通过仅跨 PRIMARY 文件组进行分区来进行表分区。
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
1.  在标准菜单栏上，单击 **“新建查询”** 。  
  
1.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 该示例会创建一个分区功能和一个分区方案。 将创建一个新表，该表具有指定为存储位置的分区方案。 

    ```
    -- Creates a partition function called myRangePF1 that will partition a table into four partitions  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
        AS RANGE LEFT FOR VALUES (1, 100, 1000) ;  
    GO  
    -- Creates a partition scheme called myRangePS1 that applies myRangePF1 to the PRIMARY filegroup 
    CREATE PARTITION SCHEME myRangePS1  
        AS PARTITION myRangePF1  
        ALL TO ('PRIMARY') ;  
    GO  
    -- Creates a partitioned table called PartitionTable that uses myRangePS1 to partition col1  
    CREATE TABLE PartitionTable (col1 int PRIMARY KEY, col2 char(10))  
        ON myRangePS1 (col1) ;  
    GO
    ```  

  
#### <a name="to-determine-if-a-table-is-partitioned"></a>确定表是否分区  
  
1.  如果表 `PartitionTable` 已分区，以下查询将返回一个或多个行。 如果表未分区，则不返回任何行。  
  
    ```  
    SELECT *   
    FROM sys.tables AS t   
    JOIN sys.indexes AS i   
        ON t.[object_id] = i.[object_id]   
        AND i.[type] IN (0,1)   
    JOIN sys.partition_schemes ps   
        ON i.data_space_id = ps.data_space_id   
    WHERE t.name = 'PartitionTable';   
    GO  
    ```  
  
#### <a name="to-determine-the-boundary-values-for-a-partitioned-table"></a>确定已分区表的边界值  
  
1.  以下查询对于 `PartitionTable` 表中的每个分区返回边界值。  
  
    ```  
    SELECT t.name AS TableName, i.name AS IndexName, p.partition_number, p.partition_id, i.data_space_id, f.function_id, f.type_desc, r.boundary_id, r.value AS BoundaryValue   
    FROM sys.tables AS t  
    JOIN sys.indexes AS i  
        ON t.object_id = i.object_id  
    JOIN sys.partitions AS p  
        ON i.object_id = p.object_id AND i.index_id = p.index_id   
    JOIN  sys.partition_schemes AS s   
        ON i.data_space_id = s.data_space_id  
    JOIN sys.partition_functions AS f   
        ON s.function_id = f.function_id  
    LEFT JOIN sys.partition_range_values AS r   
        ON f.function_id = r.function_id and r.boundary_id = p.partition_number  
    WHERE t.name = 'PartitionTable' AND i.type <= 1  
    ORDER BY p.partition_number;  
    ```  
  
#### <a name="to-determine-the-partition-column-for-a-partitioned-table"></a>确定已分区表的分区列  
  
1.  以下查询返回表的分区列的名称。 `PartitionTable`.  
  
    ```  
    SELECT   
        t.[object_id] AS ObjectID   
        , t.name AS TableName   
        , ic.column_id AS PartitioningColumnID   
        , c.name AS PartitioningColumnName   
    FROM sys.tables AS t   
    JOIN sys.indexes AS i   
        ON t.[object_id] = i.[object_id]   
        AND i.[type] <= 1 -- clustered index or a heap   
    JOIN sys.partition_schemes AS ps   
        ON ps.data_space_id = i.data_space_id   
    JOIN sys.index_columns AS ic   
        ON ic.[object_id] = i.[object_id]   
        AND ic.index_id = i.index_id   
        AND ic.partition_ordinal >= 1 -- because 0 = non-partitioning column   
    JOIN sys.columns AS c   
        ON t.[object_id] = c.[object_id]   
        AND ic.column_id = c.column_id   
    WHERE t.name = 'PartitionTable' ;   
    GO  
    ```  
  
 有关详细信息，请参见:  
  
-   [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)  
  
-   [CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md)  
  
-   [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)  
  
-   [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
  
  
