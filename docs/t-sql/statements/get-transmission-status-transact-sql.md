---
description: GET_TRANSMISSION_STATUS (Transact-SQL)
title: GET_TRANSMISSION_STATUS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STATUS_TSQL
- TRANSMISSION
- TRANSMISSION_TSQL
- GET_TRANSMISSION_STATUS
- STATUS
- GET_TRANSMISSION_STATUS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker], transmission status
- Service Broker errors, transmission status
- transmission status information
- status information [SQL Server], conversations
- GET_TRANSMISSION_STATUS statement
ms.assetid: 621805d5-49ed-4764-b3cb-2ae4a3bf797e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 09666ba2e475f2f830654402603e4abde798e7dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186647"
---
# <a name="get_transmission_status-transact-sql"></a>GET_TRANSMISSION_STATUS (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回某一会话方上次传输的状态。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
GET_TRANSMISSION_STATUS ( conversation_handle )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *conversation_id*  
 会话的会话句柄。 此参数的类型为 uniqueidentifier。  
  
## <a name="return-types"></a>返回类型  
 **nchar**  
  
## <a name="remarks"></a>备注  
 返回一个字符串，该字符串说明指定会话上次传输尝试的状态。 在下列情况下都将返回一个空字符串：上次传输尝试成功；尚未进行任何传输尝试；或 conversation_handle 不存在。  
  
 该函数返回的信息与管理视图 sys.transmission_queue 的 last_transmission_error 列中显示的信息相同。 但是，该函数可用于查找那些当前传输队列中没有消息的会话的传输状态。  
  
> [!NOTE]  
>  GET_TRANSMISSION_STATUS 不提供在当前实例中没有会话端点的消息的信息。 也就是说，不提供任何有关要转发的消息的信息。  
  
## <a name="examples"></a>示例  
 下面的示例报告会话句柄为 `58ef1d2d-c405-42eb-a762-23ff320bddf0` 的会话的传输状态。  
  
```sql  
SELECT Status =  
    GET_TRANSMISSION_STATUS('58ef1d2d-c405-42eb-a762-23ff320bddf0') ;  
```  
  
 下面是示例结果集，由于行的长度原因而进行了编辑：  
  
 ```
 Status  
 ------------------------------- 
 The Service Broker protocol transport is disabled or not configured.
 ```  
  
 在这种情况下，没有将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置为允许 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 通过网络进行通信。  
  
## <a name="see-also"></a>另请参阅  
 [sys.conversation_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)   
 [sys.transmission_queue (Transact-SQL)](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)  
  
  
