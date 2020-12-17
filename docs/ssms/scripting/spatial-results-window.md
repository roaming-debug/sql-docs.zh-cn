---
title: “空间结果”窗口
description: 了解如何使用“空间结果”窗口，该窗口提供了用于查看空间数据的可视化映射工具。 若要查看空间结果，查询结果中必须包括一个包含几何图形或地域数据的空间列。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: c2d5a477-6496-4d01-adee-7322ebdfadf3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c9dc1352570607686935b040fcc7a28853dd6644
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466258"
---
# <a name="spatial-results-window"></a>“空间结果”窗口
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  **“空间结果”** 窗口提供了用于查看空间数据的可视化映射工具。 若要查看空间结果，查询结果中必须包括一个包含几何图形或地域数据的空间列。  
  
> [!NOTE]  
>  仅当结果返回到 **“结果”** 窗口中的某个网格时， **“空间结果”** 窗口才可用。 如果指定将结果作为文本返回，则此窗口不可用。  
  
## <a name="options"></a>选项  
 **选择空间列**  
 在查询结果中，从空间列指定要查看的空间列。 一次只能选择一个列。  
  
 **选择标签列**  
 在查询结果中，从返回的列指定要标记空间数据的非空间列。 一次只能选择一个列。  
  
 如果某个查询中仅返回了点实例，则此选项不可用。  
  
 **选择投影**  
 显示以下四个投影之一中的地理数据：Equirectangular、Mercator、Robinson 或 Bonne。  
  
 此选项不适用于几何图形数据。  
  
 **Zoom**  
 按指数比例调整映射显示。  
  
 **显示网格线**  
 打开或关闭坐标网格线。  
  
 对于多边形形状，仅当形状大到能够容纳标签文本时，才显示标签。 若要显示小形状的标签，请调整缩放比例。  
  
> [!NOTE]  
>  不能标记点实例。  
  
## <a name="see-also"></a>另请参阅  
 [在对象资源管理器中查看空间数据](./view-spatial-data-in-object-explorer.md)   
 [空间数据 (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)   
 [数据库引擎查询编辑器 (SQL Server Management Studio)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)   
 [查询和文本编辑器 (SQL Server Management Studio)](../f1-help/database-engine-query-editor-sql-server-management-studio.md?view=sql-server-ver15)  
  
