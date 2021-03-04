---
title: 使用 SQL ServerPowerShell 路径
description: 了解如何使用 cmdlet 或提供程序路径标识的对象方法和属性来操作和检索信息。
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 0b6878c7e0e6435bbb763629547437e9c22b062e
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838596"
---
# <a name="work-with-sql-server-powershell-paths"></a>使用 SQL ServerPowerShell 路径

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

在您导航到 [!INCLUDE[ssDE](../includes/ssde-md.md)] 提供程序路径中的某一节点后，可通过使用与该节点相关联的 [!INCLUDE[ssDE](../includes/ssde-md.md)] 管理对象中的方法和属性，执行工作或检索信息。  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

在导航到 [!INCLUDE[ssDE](../includes/ssde-md.md)] 提供程序路径中的节点之后，可以执行两种类型的操作：  

- 可以运行作用于节点的 Windows PowerShell cmdlet，如 **Rename-Item**。  

- 可以调用相关联的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 管理对象模型中的方法，如 SMO。 例如，如果你导航到路径中的 Databases 节点，则可以使用 <xref:Microsoft.SqlServer.Management.Smo.Database> 类的方法和属性。  

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供程序用于管理 [!INCLUDE[ssDE](../includes/ssde-md.md)]实例中的对象， 而不能用于处理数据库中的数据。 如果您已经导航到表或视图，则不能使用该提供程序选择、插入、更新或删除数据。 可以使用 **Invoke-Sqlcmd** cmdlet 来查询或更改 Windows PowerShell 环境内表和视图中的数据。 有关详细信息，请参阅 [Invoke-Sqlcmd cmdlet](/powershell/module/sqlserver/invoke-sqlcmd)。  

##  <a name="listing-methods-and-properties"></a><a name="ListPropMeth"></a> 列出方法和属性  

**列出方法和属性**  

若要查看可供特定对象或对象类使用的方法和属性，请使用 **Get-Member** cmdlet。  

### <a name="examples-listing-methods-and-properties"></a>示例：列出方法和属性

下面的示例将 Windows PowerShell 变量设置为 SMO <xref:Microsoft.SqlServer.Management.Smo.Database> 类并列出其方法和属性：  

```powershell
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar | Get-Member -Type Methods  
$MyDBVar | Get-Member -Type Properties  
```  
  
 还可以使用 **Get-Member** 列出与 Windows PowerShell 路径的结束节点相关联的方法和属性。  
  
 下面的示例导航到 SQLSERVER: 路径中的 Databases 节点，并列出集合属性：  
  
```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
Get-Item . | Get-Member -Type Properties  
```  
  
 下面的示例导航到 SQLSERVER: 路径中的 AdventureWorks2012 节点，并列出对象属性：  
  
```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012  
Get-Item . | Get-Member -Type Properties  
```  

##  <a name="using-methods-and-properties"></a><a name="UsePropMeth"></a> 使用方法和属性  

**使用 SMO 方法和属性**  

若要从 [!INCLUDE[ssDE](../includes/ssde-md.md)] 提供程序路径对对象执行操作，您可以使用 SMO 方法和属性。  

### <a name="examples-using-methods-and-properties"></a>示例：使用方法和属性

下面的示例使用 SMO **Schema** 属性，从 AdventureWorks2012 中的 Sales 架构中获取表的列表：  

```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Tables  
Get-ChildItem | where {$_.Schema -eq "Sales"}  
```

此示例使用 SMO **Script** 方法生成一个包含 **CREATE VIEW** 语句的脚本，你必须使用该脚本在 AdventureWorks2012 中重新创建视图：  

```powershell
Remove-Item C:\PowerShell\CreateViews.sql  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Views  
foreach ($Item in Get-ChildItem) { $Item.Script() | Out-File -Filepath C:\PowerShell\CreateViews.sql -append }  
```

下面的示例使用 SMO **Create** 方法创建一个数据库，然后使用 **State** 属性显示该数据库是否存在：  

```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar.Parent = (Get-Item ..)  
$MyDBVar.Name = "NewDB"  
$MyDBVar.Create()  
$MyDBVar.State  
```

## <a name="see-also"></a>另请参阅

- [SQL Server PowerShell 提供程序](sql-server-powershell-provider.md)
- [导航 SQL Server PowerShell 路径](navigate-sql-server-powershell-paths.md)
- [将 URN 转换为 SQL Server 提供程序路径](/powershell/module/sqlserver/Convert-UrnToPath)
- [SQL Server PowerShell](sql-server-powershell.md)