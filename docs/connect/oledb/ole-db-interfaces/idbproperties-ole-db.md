---
title: IDBProperties (OLE DB driver) | Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 中的 IDBProperties 接口，其中包括 IDBProperties::GetPropertyInfo 方法。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8cb88bb3064dde4f997095cb8d39e61d26e0ab67
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752597"
---
# <a name="idbproperties-ole-db"></a>IDBProperties (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB 标准规范允许提供程序为 DBPROPINFO::vValues 指定 VT_EMPTY  。 但是，在使用 DBPROPSET_ROWSETALL 调用 IDBProperties::GetPropertyInfo 来检索行集属性时，OLE DB Driver for SQL Server OLE DB 始终返回 VT_EMPTY。  
  
## <a name="see-also"></a>另请参阅  
 [接口 (OLE DB)](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md) 
  
  
