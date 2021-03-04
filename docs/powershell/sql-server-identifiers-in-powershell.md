---
title: PowerShell 中的 SQL Server 标识符
description: 了解 Windows PowerShell 提供程序用于公开数据层次结构的路径，并了解对这些路径中不受 PowerShell 支持的某些字符进行编码的需要。
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
helpviewer_keywords:
- Cmdlets [SQL Server], Encode-Sqlname
- PowerShell [SQL Server], identifiers
- Encode-Sqlname cmdlet
- PowerShell [SQL Server], Encode-Sqlname
- Decode-Sqlname cmdlet
- PowerShell [SQL Server], Decode-Sqlname
- identifiers [SQL Server], PowerShell
- Cmdlets [SQL Server], Decode-Sqlname
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 4940f7ac1b8643eec0223464013cec10c1f90f9f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839418"
---
# <a name="sql-server-identifiers-in-powershell"></a>PowerShell 中的 SQL Server 标识符

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

用于 Windows PowerShell 的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供程序使用 Windows PowerShell 路径中的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 标识符。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 标识符可包含 Windows PowerShell 不支持在路径中使用的字符。 在 Windows PowerShell 路径中使用标识符时，必须对这些字符进行转义或者对它们使用特殊的编码。  
  
[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="sql-server-identifiers-in-windows-powershell-paths"></a>Windows PowerShell 路径中的 SQL Server 标识符  

Windows PowerShell 提供程序使用类似于 Windows 文件系统的路径结构来公开数据层次结构。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供程序实现了 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 对象的路径。 对于 [!INCLUDE[ssDE](../includes/ssde-md.md)]，驱动器设置为 SQLSERVER:，第一个文件夹设置为 \SQL，数据库对象作为容器和项来引用。 这是 Purchasing 架构中 Vendor 表的路径，该架构位于默认 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] 实例的 [!INCLUDE[ssDE](../includes/ssde-md.md)]数据库中：  
  
```powershell
SQLSERVER:\SQL\MyComputer\DEFAULT\Databases\AdventureWorks2012\Tables\Purchasing.Vendor  
```  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 标识符是 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 对象的名称，如表名或列名。 共有两种类型的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 标识符：  
  
-   常规标识符限制为一组在 Windows PowerShell 路径中同样受到支持的字符。 无需更改这些名称，即可在 Windows PowerShell 路径中使用它们。  
  
-   分隔标识符可以使用 Windows PowerShell 路径名称中不支持的字符。 如果用中括号将分隔标识符括起来 ([IdentifierName])，则可以将它们称为带中括号的标识符；如果将它们用双引号引起来，则可以将它们称为带引号的标识符 ("IdentifierName")。 如果分隔标识符使用 Windows PowerShell 路径中不支持的字符，那么，必须先对这些字符进行编码或转义，才能将标识符用作容器名称或项名称。 可以针对所有字符进行编码。 而某些字符（如冒号字符 (:)）则不能进行转义。  
  
## <a name="sql-server-identifiers-in-cmdlets"></a>cmdlet 中的 SQL Server 标识符

 某些 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] cmdlet 具有一个将标识符作为输入的参数。 参数值通常以带引号的字符串常量或以字符串变量形式提供。 如果标识符以字符串常量或变量形式提供，则不会与 Windows PowerShell 支持的字符集发生冲突。  

## <a name="sql-server-identifier-tasks"></a>SQL Server 标识符任务  

|任务说明|项目|  
|----------------------|-----------|  
|介绍如何指定一个实例名称，包括运行该实例的计算机的名称。|[在 SQL Server PowerShell 提供程序中指定实例](specify-instances-in-the-sql-server-powershell-provider.md)|  
|介绍如何为 Windows PowerShell 路径中不支持的分隔标识符中的字符指定十六进制编码。 此外还介绍了如何解码十六进制字符。|[对 SQL Server 标识符进行编码和解码](encode-and-decode-sql-server-identifiers.md)|  
|介绍如何为 PowerShell 路径中不支持的字符使用 Windows PowerShell 转义符。|[对 SQL Server 标识符进行转义](escape-sql-server-identifiers.md)|  

## <a name="see-also"></a>另请参阅

- [SQL Server PowerShell 提供程序](sql-server-powershell-provider.md)
- [SQL Server PowerShell](sql-server-powershell.md)
- [数据库标识符](../relational-databases/databases/database-identifiers.md)