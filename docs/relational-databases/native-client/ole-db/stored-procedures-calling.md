---
description: 存储过程-调用 SQL Server Native Client
title: 调用存储过程 (OLE DB) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- calling stored procedures
- RPC escape sequence
- OLE DB, stored procedures
- parameters [SQL Server Native Client], OLE DB
- ODBC CALL escape sequence
- stored procedures [OLE DB], calling
- SQL Server Native Client OLE DB provider, stored procedures
ms.assetid: 8e5738e5-4bbe-4f34-bd69-0c0633290bdd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9e640cfb186e29f73f11f48cbd34a9cc4f7866e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744837"
---
# <a name="stored-procedures---calling-in-sql-server-native-client"></a>存储过程-调用 SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  存储过程可以有零个或多个参数。 它还可以返回值。 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序时，可以通过以下方式传递存储过程的参数：  
  
-   对数据值进行硬编码。  
  
-   使用参数标记 (?) 指定参数，将程序变量绑定到参数标记，然后将数据值放在程序变量中。  
  
> [!NOTE]  
>  在使用 OLE DB 和命名参数调用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 存储过程时，参数名称必须以“\@”字符开头。 这是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 特有的限制。 与 MDAC 相比，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口会更加严格地强制实施此限制。  
  
 为了支持参数，ICommandWithParameters 接口是对命令对象公开的  。 若要使用参数，使用者首先应通过调用 ICommandWithParameters::SetParameterInfo 方法（或者根据需要准备一个调用 GetParameterInfo 方法的调用语句）来向访问接口描述这些参数   。 然后，使用者创建一个指定缓冲区结构的取值函数并将参数值放入该缓冲区中。 最后，它将取值函数的句柄和指向此缓冲区的指针传递给 Execute  。 以后在调用 Execute 时，使用者将新参数值放入此缓冲区并使用取值函数句柄和缓冲区指针调用 Execute   。  
  
 在能成功地准备使用参数调用临时存储过程的命令之前，该命令必须首先调用 ICommandWithParameters::SetParameterInfo 以定义参数信息  。 这是因为临时存储过程的内部名称与客户端使用的外部名称不同，并且 SQLOLEDB 无法通过查询系统表来确定临时存储过程的参数信息。  
  
 以下是参数绑定过程涉及的步骤：  
  
1.  在 DBPARAMBINDINFO 结构的数组中填写参数信息，即，参数名、参数数据类型特定于访问接口的名称或者标准数据类型名称，等等。 数组中的每个结构都描述一个参数。 然后将此数组传递给 SetParameterInfo 方法  。  
  
2.  调用 ICommandWithParameters::SetParameterInfo 方法以向访问接口描述参数  。 SetParameterInfo 指定每个参数的本机数据类型  。 SetParameterInfo 参数包括  ：  
  
    -   要设定其类型信息的参数的数量。  
  
    -   要设定其类型信息的参数序号的数组。  
  
    -   DBPARAMBINDINFO 结构的数组。  
  
3.  通过使用 IAccessor::CreateAccessor 命令创建参数取值函数  。 取值函数指定缓冲区的结构并将参数值放在此缓冲区中。 CreateAccessor 命令从一组绑定创建取值函数  。 使用者使用 DBBINDING 结构数组来描述这些绑定。 每个绑定都将单个参数关联到使用者的缓冲区，并且包含诸如以下所示的信息：  
  
    -   应用绑定的参数的序号。  
  
    -   所绑定的内容（数据值、其长度以及其状态）。  
  
    -   缓冲区中到这些部分中每个部分的偏移量。  
  
    -   数据值存在于使用者的缓冲区中时的长度和类型。  
  
     取值函数通过其类型为 HACCESSOR 的句柄进行标识。 此句柄由 CreateAccessor 方法返回  。 只要使用者使用完取值函数，使用者就必须调用 ReleaseAccessor 方法释放它占用的内存  。  
  
     当使用者调用某个方法（如 ICommand::Execute）时，会传递取值函数的句柄以及指向缓冲区自身的指针  。 访问接口使用该取值函数确定如何传送缓冲区中包含的数据。  
  
