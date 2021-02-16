---
title: 配置 Always Encrypted 的 enclave 类型服务器配置选项 | Microsoft Docs
description: 了解如何启用或禁用 Always Encrypted 的安全 enclave。 了解如何确认 enclave 是否已正确初始化。
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: a35a08152dcb1c551c493ac88b0073f07e1280dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059482"
---
# <a name="configure-the-enclave-type-for-always-encrypted-server-configuration-option"></a>配置 Always Encrypted 的 enclave 类型服务器配置选项

[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

本文介绍如何为具有安全 enclave 的 Always Encrypted 启用或禁用安全 enclave。 有关详细信息，请参阅[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md) 和 [在 SQL Server 中配置安全 enclave](../../relational-databases/security/encryption/always-encrypted-enclaves-configure-enclave-type.md)。

列加密 enclave 类型服务器配置选项控制用于 Always Encrypted 的安全 enclave 类型。 该选项可以设置为以下值之一：  
  
|值|说明|  
|-------------------|-----------------| 
|0|没有安全 enclave。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 不会为 Always Encrypted 初始化安全 enclave。 因此，具有安全 enclave 的 Always Encrypted 功能将不可用。|  
|1|基于虚拟化的安全性 (VBS)。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 会尝试初始化基于虚拟化的安全性 (VBS) enclave。

> [!IMPORTANT]
> 对列加密 enclave 类型的更改直到重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例才会生效。
   
可以使用 [sys.configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) 视图检查配置的 enclave 类型值和当前生效的 enclave 类型值。 

若要确认是否在上次重新启动 [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] 之后已正确初始化了具有当前生效类型（大于 0）的 enclave，请检查 [sys.dm_column_encryption_enclave (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-column-encryption-enclave.md) 视图：
 - 如果该视图恰好包含一行，则 enclave 已正确初始化。 
 - 如果该视图不包含任何行，请检查 SQL Server 错误日志中是否存在 enclave 初始化错误 - 请参阅[查看 SQL Server 错误日志 (SQL Server Management Studio)](../../relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md)。

有关如何配置 VBS enclave 的分步说明，请参阅[在 SQL Server 中启用具有安全 enclave 的 Always Encrypted](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md#step-3-enable-always-encrypted-with-secure-enclaves-in-sql-server)。

## <a name="examples"></a>示例  
 以下示例启用安全 enclave 并将 enclave 类型设置为 VBS：

```sql  
sp_configure 'column encryption enclave type', 1;  
GO  
RECONFIGURE;  
GO  
```  

以下示例禁用安全 enclave：  

```sql  
sp_configure 'column encryption enclave type', 0;  
GO  
RECONFIGURE;  
GO  
```  

以下查询检索配置的 enclave 类型和当前生效的 enclave 类型：

```sql  
USE [master];
GO
SELECT
[value]
, CASE [value] WHEN 0 THEN 'No enclave' WHEN 1 THEN 'VBS' ELSE 'Other' END AS [value_description]
, [value_in_use]
, CASE [value_in_use] WHEN 0 THEN 'No enclave' WHEN 1 THEN 'VBS' ELSE 'Other' END AS [value_in_use_description]
FROM sys.configurations
WHERE [name] = 'column encryption enclave type'; 
```  
## <a name="next-steps"></a>后续步骤
 [管理具有安全 enclave 的 Always Encrypted 的密钥](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)

## <a name="see-also"></a>另请参阅  
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sys.configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.dm_column_encryption_enclave (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-column-encryption-enclave.md)   
