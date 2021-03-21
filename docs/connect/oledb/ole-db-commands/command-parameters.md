---
title: 命令参数（OLE DB 驱动程序）| Microsoft Docs
description: 了解命令参数，包括 OLE DB Driver for SQL Server 支持 SQL 语句和过程调用命令的类型。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- parameters [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, parameters
- OLE DB Driver for SQL Server, commands
- parameters [OLE DB Driver for SQL Server], OLE DB
- commands [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a36e123a61cc31e80eb667aab51082870f7970de
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737837"
---
# <a name="command-parameters"></a>命令参数
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  在命令文本中参数用问号字符标记。 例如，以下 SQL 语句标记了单个输入参数：  
  
```  
{call SalesByCategory('Produce', ?)}  
```  
  
 为减少网络流量以提高性能，适用于 SQL Server 的 OLE DB 驱动程序不自动获取参数信息，除非在执行命令之前调用了 ICommandWithParameters::GetParameterInfo 或 ICommandPrepare::Prepare   。 这意味着 OLE DB Driver for SQL Server 不会自动执行以下操作：  
  
-   验证使用 ICommandWithParameters::SetParameterInfo 指定的数据类型的正确性  。  
  
-   将取值函数绑定信息中指定的 DBTYPE 映射到参数的正确 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。  
  
 如果这两个方法指定的数据类型与参数的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型不兼容，那么应用程序在使用该方法时将可能收到错误或产生精度损失。  
  
 若要确保不发生这种情况，应用程序应当：  
  
-   如果硬编码 ICommandWithParameters::SetParameterInfo，则应确保 pwszDataSourceType 与参数的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型匹配。  
  
-   如果硬编码取值函数，则应确保绑定到参数的 DBTYPE 值与参数的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型具有相同类型。  
  
-   对应用程序进行编码以调用 ICommandWithParameters::GetParameterInfo，以便访问接口可以动态获取参数的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。 请注意，这会导致与服务器之间额外的网络往返。  
  
> [!NOTE]  
>  对于包含 FROM 子句的任意 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] UPDATE 或 DELETE 语句，对于依赖于包含参数的子查询的任意 SQL 语句，对于在比较和类似表达式中包含参数标记或包含限定谓词的 SQL 语句，或其参数之一为函数参数的查询，访问接口不支持调用 ICommandWithParameters::GetParameterInfo。 在对 SQL 语句进行批处理时，对于批处理中第一个语句后的语句中的参数标记，访问接口也不支持调用 ICommandWithParameters::GetParameterInfo  。 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 命令中不允许使用注释 (/* \*/)。  
  
 OLE DB Driver for SQL Server 支持 SQL 语句命令中的输入参数。 在过程调用命令中，OLE DB Driver for SQL Server 支持输入、输出以及输入/输出参数。 输出参数值在运行时（仅当没有行集返回时）或当应用程序用尽返回的所有行集时，返回到应用程序。 若要确保返回值有效，可使用 IMultipleResults 强制使用行集  。  
  
 在 DBPARAMBINDINFO 结构中无需指定存储过程参数的名称。 pwszName 成员值可使用 NULL，以指示适用于 SQL Server 的 OLE DB 驱动程序应忽视参数名称，并只使用在 ICommandWithParameters::SetParameterInfo 的 rgParamOrdinals 成员中指定的序号    。 如果命令文本中既包含命名参数又包含未命名参数，则必须在所有命名参数之前指定所有未命名参数。  
  
 如果指定了存储过程参数的名称，则适用于 SQL Server 的 OLE DB 驱动程序会检查该名称以确保其有效。 如果 OLE DB Driver for SQL Server 从使用者处收到错误的参数名称，则会返回一个错误。  
  
> [!NOTE]  
>  为公开对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] XML 和用户定义类型 (UDT) 的支持，适用于 SQL Server 的 OLE DB 驱动程序实现了新的 [ISSCommandWithParameters](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md) 接口。  
  
## <a name="see-also"></a>另请参阅  
 [命令](../../oledb/ole-db-commands/commands.md)  
  
  
