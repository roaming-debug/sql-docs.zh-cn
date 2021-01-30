---
description: 行为更改和 ODBC 3.x 驱动程序
title: 行为更改和 ODBC 2.x 驱动程序 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sql_attr_odbc_version [ODBC]
- backward compatibility [ODBC], behavioral changes
- compatibility [ODBC], behavioral changes
ms.assetid: 88a503cc-bff7-42d9-83ff-8e232109ed06
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a76e950ad8c929d701a3c967b3c5f5422fd3db89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212496"
---
# <a name="behavioral-changes-and-odbc-3x-drivers"></a>行为更改和 ODBC 3.x 驱动程序
环境属性 SQL_ATTR_ODBC_VERSION 向驱动程序指示它是否需要显示 ODBC 2.x *行为或 odbc* *1.x 行为。* 设置 SQL_ATTR_ODBC_VERSION 环境属性的方式取决于应用程序。 ODBC *2.x* 应用程序必须调用 **SQLSetEnvAttr** ，以便在调用 **SQLAllocHandle** 来分配环境句柄之后，在调用 **SQLAllocHandle** 以分配连接句柄之前，设置此属性。 如果无法执行此操作，驱动程序管理器将返回 SQLSTATE HY010 (函数序列错误) 对 **SQLAllocHandle** 的后调用。  
  
> [!NOTE]  
>  有关行为更改和应用程序行为的详细信息，请参阅 [行为更改](../../../odbc/reference/develop-app/behavioral-changes.md)。  
  
 通过 ODBC *1.x 标头* 文件重新编译 *的 odbc 2.x 应用程序和* odbc *2.X 应用程序* 不调用 **SQLSetEnvAttr**。 但是，它们调用 **SQLAllocEnv** 而不是 **SQLAllocHandle** 来分配环境句柄。 因此，当应用程序在驱动程序管理器中调用 **SQLAllocEnv** 时，驱动程序管理器将调用驱动程序中的 **SQLAllocHandle** 和 **SQLSetEnvAttr** 。 因此 *，ODBC 2.x* 驱动程序始终可以计算此特性的设置。  
  
 如果使用 ODBC_STD 编译标志编译的符合标准的应用程序调用 **SQLAllocEnv** (这可能是由于 ISO) 中未弃用 **SQLAllocEnv** 而导致的，则调用将在编译时映射到 **SQLAllocHandleStd** 。 在运行时，应用程序调用 **SQLAllocHandleStd**。 驱动程序管理器将 SQL_ATTR_ODBC_VERSION 环境属性设置为 SQL_OV_ODBC3。 对 **SQLAllocHandleStd** 的调用等效于对使用 *HandleType* SQL_HANDLE_ENV 的 **SQLAllocHandle** 的调用，并调用 **SQLSetEnvAttr** 将 SQL_ATTR_ODBC_VERSION 设置为 SQL_OV_ODBC3。  
  
 在某些驱动程序体系结构中，*驱动程序需要**作为 odbc 2.x 驱动程序* 或 odbc 1.x 驱动程序出现，具体取决于连接。 在这种情况下，驱动程序实际上可能不是驱动程序，而是位于驱动程序管理器和另一驱动程序之间的一个层。 例如，它可能模拟 ODBC Spy 等驱动程序。 在另一个示例中，它可能充当一个网关，如 EDA/SQL。 若要显示为 ODBC 1.x 驱动程序，此类 *驱动程序必须* 能够导出 **SQLAllocHandle**，并显示 *为 odbc 2.x* 驱动程序，必须能够导出 **SQLAllocConnect**、 **SQLAllocEnv** 和 **SQLAllocStmt**。 当要分配环境、连接或语句时，驱动程序管理器会检查此驱动程序是否导出 **SQLAllocHandle**。 由于驱动程序的原因，驱动程序管理器将在驱动程序中调用 **SQLAllocHandle** 。 如果 *驱动程序正在使用 ODBC 2.x* 驱动程序，则驱动程序必须根据需要将对 **SQLAllocHandle** 的调用映射到 **SQLAllocConnect**、 **SQLAllocEnv** 或 **SQLAllocStmt**。 当表现 *为 ODBC 2.x* 驱动程序时，它还必须对 **SQLSetEnvAttr** 调用执行任何操作。  
  
 本部分包含以下主题。  
  
-   [日期时间数据类型](../../../odbc/reference/appendixes/datetime-data-types.md)  
  
-   [C 数据类型的后向兼容性](../../../odbc/reference/appendixes/backward-compatibility-of-c-data-types.md)  
  
-   [定长书签](../../../odbc/reference/appendixes/fixed-length-bookmarks.md)  
  
-   [SQLGetInfo 支持](../../../odbc/reference/appendixes/sqlgetinfo-support.md)  
  
-   [返回 SQL_NO_DATA](../../../odbc/reference/appendixes/returning-sql-no-data.md)  
  
-   [调用 SQLSetPos 以插入数据](../../../odbc/reference/appendixes/calling-sqlsetpos-to-insert-data.md)  
  
-   [按序号加载](../../../odbc/reference/appendixes/loading-by-ordinal.md)
