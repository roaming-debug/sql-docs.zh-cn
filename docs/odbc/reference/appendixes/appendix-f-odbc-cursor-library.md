---
description: 附录 F：ODBC 游标库
title: 附录 F： ODBC 游标库 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], about cursor library
- ODBC cursor library [ODBC]
- cursor library [ODBC], about cursor library
- cursor library [ODBC]
ms.assetid: a03084df-4e48-48ef-917d-4a3fae48a605
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 44fcba1e4c4b2b7f82c99c82399884d7c9bdad85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212528"
---
# <a name="appendix-f-odbc-cursor-library"></a>附录 F：ODBC 游标库
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 ODBC 游标库 ( # A0) 为符合第1级 API 一致性级别的任何驱动程序支持块可滚动游标，并可由开发人员使用其应用程序或驱动程序进行重新分发。 游标库还支持 **SELECT** 语句所生成的结果集的定位 update 和 delete 语句。 尽管它仅支持静态和只进游标，但游标库满足了许多应用程序的需求。 此外，它还可以提供良好的性能，尤其是对于规模较小的结果集以及没有良好游标支持的应用程序。  
  
 游标库是位于驱动程序管理器和驱动程序之间 (DLL) 的动态链接库。 当应用程序调用函数时，驱动程序管理器将调用游标库中的函数，该函数执行该函数或在指定的驱动程序中调用它。 对于给定的连接，应用程序指定是否始终使用游标库，如果驱动程序不支持可滚动的游标，则使用它，或者从未使用过。  
  
 游标库显示为驱动程序管理器的驱动程序。 如果游标库位于驱动程序管理器和 ODBC 2.x *驱动程序* 之间，则游标库将显示为 odbc *2.x 驱动程序*。 如果游标库位于驱动程序管理器和 ODBC 2.x *驱动程序之间*，则游标库将显示为 odbc 1.x *驱动程序。* 游标库表现的行为取决于它所使用的驱动程序的版本，但绑定偏移量除外，这是对 ODBC *2.x 和 odbc* *2.x 驱动程序* 都支持的。  
  
 若要实现 **SQLFetch** 和 **SQLFetchScroll** 中的块游标，游标库将重复调用驱动程序中的 **SQLFetch** 。 若要实现滚动，它会将检索到的数据缓存在内存和磁盘文件中。 当应用程序请求新行集时，游标库根据需要从驱动程序或缓存中检索它。  
  
 为了实现定位的 update 和 delete 语句，游标库使用 **WHERE** 子句构造一个 **update** 或 **delete** 语句，该语句指定行中每个绑定列的缓存值。 当执行定位的 update 语句时，游标库将从行集缓冲区中的值更新其缓存。  
  
 有关 ODBC 游标库的详细信息，请参阅本附录中的以下部分：  
  
-   [使用 ODBC 游标库](../../../odbc/reference/appendixes/using-the-odbc-cursor-library.md)  
  
-   [执行定位更新和删除语句](../../../odbc/reference/appendixes/executing-positioned-update-and-delete-statements.md)  
  
-   [游标库代码示例](../../../odbc/reference/appendixes/cursor-library-code-example.md)  
  
-   [实现说明](../../../odbc/reference/appendixes/implementation-notes.md)  
  
-   [ODBC 游标库错误代码](../../../odbc/reference/appendixes/odbc-cursor-library-error-codes.md)
