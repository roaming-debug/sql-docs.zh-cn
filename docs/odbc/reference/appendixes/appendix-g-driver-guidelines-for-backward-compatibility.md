---
description: 附录 G：驱动程序后向兼容性准则
title: 附录 G：用于向后兼容的驱动程序准则 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC drivers [ODBC], backward compatibility
- backward compatibility [ODBC], drivers
- compatibility [ODBC], drivers
ms.assetid: 911cd335-f2c0-4d03-9739-1078308a678a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 337640ec943833dc333483fe2256fb8204c92e9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212516"
---
# <a name="appendix-g-driver-guidelines-for-backward-compatibility"></a>附录 G：驱动程序后向兼容性准则
本附录提供适用于 ODBC 3 的驱动程序编写器的信息。*x* 需要支持 ODBC 2 的驱动程序。*x* 应用程序。 有关向后兼容性的详细信息，请参阅 [向后兼容性和标准符合性](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md)。  
  
 本部分包含以下主题。  
  
-   [Odbc 1.X 驱动程序的块游标、可滚动游标和后向兼容性](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) -新功能是 odbc 3 中存在的功能。*x* 且不在 ODBC 2 中。*x*。 ODBC 3。*x* 驱动程序通常不必担心与新功能的向后兼容性，因为 ODBC 2。*x* 应用程序永远不会使用它们。 这种情况的唯一例外是，与 **SQLFetch**、 **SQLFetchScroll**、 **SQLSetPos** 和 **SQLExtendedFetch** 相关的功能;有关详细信息，请参阅本附录后面的。  
  
-   [映射不推荐使用的函数](../../../odbc/reference/appendixes/mapping-deprecated-functions.md) -重复功能是在 ODBC 3 中以不同方式实现的功能。*x* 和 ODBC 2。*x*。 ODBC 3。*x* 驱动程序不必担心向后兼容重复功能，因为驱动程序管理器始终映射 ODBC 2。*x* 功能到 ODBC 3。*x* 功能调用 ODBC 3 时。*x* 驱动程序。 因此，ODBC 3。*x* 驱动程序仅查看 ODBC 3。*x* 功能。 有关这些映射的详细信息，请参阅本附录后面的。  
  
-   行为 [更改和 odbc 2.X 驱动程序](../../../odbc/reference/appendixes/behavioral-changes-and-odbc-3-x-drivers.md)-行为更改是在 ODBC 3 中以不同方式处理的功能。*x* 和 ODBC 2。*x*。 ODBC 3。*x* 驱动程序必须担心行为更改，并针对应用程序设置的 SQL_ATTR_ODBC_VERSION 环境属性进行操作。
