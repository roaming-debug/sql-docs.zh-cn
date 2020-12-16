---
description: 管理由系统控制版本的临时表中历史数据的保留期
title: 管理版本由系统控制的临时表中的历史数据
ms.custom: seo-lt-2019
ms.date: 05/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 7925ebef-cdb1-4cfe-b660-a8604b9d2153
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f742ece496377a224a67b12223b09d198327812a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484489"
---
# <a name="manage-retention-of-historical-data-in-system-versioned-temporal-tables"></a>管理由系统控制版本的临时表中历史数据的保留期


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


对于版本由系统控制的临时表，历史记录表可能比常规表更容易增大数据库大小，尤其是在以下条件下：

- 你长期保留历史数据
- 你采用频繁更新或删除的数据修改模式

大型以及不断增长的历史记录表可能会成为一个问题，这不单单体现在存储成本的增加上，而且还会降低临时查询的性能。 因此，针对管理历史记录表中的数据制定一个数据保留策略，是规划和管理每个时态表的生命周期的一个重要方面。

## <a name="data-retention-management-for-history-table"></a>历史记录表的数据保留管理

管理临时表数据的保留首先要确定每个临时表的所需保留期。 在大多数情况下，应该将保留策略视为使用临时表的应用程序的业务逻辑的一部分。 例如，数据审核和按时间顺序查看方案中的应用程序对历史数据必须可供在线查询的时间有严格的要求。

确定数据保留期后，下一步就是制定一个计划，规定如何管理历史数据、将历史数据存储在何处，以及如何删除超过保留要求的历史数据。 可通过以下四种方法管理时态历史记录表中的历史数据：

