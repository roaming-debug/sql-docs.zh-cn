---
title: OLE DB Driver for SQL Server 中的 UTF-16 支持 | Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 中的 UTF-16 支持，以及它何时向缓冲区添加高代理码位。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07be222f6afb7bd2463c68cf0a540f14471dc81d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751087"
---
# <a name="utf-16-support-in-ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序中的 UTF-16 支持
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  自 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 起，如果出现下列情况，适用于 SQL Server 的 OLE DB 驱动程序将不向缓冲区添加高代理项码位：如果在绑定列结果或输出参数时提供了长度固定的缓冲区，如果在缓冲区中终止符前面写入的 wchar 字符是代理项对的高代理项码位，以及如果下一个 wchar 字符是一个低代理项码位   。  
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序功能](../../oledb/features/oledb-driver-for-sql-server-features.md)   
  
  
