---
description: 游标类型
title: 游标类型 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, cursors
- ODBC applications, cursors
- cursors [ODBC], types
- ODBC cursors, types
ms.assetid: 3a916cc7-f352-42cb-8b83-f78e06cef991
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9bffc8d5197117533913dca63c171fb4fffab588
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438650"
---
# <a name="cursor-types"></a>游标类型
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 定义 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] NATIVE Client ODBC 驱动程序支持的四种游标类型。 这些游标的功能不同于检测对结果集及其消耗的资源（如 **tempdb** 中的内存和空间）的更改。 游标仅当尝试重新提取行时才会检测到对这些行的更改；数据源无法通知游标对当前提取行的更改。 游标检测并非由游标执行的更改的功能也受事务隔离级别的影响。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持以下四种 ODBC 游标类型：  
  
-   只进游标不支持滚动；它们只支持游标按从头到尾的顺序提取行。  
  
-   静态游标在打开游标时在 **tempdb** 中生成。 它们始终显示打开游标时的结果集。 它们从不会反映对数据的更改。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 静态游标始终是只读的。 由于静态服务器游标是在 **tempdb** 中作为工作表生成的，因此游标结果集的大小不能超过所允许的最大行大小 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   打开由键集驱动的游标时，该游标的结果集中各行的成员身份和顺序是固定的。 可通过游标显示对非键列的更改。  
  
-   动态游标与静态游标相对。 动态游标反映对结果集中的行的所有更改。 结果集中的行数据值、顺序和成员在每次提取时都会改变。  
  
## <a name="see-also"></a>另请参阅  
 [使用游标 &#40;ODBC&#41;](../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
  
  
