---
title: ISSDataClassification | Microsoft Docs
description: ISSDataClassification 接口
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification
apitype: COM
helpviewer_keywords:
- ISSDataClassification interface
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 23c575e31f151156165e09c34baf6c81b44b209b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342485"
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
