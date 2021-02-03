---
description: 分析函数 (Transact-SQL)
title: 分析函数 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 60fbff84-673b-48ea-9254-6ecdad20e7fe
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bda6bd28e3a78acc605592d319e7e5a833037ef0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204675"
---
# <a name="analytic-functions-transact-sql"></a>分析函数 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server 支持以下分析函数：

- [CUME_DIST (Transact-SQL)](../../t-sql/functions/cume-dist-transact-sql.md)
- [FIRST_VALUE (Transact-SQL)](../../t-sql/functions/first-value-transact-sql.md)
- [LAG &#40;Transact-SQL&#41;](../../t-sql/functions/lag-transact-sql.md)
- [LAST_VALUE (Transact-SQL)](../../t-sql/functions/last-value-transact-sql.md)
- [LEAD &#40;Transact-SQL&#41;](../../t-sql/functions/lead-transact-sql.md)
- [PERCENT_RANK (Transact-SQL)](../../t-sql/functions/percent-rank-transact-sql.md)
- [PERCENTILE_CONT (Transact-SQL)](../../t-sql/functions/percentile-cont-transact-sql.md)  
- [PERCENTILE_DISC (Transact-SQL)](../../t-sql/functions/percentile-disc-transact-sql.md)
  
分析函数基于一组行计算聚合值。 但是，与聚合函数不同，分析函数可能针对每个组返回多行。 可以使用分析函数来计算移动平均线、运行总计、百分比或一个组内的前 N 个结果。
 
## <a name="see-also"></a>另请参阅

[OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)