- [Stretch 数据库](#using-stretch-database-approach)
- [表分区](#using-table-partitioning-approach)
- [自定义清理脚本](#using-custom-cleanup-script-approach)
- [保留策略](#using-temporal-history-retention-policy-approach)

 使用其中每种方法时，迁移或清理历史记录数据的逻辑将基于对应于当前表期末时间的列。 每行的期末时间值确定行版本“结束”（即放入历史记录表）的时刻。 例如，条件 `SysEndTime < DATEADD (DAYS, -30, SYSUTCDATETIME ())` 指定超过一个月的历史数据需要删除并从历史记录表中移出。

> [!NOTE]
> 本主题中的示例使用此[临时表实例](creating-a-system-versioned-temporal-table.md)。

## <a name="using-stretch-database-approach"></a>使用 Stretch Database 方法

> [!NOTE]
> Stretch Database 方法仅适用于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 而不适用于 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。

[Stretch Database](../../sql-server/stretch-database/stretch-database.md) 中的 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 以透明方式将历史数据迁移到 Azure。 为了提高安全性，你可以使用 SQL Server 的 [始终加密](../security/encryption/always-encrypted-database-engine.md) 功能来加密动态数据。 此外，你可以使用 [行级别安全性](../../relational-databases/security/row-level-security.md) 和其他高级 SQL Server 安全功能以及 Temporal 和 Stretch Database 来保护数据。

使用 Stretch Database 方法，可以将部分或所有临时历史记录表延伸到 Azure，而 SQL Server 以无提示方式将历史数据移到 Azure。 对历史记录表启用延伸不会更改你在修改数据和执行临时查询时与临时表的交互方式。

- **延伸整个历史记录表：** 如果主要方案是在环境中审核数据，并且数据更改较为频繁，而对历史数据的查询相对很少，则可以为整个历史记录表配置 Stretch Database。 换而言之，如果临时查询的性能并不重要，则使用此方法。 在这种情况下，Azure 提供的成本效益可能极具吸引力。 在延伸整个历史记录表时，可以使用延伸向导或 Transact-SQL。 下面列出了两种方法的示例。

- **延伸一部分历史记录表：** 如果方案主要涉及到查询最近的历史数据，但你希望可以在需要时选择查询较旧的历史数据，同时以较低成本远程存储此数据，则可以仅为一部分历史记录表配置 Stretch Database 以提高性能。 使用 Transact-SQL 可以实现此目的，方法是指定谓词函数来选择要从历史记录表中迁移的行而不是迁移所有行。 当你处理临时表时，通常有用的做法是基于时间条件（即，基于历史记录表中行版本的期限）移动数据。

  使用确定性谓词函数可将一部分历史记录保留在包含当前数据的同一数据库中，其余部分将迁移到 Azure。 有关示例和限制，请参阅 [使用筛选器函数选择要迁移的行 (Stretch Database)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md)。 由于不确定性函数无效，如果你要滑动窗口的方式转移历史记录数据，需要定期更改内联谓词函数的定义，使本地保留的行窗口在期限内保持恒定。 滑动窗口可让你不断地将一个月以前的历史数据移到 Azure。 此方法的示例如下所示。

> [!NOTE]
> Stretch Database 会将数据迁移到 Azure。 因此，你必须拥有 Azure 帐户，以及计费的订阅。 要获取免费试用的 Azure 帐户，请单击[免费试用一个月](https://azure.microsoft.com/pricing/free-trial/)。

你可以使用延伸向导或 Transact-SQL 来为延伸配置临时历史记录表，并且可以在版本由系统控制设置为“打开”  时，为临时历史记录表启用延伸。 不允许延伸当前表，因为延伸当前表没有意义。

### <a name="using-the-stretch-wizard-to-stretch-the-entire-history-table"></a>使用延伸向导延伸整个历史记录表

适用于初学者的最简单方法是使用延伸向导为整个数据库启用延伸，然后在延伸向导中选择临时历史记录表（本示例假设你已将 Department 表配置为其他空数据库中的版本由系统控制的临时表）。 在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]中，你无法右键单击临时历史记录表本身和单击“延伸”。

1. 右键单击数据库，指向“任务”  ，指向“延伸”  ，然后单击“启用”  以启动向导。
2. 在“选择表”  窗口中，选择临时历史记录表的复选框，然后单击“下一步”。

    ![在“选择表”页上选择历史记录表](../../relational-databases/tables/media/stretch-wizard-2-for-temporal.png "在“选择表”页上选择历史记录表")
3. 在“配置 Azure”  窗口中提供你的登录凭据。 登录到 Microsoft Azure 或注册一个帐户。 选择要使用的订阅并选择 Azure 区域。 然后创建一个新的服务器或选择现有的服务器。 单击“下一步”。

    ![创建新的 Azure 服务器 - Stretch Database 向导](../../relational-databases/tables/media/stretch-wizard-4.png "创建新的 Azure 服务器 - Stretch Database 向导")
4. 在“安全凭据”  窗口中，提供数据库主密钥密码来保护源 SQL Server 数据库凭据，然后单击“下一步”。

    ![Stretch Database 向导的“安全凭据”页](../../relational-databases/tables/media/stretch-wizard-6.png "Stretch Database 向导的“安全凭据”页")
5. 在“选择 IP 地址”窗口中，提供 SQL Server 的 IP 地址范围，以便 Azure 服务器能够与 SQL Server 通信（如果你选择已存在防火墙规则的现有服务器，则只需单击“下一步”即可使用现有的防火墙规则）  。 单击“下一步”，然后单击“完成”以启用 Stretch Database 并延伸临时历史记录表   。

    ![选择 Stretch Database 向导的“IP 地址”页](../../relational-databases/tables/media/stretch-wizard-7.png "选择 Stretch Database 向导的“IP 地址”页")
6. 向导完成后，验证数据库已成功启用延伸。 请注意，对象资源管理器中的图标指示数据库已延伸。

> [!NOTE]
> 如果“启用数据库延伸”失败，请查看错误日志。 一种常见的错误是防火墙规则配置不正确。

另请参阅：

- [为数据库启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)
- [通过运行“为数据库启用延伸向导”开始操作](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md)
- [为表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)

### <a name="using-transact-sql-to-stretch-the-entire-history-table"></a>使用 Transact-SQL 来延伸整个历史记录表

也可以使用 Transact-SQL 在本地启用延伸和 [为数据库启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)。 然后，你可以[使用 Transact-SQL 在表上启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)。 对于以前为 Stretch Database 启用的数据库，请执行下面的 Transact-SQL 脚本，以延伸版本由系统控制的现有的临时历史记录表：

```sql
ALTER TABLE <history table name>
SET (REMOTE_DATA_ARCHIVE = ON (MIGRATION_STATE = OUTBOUND));
```

### <a name="using-transact-sql-to-stretch-a-portion-of-the-history-table"></a>使用 Transact-SQL 来延伸部分历史记录表

若仅延伸历史记录表的一部分，首先创建 [内联谓词函数](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md)。 对于本示例，假设你最初在 2015 年 12 月 1 日配置了内联谓词函数，现在希望将 2015 年 11 月 1 日以前的所有历史记录数据延伸到 Azure。 若要完成此操作，首先创建以下函数：

```sql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20151101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2015-11-01T00:00:00', 101) ;
```

接下来，使用以下脚本将筛选器谓词添加到历史记录表，并将迁移状态设置为 OUTBOUND，以启用历史记录表的基于谓词的数据迁移。

```sql
ALTER TABLE <history table name>
SET (
      REMOTE_DATA_ARCHIVE = ON
        (
          FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20151101 (SysEndTime)
            , MIGRATION_STATE = OUTBOUND
        )
    )
;
```

要维护滑动窗口，需每日保持谓词函数精确（即每天更改筛选行条件）。 下面的脚本是需要在 2015 年 12 月 2 日执行的脚本：

```sql
BEGIN TRAN
/*(1) Create new predicate function definition */
  CREATE FUNCTION dbo.fn_StretchBySystemEndTime20151102(@systemEndTime datetime2)
   RETURNS TABLE
    WITH SCHEMABINDING
      AS
        RETURN SELECT 1 AS is_eligible
          WHERE @systemEndTime < CONVERT(datetime2,'2015-11-02T00:00:00', 101)
  GO
 
/*(2) Set the new function as filter predicate */
  ALTER TABLE <history table name>
    SET
      (
        REMOTE_DATA_ARCHIVE = ON
          (
            FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20151102(SysEndTime),
              MIGRATION_STATE = OUTBOUND
          )
      )
COMMIT ;
```

使用 SQL Server 代理或某些其他计划机制，以确保始终使用有效的谓词函数定义。

## <a name="using-table-partitioning-approach"></a>使用表分区方法

[表分区](../partitions/create-partitioned-tables-and-indexes.md) 可以使大型表更易于管理并且更灵活。 借助表分区方法，可以使用历史记录表分区来根据时间条件实现自定义数据清理或脱机存档。 在使用分区消除查询数据历史记录子集中的临时表时，表分区还可带来性能优势。

使用表分区，你可以实施滑动窗口方法，将最早的历史数据部分移出历史记录表，并使保留部分的大小在期限上保持恒定 - 根据所需的保留期来保留历史记录表中的数据。 当 SYSTEM_VERSIONING 为 ON 时，支持将数据换出历史记录表的操作，这意味着，你可以在不造成维护时段或阻碍常规工作负载的情况下清除一部分历史记录数据。

> [!NOTE]
> 若要执行分区切换，历史记录表中的聚集索引必须与分区架构相符（必须包含 SysEndTime）。 系统创建的默认历史记录表包含一个聚集索引，其中包括 SysEndTime 和 SysStartTime 列，并且最适合于用于分区、插入新历史记录数据和典型的临时查询。 有关详细信息，请参阅 [Temporal Tables](../../relational-databases/tables/temporal-tables.md)。

使用滑动窗口方法时需要执行两组任务：

- 分区配置任务
- 重复性分区维护任务

为便于演示，假设我们想要将历史数据保留 6 个月，并且想要在一个独立的分区中保留每个月的数据。 此外，假设我们在 2015 年 9 月激活了版本由系统控制。

分区配置任务将为历史记录表创建初始分区配置。 对于本示例，我们将创建与滑动窗口（以月为单位）相符的分区数，加上一个预先准备的附加空分区（如下所述）。 此配置可确保当我们首次启动重复性分区维护任务时系统能够正确存储新数据，同时保证我们永远不会拆分包含数据的分区，从而避免很高的数据移动开销。 应参考以下示例脚本，使用 Transact-SQL 执行此任务。

下图显示了将数据保留 6 个月的初始分区配置。

![显示将数据保留 6 个月的初始分区配置的示意图。](../../relational-databases/tables/media/partitioning.png "分区")

> [!NOTE]
> 有关配置分区时使用 RANGE LEFT 与 RANGE RIGHT 对性能产生的影响，请参阅下面的“表分区的性能注意事项”。

第一个和最后一个分区分别以下限和上限“打开”，以确保每个新行具有目标分区，而不管分区依据列中的值如何。 随着时间的推移，历史记录表中的新行将移入更高的分区。 当第 6 个分区被填满时，即表示达到了目标保留期。 这是首次开始执行重复性分区维护任务的时刻（需要将该任务计划为定期运行，在本示例中为每月运行一次）。

下图演示了重复性分区维护任务（参阅下面的详细步骤）。

![显示重复性分区维护任务的示意图。](../../relational-databases/tables/media/partitioning2.png "分区 2")

重复性分区维护任务的详细步骤如下：

1. SWITCH OUT：创建临时表，然后结合 SWITCH PARTITION 参数使用 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) 语句在历史记录表与临时表之间切换分区（参阅示例 C：在表之间切换分区）。

    ```sql
    ALTER TABLE <history table> SWITCH PARTITION 1 TO <staging table>
    ```

    切换分区后，你可以选择存档临时表中的数据，然后删除或截断临时表，以便下一次可以执行此重复性分区维护任务。
2. MERGE RANGE：结合 MERGE RANGE 使用 [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md) 将空分区 1 与分区 2 合并（参阅示例 B）。 通过使用此函数删除下限，可以有效地将空分区 1 与以前的分区 2 合并，以构成新分区 1。 其他分区也会有效地更改其序号。
3. SPLIT RANGE：结合 SPLIT RANGE 使用 [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md) 创建新的空分区 7（参阅示例 A）。 通过使用此函数添加新上限，可以有效地为下一个月创建独立的分区。

### <a name="use-transact-sql-to-create-partitions-on-history-table"></a>使用 Transact-SQL 在历史记录表中创建分区

使用以下代码窗口中的 Transact-SQL 脚本来创建分区函数和分区架构，然后重新创建要与分区架构进行分区对齐的聚集索引以及分区。 对于本示例，我们将使用从 2015 年 9 月开始的的每月分区创建六个月的滑动窗口方法。

```sql
BEGIN TRANSACTION
/*Create partition function*/
    CREATE PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime] (datetime2(7))
        AS RANGE LEFT FOR VALUES
          (
            N'2015-09-30T23:59:59.999'
          , N'2015-10-31T23:59:59.999'
          , N'2015-11-30T23:59:59.999'
          , N'2015-12-31T23:59:59.999'
          , N'2016-01-31T23:59:59.999'
          , N'2016-02-29T23:59:59.999'
          )
/*Create partition scheme*/
    CREATE PARTITION SCHEME [sch_Partition_DepartmentHistory_By_SysEndTime]
        AS PARTITION [fn_Partition_DepartmentHistory_By_SysEndTime]
            TO ([PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY])
/*Re-create index to be partition-aligned with the partitioning schema*/
    CREATE CLUSTERED INDEX [ix_DepartmentHistory] ON [dbo].[DepartmentHistory]
        (
            [SysEndTime] ASC
          , [SysStartTime] ASC
        )  
    WITH
        (
            PAD_INDEX = OFF
          , STATISTICS_NORECOMPUTE = OFF
          , SORT_IN_TEMPDB = OFF
          , DROP_EXISTING = ON
          , ONLINE = OFF
          , ALLOW_ROW_LOCKS = ON
          , ALLOW_PAGE_LOCKS = ON
          , DATA_COMPRESSION = PAGE
        )
    ON [sch_Partition_DepartmentHistory_By_SysEndTime] ([SysEndTime])

COMMIT TRANSACTION;
```

### <a name="using-transact-sql-to-maintain-partitions-in-sliding-window-scenario"></a>使用 Transact-SQL 来维护滑动窗口方案中的分区

使用以下代码窗口中的 Transact-SQL 脚本来维护滑动窗口方案中的分区。 对于本示例，我们将使用 MERGE RANGE 切出 2015 年 9 月的分区，然后使用 SPLIT RANGE 添加 2016 年 3 月的新分区。

```sql
BEGIN TRANSACTION
/*(1) Create staging table */
    CREATE TABLE [dbo].[staging_DepartmentHistory_September_2015]
        (
            [DeptID] [int] NOT NULL
          , [DeptName] [varchar](50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
          , [ManagerID] [int] NULL
          , [ParentDeptID] [int] NULL
          , [SysStartTime] [datetime2](7) NOT NULL
          , [SysEndTime] [datetime2](7) NOT NULL
        ) ON [PRIMARY]
    WITH
        (
            DATA_COMPRESSION = PAGE
        )
/*(2) Create index on the same filegroups as the partition that will be switched out*/
    CREATE CLUSTERED INDEX [ix_staging_DepartmentHistory_September_2015]
        ON [dbo].[staging_DepartmentHistory_September_2015]
        (
            [SysEndTime] ASC
          , [SysStartTime] ASC
        )
    WITH
        (
            PAD_INDEX = OFF
          , SORT_IN_TEMPDB = OFF
          , DROP_EXISTING = OFF
          , ONLINE = OFF
          , ALLOW_ROW_LOCKS = ON
          , ALLOW_PAGE_LOCKS = ON
        )
    ON [PRIMARY]
/*(3) Create constraints matching the partition that will be switched out*/
    ALTER TABLE [dbo].[staging_DepartmentHistory_September_2015] WITH CHECK
        ADD CONSTRAINT [chk_staging_DepartmentHistory_September_2015_partition_1]
            CHECK ([SysEndTime]<=N'2015-09-30T23:59:59.999')
    ALTER TABLE [dbo].[staging_DepartmentHistory_September_2015]
        CHECK CONSTRAINT [chk_staging_DepartmentHistory_September_2015_partition_1]
/*(4) Switch partition to staging table*/
    ALTER TABLE [dbo].[DepartmentHistory]
        SWITCH PARTITION 1 TO [dbo].[staging_DepartmentHistory_September_2015]
        WITH (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 MINUTES, ABORT_AFTER_WAIT = NONE))
/*(5) [Commented out] Optionally archive the data and drop staging table
      INSERT INTO [ArchiveDB].[dbo].[DepartmentHistory]
      SELECT * FROM [dbo].[staging_DepartmentHistory_September_2015];
      DROP TABLE [dbo].[staging_DepartmentHIstory_September_2015];
*/
/*(6) merge range to move lower boundary one month ahead*/
    ALTER PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime]()
        MERGE RANGE(N'2015-09-30T23:59:59.999')
/*(7) Create new empty partition for "April and after" by creating new boundary point and specifying NEXT USED file group*/
    ALTER PARTITION SCHEME [sch_Partition_DepartmentHistory_By_SysEndTime] NEXT USED [PRIMARY]
        ALTER PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime]() SPLIT RANGE(N'2016-03-31T23:59:59.999')
COMMIT TRANSACTION
```

你可以稍微修改上述脚本，并将其用于每月的例行维护过程：

1. 在步骤 (1) 中，为想要删除的月份创建新的临时表（在本示例中，下一个月是 10 月）。
2. 在步骤 (3) 中，创建并检查与想要删除数据的月份匹配的约束：对于 10 月分区为 `[SysEndTime]<=N'2015-10-31T23:59:59.999'`。
3. 在步骤 (4) 中，将分区 1 切换到新创建的临时表。
4. 在步骤 (6) 中，通过合并下限来更改分区函数：移出 10 月份的数据后为 `MERGE RANGE(N'2015-10-31T23:59:59.999'`。
5. 在步骤 (7) 中，通过创建新上限来拆分分区函数：移出 10 月份的数据后为 `SPLIT RANGE (N'2016-04-30T23:59:59.999'` 。

但是，最佳解决方案是定期运行一个通用 Transact-SQL 脚本，该脚本无需经过修改即可每月执行相应的操作。 可以通用化上述脚本，以便能够使用提供的参数（需要合并的下限，以及要使用分区拆分创建的新边界）运行。 为了避免每个月创建临时表，你可以事先创建一个临时表，然后通过更改 check 约束以匹配要切出的分区，来重复使用该表。查看以下页面，获得有关如何使用 Transact-SQL 脚本 [完全自动化滑动窗口](/previous-versions/sql/sql-server-2005/administrator/aa964122(v=sql.90)) 。

### <a name="performance-considerations-with-table-partitioning"></a>表分区的性能注意事项

必须执行 MERGE 和 SPLIT RANGE 操作以避免任何数据移动，因为数据移动可能会产生极高的性能开销。 有关详细信息，请参阅[修改分区函数](../../relational-databases/partitions/modify-a-partition-function.md)。当使用 [CREATE PARTITION FUNCTION (Transact SQL )](../../t-sql/statements/create-partition-function-transact-sql.md) 时，可以使用RANGE LEFT 而不是 RANGE RIGHT 来实现此目的。

首先，让我们以直观的方式解释 RANGE LEFT 和 RANGE RIGHT 选项的含义：

![显示 RANGE LEFT 和 RANGE RIGHT 选项的示意图。](../../relational-databases/tables/media/partitioning3.png "分区 3")

当你将某个分区函数定义为 RANGE LEFT 时，指定的值为分区的上限。 当你使用 RANGE RIGHT 时，指定的值为分区的下限。 当你使用 MERGE RANGE 操作从分区函数定义中删除边界时，底层实现也会删除包含边界的分区。 如果该分区不为空，则会因 MERGE RANGE 操作而将数据移到分区。

在滑动窗口方案中，我们始终要删除最低的分区边界。

- RANGE LEFT 用例：在 RANGE LEFT 用例中，最低分区边界属于分区 1，而该分区是空的（在换出分区后），因此执行 MERGE RANGE 不会发生任何数据移动。
- RANGE RIGHT 用例：在 RANGE RIGHT 用例中，最低分区边界属于分区 2，而该分区不是空的，因为我们假设分区 1 是通过换出清空的。在这种情况下，执行 MERGE RANGE 将发生数据移动（分区 2 中的数据将移到分区 1）。 为了避免此问题，滑动窗口方案中的 RANGE RIGHT 需要一个始终为空的分区 1。 这意味着，如果我们使用 RANGE RIGHT，则应创建并维护一个与 RANGE LEFT 用例比较的附加分区。

**结论**：要进行分区管理和避免数据移动，在滑动分区中使用 RANGE LEFT 要方便得多。 但是，使用 RANGE RIGHT 定义分区要稍微简单一些，因为你无需处理日期时间的时钟周期问题。

## <a name="using-custom-cleanup-script-approach"></a>使用自定义清理脚本方法

如果 Stretch Database 和表分区方法不可行，则第三种方法是使用自定义清理脚本从历史记录表中删除数据。 仅当 **SYSTEM_VERSIONING = OFF** 时，才可以从历史记录表中删除数据。 为了避免数据不一致，请在维护时段（修改数据的工作负载处于非活动状态）或在事务中（有效阻止其他工作负载）执行清理。 此操作需要对当前和历史记录表拥有 **CONTROL** 权限。

为了将对常规应用程序和用户查询的阻碍减到最小，请在事务中执行清理脚本时，以一定的延迟删除较小区块中的数据。 尽管在所有情况下要删除的每个数据区块的最佳大小并没有定论，但在单个事务中删除超过 10,000 行可能会造成严重的影响。

清理逻辑对于每个临时表是相同的，因此，可以通过你计划的、要针对你要限制其数据历史记录的每个临时表定期运行的泛型存储过程，来相对轻松地自动化该逻辑。

下图演示了应该如何针对单个表组织你的清理逻辑，以降低对运行中工作负载的影响。

![一张示意图，显示该如何针对一张表整理你的清理逻辑，以降低对运行中工作负载的影响。](../../relational-databases/tables/media/customcleanupscriptdiagram.png "CustomCleanUpScriptDiagram")

下面是有关实施该过程的概要指导。 将清理逻辑计划为每天运行，并循环访问所有需要清理数据的临时表。 使用 SQL Server 代理或其他工具来计划此过程：

- 通过多次迭代删除较小区块中每个临时表的历史数据（从最早的行开始，到最新的行为止），并避免在单个事务中删除所有行，如上图所示。
- 将每个迭代实现为泛型存储过程的调用，以便从历史记录表中删除一部分数据（参阅以下代码示例以了解此过程）。
- 计算每次调用该过程时，需要在单个临时表中删除的行数。 根据该数字以及你想要使用的迭代数，动态确定每个过程调用的拆分点。
- 计划针对单个表的迭代之间的延迟时段，以降低对访问临时表的应用程序的影响。

以下代码段中显示了一个用于删除单个临时表的数据的存储过程（请仔细研究此代码，并在将它应用到环境之前进行调整）：

```sql
DROP PROCEDURE IF EXISTS sp_CleanupHistoryData;
GO

CREATE PROCEDURE sp_CleanupHistoryData
        @temporalTableSchema sysname
      , @temporalTableName sysname
      , @cleanupOlderThanDate datetime2
AS
    DECLARE @disableVersioningScript nvarchar(max) = '';
    DECLARE @deleteHistoryDataScript nvarchar(max) = '';
    DECLARE @enableVersioningScript nvarchar(max) = '';

DECLARE @historyTableName sysname
DECLARE @historyTableSchema sysname
DECLARE @periodColumnName sysname

/*Generate script to discover history table name and end of period column for given temporal table name*/
EXECUTE sp_executesql
    N'SELECT @hst_tbl_nm = t2.name, @hst_sch_nm = s.name, @period_col_nm = c.name
        FROM sys.tables t1
            JOIN sys.tables t2 on t1.history_table_id = t2.object_id
        JOIN sys.schemas s on t2.schema_id = s.schema_id
            JOIN sys.periods p on p.object_id = t1.object_id
           JOIN sys.columns c on p.end_column_id = c.column_id and c.object_id = t1.object_id
                  WHERE
                 t1.name = @tblName and s.name = @schName'
                , N'@tblName sysname
                , @schName sysname
                , @hst_tbl_nm sysname OUTPUT
                , @hst_sch_nm sysname OUTPUT
                , @period_col_nm sysname OUTPUT'
                , @tblName = @temporalTableName
                , @schName = @temporalTableSchema
                , @hst_tbl_nm = @historyTableName OUTPUT
                , @hst_sch_nm = @historyTableSchema OUTPUT
                , @period_col_nm = @periodColumnName OUTPUT
  
IF @historyTableName IS NULL OR @historyTableSchema IS NULL OR @periodColumnName IS NULL
    THROW 50010, 'History table cannot be found. Either specified table is not system-versioned temporal or you have provided incorrect argument values.', 1

/*Generate 3 statements that will run inside a transaction:
  (1) SET SYSTEM_VERSIONING = OFF,
  (2) DELETE FROM history_table,
  (3) SET SYSTEM_VERSIONING = ON
  On SQL Server 2016, it is critical that (1) and (2) run in separate EXEC statements, or SQL Server will generate the following error:
  Msg 13560, Level 16, State 1, Line XXX
  Cannot delete rows from a temporal history table '<database_name>.<history_table_schema_name>.<history_table_name>'.
*/
SET @disableVersioningScript = @disableVersioningScript + 'ALTER TABLE [' + @temporalTableSchema + '].[' + @temporalTableName + '] SET (SYSTEM_VERSIONING = OFF)'
SET @deleteHistoryDataScript = @deleteHistoryDataScript + ' DELETE FROM [' + @historyTableSchema + '].[' + @historyTableName + ']
    WHERE ['+ @periodColumnName + '] < ' + '''' + convert(varchar(128), @cleanupOlderThanDate, 126) + ''''
SET @enableVersioningScript = @enableVersioningScript + ' ALTER TABLE [' + @temporalTableSchema + '].[' + @temporalTableName + ']
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = [' + @historyTableSchema + '].[' + @historyTableName + '], DATA_CONSISTENCY_CHECK = OFF )); '

BEGIN TRAN
    EXEC (@disableVersioningScript);
    EXEC (@deleteHistoryDataScript);
    EXEC (@enableVersioningScript);
COMMIT;
```

## <a name="using-temporal-history-retention-policy-approach"></a>使用时态历史记录保留策略方法

> [!NOTE]
> 时态历史记录保留策略方法适用于 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 和 SQL Server 2017（从 CTP 1.3 开始）。

可在单个表级别配置时态历史记录保留，这使用户可以创建灵活的期限策略。 应用时态保留很简单：仅需要在创建表或更改架构期间设置一个参数。

定义保留策略后，Azure SQL 数据库开始定期检查是否有符合数据自动清理条件的历史数据行。 识别匹配行并将其从历史记录表中删除，这些操作以透明方式在系统计划和运行的后台任务中进行。 历史记录表行的期限条件根据表示 SYSTEM_TIME 期限结束时间的列进行检查。 例如，如果保留期设置为 6 个月，则应清理的表行应符合以下条件：

```sql
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
```
上面的示例假设 ValidTo 列对应于 SYSTEM_TIME 的结束期限。

### <a name="how-to-configure-retention-policy"></a>如何配置保留策略

配置时态表的保留策略之前，请首先检查是否已在数据库级别启用时态历史记录保留：

```sql
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
```

数据库标志 is_temporal_history_retention_enabled 默认设置为 ON，但用户可以使用 ALTER DATABASE 语句对其进行更改  。 此标志还会在时间点还原操作后自动设置为 OFF。 若要为数据库启用时态历史记录保留清理，请执行以下语句：

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION ON
```

创建表期间通过指定 HISTORY_RETENTION_PERIOD 参数的值对保留策略进行配置：

```sql
CREATE TABLE dbo.WebsiteUserInfo
(
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )
 WITH
 (
    SYSTEM_VERSIONING = ON
    (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
    )
 );
```

可使用不同的时间单位指定保留期：天、周、月和年。 如果忽略了 HISTORY_RETENTION_PERIOD，则假定保留期为 INFINITE。 也可以显式使用 INFINITE 关键字。
在某些情况下，你可能想要在创建表后配置保留策略或更改以前配置的值。 此时应使用 ALTER TABLE 语句：

```sql
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
```

若要查看保留策略的当前状态，请使用以下查询（此查询将数据库级别的时态保留启用标志与单个表的保留期相联接）：

```sql
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
```

### <a name="how-sql-database-deletes-aged-rows"></a>SQL 数据库如何删除陈旧行

清理过程具体取决于历史记录表的索引布局。 必须注意，只能为具有聚集索引（B 树或列存储）的历史记录表配置有限期保留策略。 对于具有有限保留期的所有时态表，系统会创建一个后台任务来执行陈旧数据清理。 行存储（B 树）聚集索引的清理逻辑会删除较小区块（最多 10K）的过期行，以减轻数据库日志和 I/O 子系统的压力。 虽然清理逻辑使用要求的 B 树索引，但无法完全保证删除超过保留期的行的顺序。 因此，请不要对应用程序中的清理顺序有任何期待  。

聚集列存储的清理任务可同时移除整个行组（每组通常包含 100 万行），效率非常高，高速生成历史数据时尤其如此。

![聚集列存储保留](../../relational-databases/tables/media/cciretention.png "聚集列存储保留")

聚集列存储索引具有优秀的数据压缩和高效的保留清理能力，是工作负载快速生成大量历史数据时的最佳选择。 此模式尤其适用于密集型事务处理工作负载，此类工作负载使用时态表跟踪和审计更改、执行趋势分析或 IoT 数据引入。

有关更多详细信息，请参阅[使用保留策略管理时态表中的历史数据](/azure/sql-database/sql-database-temporal-tables-retention-policy)。

## <a name="next-steps"></a>后续步骤

- [临时表](../../relational-databases/tables/temporal-tables.md)
- [系统版本控制临时表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [临时表系统一致性检查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [临时表分区](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [临时表注意事项和限制](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [临时表安全性](../../relational-databases/tables/temporal-table-security.md)
- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [临时表元数据视图和函数](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)