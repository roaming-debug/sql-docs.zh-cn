---
description: 执行定位更新和删除语句
title: 执行定位的 Update 和 Delete 语句 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- positioned deletes [ODBC]
- cursor library [ODBC], positioned update or delete
- positioned updates [ODBC]
- ODBC cursor library [ODBC], positioned update or delete
ms.assetid: 1d64f309-2a6e-4ad1-a6b5-e81145549c56
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d10b2e5c625a222e8e09a5e783291ce613a492b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194826"
---
# <a name="executing-positioned-update-and-delete-statements"></a>执行定位更新和删除语句
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 在应用程序使用 **SQLFetchScroll** 获取数据块后，它可以在块中更新或删除数据。 若要执行定位更新或删除，请执行以下操作：  
  
1.  调用 **SQLSetPos** 将游标定位在要更新或删除的行上。  
  
2.  使用以下语法构造定位的 update 或 delete 语句：  
  
     **更新***表-名称*  
  
     **设置***列标识符* **=** {*expression* &#124; **NULL**}  
  
     [**，** *列标识符* **=** {*expression* &#124; **NULL**}]  
  
     **当前***游标名称*  
  
     **从表中删除** *-* 名称的 **当前***游标名称*  
  
     在定位的 update 语句中构造 **SET** 子句的最简单方法是，对每个要更新的列使用参数标记，并使用 **SQLBindParameter** 将这些参数绑定到要更新的行的行集缓冲区。 在这种情况下，参数的 C 数据类型将与行集缓冲区的 C 数据类型相同。  
  
3.  更新当前行的行集缓冲区（如果它将执行定位的 update 语句）。 成功执行定位的 update 语句后，游标库会将当前行中每列的值复制到其缓存中。  
  
    > [!CAUTION]  
    >  如果在执行定位的 update 语句之前，应用程序不能正确更新行集缓冲区，则在执行该语句后，缓存中的数据将不正确。  
  
4.  使用与与游标关联的语句不同的语句执行定位的 update 或 delete 语句。  
  
    > [!CAUTION]  
    >  由游标库构造的用于标识当前行的 **WHERE** 子句可能无法标识任何行、标识不同行或标识多个行。 有关详细信息，请参阅 [构造搜索语句](../../../odbc/reference/appendixes/constructing-searched-statements.md)。  
  
 所有定位的 update 和 delete 语句都需要一个游标名称。 若要指定游标名称，应用程序会在打开游标之前调用 **SQLSetCursorName** 。 若要使用驱动程序生成的游标名称，应用程序会在打开游标后调用 **SQLGetCursorName** 。  
  
 游标库执行定位的 update 或 delete 语句后，游标库维护的状态数组、行集缓冲区和缓存将包含下表中显示的值。  
  
|使用的语句|行状态数组中的值|中的值<br /><br /> 行集缓冲区|中的值<br /><br /> 缓存缓冲区|  
|--------------------|-------------------------------|----------------------------------|---------------------------------|  
|定位更新|SQL_ROW_UPDATED|新值 [1]|新值 [1]|  
|定位删除|SQL_ROW_DELETED|旧值|旧值|  
  
 [1] 应用程序必须先更新行集缓冲区中的值，然后再执行定位的 update 语句;执行定位的 update 语句后，游标库将行集缓冲区中的值复制到其缓存中。
