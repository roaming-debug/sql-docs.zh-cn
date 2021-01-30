---
description: 书签 C 数据类型
title: 书签 C 数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- C data types [ODBC], bookmark C data type
- pseudo-type identifiers [ODBC], bookmark C data type
- data types [ODBC], pseudo-type identifiers
- bookmarks [ODBC]
- bookmark C data type [ODBC]
ms.assetid: add88e48-ada3-4c0c-a5ac-e78903d3ff41
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0df39afc58e241dd3736bca6f5fa45e31d102668
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212487"
---
# <a name="bookmark-c-data-type"></a>书签 C 数据类型
书签 C 数据类型允许应用程序检索书签。 书签 C 类型仅用于检索长度可变的书签值;不应将它们转换为其他数据类型。 应用程序从包含 **SQLBulkOperations** (的结果集的列0检索书签，操作 SQL_ADD) 、 **SQLFetch**、 **SQLFetchScroll** 或 **SQLGetData**。 有关详细信息，请参阅 [书签](../../../odbc/reference/develop-app/bookmarks-odbc.md)。  
  
 下表列出了书签 C 数据类型的 *CType* 值、实现了书签 c 数据类型的 ODBC c 数据类型，以及来自 SQL .h 的此数据类型的定义。  
  
> [!NOTE]
>  SQL_C_BOOKMARK 的数据类型已弃用。 ODBC *2.x* 应用程序不应使用 SQL_C_BOOKMARK。 仅当 ODBC 2.x 驱动程序需要使用 ODBC 2.x *应用程序时*，才需要支持 *SQL_C_BOOKMARK。* 当应用程序 *与 ODBC 2.x 驱动程序一起* 使用时，驱动程序管理器会将 SQL_C_VARBOOKMARK 映射到 SQL_C_BOOKMARK。  
  
|C 类型标识符|ODBC C typedef|C 类型|  
|-----------------------|--------------------|------------|  
|SQL_C_BOOKMARK<br />（不推荐使用）|书签|unsigned long int|  
|SQL_C_VARBOOKMARK|SQLCHAR|unsigned char *|
