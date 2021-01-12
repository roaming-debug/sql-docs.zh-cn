---
description: sys.assembly_references (Transact-SQL)
title: sys.assembly_references (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- assembly_references
- sys.assembly_references_TSQL
- assembly_references_TSQL
- sys.assembly_references
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assembly_references catalog view
ms.assetid: 50a5ed42-2d5b-4a11-a0d2-9a02241b078d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 278214acd7a8a0fd0b78fb8bb77d30e73f68000c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099230"
---
# <a name="sysassembly_references-transact-sql"></a>sys.assembly_references (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对于一个直接引用另一个的每一对程序集，相应地包含一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|该引用所属程序集的 ID。|  
|**referenced_assembly_id**|**int**|被引用的程序集的 ID。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 CLR 程序集目录视图 ](../../relational-databases/system-catalog-views/clr-assembly-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ASSEMBLYPROPERTY &#40;Transact-sql&#41;](../../t-sql/functions/assemblyproperty-transact-sql.md)  
  
  
