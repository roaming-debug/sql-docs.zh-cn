---
title: ISSDataClassification | Microsoft Docs
description: ISSDataClassification 接口
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification
apitype: COM
helpviewer_keywords:
- ISSDataClassification interface
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 8ff73fec5480e12c401f44e22325d433f421c69e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753097"
---
# <a name="issdataclassification"></a>ISSDataClassification
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  ISSDataClassification 接口提供检索从 SQL Server 接收到的活动行集的敏感度分类数据的功能。
  

## <a name="methods"></a>方法

|方法|描述|  
|------------|-----------------|  
|[ISSDataClassification::GetSensitivityClassification](../../oledb/ole-db-interfaces/issdataclassification-getsensitivityclassification-ole-db.md)|返回指向包含敏感度分类信息的 SENSITIVITYCLASSIFICATION 结构的指针。|  

## <a name="see-also"></a>另请参阅  
 [接口 &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)   
 [行集](../ole-db-rowsets/rowsets.md)   
 [使用数据分类](../features/using-data-classification.md)
