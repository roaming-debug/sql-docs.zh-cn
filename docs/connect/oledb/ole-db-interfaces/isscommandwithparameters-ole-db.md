---
title: ISSCommandWithParameters（OLE DB 驱动程序）
description: 了解 ISSCommandWithParameters 接口参数如何支持 OLE DB Driver for SQL Server 中的 SQL Server XML 和用户定义的类型。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSCommandWithParameters (OLE DB)
apitype: COM
helpviewer_keywords:
- ISSCommandWithParameters interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a30947176efb820a7105f005e3d372f090d4190
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172167"
---
# <a name="isscommandwithparameters-ole-db"></a>ISSCommandWithParameters (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  ISSCommandWithParameters 接口公开了对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] XML 和用户定义类型 (UDT) 的支持。 这一可选接口继承自核心 OLE DB 接口 ICommandWithParameters  。 除了从 ICommandWithParameters 继承的三个方法（GetParameterInfo、MapParameterNames 和 SetParameterInfo）之外，ISSCommandWithParameters 还提供了两个用于处理服务器特定数据类型的新方法      。  
  
> [!NOTE]  
>  当使用服务组件时，可以使用 ISSCommandWithParameters 接口，但是服务组件不会使用此接口  。  
  
|方法|说明|  
|------------|-----------------|  
|[ISSCommandWithParameters::GetParameterProperties &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md)|对传递到该命令的每个 UDT 或 XML 参数返回数组中的一个 SSPARAMPROPS 属性集结构，但是对于其他类型的参数，不会返回任何内容  。|  
|[ISSCommandWithParameters::SetParameterProperties &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md)|按照序号基于每个参数设置参数属性，或者通过指定 SSPARAMPROPS 结构数组来设置大容量参数属性  。|  
  
## <a name="see-also"></a>另请参阅  
 [接口 &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)    
 [使用 XML 数据类型](../../oledb/features/using-xml-data-types.md)   
 [使用用户定义类型](../../oledb/features/using-user-defined-types.md)  
  
  
