---
description: 绑定与未绑定的 Text 和 Image 列
title: 绑定与未绑定的 Text 和 Image 列 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
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
- image columns [ODBC]
ms.assetid: ffd3442e-d880-46e9-b848-2365a09a2406
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ede2c58f6b5a1d3710aa6d4a06c0ffe61761f7d7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755037"
---
# <a name="bound-vs-unbound-text-and-image-columns"></a>绑定与未绑定的 Text 和 Image 列
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  使用服务器游标时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序会经过优化，以便在执行 **SQLFetch** 时不传输未绑定的 **text**、 **ntext** 或 **image** 列的数据。 在应用程序为列发出 [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md)之前，不会实际从服务器中检索 **text**、 **ntext** 或 **image** 数据。  
  
 许多应用程序都可以编写，这样用户只需在游标中上下滚动，就不会显示 **text**、 **ntext** 或 **image** 数据。 当用户选择行来获取更多详细信息时，应用程序可以调用 **SQLGetData** 来检索 **text**、 **ntext** 或 **image** 数据。 这会阻止为用户未选择的任何行传输 **text**、 **ntext** 或 **image** 数据，从而防止传输极大量的数据。  
  
## <a name="see-also"></a>另请参阅  
 [管理文本和图像列](../../relational-databases/native-client-odbc-text-image-columns/managing-text-and-image-columns.md)   
 [游标行为](../../relational-databases/native-client-odbc-cursors/cursor-behaviors.md)  
  
  
