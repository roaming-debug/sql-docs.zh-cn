---
description: dbo.sysdownloadlist (Transact-SQL)
title: dbo.sysdownloadlist (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysdownloadlist
- sysdownloadlist_TSQL
- dbo.sysdownloadlist_TSQL
- sysdownloadlist
dev_langs:
- TSQL
helpviewer_keywords:
- sysdownloadlist system table
ms.assetid: 71087a4c-e829-488e-aa7d-a9476e2b4779
author: cawrites
ms.author: chadam
ms.openlocfilehash: b523812f7923f6ba68a40fe70ab2076950a3463a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098708"
---
# <a name="dbosysdownloadlist-transact-sql"></a>dbo.sysdownloadlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含所有目标服务器的下载指令队列。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|对提供行的自然插入顺序的列进行标识。|  
|**source_server**|**sysname**|源服务器的名称。|  
|**operation_code**|**tinyint**|作业的操作代码：<br /><br /> **1** = INS (插入) <br /><br /> **2** = UPD (更新) <br /><br /> **3** = DEL (delete) <br /><br /> **4** = 开始<br /><br /> **5** = 停止|  
|**object_type**|**tinyint**|对象类型代码。|  
|**object_id** <sup>1</sup>|**uniqueidentifier**|对象标识号。|  
|**target_server**|**sysname**|目标服务器的名称。|  
|**error_message**|**nvarchar(1024)**|目标服务器处理特定行时，遇到错误时出现的错误消息。|  
|**date_posted**|**datetime**|将作业发布到目标服务器的日期和时间。|  
|**date_downloaded**|**datetime**|上次下载作业的日期和时间。|  
|**status**|**tinyint**|作业的状态：<br /><br /> **0** = 尚未下载<br /><br /> **1** = 已成功下载|  
|**deleted_object_name**|**sysname**|删除的对象的名称。|  
  
 <sup>1</sup> **object_id** 列的值可以是 **-1**，如果 **operation_code** 列是 DELETE 值，则对应于值 ALL。  
  
  
