---
description: LOCALDB_ERROR_INSUFFICIENT_BUFFER
title: LOCALDB_ERROR_INSUFFICIENT_BUFFER |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: reference
ms.assetid: ff67bda8-7e5c-4b06-8d7b-9985b6059a98
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5c0315eb0d4318fd7e6c82ad19676af37e0391d8
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506191"
---
# <a name="localdb_error_insufficient_buffer"></a>LOCALDB_ERROR_INSUFFICIENT_BUFFER
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
## <a name="details"></a>详细信息  
  
| Attribute | 值 |
| --------- | ----- |
|产品名称|SQL Server|  
|事件 ID|276|  
|事件源|SQL Server 本地数据库运行时 12.0|  
|组件|本地数据库运行时 API|  
|消息正文|传递给本地数据库实例 API 方法的缓冲区太小。|  
  
## <a name="explanation"></a>说明  
 输入的缓冲区太小，并且没有要求截断。  
  
## <a name="user-action"></a>用户操作  
 提供指定大小的缓冲区。  
  
  
