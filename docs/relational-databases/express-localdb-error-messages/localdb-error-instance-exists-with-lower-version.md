---
description: LOCALDB_ERROR_INSTANCE_EXISTS_WITH_LOWER_VERSION
title: LOCALDB_ERROR_INSTANCE_EXISTS_WITH_LOWER_VERSION |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: reference
ms.assetid: a7c5ce08-8841-49a3-b252-116807ba469a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 281df1ca5ecf53526982ae45e5e8bdd82db08777
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506233"
---
# <a name="localdb_error_instance_exists_with_lower_version"></a>LOCALDB_ERROR_INSTANCE_EXISTS_WITH_LOWER_VERSION
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
## <a name="details"></a>详细信息  
  
| Attribute | 值 |
| --------- | ----- |
|产品名称|SQL Server|  
|事件 ID|258|  
|事件源|SQL Server 本地数据库运行时 12.0|  
|组件|本地数据库运行时 API|  
|消息正文|无法使用指定的版本创建本地数据库实例。 已存在同名的实例，但是它的版本低于指定的版本。|  
  
## <a name="explanation"></a>说明  
 指定的实例已存在，但其版本低于请求的版本。  
  
## <a name="user-action"></a>用户操作  
 删除现有实例，然后重试操作。  
  
  
