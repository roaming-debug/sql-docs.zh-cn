---
description: 行状态
title: 行状态 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- row status [ODBC]
- cache [ODBC]
ms.assetid: 0f0b1fb6-f697-4ced-811c-2908e210bc71
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 491e0b420e31f1fe8e66c73f9ab8d89cb1e7e1cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187155"
---
# <a name="row-status"></a>行状态
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 游标库在缓存中为行状态创建一个缓冲区。 游标库检索行状态数组的值， (通过此缓冲区中) SQL_ATTR_ROW_STATUS_PTR 语句特性指定。 对于每一行，游标库将此缓冲区设置为：  
  
-   SQL_ROW_DELETED 在该行上执行定位的 delete 语句时。  
  
-   SQL_ROW_ERROR 在使用 **SQLFetch** 从数据源检索行时遇到错误。  
  
-   SQL_ROW_SUCCESS 成功从 **SQLFetch** 的数据源中提取行时。  
  
-   在行上执行定位的 update 语句时，SQL_ROW_UPDATED。
