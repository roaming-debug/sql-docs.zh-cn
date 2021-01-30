---
description: sys.fn_virtualservernodes (Transact-SQL)
title: sys.fn_virtualservernodes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_virtualservernodes
- fn_virtualservernodes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- nodes [Faillover Clustering], virtual servers
- nodes [Faillover Clustering]
- virtual servers [Faillover Clustering]
- failover clustering [SQL Server], nodes
- fn_virtualservernodes function
- sys.fn_virtualservernodes function
ms.assetid: 257f3b8d-93c0-4444-87f1-ea211bd8cad0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1623ad4fe9de10eb8ad896b62a7fc698df2f9f0a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187385"
---
# <a name="sysfn_virtualservernodes-transact-sql"></a>sys.fn_virtualservernodes (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  返回可运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的故障转移群集实例节点的列表。 此信息在故障转移群集环境中很有用。  
  
> [!IMPORTANT]
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 提供此系统函数是为了向后兼容。 建议改用 [&#40;transact-sql&#41;sys.dm_os_cluster_nodes ](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
fn_virtualservernodes()  
```  
  
## <a name="tables-returned"></a>返回的表  
 如果当前服务器是群集服务器， **fn_virtualservernodes** 将返回已在其上定义此实例的故障转移群集实例节点的列表 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 如果当前服务器实例不是群集服务器， **fn_virtualservernodes** 将返回空的行集。  
  
## <a name="permissions"></a>权限  
 用户必须对实例具有 VIEW SERVER STATE 权限 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `fn_virtualservernodes` 对群集服务器实例进行查询：  
  
```  
SELECT * FROM fn_virtualservernodes();  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 NodeName  
  
 -------\-  
  
 SS3-CLUSN1  
  
 SS3-CLUSN2  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_os_cluster_nodes &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [sys.fn_servershareddrives &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-servershareddrives-transact-sql.md)  
  
  
