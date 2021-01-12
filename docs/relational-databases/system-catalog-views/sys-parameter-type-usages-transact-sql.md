---
description: sys.parameter_type_usages (Transact-SQL)
title: sys.parameter_type_usages (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.parameter_type_usages
- sys.parameter_type_usages_TSQL
- parameter_type_usages_TSQL
- parameter_type_usages
dev_langs:
- TSQL
helpviewer_keywords:
- sys.parameter_type_usages catalog view
ms.assetid: af0e167b-bffb-4525-84ec-3607f9268d3d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d4504c8c85852b418c5958dff144e8aa17eb50aa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096758"
---
# <a name="sysparameter_type_usages-transact-sql"></a>sys.parameter_type_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为每个用户定义类型的参数返回一行。  
  
> [!NOTE]  
>  对于带编号过程的参数，此视图不返回行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|此参数所属对象的 ID。|  
|**parameter_id**|**int**|参数的 ID。 在对象中是唯一的。|  
|**user_type_id**|**int**|用户定义类型的 ID。<br /><br /> 若要返回类型的名称，请在此列上联接到 [sys.databases](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) 目录视图。|  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的标量类型目录视图 ](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
