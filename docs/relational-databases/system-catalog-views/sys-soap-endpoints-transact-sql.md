---
description: sys.soap_endpoints (Transact-SQL)
title: sys.soap_endpoints (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- soap_endpoints_TSQL
- sys.soap_endpoints
- soap_endpoints
- sys.soap_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.soap_endpoints catalog view
ms.assetid: f50dcbfc-02ed-4a19-9c07-c78a5a1b3224
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a0a41f08d7600ae25541924953c407ef12a2ab20
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096694"
---
# <a name="syssoap_endpoints-transact-sql"></a>sys.soap_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 携带 SOAP 类型负载的服务器中的每个端点在该表中对应一行。 对于此视图中的每一行，都有一个对应的行，其中包含具有 HTTP 配置元数据的 **sys.http_endpoints** 目录视图中的相同 **endpoint_id** 。  
  
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**< 继承列>**||有关此视图所继承的列的列表，请参阅 [sys.databases &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)。|  
|**is_sql_language_enabled**|**bit**|1 = 指定了 BATCHES = ENABLED 选项，表示端点上允许使用即席 SQL 批。|  
|**wsdl_generator_procedure**|**nvarchar (776)**|实现此方法的存储过程的三部分名称。<br /><br /> 方法名称要求使用严格的三部分语法。 不允许使用一部分、两部分或四部分名称。|  
|**default_database**|**sysname**|在 DATABASE = 选项中给定的默认数据库名称。<br /><br /> NULL = 指定了 DEFAULT。|  
|**default_namespace**|**nvarchar (384)**|命名空间 = 选项中指定的默认命名空间，如果改为指定默认命名空间，则为 `https://tempuri.org` 。|  
|**default_result_schema**|**tinyint**|SCHEMA = 选项的默认值。<br /><br /> 0 = NONE<br /><br /> 1 = STANDARD|  
|**default_result_schema_desc**|**nvarchar(60)**|对 SCHEMA = 选项的默认值的说明。<br /><br /> NONE<br /><br /> STANDARD|  
|**is_xml_charset_enforced**|**bit**|0 = 指定了 CHARACTER_SET = SQL 选项。<br /><br /> 1 = 指定了 CHARACTER_SET = XML 选项。|  
|**is_session_enabled**|**bit**|0 = 指定了 SESSION = DISABLE 选项。<br /><br /> 1 = 指定了 SESSION = ENABLED 选项。|  
|**session_timeout**|**int**|在 SESSION_TIMEOUT = 选项中指定的值。|  
|**login_type**|**nvarchar(60)**|此端点允许的身份验证类型。<br /><br /> WINDOWS<br /><br /> MIXED|  
|**header_limit**|**int**|允许的最大 SOAP 标头大小。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [终结点目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
