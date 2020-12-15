---
description: 执行时数据和 Text、ntext 或 Image 列
title: 执行时数据和 Text、ntext、Image
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- text columns [ODBC]
- SQL Server Native Client ODBC driver, image columns
- SQL Server Native Client ODBC driver, text columns
- data types [ODBC], image
- data types [ODBC], text
- columns [ODBC]
- ODBC data types, image columns
- ODBC data types, text columns
- data-at-execution
- ODBC data-at-execution
- image columns [ODBC]
ms.assetid: 67ffb1a6-f38d-4712-ba64-96bdd41ec2b2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 32f2bb59a0632c7070230750c720398a419b96b0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464918"
---
# <a name="data-at-execution-and-text-ntext-or-image-columns"></a>执行时数据和 Text、ntext 或 Image 列
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  使用 ODBC 执行时数据功能，应用程序能够对绑定列或参数使用非常大的数据量。 检索非常大的 **text**、 **ntext** 或 **image** 列时，应用程序可能无法简单地分配大型缓冲区，将列绑定到缓冲区，并提取行。 更新非常大的 **text**、 **ntext** 或 **image** 列时，应用程序可能无法简单地分配大型缓冲区，将其绑定到 SQL 语句中的参数标记，然后执行该语句。 在这些情况下，应用程序必须将 [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) 或 [SQLPutData](../../relational-databases/native-client-odbc-api/sqlputdata.md) 与它的执行时数据选项一起使用。  
  
## <a name="see-also"></a>另请参阅  
 [管理 Text 和 Image 列](../../relational-databases/native-client-odbc-text-image-columns/managing-text-and-image-columns.md)  
  
  