4.  填写 DBPARAMS 结构。 在运行时，从中获取输入参数值和向其中写入输出值的使用者变量会在 DBPARAMS 结构中被传递给 ICommand::Execute  。 DBPARAMS 结构包括三个元素：  
  
    -   指向缓冲区的指针，根据取值函数句柄指定的绑定，访问接口将从该缓冲区中检索输入参数数据并将输出参数数据返回到该缓冲区中。  
  
    -   缓冲区中参数组的数量。  
  
    -   在步骤 3 中创建的取值函数句柄。  
  
5.  通过使用 ICommand::Execute 执行命令  。  

## <a name="methods-of-calling-a-stored-procedure"></a>调用存储过程的方法  
 在中执行存储过程时 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序支持：  
  
-   ODBC CALL 转义序列。  
  
-   远程过程调用 (RPC) 转义序列。  
  
-   [!INCLUDE[tsql](../../../includes/tsql-md.md)] EXECUTE 语句。  
  
### <a name="odbc-call-escape-sequence"></a>ODBC CALL 转义序列  
 如果知道参数信息，可调用 ICommandWithParameters::SetParameterInfo 方法向访问接口描述参数  。 否则，在使用 ODBC CALL 语法调用存储过程时，访问接口将调用一个 Helper 函数来查找存储过程的参数信息。  
  
 如果您不知道确切的参数信息（参数元数据），建议使用 ODBC CALL 语法。  
  
 使用 ODBC CALL 转义序列调用过程的常用语法是：  
  
 {[**？ =**]**调用**_procedure_name_[**(**[*参数*] [**，**[*参数*]] .。。**)**]}  
  
 例如：  
  
```  
{call SalesByCategory('Produce', '1995')}  
```  
  
### <a name="rpc-escape-sequence"></a>RPC 转义序列  
 使用 RPC 转义序列调用存储过程的语法与 ODBC CALL 语法类似。 如果要多次调用过程，则在调用存储过程的三种方法中，RPC 转义序列可以提供最佳的性能。  
  
 如果使用 RPC 转义序列执行存储过程，则访问接口不会调用任何 Helper 函数来确定参数信息（使用 ODBC CALL 语法时则会调用 Helper 函数）。 RPC 语法比 ODBC CALL 语法简单，因此命令的分析速度更快，从而提高了性能。 在这种情况下，需要通过执行 ICommandWithParameters::SetParameterInfo 来提供参数信息  。  
  
 RPC 转义序列要求您具有返回值。 如果存储过程不返回值，则服务器默认返回 0。 此外，您无法对存储过程打开 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 游标。 存储过程是隐式准备的，对 ICommandPrepare::Prepare 的调用会失败  。 由于无法准备 RPC 调用，因此也就无法查询列元数据；IColumnsInfo::GetColumnInfo 和 IColumnsRowset::GetColumnsRowset 会返回 DB_E_NOTPREPARED。  
  
 如果知道所有参数元数据，建议采用 RPC 转义序列方式执行存储过程。  
  
 以下是使用 RPC 转义序列调用存储过程的一个例子：  
  
```  
{rpc SalesByCategory}  
```  
  
 有关展示 RPC 转义序列的示例应用程序，请参阅[执行存储过程（使用 RPC 语法）并处理返回代码和输出参数 &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-how-to/results/execute-stored-procedure-with-rpc-and-process-output.md)。  
  
### <a name="transact-sql-execute-statement"></a>Transact-SQL EXECUTE 语句  
 与 [EXECUTE](../../../t-sql/language-elements/execute-transact-sql.md) 语句相比，ODBC CALL 转义序列和 RPC 转义序列是调用存储过程的首选方法。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序使用的 RPC 机制 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 来优化命令处理。 此 RPC 协议通过避免在服务器上进行大量参数处理和语句分析来提高性能。  
  
 下面是 [!INCLUDE[tsql](../../../includes/tsql-md.md)] EXECUTE  语句示例：  
  
```  
EXECUTE SalesByCategory 'Produce', '1995'  
```  
  
## <a name="see-also"></a>另请参阅  
 [存储过程](../../../relational-databases/native-client/ole-db/stored-procedures.md)  
  
  
