---
description: ALTER BROKER PRIORITY (Transact-SQL)
title: ALTER BROKER PRIORITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_BROKER_TSQL
- ALTER BROKER PRIORITY
- ALTER BROKER
- ALTER_BROKER_PRIORITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER BROKER PRIORITY statement
- ssbdiagnose
ms.assetid: 15fda1b2-e4dd-4f9d-935a-2e38926075b2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4f6d90dce7df2acb45c28ac7ebb74c4d03817fec
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124302"
---
# <a name="alter-broker-priority-transact-sql"></a>ALTER BROKER PRIORITY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会话优先级的属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
ALTER BROKER PRIORITY ConversationPriorityName  
FOR CONVERSATION  
{ SET ( [ CONTRACT_NAME = {ContractName | ANY } ]  
        [ [ , ] LOCAL_SERVICE_NAME = {LocalServiceName | ANY } ]  
        [ [ , ] REMOTE_SERVICE_NAME = {'RemoteServiceName' | ANY } ]  
        [ [ , ] PRIORITY_LEVEL = { PriorityValue | DEFAULT } ]  
              )  
}  
[;]  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 ConversationPriorityName  
 指定要更改的会话优先级的名称。 名称必须引用当前数据库中的一个会话优先级。  
  
 SET  
 指定用于确定会话优先级是否应用于会话的条件。 SET 是必需的并且必须至少包含一个条件：CONTRACT_NAME、LOCAL_SERVICE_NAME、REMOTE_SERVICE_NAME 或 PRIORITY_LEVEL。  
  
 CONTRACT_NAME = {ContractName | ANY}  
 指定要用作会话优先级是否应用于会话的判定条件的约定的名称。 ContractName 是一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 标识符，并且必须指定当前数据库中的协定的名称。  
  
 ContractName  
 指定此会话优先级只能应用于启动会话的 BEGIN DIALOG 语句指定了 ON CONTRACT ContractName 的会话。  
  
 ANY  
 指定此会话优先级可应用于任何会话，而不考虑它使用的约定如何。  
  
 如果未指定 CONTRACT_NAME，则会话优先级的约定属性不会更改。  
  
 LOCAL_SERVICE_NAME = {LocalServiceName | ANY}  
 指定要用作确定会话优先级是否应用于会话端点的条件的服务名称。  
  
 LocalServiceName 是一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 标识符，并且必须指定当前数据库中的服务的名称。  
  
 LocalServiceName  
 指定此会话优先级可以应用于以下各项：  
  
-   其发起方服务名称与 LocalServiceName 匹配的任何发起方会话端点。  
  
-   其目标服务名称与 LocalServiceName 匹配的任何目标会话端点。  
  
 ANY  
 -   指定此会话优先级可应用于任何会话端点，而不管端点使用的本地服务的名称如何。  
  
 如果未指定 LOCAL_SERVICE_NAME，则会话优先级的本地服务属性不会更改。  
  
 REMOTE_SERVICE_NAME = {'RemoteServiceName' | ANY}  
 指定要用作确定会话优先级是否应用于会话端点的条件的服务名称。  
  
 RemoteServiceName 是 nvarchar(256) 类型的文本。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会逐字节进行比较以便与 RemoteServiceName 字符串匹配。 这种比较区分大小写，并且不考虑当前的排序规则。 目标服务可以位于当前[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中，也可以位于远程[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中。  
  
 'RemoteServiceName'  
 指定将会话优先级分配给以下各项：  
  
-   其关联目标服务名称与 RemoteServiceName 匹配的任何发起方会话端点。  
  
-   其关联发起方服务名称与 RemoteServiceName 匹配的任何目标会话端点。  
  
 ANY  
 指定会话优先级应用于任何会话端点，而不管与端点关联的远程服务的名称如何。  
  
 如果未指定 REMOTE_SERVICE_NAME，则会话优先级的远程服务属性不会更改。  
  
 PRIORITY_LEVEL = { PriorityValue | DEFAULT }  
 指定要分配给使用在会话优先级中指定的约定和服务的任何会话端点的优先级。 PriorityValue 必须是一个从 1（优先级最低）到 10（优先级最高）的整数文本。  
  
 如果未指定 PRIORITY_LEVEL，则会话优先级的优先级属性不会更改。  
  
## <a name="remarks"></a>备注  
 由 ALTER BROKER PRIORITY 更改的任何属性都不会应用于现有会话。 现有会话将继续使用启动时所分配的优先级。  
  
 有关详细信息，请参阅 [CREATE BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/create-broker-priority-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 创建会话优先级的权限默认授予 db_ddladmin 或 db_owner 固定数据库角色以及 sysadmin 固定服务器角色的成员  。 需要对数据库拥有 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-changing-only-the-priority-level-of-an-existing-conversation-priority"></a>A. 只更改现有会话优先级的优先级。  
 更改优先级，但不更改约定、本地服务或远程服务属性。  
  
```sql  
ALTER BROKER PRIORITY SimpleContractDefaultPriority  
    FOR CONVERSATION  
    SET (PRIORITY_LEVEL = 3);  
```  
  
### <a name="b-changing-all-of-the-properties-of-an-existing-conversation-priority"></a>B. 更改现有会话优先级的所有属性。  
 更改优先级、约定、本地服务和远程服务属性。  
  
```sql  
ALTER BROKER PRIORITY SimpleContractPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContractB,  
         LOCAL_SERVICE_NAME = TargetServiceB,  
         REMOTE_SERVICE_NAME = N'InitiatorServiceB',  
         PRIORITY_LEVEL = 8);  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [DROP BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [sys.conversation_priorities (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-priorities-transact-sql.md)  
  
  
