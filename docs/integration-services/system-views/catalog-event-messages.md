---
description: catalog.event_messages
title: catalog.event_messages | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: a31a654f-31e9-4da1-aabf-182b07848e36
author: chugugrace
ms.author: chugu
ms.openlocfilehash: da96f49143a237741722c37cb9eda82754b24f52
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835338"
---
# <a name="catalogevent_messages"></a>catalog.event_messages 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  显示有关在操作期间已记入日志的消息的信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|Event_message_ID|bigint|事件消息的唯一 ID。|  
|Operation_id|bigint|操作的类型。<br /><br /> 有关操作类型的列表，请参阅 [catalog.operations（SSISDB 数据库）](../../integration-services/system-views/catalog-operations-ssisdb-database.md)。|  
|Message_time|datetimeoffset(7)|创建消息的时间。|  
|Message_type|smallint|所显示的消息的类型。 有关消息类型的详细信息，请参阅 [catalog.operation_messages（SSISDB 数据库）](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)。|  
|Message_source_type|smallint|消息的源。|  
|消息|nvarchar(max)|消息的文本。|  
|Extended_info_id|bigint|与在 [catalog.extended_operation_info（SSISDB 数据库）](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md)视图中找到的操作消息相关的附加信息的 ID。|  
|Package_name|nvarchar(260)|包文件的名称。|  
|Event_name|nvarchar(1024)|与消息关联的运行时事件。|  
|Message_source_name|nvarchar(4000)|作为消息源的包组件。|  
|Message_source_id|nvarchar(38)|消息源的唯一 ID。|  
|Subcomponent_name|nvarchar(4000)|作为消息源的数据流组件。<br /><br /> 当 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 引擎返回消息时，SSIS.Pipeline 将出现在该列中。|  
|Package_path|nvarchar(max)|包内组件的唯一路径。|  
|Execution_path|nvarchar(max)|从父包到执行该组件的点的完整路径。<br /><br /> 此路径还捕获组件的迭代。|  
|threadID|int|将消息记入日志时正在执行的线程的 ID。|  
|Message_code|int|与此消息关联的代码。|  
  
## <a name="remarks"></a>备注  
 此视图显示下列消息源类型。  
  
|**message_source_type**|描述|  
|-------------------------------|-----------------|  
|10|入口 API，如 T-SQL 和 CLR 存储过程|  
|20|用来运行包的外部进程 (ISServerExec.exe)|  
|30|包级别对象|  
|40|控制流任务|  
|50|控制流容器|  
|60|数据流任务|  
  
## <a name="permissions"></a>权限  
 此视图需要下列权限之一：  
  
-   针对操作的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格。  
  
-   **sysadmin** 服务器角色的成员资格。  
  
## <a name="see-also"></a>另请参阅  
 [catalog.event_message_context](../../integration-services/system-views/catalog-event-message-context.md)  
  
  
