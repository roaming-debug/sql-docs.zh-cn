---
description: " (Native Client OLE DB 提供程序的行书签) "
title: 行书签 (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- bookmarks [OLE DB]
- SQL Server Native Client OLE DB provider, bookmarks
- rowsets [OLE DB], bookmarks
- OLE DB rowsets, bookmarks
ms.assetid: 7d9076f2-bf9c-452e-b816-70371a0c1644
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8067f7e720eb5deb0825abb8c5093dc969a91d17
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753617"
---
# <a name="bookmarks-in-sql-server-native-client"></a>SQL Server Native Client 中的书签
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  书签支持使用者快速返回到某行。 利用书签，使用者可以根据书签值对行进行随机访问。 行集中的列 0 为书签列。 使用者可将绑定结构的 dwFlag 字段值设置为 DBCOLUMNSINFO_ISBOOKMARK，以指示将该列用作书签。 使用者还可将行集属性 DBPROP_BOOKMARKS 设置为 VARIANT_TRUE。 这样可使行集中包含列 0。 然后，可使用 IRowsetLocate::GetRowsAt 方法提取行（起始行的位置为书签加上一个偏移量得到的位置）  。  
  
## <a name="see-also"></a>另请参阅  
 [行集](../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
  
