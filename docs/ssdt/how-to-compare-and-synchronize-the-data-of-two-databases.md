---
title: 比较和同步两个数据库的数据
description: 了解如何比较和同步两个数据库的数据。 了解如何设置比较、查看差异及更新目标。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.datacompare.connection.datasources.f1
- sql.data.tools.datacompare.watermark.f1
- sql.data.tools.datacompare.connection.objectselection.f1
ms.assetid: 2148e517-ed42-41c6-b753-1ac625f594c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 73dcdd57b018f980291eceefa1be37d1b51df13a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018437"
---
# <a name="how-to-compare-and-synchronize-the-data-of-two-databases"></a>如何：比较和同步两个数据库的数据

您可以将两个数据库中包含的数据进行比较。 比较的数据库分别称作“源”和“目标”。  
  
> [!NOTE]  
> 数据库项目 和 .dacpac 或 .bacpac 包不能作为数据比较中的源或目标。  
  
在比较数据时，将生成一个数据操作语言 (DML) 脚本，你可以使用该脚本通过更新目标数据库中的部分或所有数据来同步不一致的数据库。 完成数据比较后，其结果将显示在 Visual Studio 的“数据比较”窗口中。  
  
完成比较后，您可以执行其他步骤：  
  
-   可以查看两个数据库之间的差异。 有关详细信息，请参阅[查看数据差异](#ViewDifferences)。  
  
-   可以更新目标的部分或全部以与源匹配。 有关详细信息，请参阅[同步数据库数据](#Synchronize)。  
  
有关详细信息，请参阅[将一个或多个表中的数据与引用数据库中的数据进行比较和同步](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md)。  
  
> [!NOTE]  
> 还可以比较两个数据库的架构或同一数据库的两个版本的架构。 有关详细信息，请参阅[操作说明：使用架构比较来比较不同数据库定义](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)。  
  
## <a name="comparing-database-data"></a><a name="CompareDatabaseData"></a>比较数据库数据  
  
#### <a name="to-compare-data-by-using-the-new-data-comparison-wizard"></a>使用新建数据比较向导比较数据  
  
1.  在“SQL”菜单上，指向“数据比较”，然后单击“新建数据比较”。  
  
    新建数据比较向导随即出现。 “数据比较”窗口也将随即打开，并且 Visual Studio 会自动为其分配一个名称，如 DataCompare1。  
  
2.  标识源数据库和目标数据库。  
  
    如果“源数据库”列表或“目标数据库”列表为空，则请单击“新建连接”。 在“连接属性”对话框中，标识数据库所在的服务器以及在连接到数据库时将使用的身份验证的类型。 然后，单击“确定”关闭“连接属性”对话框并返回到该数据比较向导。  
  
    在该数据比较向导的第一页上，确认每个数据库的信息均正确，再指定要包含在结果中的记录，然后单击“下一步”。 该数据比较向导的第二页随即出现，该页显示了数据库中的表和视图的层次列表。  
  
3.  选中要比较的表和视图所对应的复选框。 （可选）展开数据库对象的节点，然后选中这些对象中要比较的列所对应的复选框。  
  
    > [!NOTE]  
    > 表和视图必须满足两个条件才能出现在列表中。 第一个条件是，源数据库中对象的架构和目标数据库中对象的架构必须匹配。 第二个条件是，该列表中仅显示具有主键、唯一键、唯一索引或唯一约束的表和视图。 如果任何表或视图均未满足这两个条件，则该列表将为空。  
  
4.  如果存在多个键，可以使用“比较键”列指定要作为数据比较依据的键。 例如，可以指定是基于主键列还是其他（唯一可标识）键列进行比较。  
  
5.  单击“完成”。  
  
    比较开始。  
  
    > [!NOTE]  
    > 可通过打开“SQL”菜单，再单击“数据比较”，然后单击“停止数据比较”来停止正在进行的数据比较操作。  
  
    完成比较后，可以查看两个数据库之间的数据差异。 还可以更新目标数据库中的部分或所有数据以与源数据库中的数据匹配。  
  
#### <a name="to-compare-data-by-using-the-visual-studio-automation-model"></a>使用 Visual Studio 自动化模型比较数据  
  
1.  打开“视图”菜单，指向“其他窗口”，然后单击“命令窗口”。  
  
2.  在命令窗口中，键入以下命令：  
  
    ```  
    Sql.NewDataComparison /SrcServerName sServerName /SrcDatabaseName sDatabaseName /SrcUserName sUserName /SrcPassword sPassword /SrcDisplayName sDisplayName /TargetServerName tServerName /TargetDatabaseName tDatabaseName /TargeUserName tUserName /TargetPassword tPassword /TargetDisplayName tDisplayName  
    ```  
  
    将占位符（sServerName、sDatabaseName、sUserName、sPassword、sDisplayName、tServerName、tDatabaseName、tUserName、tPassword 和 tDisplayName）替换为源和目标数据库的值。  
  
    如果未指定源和目标，则将显示“新建数据比较”对话框。 有关 Sql.NewDataComparison 命令的参数的详细信息，请参阅 [Visual Studio Team System 的数据库功能的自动化命令参考](/previous-versions/visualstudio/visual-studio-2010/dd470565(v=vs.100))。  
  
    将指定的源数据库和目标数据库中的数据进行比较。 结果将显示在“数据比较”会话中。 有关如何查看结果或同步数据的详细信息，请参阅[查看数据差异](#ViewDifferences)和[同步数据库数据](#Synchronize)。  
  
## <a name="viewing-data-differences"></a><a name="ViewDifferences"></a>查看数据差异  
在比较两个数据库中的数据后，“数据比较”将列出比较的每个数据库对象及其状态。 还可以查看每个对象中的记录的结果（按状态分组）。 有关状态指定的详细信息，请参阅[将一个或多个表中的数据与引用数据库中的数据进行比较和同步](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md)。  
  
查看差异后，可以针对部分或全部不同的、缺少的或新的对象或记录更新目标以便与源匹配。 有关详细信息，请参阅[同步数据库数据](#Synchronize)。  
  
#### <a name="to-view-data-differences"></a>查看数据差异  
  
1.  比较源数据库和目标数据库中的数据。 有关详细信息，请参阅[比较数据库数据](#CompareDatabaseData)。  
  
2.  （可选）执行下列一种或两种操作：  
  
    -   默认情况下，将显示所有对象的结果，不管其状态如何。 若要仅显示具有特定状态的那些对象，请单击“筛选器”列表中的选项。  
  
    -   若要查看某个特定对象内的记录的结果，请在主结果窗格中单击该对象，再单击“记录视图”窗格中的某个选项卡。 每个选项卡都将显示该对象内具有特定状态的所有记录：不同、只在源中、只在目标中以及相同。 数据按记录和列显示。  
  
## <a name="synchronizing-database-data"></a><a name="Synchronize"></a>同步数据库数据  
比较两个数据库中的数据后，可以通过更新目标的部分或全部来同步这些数据以与源匹配。 可以比较两种数据库对象中的数据：表和视图。  
  
#### <a name="to-update-target-data-by-using-the-write-updates-command"></a>使用“写入更新”命令更新目标数据  
  
1.  比较源数据库和目标数据库中的数据。 有关详细信息，请参阅[比较数据库数据](#CompareDatabaseData)。  
  
    完成比较后，“数据比较”窗口将列出已比较的对象的结果。 有关不相同的对象的信息将显示在四个列（分别名为“不同的记录”、“只在源中”、“只在目标中”和“相同的记录”）中。 对于每个此类对象，这些列将显示已找到的不相同的记录数以及更新操作将更改的记录数。 这两个数字开始是相同的，但在步骤 4 中，可以更改要更新的对象。  
  
    有关详细信息，请参阅[将一个或多个表中的数据与引用数据库中的数据进行比较和同步](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md)。  
  
2.  在“数据比较”窗口的表中，单击一个行。  
  
    详细信息窗格将显示所单击的数据库对象中的记录的结果。 记录在选项卡上按状态分组，可将其用来指定将从源传播到目标的数据。  
  
3.  在详细信息窗格中，单击名称包含非零 (0) 数字的选项卡。  
  
    “只在目标中”表的“更新”列包含可用于选择要更新的行的复选框。 默认情况下，每个复选框均处于选中状态。  
  
4.  清除不想使用源数据更新的目标记录所对应的复选框。  
  
    清除复选框后将减少要更新的记录数，并且显示内容将发生更改以反映您的操作。 此数字显示在详细信息窗格的状态行和主结果窗格中的对应列中，如步骤 1 中所述。  
  
5.  （可选）单击“生成脚本”。  
  
    Transact\-SQL 编辑器窗口随即打开，并显示将用于更新目标的数据操作语言 (DML) 脚本。  
  
6.  若要同步不同的、缺少的或新的记录，请单击“更新目标”。  
  
    > [!NOTE]  
    > 更新目标数据库时，可以通过单击“停止写入目标”来取消该操作。  
  
    将使用源中对应记录的数据更新目标中选定记录的数据。  
  
    > [!NOTE]  
    > 如果选择更新索引视图，则“更新目标”操作可能会失败（如果此操作导致在同一个表中插入重复的键）。  
  
#### <a name="to-update-target-data-by-using-a-transact-sql-script"></a>使用 Transact\-SQL 脚本更新目标数据  
  
1.  比较源数据库和目标数据库中的数据。 有关详细信息，请参阅[比较数据库数据](#CompareDatabaseData)。  
  
    完成比较后，“数据比较”窗口将列出已比较的对象。 有关详细信息，请参阅[将一个或多个表中的数据与引用数据库中的数据进行比较和同步](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md)。  
  
2.  （可选）在详细信息窗格中，清除目标中您不希望更新的记录所对应的复选框，如上一过程中所述。  
  
3.  单击“生成脚本”。  
  
    一个新窗口随即出现，其中显示Transact\-SQL 脚本，该脚本将传播使目标中的数据与源中的数据匹配所必需进行的更改。 将为该新窗口指定一个名称，如 DataUpdate_Database_1.sql。  
  
    该脚本反映您在详细信息窗格中所做的更改。 例如，您可能清除了 [dbo].[Shippers] 表的“只在目标中”页中某个给定行的复选框。 在此情况下，该脚本将不会更新该行。  
  
4.  （可选）在“DataUpdate_Database_1.sql”窗口中编辑该脚本。  
  
5.  （可选，但建议这样做）备份目标数据库。  
  
6.  单击“执行”以更新目标数据库。  
  
    指定要更新的目标数据库的连接。  
  
    > [!IMPORTANT]  
    > 默认情况下，更新将在事务范围内进行。 如果出现错误，您可以回滚整个更新。 可以更改此行为。  
  
    将使用源中对应记录的数据更新目标中选定记录的数据。  
  
## <a name="see-also"></a>另请参阅  
[将一个或多个表中的数据与引用数据库中的数据进行比较和同步](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md)  
