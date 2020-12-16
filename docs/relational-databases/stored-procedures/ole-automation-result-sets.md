---
title: OLE 自动化结果集 | Microsoft Docs
description: 了解以下内容：如果 OLE 自动化属性或方法返回的一维或二维数组中的数据，则该数组将作为结果集返回到客户端。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data types [SQL Server], OLE Automation
- two-dimensional arrays
- one-dimensional arrays
- result sets [SQL Server], OLE Automation
- OLE Automation [SQL Server], result sets
- arrays [SQL Server]
ms.assetid: b2f99e33-2303-427c-94b9-9d55f8e2a6ab
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cae5f0fb362b0e3e303a17b1fdfbf5beca3abe2c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479118"
---
# <a name="ole-automation-result-sets"></a>OLE 自动化结果集
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  如果 OLE 自动化属性或方法返回的数据是一维或二维数组，则该数组将作为结果集返回到客户端：  
  
-   一维数组作为单行结果集返回给客户端，其中的列数与数组中的元素数相等。 例如，array(10) 作为 10 列单行返回。  
  
-   二维数组作为结果集返回给客户端，其中的列数与数组第一维中的元素数相同，行数与数组第二维中的元素数相同。 例如，array(2,3) 作为 2 列 3 行返回。  
  
 当属性返回值或方法返回值是数组时，sp_OAGetProperty 或 sp_OAMethod 将向客户端返回结果集。 （方法输出参数不能是数组。这些过程可扫描数组中的所有数据值，以确定用于结果集中各列的相应 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型和数据长度。 对于某个特定的列，这些过程将使用显示该列中的所有数据值所需要的数据类型和长度。  
  
 如果一列中的所有数据值具有相同的数据类型，此数据类型将用于整个列。 当一个列中的数据值的数据类型不同时，将根据下表选择整个列的数据类型。 若要使用下表，请沿着左侧行轴找到一种数据类型，再沿着最上面的列轴找到第二种数据类型。 行和列的交点会说明结果集这一列的数据类型。  
  
|   | **int** | **float** | **money** | **datetime** | **varchar** | **nvarchar** |
| - | ------- | --------- | --------- | ------------ | ----------- | ------------ |
|**int**|**int**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**float**|**float**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**money**|**money**|**money**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**datetime**|**varchar**|**varchar**|**varchar**|**datetime**|**varchar**|**nvarchar**|  
|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**nvarchar**|  
|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|  
  
## <a name="related-content"></a>相关内容  
 [OLE 自动存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)  
  
 [Ole Automation Procedures 服务器配置选项](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)  
  
  
