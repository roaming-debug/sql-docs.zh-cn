---
description: " (Azure Synapse Analytics sp_pdw_add_network_credentials) "
title: sp_pdw_add_network_credentials
titleSuffix: Azure Synapse Analytics
ms.date: 03/14/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0729eeff-ac7e-43f0-80fa-ff5346a75985
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.custom: seo-dt-2019
ms.openlocfilehash: 68a4abf4e532719d67032c2eeb64a0027c1c88a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271794"
---
# <a name="sp_pdw_add_network_credentials-azure-synapse-analytics"></a> (Azure Synapse Analytics sp_pdw_add_network_credentials) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  这会在中存储网络凭据 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ，并将其与服务器关联。 例如，使用此存储过程可提供 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 适当的读/写权限，以便在目标服务器上执行数据库备份和还原操作，或创建用于 TDE 的证书的备份。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_add_network_credentials 'target_server_name',  'user_name', 'password'  
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>参数  
 "*target_server_name*"  
 指定目标服务器主机名或 IP 地址。 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将使用传递到此存储过程的用户名和密码凭据访问此服务器。  
  
 若要通过未使用的网络进行连接，请使用目标服务器的未使用的 IP 地址。  
  
 *target_server_name* 定义为 nvarchar (337) 。  
  
 'user_name'  
 指定有权访问目标服务器的 user_name。 如果目标服务器已有凭据，则这些凭据将更新为新凭据。  
  
 *user_name* 定义为 nvarchar (513) 。  
  
 '*password*"  
 指定 *user_name* 的密码。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="permissions"></a>权限  
 需要 **ALTER SERVER STATE** 权限。  
  
## <a name="error-handling"></a>错误处理  
 如果在控制节点和所有计算节点上添加凭据失败，则会发生错误。  
  
## <a name="general-remarks"></a>一般备注  
 此存储过程将向的 NetworkService 帐户添加网络凭据 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。 NetworkService 帐户 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在控制节点和计算节点上运行 SMP 的每个实例。 例如，在运行备份操作时，控制节点和每个计算节点将使用 NetworkService 帐户凭据获取目标服务器的读取和写入权限。  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-add-credentials-for-performing-a-database-backup"></a>A. 添加用于执行数据库备份的凭据  
 下面的示例将域用户 seattle\david 的用户名和密码凭据与 IP 地址为10.172.63.255 的目标服务器相关联。 用户 seattle\david 对目标服务器具有读/写权限。 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将存储这些凭据，并根据备份和还原操作的需要，使用这些凭据在目标服务器上进行读取和写入。  
  
```sql  
EXEC sp_pdw_add_network_credentials '10.172.63.255', 'seattle\david', '********';  
```  
  
 Backup 命令要求以 IP 地址的形式输入服务器名称。  
  
> [!NOTE]  
>  若要通过不受带宽执行数据库备份，请确保使用备份服务器的未受控制的 IP 地址。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Azure Synapse Analytics sp_pdw_remove_network_credentials&#41;](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md)  
  
  

