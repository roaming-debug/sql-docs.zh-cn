---
description: 复制函数 - PUBLISHINGSERVERNAME
title: PUBLISHINGSERVERNAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PUBLISHINGSERVERNAME_TSQL
- PUBLISHINGSERVERNAME
dev_langs:
- TSQL
helpviewer_keywords:
- PUBLISHINGSERVERNAME function
- Publishers [SQL Server replication], names
ms.assetid: e7c278e5-ab23-419e-ab3e-3bb20b0636df
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 1d51901aef802b8c490b138781d9174fea1375a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182279"
---
# <a name="replication-functions---publishingservername"></a>复制函数 - PUBLISHINGSERVERNAME
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为参与数据库镜像会话的已发布数据库返回起始发布服务器的名称。 此函数在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器实例的发布数据库中执行。 使用它可确定已发布数据库的起始发布服务器。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
PUBLISHINGSERVERNAME()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **nvarchar**  
  
## <a name="remarks"></a>备注  
 PUBLISHINGSERVERNAME 用于所有类型的复制。  
  
 当数据库镜像会话存在于发布服务器与镜像伙伴实例之间的发布数据库中时，将使用 PUBLISHINGSERVERNAME。  
  
 必须在发布数据库的上下文中执行此函数。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 镜像服务器实例的发布数据库中执行 PUBLISHINGSERVERNAME 时，将返回已发布数据库源自的发布服务器实例的名称。 在镜像服务器实例的未发布数据库中或在故障转移后从镜像服务器实例发布的数据库中执行此函数时，将返回此镜像服务器实例的名称。 在起始发布服务器实例中执行此函数时，将返回此发布服务器的名称。  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像和复制 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [复制函数 (Transact-SQL)]()  
  
