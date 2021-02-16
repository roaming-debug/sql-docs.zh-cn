---
title: clr enabled 服务器配置选项 | Microsoft Docs
description: 了解如何使用 clr enabled 选项指定 SQL Server 是否可以运行用户程序集。 了解何时不支持执行公共语言运行时。
ms.custom: ''
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- assemblies [CLR integration], verifying can run
- clr enabled option
ms.assetid: 0722d382-8fd3-4fac-b4a8-cd2b7a7e0293
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 600b70e751fe4fcd3d80c7e86bd494ecc8cf1758
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "100347028"
---
# <a name="clr-enabled-server-configuration-option"></a>clr enabled 服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  可以使用 clr enabled 选项指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]是否可以运行用户程序集。 clr enabled 选项提供下列值： 
  
|值|说明|  
|-----------|-----------------|  
|0|不允许在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]上执行程序集。|  
|1|允许在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]上执行程序集。|  
  
仅 WOW64。 重启 WOW64 服务器使设置更改生效。 其他服务器类型不需要重启。  

运行 RECONFIGURE 时，clr enabled 选项的运行值将从 1 更改为 0，所有包含用户程序集的应用程序域将立即被卸载。  
  
>  **轻型池不支持执行公共语言运行时(CLR)** 。禁用以下两个选项中的一个：“clr enabled”或“lightweight pooling”。 依赖于 CLR 并且在纤程模式下无法正常工作的功能包括：hierarchy 数据类型、`FORMAT` 函数、复制和基于策略的管理。  
> 
> [!WARNING]
>  CLR 在 .NET Framework 中使用代码访问安全性 (CAS)（不可再作为安全边界）。 使用 `PERMISSION_SET = SAFE` 创建的 CLR 程序集可以访问外部系统资源、调用非托管代码以及获取 sysadmin 特权。 从 [!INCLUDE[sssql17](../../includes/sssql17-md.md)] 开始，引入了名为 `clr strict security` 的 `sp_configure` 选项，以增强 CLR 程序集的安全性。 默认启用 `clr strict security`，并将 `SAFE` 和 `EXTERNAL_ACCESS` 程序集与标记为 `UNSAFE` 的程序集同等对待。 可禁用 `clr strict security` 选项以实现后向兼容性，但不建议这样做。 Microsoft 建议所有程序集都通过证书或非对称密钥进行签名，且该证书或非对称密钥具有已在主数据库中获得 `UNSAFE ASSEMBLY` 权限的相应登录名。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员还可以将程序集添加到数据库引擎应信任的程序集列表。 有关详细信息，请参阅 [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md)。
  
## <a name="example"></a>示例  
 下面的示例首先显示 clr enabled 选项的当前设置，然后通过将选项值设置为 1 启用该选项。 若要禁用该选项，请将此值设置为 0。  
  
```sql  
EXEC sp_configure 'clr enabled';  
EXEC sp_configure 'clr enabled' , '1';  
RECONFIGURE;    
```  
  
## <a name="see-also"></a>另请参阅  
 [lightweight pooling 服务器配置选项](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [lightweight pooling 服务器配置选项](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)  
  
  
