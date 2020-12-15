---
description: 提取结果数据
title: 提取结果数据 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLFetchScroll function
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- data types [ODBC], fetching
- SQLBindCol function
- result sets [ODBC], fetching
- fetching [ODBC]
- ODBC data types, fetching
- SQLFetch function
- SQL Server Native Client ODBC driver, data types
- SQLGetData function
ms.assetid: b289c7fb-5017-4d7e-a2d3-19401e9fc4cd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 97781af1d4bc7dbde4c26f6a64b02ea3024c42aa
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438248"
---
# <a name="fetching-result-data"></a>提取结果数据
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 应用程序具有三个用于提取结果数据的选项。  
  
 第一个选项基于 [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md)。 在提取结果集之前，应用程序使用 **SQLBindCol** 将结果集中的每列绑定到程序变量。 绑定列后，驱动程序会在每次应用程序调用 **SQLFetch** 或 [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)时，将当前行的数据传输到绑定到结果集列的变量中。 如果结果集列和程序变量具有不同的数据类型，则驱动程序将处理数据转换。 如果应用程序 SQL_ATTR_ROW_ARRAY_SIZE 设置为大于1，则它可以将结果列绑定到变量数组，这将在每次调用 **SQLFetchScroll** 时都进行填充。  
  
 第二个选项基于 [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md)。 应用程序不使用 **SQLBindCol** 将结果集列绑定到程序变量。 每次调用 **SQLFetch** 后，应用程序将为结果集中的每一列调用 **SQLGetData** 一次。 **SQLGetData** 指示驱动程序将数据从特定的结果集列传输到特定的程序变量，并指定列和变量的数据类型。 如果结果集列和程序变量具有不同的数据类型，则上述操作将允许驱动程序转换数据。 **Text**、 **ntext** 和 **image** 列通常太大，无法放入程序变量中，但仍可使用 **SQLGetData** 进行检索。 如果 "结果" 列中的 **text**、 **ntext** 或 **image** 数据大于程序变量，则 **SQLGetData** 将返回 SQL_SUCCESS_WITH_INFO 和 SQLSTATE 01004 (字符串数据，并在) 右截断。 对 **SQLGetData** 的后续调用返回 **文本** 或 **图像** 数据的连续块。 在到达数据末尾后， **SQLGetData** 将返回 SQL_SUCCESS。 如果 SQL_ATTR_ROW_ARRAY_SIZE 大于 1，则每个提取都将返回一组行或行集。 使用 **SQLGetData** 之前，必须先使用 **SQLSetPos** 将行集中的特定行指定为当前行。  
  
 第三种方法是使用 **SQLBindCol** 和 **SQLGetData** 的混合。 例如，应用程序可以绑定一个结果集的前10列，然后在每次提取时调用 **SQLGetData** 三次，以从三个未绑定的列中检索数据。 当结果集包含一个或多个 **text** 或 **image** 列时，通常使用此方法。  
  
 根据为结果集设置的游标选项，应用程序还可以使用 **SQLFetchScroll** 的滚动选项在结果集的周围滚动。  
  
 过度使用 **SQLBindCol** 将结果集列绑定到程序变量代价高昂，因为 **SQLBINDCOL** 会导致 ODBC 驱动程序分配内存。 将结果列绑定到变量时，该绑定始终有效，直到调用 [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) 释放语句句柄，或调用 [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) 并将 *fOption* 设置为 SQL_UNBIND。 在该语句完成前绑定不自动撤消。  
  
 通过这一逻辑，您可以高效地处理使用不同参数多次执行同一 SELECT 语句的情况。 由于结果集保留相同的结构，因此你可以将结果集绑定一次，处理所有 SELECT 语句，然后调用 **SQLFreeStmt** ，并将 *fOption* 设置为在上次执行后 SQL_UNBIND。 不应调用 **SQLBindCol** 来绑定结果集中的列，而无需先调用 **SQLFreeStmt** 并将 *fOption* 设置为 SQL_UNBIND，以释放以前的任何绑定。  
  
 当使用 **SQLBindCol** 时，可以执行按行绑定或按列绑定。 按行绑定比按列绑定稍快。  
  
 您可以使用 **SQLGetData** 来逐列检索数据，而不是使用 **SQLBindCol** 绑定结果集列。 如果结果集只包含几行，则使用 **SQLGetData** 而不是 **SQLBindCol** 的速度更快;否则， **SQLBindCol** 可提供最佳性能。 如果不始终将数据置于同一组变量中，则应使用 **SQLGetData** ，而不是经常重新绑定。 在所有列都与 **SQLBindCol** 绑定后，只能对选择列表中的列使用 **SQLGetData** 。 列还必须出现在已使用 **SQLGetData** 的任何列之后。  
  
 用于处理将数据移入或移出程序变量的 ODBC 函数（如 **SQLGetData**、 **SQLBindCol** 和 [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md)）支持隐式数据类型转换。 例如，如果应用程序将某一整数列绑定到某一字符串程序变量，则驱动程序将会首先自动把数据从整数转换为字符，然后将其置于该程序变量中。  
  
 应用程序中的数据转换应尽可能少。 如果对应用程序执行的处理不要求数据转换，则应用程序应将列和参数绑定到同一数据类型的程序变量。 但是，如果数据必须从一种类型转换为另一种类型，让驱动程序执行转换比在应用程序中执行转换的效率更高。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序通常只将数据直接从网络缓冲区传输到应用程序的变量。 请求驱动程序执行数据转换将强制驱动程序把数据存入缓冲区并使用 CPU 周期来转换数据。  
  
 除了 **text**、 **ntext** 和 **image** 数据之外，程序变量还应足够大以保存从列传入的数据。 如果某一应用程序尝试检索结果集数据，而放置这些数据的变量太小以致无法保存数据，则驱动程序将生成一个警告。 这强制驱动程序为消息分配内存，并且驱动程序和应用程序都必须将 CPU 周期花在处理消息和执行错误处理上。 应用程序应或者分配在大小上足以存放检索的数据的变量，或者在选择列表中使用 SUBSTRING 函数减小结果集中列的大小。  
  
 在使用 SQL_C_DEFAULT 指定 C 变量的类型时必须小心。 SQL_C_DEFAULT 指定 C 变量的类型与列或参数的 SQL 数据类型匹配。 如果为 **ntext**、 **nchar** 或 **nvarchar** 列指定了 SQL_C_DEFAULT，则会将 Unicode 数据返回到应用程序。 如果尚未对应用程序进行编码以处理 Unicode 数据，则上述操作可能导致不同的问题。 SQL_GUID) 数据类型的 **uniqueidentifier** (会出现相同的问题类型。  
  
 **text**、 **ntext** 和 **image** 数据通常太大，无法放入单个程序变量，通常使用 **SQLGetData** 而不是 **SQLBindCol** 进行处理。 使用服务器游标时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序会经过优化，以便在提取行时不传输未绑定的 **text**、 **ntext** 或 **image** 列的数据。 在应用程序为列发出 **SQLGetData** 之前，不会实际从服务器中检索 **text**、 **ntext** 或 **image** 数据。  
  
 此优化可应用于应用程序，以便用户在滚动和向下滚动时不显示 **text**、 **ntext** 或 **image** 数据。 用户选择行后，应用程序可以调用 **SQLGetData** 来检索 **text**、 **ntext** 或 **image** 数据。 这会为用户不选择的任何行保存 **文本**、 **ntext** 或 **图像** 数据的传输，并且可以保存非常大的数据量。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;处理结果 ](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
