---
title: 在 Windows PowerShell 中加载 SMO 程序集
description: 了解如何在不使用 SQL Server PowerShell 提供程序的 Windows PowerShell 脚本中加载 SQL Server 管理对象 (SMO) 程序集。
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 81fb0cc3452cc04228e1f0e34f5be94f6cefd7af
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838058"
---
# <a name="load-the-smo-assemblies-in-windows-powershell"></a>在 Windows PowerShell 中加载 SMO 程序集

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本文介绍如何在不使用 SQL Server PowerShell 提供程序的 Windows PowerShell 脚本中加载 SQL Server 管理对象 (SMO) 程序集。  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

加载 SMO 程序集的首选机制是加载 SqlServer 模块。 该模块中包括的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供程序将自动加载 SMO 程序集，还会实现可扩展 PowerShell 脚本中的 SMO 对象的有用性的功能。

有两种情况可能需要您直接加载 SMO 程序集：  

- 脚本在从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 管理单元引用该提供程序或 cmdlet 的第一个命令之前引用了一个 SMO 对象。  

- 你需要从不使用提供程序或 cmdlet 的另一种语言（例如 C# 或 Visual Basic）移植 SMO 代码。  

## <a name="example-loading-the-sql-server-management-objects"></a>示例：加载 SQL Server 管理对象

下面的代码加载 SMO 程序集：  

```powershell
# Loads the SQL Server Management Objects (SMO)  

$ErrorActionPreference = "Stop"
  
$sqlpsreg="HKLM:\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.SqlServer.Management.PowerShell.sqlps"  
  
if (Get-ChildItem $sqlpsreg -ErrorAction "SilentlyContinue")  
{  
    throw "SQL Server Provider for Windows PowerShell is not installed."  
}  
else  
{  
    $item = Get-ItemProperty $sqlpsreg  
    $sqlpsPath = [System.IO.Path]::GetDirectoryName($item.Path)  
}  
  
$assemblylist =
"Microsoft.SqlServer.Management.Common",  
"Microsoft.SqlServer.Smo",  
"Microsoft.SqlServer.Dmf ",  
"Microsoft.SqlServer.Instapi ",  
"Microsoft.SqlServer.SqlWmiManagement ",  
"Microsoft.SqlServer.ConnectionInfo ",  
"Microsoft.SqlServer.SmoExtended ",  
"Microsoft.SqlServer.SqlTDiagM ",  
"Microsoft.SqlServer.SString ",  
"Microsoft.SqlServer.Management.RegisteredServers ",  
"Microsoft.SqlServer.Management.Sdk.Sfc ",  
"Microsoft.SqlServer.SqlEnum ",  
"Microsoft.SqlServer.RegSvrEnum ",  
"Microsoft.SqlServer.WmiEnum ",  
"Microsoft.SqlServer.ServiceBrokerEnum ",  
"Microsoft.SqlServer.ConnectionInfoExtended ",  
"Microsoft.SqlServer.Management.Collector ",  
"Microsoft.SqlServer.Management.CollectorEnum",  
"Microsoft.SqlServer.Management.Dac",  
"Microsoft.SqlServer.Management.DacEnum",  
"Microsoft.SqlServer.Management.Utility"  
  
foreach ($asm in $assemblylist)  
{  
    $asm = [Reflection.Assembly]::LoadWithPartialName($asm)  
}  
  
Push-Location  
cd $sqlpsPath  
update-FormatData -prependpath SQLProvider.Format.ps1xml
Pop-Location  
```

## <a name="see-also"></a>另请参阅

- [SQL Server PowerShell](sql-server-powershell.md)