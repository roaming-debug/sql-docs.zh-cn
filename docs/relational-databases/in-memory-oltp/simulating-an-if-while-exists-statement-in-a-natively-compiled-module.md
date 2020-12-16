---
title: 模拟 IF-WHILE EXISTS - 本机编译的模块
description: 了解如何在条件语句中模拟 EXISTS 子句，SQL Server 中的本机编译存储过程不支持此子句。
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: c0e187c1-cbd9-463c-b417-8a734574f102
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0438806d15caf806c4598f3d2b3882011d77d0f5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485199"
---
# <a name="simulating-an-if-while-exists-statement-in-a-natively-compiled-module"></a>在本机编译模块中模拟 IF-WHILE EXISTS 语句
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本机编译的存储过程不支持条件语句中的 **EXISTS** 子句，例如 IF 和 WHILE。  
  
 下面的示例说明了结合使用 BIT 变量与 SELECT 语句模拟 EXISTS 子句的解决方法：  
  
```sql  
DECLARE @exists BIT = 0  
SELECT TOP 1 @exists = 1 FROM MyTable WHERE ...  
IF @exists = 1  
```  
  
## <a name="see-also"></a>另请参阅  
 [本机编译的存储过程的迁移问题](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [内存中 OLTP 不支持的 Transact-SQL 构造](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  
