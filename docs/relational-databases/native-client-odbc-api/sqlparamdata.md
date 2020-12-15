---
description: SQLParamData
title: SQLParamData |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLParamData function
ms.assetid: 92349482-ea22-4a6a-8484-e9c6566794fa
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6fc37208fffff63736682414e4537769b588ecbc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485029"
---
# <a name="sqlparamdata"></a>SQLParamData
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  当 SQLParamData 返回与表值参数关联的 *ValuePtrPtr* 时，应用程序应调用 SQLPutData 与 *StrLen_Or_Ind*。 如果 *StrLen_Or_Ind* 的值大于0，则表示应用程序已准备就绪，可供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 收集下一个表值参数行的参数数据。 如果 *StrLen_Or_Ind* 的值为0，则表示表值参数没有更多的数据行。 有关详细信息，请参阅 [Table-Valued 参数和列值的绑定和数据传输](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md)。  
  
 有关表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLParamData](../../odbc/reference/syntax/sqlparamdata-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
