---
description: 缓存的位置
title: 缓存的位置 |Microsoft Docs
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
- cache [ODBC]
ms.assetid: 240d6162-4da6-4b1f-96c7-f379f4ecb16f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b500aa9b7955c91c6ca39b3f7f830b2022dc4a57
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184362"
---
# <a name="location-of-cache"></a>缓存的位置
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 游标库将数据缓存在内存中，在 Windows®临时文件中。 这会限制游标库只能由可用磁盘空间处理的结果集的大小。 当要缓存的数据在游标库缓存末尾插入时，将使用临时文件。 而是添加要缓存的数据，以取代缓存中最后保存的数据块。 上次保存的数据块保存在临时文件中。 如果游标库异常终止，例如当电源发生故障时，它可以将 Windows 临时文件保留在磁盘上。 它们命名为 ~ CTT *nnnn*，并在当前目录中创建。  
  
> [!NOTE]  
>  如果 Microsoft® WindowsNT®中的游标库尝试缓存当前目录中的临时文件中的数据，而该应用程序是从只读共享还是紧凑型磁盘 (如 Microsoft 基础类库示例) ，则 SQLSTATE HY000 (常规 Error-Unable 来创建文件缓冲区) 将返回该文件。
