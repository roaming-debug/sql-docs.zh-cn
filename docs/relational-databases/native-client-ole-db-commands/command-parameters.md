---
description: '命令参数 (Native Client OLE DB 提供程序) '
title: 命令参数 (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- parameters [SQL Server Native Client]
- SQL Server Native Client OLE DB provider, parameters
- SQL Server Native Client OLE DB provider, commands
- parameters [SQL Server Native Client], OLE DB
- commands [OLE DB]
ms.assetid: 072ead49-ebaf-41eb-9a0f-613e9d990f26
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1777265db1c1f32d4c72ee91d6af7e8ac73d7221
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439916"
---
# <a name="sql-server-native-client-command-parameters"></a>SQL Server Native Client 命令参数
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在命令文本中参数用问号字符标记。 例如，以下 SQL 语句标记了单个输入参数：  
  
```  
{call SalesByCategory('Produce', ?)}  
```  
  
 为了通过减少网络流量提高性能， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 除非在执行命令之前调用 **ICommandWithParameters：： GetParameterInfo** 或 **ICommandPrepare：:P 准备** ，否则 Native Client OLE DB 提供程序不会自动派生参数信息。 这意味着 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序不会自动执行以下操作：  
  
-   验证使用 ICommandWithParameters::SetParameterInfo 指定的数据类型的正确性  。  
  
-   将取值函数绑定信息中指定的 DBTYPE 映射到参数的正确 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。  
  
 如果这两个方法指定的数据类型与参数的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型不兼容，那么应用程序在使用该方法时将可能收到错误或产生精度损失。  
  
 若要确保不发生这种情况，应用程序应当：  
  
-   如果硬编码 ICommandWithParameters::SetParameterInfo，则应确保 pwszDataSourceType 与参数的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型匹配。  
  
-   如果硬编码取值函数，则应确保绑定到参数的 DBTYPE 值与参数的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型具有相同类型。  
  
-   对应用程序进行编码以调用 ICommandWithParameters::GetParameterInfo，以便访问接口可以动态获取参数的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。 请注意，这会导致与服务器之间额外的网络往返。  
  
> [!NOTE]  
>  对于包含 FROM 子句的任意 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] UPDATE 或 DELETE 语句，对于依赖于包含参数的子查询的任意 SQL 语句，对于在比较和类似表达式中包含参数标记或包含限定谓词的 SQL 语句，或其参数之一为函数参数的查询，访问接口不支持调用 ICommandWithParameters::GetParameterInfo。 在对 SQL 语句进行批处理时，对于批处理中第一个语句后的语句中的参数标记，访问接口也不支持调用 ICommandWithParameters::GetParameterInfo  。 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令中不允许使用注释 (/* \*/)。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序支持 SQL 语句命令中的输入参数。 在过程调用命令中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序支持输入参数、输出参数和输入/输出参数。 输出参数值在运行时（仅当没有行集返回时）或当应用程序用尽返回的所有行集时，返回到应用程序。 若要确保返回值有效，可使用 IMultipleResults 强制使用行集  。  
  
 在 DBPARAMBINDINFO 结构中无需指定存储过程参数的名称。 使用 NULL 作为 *pwszName* 成员的值，以指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序应忽略参数名，并且仅使用 **ICommandWithParameters：： SetParameterInfo** 的 *rgParamOrdinals* 成员中指定的序号。 如果命令文本中既包含命名参数又包含未命名参数，则必须在所有命名参数之前指定所有未命名参数。  
  
 如果指定了存储过程参数的名称，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将检查该名称，以确保该名称有效。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]当 Native Client OLE DB 提供程序从使用者接收到错误的参数名称时，它将返回错误。  
  
> [!NOTE]  
>  为了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (UDT) 公开对 XML 和用户定义类型的支持， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序实现新的 [ISSCommandWithParameters](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md) 接口。  
  
## <a name="see-also"></a>另请参阅  
 [命令](../../relational-databases/native-client-ole-db-commands/commands.md)  
  
  
