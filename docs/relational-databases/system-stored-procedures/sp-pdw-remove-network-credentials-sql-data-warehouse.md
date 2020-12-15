---
description: " (Azure Synapse Analytics sp_pdw_remove_network_credentials) "
title: sp_pdw_remove_network_credentials
titleSuffix: Azure Synapse Analytics
ms.date: 03/14/2017
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.subservice: design
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: c12696a2-5939-402b-9866-8a837ca4c0a3
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.custom: seo-dt-2019
ms.openlocfilehash: 2e69f8446bc7a98b6aa3bae68a169d5e88759ea2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461538"
---
# <a name="sp_pdw_remove_network_credentials-azure-synapse-analytics"></a> (Azure Synapse Analytics sp_pdw_remove_network_credentials) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  这会删除存储在中 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 用于访问网络文件共享的网络凭据。 例如，使用此存储过程删除在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 驻留在自己的网络中的服务器上执行备份和还原操作的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_remove_network_credentials 'target_server_name'  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>自变量  
 "*target_server_name*"  
 指定目标服务器主机名或 IP 地址。 将从中删除用于访问此服务器的凭据 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。 这不会更改或删除你自己的团队管理的实际目标服务器上的任何权限。  
  
 *target_server_name* 定义为 nvarchar (337) 。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="permissions"></a>权限  
 需要 **ALTER SERVER STATE** 权限。  
  
## <a name="error-handling"></a>错误处理  
 如果在控制节点和所有计算节点上删除凭据不成功，则会发生错误。  
  
## <a name="general-remarks"></a>一般备注  
 此存储过程将从的 NetworkService 帐户中删除网络凭据 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。 NetworkService 帐户 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在控制节点和计算节点上运行 SMP 的每个实例。 例如，在运行备份操作时，控制节点和每个计算节点将使用 NetworkService 帐户凭据来访问目标服务器。  
  
## <a name="metadata"></a>元数据  
 若要列出所有凭据并验证是否已删除凭据，请使用 [&#40;transact-sql&#41;sys.dm_pdw_network_credentials ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md)。  
  
 若要添加凭据，请使用 [&#40;Azure Synapse Analytics&#41;sp_pdw_add_network_credentials ](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md)。  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-remove-credentials-for-performing-a-database-backup"></a>A. 删除用于执行数据库备份的凭据  
 下面的示例将删除用于访问 IP 地址为10.192.147.63 的目标服务器的用户名和密码凭据。  
  
```sql  
EXEC sp_pdw_remove_network_credentials '10.192.147.63';  
```  
  
  

