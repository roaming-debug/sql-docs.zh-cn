---
description: 创建和更新统计信息
title: 创建和更新统计信息
ms.prod: sql
ms.prod_service: database-engine
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- statistical information [SMO]
ms.assetid: 47a0a172-a969-4deb-bca9-dd04401a0fe1
author: markingmyname
ms.author: maghan
ms.reviewer: matteot
ms.custom: seo-dt-2019
ms.date: 06/04/2020
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 64b631219afcd37bbc6883ef7e1c89d6335b7095
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475528"
---
# <a name="create-and-update-statistics"></a>创建和更新统计信息

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

在 SMO 中，有关处理数据库中的查询的统计信息可以使用 <xref:Microsoft.SqlServer.Management.Smo.Statistic> 对象进行收集。

使用和对象可以为任何列创建统计信息 <xref:Microsoft.SqlServer.Management.Smo.Statistic> <xref:Microsoft.SqlServer.Management.Smo.StatisticColumn> 。 可以运行 <xref:Microsoft.SqlServer.Management.Smo.Statistic.Update%2A> 方法以更新 <xref:Microsoft.SqlServer.Management.Smo.Statistic> 对象中的统计信息。 可以在查询优化器中查看结果。

## <a name="example"></a>示例

若要使用所提供的任何代码示例，可以选择编程环境、编程模板和创建应用程序的编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。

## <a name="create-and-update-statistics-in-visual-basic"></a>在 Visual Basic 中创建和更新统计信息

此代码示例将对为其创建 <xref:Microsoft.SqlServer.Management.Smo.Statistic> 对象和 <xref:Microsoft.SqlServer.Management.Smo.StatisticColumn> 对象的现有数据库创建新表。

```vb
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference the AdventureWorks2012 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Reference the CreditCard table.
Dim tb As Table
tb = db.Tables("CreditCard", "Sales")
'Define a Statistic object by supplying the parent table and name arguments in the constructor.
Dim stat As Statistic
stat = New Statistic(tb, "Test_Statistics")
'Define a StatisticColumn object variable for the CardType column and add to the Statistic object variable.
Dim statcol As StatisticColumn
statcol = New StatisticColumn(stat, "CardType")
stat.StatisticColumns.Add(statcol)
'Create the statistic counter on the instance of SQL Server.
stat.Create()
```

## <a name="create-and-update-statistics-in-c"></a>在 C 中创建和更新统计信息#

此代码示例将对为其创建 <xref:Microsoft.SqlServer.Management.Smo.Statistic> 对象和 <xref:Microsoft.SqlServer.Management.Smo.StatisticColumn> 对象的现有数据库创建新表。

```csharp
public static void CreatingAndUpdatingStatistics()
{
    // Connect to the local, default instance of SQL Server.
    var srv = new Server();

    // Reference the AdventureWorks2012 database.
    var db = srv.Databases["AdventureWorks"];

    // Reference the CreditCard table.
    var tb = db.Tables["CreditCard", "Sales"];

    // Define a Statistic object by supplying the parent table and name
    // arguments in the constructor.
    var stat = new Statistic(tb, "Test_Statistics");

    // Define a StatisticColumn object variable for the CardType column
    // and add to the Statistic object variable.
    var statcol = new StatisticColumn(stat, "CardType");
    stat.StatisticColumns.Add(statcol);

    //Create the statistic counter on the instance of SQL Server.
    stat.Create();

    // List all the statistics object on the table (you will see the newly created one)
    foreach (var s in tb.Statistics.Cast<Statistic>())
        Console.WriteLine($"{s.ID}\t{s.Name}");

    // Output:
    //  2       AK_CreditCard_CardNumber
    //  1       PK_CreditCard_CreditCardID
    //  3       Test_Statistics
 }
```

## <a name="create-and-update-statistics-in-powershell"></a>在 PowerShell 中创建和更新统计信息

此代码示例将对为其创建 <xref:Microsoft.SqlServer.Management.Smo.Statistic> 对象和 <xref:Microsoft.SqlServer.Management.Smo.StatisticColumn> 对象的现有数据库创建新表。

```powershell
Import-Module SQLServer

# Connect to the local, default instance of SQL Server.  
$srv = Get-Item SQLSERVER:\SQL\localhost\DEFAULT

# Reference the AdventureWorks database.
$db = $srv.Databases["AdventureWorks"]

# Reference the CreditCard table.
$tb = $db.Tables["CreditCard", "Sales"]

# Define a Statistic object by supplying the parent table and name
# arguments in the constructor.
$stat = New-Object Microsoft.SqlServer.Management.Smo.Statistic($tb, "Test_Statistics")

# Define a StatisticColumn object variable for the CardType column
# and add to the Statistic object variable.
$statcol = New-Object Microsoft.SqlServer.Management.Smo.StatisticColumn($stat, "CardType")
$stat.StatisticColumns.Add($statcol)

# Create the statistic counter on the instance of SQL Server.
$stat.Create()

# Finally dump all the statistics (you can see the newly created one at the bottom)
$tb.Statistics

# Output:
# Name                                Last Updated Is From Index  Statistic Columns
#                                                  Creation
# ----                                ------------ -------------- -----------------
# AK_CreditCard_CardNumber      10/27/2017 2:33 PM True           {CardNumber}
# PK_CreditCard_CreditCardID    10/27/2017 2:33 PM True           {CreditCardID}
# Test_Statistics                 6/4/2020 8:11 PM False          {CardType}
```
