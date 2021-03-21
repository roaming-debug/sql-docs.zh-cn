---
description: 有日志记录的修改与无日志记录的修改
title: 已记录无日志记录修改 |Microsoft Docs
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
- logged vs. nonlogged modifications [SQL Server Native Client]
- columns [ODBC]
- ODBC data types, image columns
- nonlogged vs. logged modifications
- ODBC data types, text columns
- image columns [ODBC]
ms.assetid: 20aa5b27-4a2c-46e7-8356-beb0eebf4b7e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0b6e36cc9de30a4d83cde59941e34edd2c486950
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749017"
---
# <a name="logged-vs-unlogged-modifications"></a>有日志记录的修改与无日志记录的修改
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  应用程序可以请求 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序不记录 **text**、 **ntext** 和 **image** 修改。 但应慎用此选项。 它仅适用于 **文本**、 **ntext** 或 **图像** 数据不重要的情况，数据所有者愿意权衡恢复数据以提高性能的能力。  
  
 通过调用 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)并将 *属性* 参数设置为 SQL_SOPT_SS_ TEXTPTR_LOGGING 并将 *将 valueptr* 设置为 SQL_TL_ON 或 SQL_TL_OFF，可以控制 **text**、 **ntext** 和 **image** 修改的日志记录。  
  
## <a name="see-also"></a>另请参阅  
 [管理 Text 和 Image 列](../../relational-databases/native-client-odbc-text-image-columns/managing-text-and-image-columns.md)  
  
  
