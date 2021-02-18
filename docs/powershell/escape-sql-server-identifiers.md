---
title: 对 SQL Server 标识符进行转义
description: SQL Server 分隔标识符有时候包含 Windows PowerShell 路径中不支持的字符。 了解其中某些字符如何使用反引号字符进行转义。
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 8a73e945-daa6-4e5d-93da-10f000f1f3a2
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 25d6badaab16caef587afc9843ccc220f392acb4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354299"
---
# <a name="escape-sql-server-identifiers"></a>对 SQL Server 标识符进行转义

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

通常，可以使用反引号转义符 (`) 来对 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 分隔标识符中允许使用但是 Windows PowerShell 路径名称中不允许使用的字符进行转义。 但是，对于某些字符，不能对其进行转义。 例如，不能对 Windows PowerShell 中的冒号字符 (:) 进行转义。 必须对包含该字符的标识符进行编码。 由于编码适用于所有字符，因此编码比转义可靠。  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

反引号字符 (`) 键通常位于键盘左上角 ESC 键的下方。  

## <a name="examples"></a>示例

下面是对 # 字符进行转义的示例：  

```powershell
cd SQLSERVER:\SQL\MyComputer\MyInstance\MyDatabase\MySchema\`#MyTempTable  
```

下面是在（本地）指定为计算机名称时对括号进行转义的示例：  

```powershell
Set-Location SQLSERVER:\SQL\`(local`)\DEFAULT  
```

## <a name="see-also"></a>另请参阅

- [PowerShell 中的 SQL Server 标识符](sql-server-identifiers-in-powershell.md)
- [SQL Server PowerShell 提供程序](sql-server-powershell-provider.md)
- [SQL Server PowerShell](sql-server-powershell.md)