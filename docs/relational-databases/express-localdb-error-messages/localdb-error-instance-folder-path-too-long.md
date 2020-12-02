---
description: LOCALDB_ERROR_INSTANCE_FOLDER_PATH_TOO_LONG
title: LOCALDB_ERROR_INSTANCE_FOLDER_PATH_TOO_LONG |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: reference
ms.assetid: c178a308-8d99-47fc-8a49-5a480dc592f6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a4313aa27b2bae30f6eb989e2161b80635a2b985
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506219"
---
# <a name="localdb_error_instance_folder_path_too_long"></a>LOCALDB_ERROR_INSTANCE_FOLDER_PATH_TOO_LONG
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
## <a name="details"></a>详细信息  
  
| Attribute | 值 |
| --------- | ----- | 
|产品名称|SQL Server|  
|事件 ID|260|  
|事件源|SQL Server 本地数据库运行时 12.0|  
|组件|本地数据库运行时 API|  
|消息正文|本地数据库实例文件夹的完整路径长度大于 MAX_PATH。 此实例必须存储在以下文件夹中：%% LOCALAPPDATA%% \ Microsoft\Microsoft SQL Server 本地 DB\Instances \\<实例名称 \> 。|  
  
## <a name="explanation"></a>说明  
 应在其中存储该实例的路径的长度超过 MAX_PATH。  
  
## <a name="user-action"></a>用户操作  
 创建长度小于 MAX_PATH 的新路径。  
  
  
