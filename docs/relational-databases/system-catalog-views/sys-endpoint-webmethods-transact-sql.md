---
description: sys.endpoint_webmethods (Transact-SQL)
title: sys.endpoint_webmethods (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.endpoint_webmethods_TSQL
- sys.endpoint_webmethods
- endpoint_webmethods_TSQL
- sys.http_soap_methods_TSQL
- endpoint_webmethods
- sys.http_soap_methods
dev_langs:
- TSQL
helpviewer_keywords:
- sys.endpoint_webmethods catalog view
ms.assetid: 7dad0cf6-eafa-47cf-98cc-75ba8d3c7959
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1c57d8319df8feb793587dc07d6d55248a0fca12
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100607"
---
# <a name="sysendpoint_webmethods-transact-sql"></a>sys.endpoint_webmethods (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 在启用了 SOAP 的 HTTP 端点上定义的每个 SOAP 方法各占一行。 endpoint_id 和 namespace 列的组合是唯一的。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|endpoint_id|**int**|定义了 Web 方法的端点的 ID。|  
|命名空间|**nvarchar (384)**|Web 方法的命名空间。|  
|method_alias|**nvarchar (64)**|方法的别名。<br /><br /> 注意： [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符允许在 WSDL 方法名称中不合法的字符。<br /><br /> 别名用于将端点 WSDL 说明中公开的名称映射到调用 Web 方法时调用的实际基本 [!INCLUDE[tsql](../../includes/tsql-md.md)] 可执行对象。|  
|object_name|**nvarchar (776)**|Web 方法重定向到的对象名称，在 NAME = 选项中指定。 名称部分以句点 )  ( 分隔，并使用方括号分隔 `[``]` 。<br /><br /> 对象名称必须由三个部分组成，在 WSDL 选项中指定。|  
|result_schema|**tinyint**|确定将哪个 XSD（如果有）与响应一起发回的选项。<br /><br /> 0 = 无<br /><br /> 1 = Standard<br /><br /> 2 = Default|  
|result_schema_desc|**nvarchar(60)**|确定将哪个 XSD（如果有）与响应一起发回的选项的说明。<br /><br /> NONE<br /><br /> STANDARD<br /><br /> DEFAULT|  
|result_format|**tinyint**|确定如何在响应中设置结果格式的选项。<br /><br /> 1 = ALL_RESULTS<br /><br /> 2 = ROWSETS_ONLY<br /><br /> 3 = NONE|  
|result_format_desc|**nvarchar(60)**|确定如何在响应中设置结果格式的选项的说明。<br /><br /> ALL_RESULTS<br /><br /> ROWSETS_ONLY<br /><br /> NONE|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [终结点目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
