---
title: 从 SQL Server Management Studio 中运行 Windows PowerShell
description: 了解如何从 SQL Server Management Studio 中的对象资源管理器启动 Windows PowerShell 会话，并将路径预设为所选对象的位置。
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 9fd9b7039680b05515d1f70102408394b5443c9c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839445"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>从 SQL Server Management Studio 中运行 Windows PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

可以从 SQL Server Management Studio (SSMS) 中的“对象资源管理器”启动 Windows PowerShell 会话。 SSMS 启动 Windows PowerShell，加载 SqlServer 模块，并将路径上下文设置为“对象资源管理器”树中的相关节点。

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

在“对象资源管理器”中为某个对象指定正在运行的 PowerShell 时，SQL Server Management Studio 将启动其中已经加载和注册了 SQL Server PowerShell 管理单元的 Windows PowerShell 会话。 该会话的路径预设为你在对象资源管理器中右键单击的对象位置。

例如，如果你在“对象资源管理器”中右键单击 AdventureWorks 数据库对象并选择“启动 PowerShell”，则 Windows PowerShell 路径将设置为以下内容：

```powershell
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```

## <a name="run-powershell"></a>运行 PowerShell

### <a name="to-run-powershell-from-sql-server-management-studio"></a>从 SQL Server Management Studio 运行 PowerShell

1. 打开 **“对象资源管理器”** 。

2. 导航到要进行处理的对象的节点。

3. 右键单击该对象，然后选择“启动 PowerShell”。

## <a name="permissions"></a>权限

从 SQL Server Management Studio 打开 PowerShell 时，它不会使用管理员权限运行，这可能会阻止某些活动（如调用 WMI）。

## <a name="see-also"></a>另请参阅

- [SQL Server PowerShell](sql-server-powershell.md)