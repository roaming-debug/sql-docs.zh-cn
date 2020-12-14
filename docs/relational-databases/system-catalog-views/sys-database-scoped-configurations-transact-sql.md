---
description: 'sys.database_scoped_configurations (Transact-sql) '
title: sys.database_scoped_configurations (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- database_scoped_configurations
- database_scoped_configurations_TSQL
- sys.database_scoped_configurations
- sys.database_scoped_configurations_TSQL
helpviewer_keywords:
- sys.database_scoped_configurations catalog view
dev_langs:
- TSQL
ms.assetid: 8899310a-3464-4d38-9f2f-88396c4e7dc2
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||= azure-sqldw-latest
ms.openlocfilehash: beb536c39db6dbee3bc2e0855de20282bc994fe4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97404713"
---
# <a name="sysdatabase_scoped_configurations-transact-sql"></a>sys.database_scoped_configurations (Transact-sql) 

[!INCLUDE[tsql-appliesto-ss2016-asdb-addw-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

每个配置占一行。 

|列名称|数据类型|说明|
|-----------------|---------------|-----------------|
|**configuration_id**|**int**|配置选项的 ID。|
|**name**|**nvarchar(60)**|配置选项的名称。 有关可能的配置的信息，请参阅 [ALTER DATABASE 作用域配置 &#40;transact-sql&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)。|
|**value**|**sqlvariant**|为主副本的此配置选项设置的值。|
|**value_for_secondary**|**sqlvariant**|为辅助副本的此配置选项设置的值。|
|**is_value_default**|**bit** |指定设置的值是否为默认值。|

## <a name="permissions"></a><a name="Permissions"></a> 权限

要求 **公共** 角色具有成员身份。

## <a name="remarks"></a>备注

如果返回 NULL 作为 **value_for_secondary** 的值，这意味着辅助数据库将设置为 PRIMARY。
 
数据库作用域内配置设置随数据库一同转入。 这意味着还原或附加给定数据库时，会保留现有配置设置。

## <a name="see-also"></a>另请参阅

[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)
