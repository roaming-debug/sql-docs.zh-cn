---
description: sys.dm_exec_xml_handles (Transact-SQL)
title: sys.dm_exec_xml_handles (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_xml_handles
- dm_exec_xml_handles_TSQL
- sys.dm_exec_xml_handles_TSQL
- sys.dm_exec_xml_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_xml_handles dynamic management function
ms.assetid: a873ce0f-6955-417a-96a1-b2ef11a83633
author: pmasl
ms.author: pelopes
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3e8529f7626ffa25d86cbb73c704198082e32bcf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208060"
---
# <a name="sysdm_exec_xml_handles-transact-sql"></a>sys.dm_exec_xml_handles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  返回 **sp_xml_preparedocument** 打开的活动句柄的相关信息。  
  
## <a name="syntax"></a>语法  
  
```  
  
dm_exec_xml_handles (session_id | 0 )  
```  
  
## <a name="arguments"></a>参数  
 *session_id* |0  
 会话的 ID。 如果指定了 *session_id* ，则此函数将返回有关指定会话中的 XML 句柄的信息。  
  
 如果指定 0，该函数将返回所有会话中的所有 XML 句柄的信息。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|持有 XML 文档句柄的会话的会话 ID。|  
|**document_id**|**int**|**Sp_xml_preparedocument** 返回的 XML 文档句柄 ID。|  
|**namespace_document_id**|**int**|用于关联命名空间文档的内部句柄 ID，已作为第三个参数传递给 **sp_xml_preparedocument**。 如果没有命名空间文档，则为 NULL。|  
|**sql_handle**|**varbinary(64)**|定义句柄所在的 SQL 代码的文本句柄。|  
|**statement_start_offset**|**int**|在当前正在执行的批处理或存储过程中， **sp_xml_preparedocument** 调用发生的字符数。 可以与 **sql_handle**、 **statement_end_offset** 和 **sys.dm_exec_sql_text** 动态管理函数一起使用，以检索请求的当前正在执行的语句。|  
|**statement_end_offset**|**int**|在当前正在执行的批处理或存储过程中， **sp_xml_preparedocument** 调用发生的字符数。 可以与 **sql_handle**、 **statement_start_offset** 和 **sys.dm_exec_sql_text** 动态管理函数一起使用，以检索请求的当前正在执行的语句。|  
|**creation_time**|**datetime**|调用 **sp_xml_preparedocument** 时的时间戳。|  
|**original_document_size_bytes**|**bigint**|未分析的 XML 文档的大小（字节）。|  
|**original_namespace_document_size_bytes**|**bigint**|未分析的 XML 命名空间文档的大小（字节）。 如果没有命名空间文档，则为 NULL。|  
|**num_openxml_calls**|**bigint**|具有该文档句柄的 OPENXML 调用数。|  
|**row_count**|**bigint**|该文档句柄以前的所有 OPENXML 调用返回的行数。|  
|**dormant_duration_ms**|**bigint**|自上次 OPENXML 调用以来经过的时间（毫秒）。 如果尚未调用 OPENXML，则将返回 **sp_xml_preparedocumen** t 调用后的毫秒数。|  
  
## <a name="remarks"></a>备注  
 **Sql_handles** 的生存期用于检索 sql 文本，该文本执行对用于执行查询的缓存计划的调用 **sp_xml_preparedocument** 比用于满足。 如果查询文本在缓存中不可用，则无法使用函数结果中提供的信息来检索数据。 如果您正在运行多个大型批处理，则可能出现上述情况。  
  
## <a name="permissions"></a>权限  
 要求对服务器拥有 VIEW SERVER STATE 权限，以查看不归调用者所有的全部会话或会话 ID。 调用者始终可以查看自己的当前会话 ID 的数据。      
  
## <a name="examples"></a>示例  
 以下示例将选择所有活动句柄。  
  
```  
SELECT * FROM sys.dm_exec_xml_handles(0);  
```  
  
## <a name="see-also"></a>另请参阅  
 <br>[动态管理视图和函数 (Transact-sql) ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
 <br>[与执行相关的动态管理视图和函数 (Transact-sql) ](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
 <br>[sp_xml_preparedocument (Transact-sql) ](../system-stored-procedures/sp-xml-preparedocument-transact-sql.md)
 <br>[sp_xml_removedocument (Transact-SQL)](../system-stored-procedures/sp-xml-removedocument-transact-sql.md)


 
  
  
