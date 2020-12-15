---
description: 在 SQL Server Native Client 中对 OLE DB 使用 OUTPUT 子句
title: OUTPUT 子句，OLE DB
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 53deeb99-c088-4fde-844b-b2d91d6de1eb
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f02324e31552230cddad304ffe5ad1e3fc7ac917
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463468"
---
# <a name="using-the-output-clause-with-ole-db-in-sql-server-native-client"></a>在 SQL Server Native Client 中对 OLE DB 使用 OUTPUT 子句
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  如果在 INSERT、UPDATE、DELETE 或 MERGE 命令中使用 OUTPUT 子句，则无法获得受影响的行数。 应用程序必须对 OUTPUT 子句所返回的行集中的行数进行计数。  
  
## <a name="see-also"></a>另请参阅  
 [创建 SQL Server Native Client OLE DB 访问接口应用程序](../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)  
  
  
