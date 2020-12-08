---
title: 列存储索引建议 - 数据引擎优化顾问 (DTA)
description: 了解数据库引擎优化顾问如何分析工作负载并建议要在 SQL Server 的数据库上生成的行存储和列存储索引。
ms.custom: seo-dt-2019
ms.date: 01/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor, columnstore index
- Database Engine Tuning Advisor, columnstore and rowstore indexes
ms.assetid: 9fba1139-82cb-4244-a41f-4337a7d0c132
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 441b0c37a39efaf02ff00d64b893896cd3fe8f6f
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505371"
---
# <a name="columnstore-index-recommendations-in-database-engine-tuning-advisor-dta"></a>数据引擎优化顾问 (DTA) 中的列存储索引建议
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 
  数据仓库和分析工作负荷可以充分利用[列存储索引](../../t-sql/statements/create-columnstore-index-transact-sql.md)和传统的行存储索引。 选择为数据库生成哪种行存储和列存储索引取决于应用程序的工作负荷。 在 SQL Server 2016 中，[数据库引擎优化顾问 (DTA)](../../relational-databases/performance/database-engine-tuning-advisor.md)可以分析工作负荷，并建议要在数据库上生成的适当的行存储和列存储索引组合。 
  
 SQL Server Management Studio **16.4** 或更高版本提供此功能。 
  
## <a name="how-to-enable-columnstore-index-recommendations-in-database-engine-tuning-advisor-gui"></a>如何在数据引擎优化顾问 GUI 中启用列存储索引建议

  
  1. 启动数据库引擎优化顾问并打开新的优化会话。
  
  2. 在“常规”窗格中选择要优化的数据库和工作负荷。
  
  3. 在“优化选项”窗格中选中“建议列存储索引”复选框（见下图）。
  ![DTA 列存储索引优化选项](../../relational-databases/performance/media/dta-columnstore-indexes-tuning-option.gif)
 
  4. 选择其他优化选项，然后单击“开始分析”按钮。
  
  5. 优化完成后，查看“建议”窗格中包括列存储索引在内的所有建议（见下图）。      
  ![DTA 列存储索引建议](../../relational-databases/performance/media/dta-columnstore-index-recommendation.gif)
  
  6. 单击“定义”超链接，查看可创建建议的索引的 SQL 数据定义语言 (DDL) 语句。 默认情况下，DTA 在列存储索引名称中使用后缀 **col**，以便更轻松地标识列存储索引（见下图）。
  ![DTA 列存储索引定义](../../relational-databases/performance/media/dta-columnstore-index-definition.gif) 
  
  
  ## <a name="how-to-enable-columnstore-index-recommendations-in-dtaexe-utility"></a>如何在 dta.exe 实用工具中启用列存储索引建议

若要在使用 dta.exe 命令行实用工具时启用列存储建议，请使用 **-fc** 命令行参数。

有关 dta.exe 命令行实用工具的详细信息，请参阅 [dta 实用工具](../../tools/dta/dta-utility.md)

## <a name="see-also"></a>另请参阅
[列存储索引指南](../../relational-databases/indexes/columnstore-indexes-overview.md)       
[数据库引擎优化顾问](../../relational-databases/performance/database-engine-tuning-advisor.md)      
[教程：数据库引擎优化顾问](../../tools/dta/tutorial-database-engine-tuning-advisor.md)



  

