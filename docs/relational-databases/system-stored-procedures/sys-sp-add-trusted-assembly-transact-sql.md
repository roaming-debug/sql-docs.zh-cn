---
description: sys.sp_add_trusted_assembly (Transact-SQL)
title: sys.sp_add_trusted_assembly (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/14/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_trusted_assembly_TSQL
- sp_add_trusted_assembly
- sys.sp_add_trusted_assembly_TSQL
- sys.sp_add_trusted_assembly
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_add_trusted_assembly
ms.assetid: ''
author: VanMSFT
ms.author: vanto
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3ae54693fcefff29423092a08feafd12587d4c1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190536"
---
# <a name="syssp_add_trusted_assembly-transact-sql"></a>sys.sp_add_trusted_assembly (Transact-SQL)  
[!INCLUDE[tsql-appliesto-ss2017-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdbmi-xxxx-xxx-md.md)]

将程序集添加到服务器的受信任程序集的列表中。

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  


## <a name="syntax"></a>语法
```  
sp_add_trusted_assembly 
    [ @hash = ] 'value'
    [ , [ @description = ] 'description' ]
```  

## <a name="remarks"></a>备注  

此过程将程序集添加到  [sys.trusted_assemblies](../../relational-databases/system-catalog-views/sys-trusted-assemblies-transact-sql.md)。

## <a name="arguments"></a>参数

[ @hash =] "*value*"  
要添加到服务器的受信任程序集列表的程序集的 SHA2_512 哈希值。 即使程序集未签名或数据库未标记为可信，也可以在启用 [CLR 严格安全](../../database-engine/configure-windows/clr-strict-security.md) 时加载受信任的程序集。

[ @description =] "*description*"  
程序集的可选用户定义说明。 Microsoft 建议使用规范名称对要信任的程序集的简单名称、版本号、区域性、公钥和体系结构进行编码。 此值在公共语言运行时 (CLR) 端唯一地标识程序集，该程序集与 sys.databases 中的 clr_name 值相同。 

## <a name="permissions"></a>权限

要求具有 `sysadmin` 固定服务器角色或权限的成员身份 `CONTROL SERVER` 。

## <a name="examples"></a>示例  

下面的示例将名为的程序集添加 `pointudt` 到服务器的受信任程序集的列表中。 可以从  [sys.databases](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md)获取这些值。     

```  
EXEC sp_add_trusted_assembly 0x8893AD6D78D14EE43DF482E2EAD44123E3A0B684A8873C3F7BF3B5E8D8F09503F3E62370CE742BBC96FE3394477214B84C7C1B0F7A04DCC788FA99C2C09DFCCC, 
N'pointudt, version=0.0.0.0, culture=neutral, publickeytoken=null, processorarchitecture=msil';
```  

## <a name="see-also"></a>另请参阅  
  [sys.sp_drop_trusted_assembly](sys-sp-drop-trusted-assembly-transact-sql.md)  
  [sys.trusted_assemblies](../../relational-databases/system-catalog-views/sys-trusted-assemblies-transact-sql.md)  
  [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md)  
  [CLR 严格安全性](../../database-engine/configure-windows/clr-strict-security.md)  
  [sys.assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md)  
  [sys.dm_clr_loaded_assemblies](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)  

