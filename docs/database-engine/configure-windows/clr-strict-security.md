---
title: CLR 严格安全性 | Microsoft Docs
description: 了解如何在 SQL Server 中配置公共语言运行时 (CLR) 严格安全性。 控制 SAFE、EXTERNAL ACCESS 和 UNSAFE 权限的解释。
ms.custom: ''
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- clr strict security
- clr_strict_security_TSQL
- clr strict security
- strict_security_TSQL
helpviewer_keywords:
- assemblies [CLR integration], strick security
- clr strict security option
ms.assetid: ''
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 77d91e7c53b866cdaa6330d6ea728df2f4743037
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347020"
---
# <a name="clr-strict-security"></a>CLR 严格安全性   
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

控制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中 `SAFE`、`EXTERNAL ACCESS`、`UNSAFE` 权限的解释。   

|值 |说明 | 
|----- |----- | 
|0 |已禁用 - 为后向兼容而提供。 不建议使用 `Disabled` 值。 | 
|1 |已启用 - 导致 [!INCLUDE[ssde-md](../../includes/ssde-md.md)] 忽略程序集上的 `PERMISSION_SET` 信息，并始终将其解释为 `UNSAFE`。  从 [!INCLUDE[sssql17](../../includes/sssql17-md.md)] 开始，默认值为 `Enabled`。 | 

> [!WARNING]
>  CLR 在 .NET Framework 中使用代码访问安全性 (CAS)（不可再作为安全边界）。 使用 `PERMISSION_SET = SAFE` 创建的 CLR 程序集可以访问外部系统资源、调用非托管代码以及获取 sysadmin 特权。 从 [!INCLUDE[sssql17](../../includes/sssql17-md.md)] 开始，引入了名为 `clr strict security` 的 `sp_configure` 选项，以增强 CLR 程序集的安全性。 默认启用 `clr strict security`，并将 `SAFE` 和 `EXTERNAL_ACCESS` 程序集与标记为 `UNSAFE` 的程序集同等对待。 可禁用 `clr strict security` 选项以实现后向兼容性，但不建议这样做。 Microsoft 建议所有程序集都通过证书或非对称密钥进行签名，且该证书或非对称密钥具有已在主数据库中获得 `UNSAFE ASSEMBLY` 权限的相应登录名。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员还可以将程序集添加到数据库引擎应信任的程序集列表。 有关详细信息，请参阅 [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md)。

## <a name="remarks"></a>备注   

启用时，`CREATE ASSEMBLY` 和 `ALTER ASSEMBLY` 语句中的 `PERMISSION_SET` 选项在运行时将被忽略，但元数据中将保留 `PERMISSION_SET` 选项。 忽略选项可最大程度减少中断现有代码语句。

`CLR strict security` 是一种 `advanced option`。  

> [!IMPORTANT]
>  启用严格安全性后，未签名的任何程序集都将加载失败。 必须更改或删除并重新创建每个程序集，以便使用具有相应登录名（该登录名对应于服务器上的 `UNSAFE ASSEMBLY` 权限）的证书或非对称密钥对程序集进行签名。

## <a name="permissions"></a>权限 

### <a name="to-change-this-option"></a>更改此选项  
要求具有 `CONTROL SERVER` 权限，或者具有 `sysadmin` 固定服务器角色的成员身份。

### <a name="to-create-an-clr-assembly"></a>创建 CLR 程序集   
启用 `CLR strict security` 时，创建 CLR 程序集需要以下权限：

- 用户必须具有 `CREATE ASSEMBLY` 权限  
- 并且，还必须满足以下条件之一：  
  - 使用具有相应登录名（该登录名对应于服务器上的 `UNSAFE ASSEMBLY` 权限）的证书或非对称密钥对程序集进行签名。 建议对程序集进行签名。  
  - 数据库的 `TRUSTWORTHY` 属性设置为 `ON`，且数据库由在服务器上具有 `UNSAFE ASSEMBLY` 权限的登录名所有。 不建议使用此选项。  

  
## <a name="see-also"></a>另请参阅  
  
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [“启用 CLR”服务器配置选项](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)
