---
description: sys.database_usage (Azure SQL Database)
title: Azure SQL Database (sys.database_usage) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- database_usage
- database_usage_TSQL
- sys.database_usage_TSQL
- sys.database_usage
dev_langs:
- TSQL
helpviewer_keywords:
- database_usage
- sys.database_usage
ms.assetid: be6820de-60bf-4ddd-ace7-4077893d630f
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: e80549106907d042a16197b3ecaf4d6b2dd3f6c7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475198"
---
# <a name="sysdatabase_usage-azure-sql-database"></a>sys.database_usage (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  **注意：这仅适用于 Azure SQL 数据库 V11。**  
  
 列出服务器上数据库的数量、类型和持续时间 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 。  
  
 **Sys.database_usage** 视图包含以下列。  
  
|列名|说明|  
|-----------------|-----------------|  
|time|使用事件发生的日期。|  
|sku|数据库的服务层类型： **Web**、 **Business**、 **Basic**、 **Standard**、 **Premium**|  
|quantity|指定当天存在的 SKU 类型的数据库的最大数量。|  
  
## <a name="permissions"></a>权限  
 对此视图的只读访问权限可用于具有连接到 **master** 数据库的权限的所有用户。  
  
## <a name="remarks"></a>备注  
 **Sys.database_usage** 视图为订阅的每一天返回一行。  
  
## <a name="see-also"></a>另请参阅  
 [SQL 数据库定价详细信息](https://go.microsoft.com/fwlink/?LinkID=394978)   
 [Azure SQL Database 中的帐户和计费](/previous-versions/azure/ee621788(v=azure.100))  
  
