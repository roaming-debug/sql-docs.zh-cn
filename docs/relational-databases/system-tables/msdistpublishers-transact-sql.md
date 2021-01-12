---
description: MSdistpublishers (Transact-SQL)
title: MSdistpublishers (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdistpublishers
- MSdistpublishers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistpublishers system table
ms.assetid: 31844099-4b33-4dc9-84b4-bac70aa82598
author: cawrites
ms.author: chadam
ms.openlocfilehash: 296bedcc14194dbba3a8f91cb8ebbc6553b6481c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098187"
---
# <a name="msdistpublishers-transact-sql"></a>MSdistpublishers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本地分发服务器支持的每个远程发布服务器在 **MSdistpublishers** 表中各占一行。 该表存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|发布服务器分发服务器的名称。|  
|**distribution_db**|**sysname**|分发数据库的名称。|  
|**working_directory**|**nvarchar(255)**|用于存储发布的数据和架构文件的工作目录的名称。|  
|**security_mode**|**int**|在分发服务器上实现的安全模式：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。<br /><br /> **1** = Windows 身份验证。|  
|**login**|**sysname**|用于身份验证的登录 ID [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**password**|**nvarchar (524)**|用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的密码（已加密）。|  
|**active**|**bit**|指示远程发布服务器是否在使用本地分发服务器。|  
|**受信任**|**bit**|指示远程发布服务器是否使用与本地分发服务器相同的密码：<br /><br /> **0** = 远程发布服务器上需要用来连接到分发服务器的密码。<br /><br /> **1** = 无需密码。|  
|**third_party**|**bit**|发布服务器是否为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的安装：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ：安装。**1** = 异类数据源。|  
|**publisher_type**|**sysname**|发布服务器类型：<br /><br />   =  MSSQLSERVER [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]器.<br /><br /> **Oracle** = 标准 Oracle 发布服务器。<br /><br /> **ORACLE 网关** = Oracle 网关发布服务器。|  
|**storage_connection_string**|**nvarchar (779)**|Azure SQL 数据库存储连接字符串的值。|  

  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
