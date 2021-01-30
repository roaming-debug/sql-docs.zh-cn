---
description: sp_validname (Transact-SQL)
title: sp_validname (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_validname
- sp_validname_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_validname
ms.assetid: d51c53c2-1332-407f-b725-4983f2e710eb
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9bb57f55751cbf29f7bef5c704da3aae363976db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201860"
---
# <a name="sp_validname-transact-sql"></a>sp_validname (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  检查有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符名称。 所有非二进制和非零数据（包括可使用 **nchar**、 **nvarchar** 或 **Ntext** 数据类型存储的 Unicode 数据）都作为标识符名称的有效字符接受。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_validname [@name =] 'name'   
     [, [@raise_error =] raise_error]  
```  
  
## <a name="arguments"></a>参数  
`[ @name = ] 'name'` 要检查其有效性的 [标识符](../../relational-databases/databases/database-identifiers.md) 的名称。 *名称* 为 **sysname**，无默认值。 *名称* 不能为 NULL，不能为空字符串，并且不能包含二进制零字符。  
  
`[ @raise_error = ] raise_error` 指定是否引发错误。 *raise_error* 为 **bit**，默认值为1。 这表示将显示错误。 0 表示不会显示错误消息。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [NCHAR (Transact-SQL)](../../t-sql/functions/nchar-transact-sql.md)   
 [nchar 和 nvarchar &#40;Transact-sql&#41;](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)   
 [ntext、text 和 image &#40;Transact-sql&#41;](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
