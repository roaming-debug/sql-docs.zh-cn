---
description: 保留字 (Master Data Services)
title: 保留字
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- reserved words [Master Data Services]
- Master Data Services, reserved words
ms.assetid: 88afd0d0-4362-4394-8357-4e65388fc0fc
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f87125b2c72ac5c8242965dcb3d0407a4a15614c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340220"
---
# <a name="reserved-words-master-data-services"></a>保留字 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，当您创建模型对象或成员时，不能使用某些字词。 使用这些字词可能导致错误。  
  
> [!NOTE]  
>  您还应限制使用特殊字符（符号、连字符等）。  
  
-   [模型](../master-data-services/reserved-words-master-data-services.md#models)  
  
-   [实体](../master-data-services/reserved-words-master-data-services.md#entities)  
  
-   [显式层次结构](../master-data-services/reserved-words-master-data-services.md#exhierarchies)  
  
-   [特性](../master-data-services/reserved-words-master-data-services.md#attributes)  
  
-   [成员](../master-data-services/reserved-words-master-data-services.md#members)  
  
##  <a name="models"></a><a name="models"></a> 机型  
 如果你创建名称设置为 **Name** 或 **Code** 的模型，则不要选择“使用与模型相同的名称创建实体”，因为 **Name** 或 **Code** 不能用于实体名称。   
  
##  <a name="entities"></a><a name="entities"></a> 条目  
 对于实体名称，不能使用 **Name** 或 **Code**。  
  
##  <a name="explicit-hierarchies"></a><a name="exhierarchies"></a> 显式层次结构  
 对于显式层次结构名称，不能使用 **Name** 或 **Code**。  
  
##  <a name="attributes"></a><a name="attributes"></a> 属性  
  
-   **ID**  
  
-   **代码**  
  
-   **EnterUserName**  
  
-   **LastChgUserName**  
  
-   **名称**  
  
-   **EnterDTM**  
  
-   **EnterUserID**  
  
-   **EnterUserName**  
  
-   **LastChgDTM**  
  
-   **LastChgUserID**  
  
-   **Status_ID**  
  
-   **ValidationStatus_ID**  
  
-   **Version_ID**  
  
##  <a name="members"></a><a name="members"></a> 组员  
 对于成员，不能对 **Code** 属性值使用 **MDMMemberStatus**、 **MDMUnused** 或 **ROOT** 。  
  
## <a name="see-also"></a>另请参阅  
 [Master Data Services 概述 (MDS)](../master-data-services/master-data-services-overview-mds.md)  
  
  
