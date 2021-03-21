---
description: 准备的语句的表值参数元数据
title: TVP 预定义语句的元数据
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), metadata for prepared statements
ms.assetid: fd2fc705-2e98-4011-9822-c7e6cca4a535
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 995655e5764c3aada26317a9ef2c3acf803dcaa4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755067"
---
# <a name="table-valued-parameter-metadata-for-prepared-statements"></a>准备的语句的表值参数元数据
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  应用程序可以通过 SQLNumParams 和 SQLDescribeParam 获取已准备的过程调用的元数据。 对于表值参数， *DataTypePtr* 设置为 SQL_SS_TABLE。 可以通过 SQLGetDescField 为 SQL_CA_SS_TYPE_NAME、SQL_CA_SS_CATALOG_NAME 和 SQL_CA_SS_SCHEMA_NAME 提供额外的元数据。  
  
 SQL_CA_SS_TYPE_NAME、SQL_CA_SS_CATALOG_NAME 和 SQL_CA_SS_SCHEMA_NAME 可以与 SQLColumns 一起使用，以获取与表值参数关联的表类型的列元数据。 在这种情况下，在调用 SQLColumns 之前，必须将 SQL_SOPT_SS_NAME_SCOPE 设置为 SQL_SS_NAME_SCOPE_TABLE_TYPE。 在应用程序检索完表值参数列元数据之后，应将 SQL_SOPT_SS_NAME_SCOPE 设置回原来的默认值 SQL_SS_NAME_SCOPE_TABLE。  
  
 也可以将 SQL_CA_SS_TYPE_NAME、SQL_CA_SS_CATALOG_NAME 和 SQL_CA_SS_SCHEMA_NAME 用于 CLR 用户定义类型参数。  
  
 无法获取非存储过程调用的准备的语句的表值参数元数据。 如果试图这样做，应用程序将返回错误代码为 SQLSTATE 42000 的 SQL_ERROR，以及消息“语法错误或访问冲突”。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
