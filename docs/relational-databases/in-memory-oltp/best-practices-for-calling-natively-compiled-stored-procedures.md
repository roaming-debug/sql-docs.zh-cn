---
title: 最佳做法 - 本机编译存储过程
description: 了解本机编译的存储过程的最佳做法，这些存储过程通常用于应用程序中性能至关重要的部分。
ms.custom: seo-dt-2019
ms.date: 03/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: f39fc1c7-cfec-4a95-97f6-6b95954694b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 04035aa7d1a94050e8b4f1d248fb1313a2ac17f0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465398"
---
# <a name="best-practices-for-calling-natively-compiled-stored-procedures"></a>调用本机编译存储过程的最佳做法
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本机编译存储过程：  
  
-   通常用于应用程序中性能至关重要的部分。  
  
-   频繁执行。  
  
-   操作速度快。  
  
 使用本机编译存储过程所得的性能优势随行数和该过程所处理的逻辑数的上升而增加。 例如，如果原生编译存储过程使用以下一个或多个组件，将获得更好的性能：  
  
-   聚合。  
  
-   嵌套循环联接。  
  
-   多语句选择、插入、更新和删除操作。  
  
-   复杂表达式。  
  
-   程序逻辑，如条件语句和循环。  
  
 如果只需处理一行，则使用本机编译存储过程可能没有任何性能优势。  
  
 为避免服务器必须映射参数名称和转换类型，请确保：  
  
-   使传递给过程的参数类型与过程定义中的类型相匹配。  
  
-   在调用本机编译的存储过程时使用序数（无名称）参数。 要实现最高效的执行，请勿使用命名参数。  
  
 可通过 XEvent **natively_compiled_proc_slow_parameter_passing** 检测本机编译的存储过程的参数是否低效：
 - 不匹配的类型：**reason=parameter_conversion**
 - 命名参数：**reason=named_parameters**
 - 默认值：**reason=default** 
  
## <a name="see-also"></a>另请参阅  
 [本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)