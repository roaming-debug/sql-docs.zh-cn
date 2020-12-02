---
description: CREATE BROKER PRIORITY (Transact-SQL)
title: CREATE BROKER PRIORITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE BROKER PRIORITY
- PRIORITY_TSQL
- CREATE_BROKER_PRIORITY_TSQL
- PRIORITY
- CREATE BROKER
- BROKER_TSQL
- BROKER
- CREATE_BROKER_TSQL
- BROKER PRIORITY
- BROKER_PRIORITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE BROKER PRIORITY statement
ms.assetid: e0bbebfa-b7c3-4825-8169-7281f7e6de98
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9c50ce97995322a025feb1e201682717d7d2234d
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126165"
---
# <a name="create-broker-priority-transact-sql"></a>CREATE BROKER PRIORITY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  定义一个优先级别和一组条件，这组条件用于决定要将此优先级分配给哪些 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会话。 此优先级别分配给使用在会话优先级中指定的相同约定与服务组合的任何会话端点。 优先级的值范围为 1（低）到 10（高）。 默认值为 5。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE BROKER PRIORITY ConversationPriorityName  
FOR CONVERSATION  
[ SET ( [ CONTRACT_NAME = {ContractName | ANY } ]  
        [ [ , ] LOCAL_SERVICE_NAME = {LocalServiceName | ANY } ]  
        [ [ , ] REMOTE_SERVICE_NAME = {'RemoteServiceName' | ANY } ]  
        [ [ , ] PRIORITY_LEVEL = {PriorityValue | DEFAULT } ]  
       )  
]  
[;]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 ConversationPriorityName  
 指定此会话优先级的名称。 该名称在当前数据库中必须唯一，并且必须符合 [!INCLUDE[ssDE](../../includes/ssde-md.md)] [标识符](../../relational-databases/databases/database-identifiers.md)的规则。  
  
 SET  
 指定用于确定会话优先级是否应用于会话的条件。 如果指定了 SET，则它必须至少包含以下条件之一：CONTRACT_NAME、LOCAL_SERVICE_NAME、REMOTE_SERVICE_NAME 或 PRIORITY_LEVEL。 如果未指定 SET，则全部三个条件都设置为默认值。  
  
 CONTRACT_NAME = {ContractName | ANY}  
 指定要用作会话优先级是否应用于会话的判定条件的约定的名称。 ContractName 是一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 标识符，并且必须指定当前数据库中的协定的名称。  
  
 ContractName  
 指定此会话优先级只能应用于启动会话的 BEGIN DIALOG 语句指定了 ON CONTRACT ContractName 的会话。  
  
 ANY  
 指定此会话优先级可应用于任何会话，而不考虑它使用的约定如何。  
  
 默认值为 ANY。  
  
 LOCAL_SERVICE_NAME = {LocalServiceName | ANY}  
 指定要用作确定会话优先级是否应用于会话端点的条件的服务名称。  
  
 LocalServiceName 是 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 标识符。 它必须指定当前数据库中的服务的名称。  
  
 LocalServiceName  
 指定此会话优先级可以应用于以下各项：  
  
-   其发起方服务名称与 LocalServiceName 匹配的任何发起方会话端点。  
  
-   其目标服务名称与 LocalServiceName 匹配的任何目标会话端点。  
  
 ANY  
 -   指定此会话优先级可应用于任何会话端点，而不管端点使用的本地服务的名称如何。  
  
 默认值为 ANY。  
  
 REMOTE_SERVICE_NAME = {'RemoteServiceName' | ANY}  
 指定要用作确定会话优先级是否应用于会话端点的条件的服务名称。  
  
 RemoteServiceName 是 nvarchar(256) 类型的文本。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会逐字节进行比较以便与 RemoteServiceName 字符串匹配。 这种比较区分大小写，并且不考虑当前的排序规则。 目标服务可以位于当前[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中，也可以位于远程[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中。  
  
 'RemoteServiceName'  
 指定此会话优先级可以应用于以下各项：  
  
-   其关联目标服务名称与 RemoteServiceName 匹配的任何发起方会话端点。  
  
-   其关联发起方服务名称与 RemoteServiceName 匹配的任何目标会话端点。  
  
 ANY  
 指定此会话优先级可应用于任何会话端点，而不管与端点关联的远程服务的名称如何。  
  
 默认值为 ANY。  
  
 PRIORITY_LEVEL = { PriorityValue | DEFAULT }  
 指定要分配给使用在会话优先级中指定的约定和服务的任何会话端点的优先级。 PriorityValue 必须是一个从 1（优先级最低）到 10（优先级最高）的整数文本。 默认值为 5。  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 为会话端点分配优先级别。 优先级别控制与端点关联的操作的优先级。 每个会话均有两个会话端点：  
  
-   发起方会话端点将会话的一端与发起方服务和发起方队列相关联。 发起方会话端点是在运行 BEGIN DIALOG 语句时创建的。 与发起方会话端点关联的操作包括：  
  
    -   从发起方服务发送。  
  
    -   从发起方队列接收。  
  
    -   从发起方队列获取下一个会话组。  
  
-   目标会话端点将会话的另一端与目标服务和队列相关联。 目标会话端点是在使用该会话向目标队列发送消息时创建的。 与目标会话端点关联的操作包括：  
  
    -   从目标队列接收。  
  
    -   从目标服务发送。  
  
    -   从目标队列获取下一个会话组。  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 在创建会话端点时分配会话优先级别。 会话端点将优先级别一直保留到会话结束。 新的优先级或对现有优先级的更改都不会应用于现有会话。  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将协定和服务条件与端点属性最匹配的会话优先级中的优先级别分配给会话端点。 下表显示了匹配优先顺序：  
  
|运营合同|操作本地服务|操作远程服务|  
|------------------------|-----------------------------|------------------------------|  
|ContractName|LocalServiceName|RemoteServiceName|  
|ContractName|LocalServiceName|ANY|  
|ContractName|ANY|RemoteServiceName|  
|ContractName|ANY|ANY|  
|ANY|LocalServiceName|RemoteServiceName|  
|ANY|LocalServiceName|ANY|  
|ANY|ANY|RemoteServiceName|  
|ANY|ANY|ANY|  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 首先查找其指定的约定、本地服务和远程服务与操作所用的约定、本地服务和远程服务匹配的优先级。 如果找不到，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 将查找约定以及本地服务与操作所用的约定和本地服务匹配且远程服务指定为 ANY 的优先级。 这一过程将按照优先顺序表中列出的所有变化情况继续下去。 如果未找到匹配项，将为操作分配默认优先级 5。  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 独立地为每个会话端点分配优先级别。 若要让 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将优先级别同时分配给发起方会话端点和目标会话端点，必须确保会话优先级同时涵盖这两个端点。 如果发起方会话端点和目标会话端点分别在不同的数据库中，则必须在每个数据库中创建会话优先级。 通常为会话的两个会话端点指定相同的优先级别，但您也可以指定不同的优先级别。  
  
 优先级别总是应用于从队列接收消息或会话组标识符的操作。 当从一个[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例向另一个实例传输消息时也会应用优先级别。  
  
 进行以下消息传输时不使用优先级别：  
  
-   从 HONOR_BROKER_PRIORITY 数据库选项设置为 OFF 的数据库传输消息。 有关详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。  
  
-   在同一数据库引擎实例中的服务之间传输消息。  
  
-   如果未在数据库中创建任何会话优先级，则会为数据库中的所有 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 操作分配默认优先级 5。  
  
## <a name="permissions"></a>权限  
 用于创建会话优先级的权限默认授予 db_ddladmin 或 db_owner 固定数据库角色以及 sysadmin 固定服务器角色的成员。 需要对数据库拥有 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-assigning-a-priority-level-to-both-directions-of-a-conversation"></a>A. 将优先级别同时分配给会话的两个方向。  
 这两个会话优先级确保为在 `SimpleContract` 和 `TargetService` 之间使用 `InitiatorAService` 的所有操作分配优先级别 3。  
  
```sql  
CREATE BROKER PRIORITY InitiatorAToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = InitiatorServiceA,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 3);  
CREATE BROKER PRIORITY TargetToInitiatorAPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'InitiatorServiceA',  
         PRIORITY_LEVEL = 3);  
```  
  
### <a name="b-setting-the-priority-level-for-all-conversations-that-use-a-contract"></a>B. 为使用某个约定的所有会话设置优先级别  
 为使用名为 `7` 的约定的所有操作分配优先级别 `SimpleContract`。 前提是没有同时指定了 `SimpleContract` 和本地或远程服务的任何其他优先级。  
  
```sql 
CREATE BROKER PRIORITY SimpleContractDefaultPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 7);  
```  
  
### <a name="c-setting-a-base-priority-level-for-a-database"></a>C. 为数据库设置基本优先级别。  
 为两个特定服务定义会话优先级，然后定义一个将与所有其他会话端点匹配的会话优先级。 这不会替换默认优先级（默认优先级始终为 5），但会最大限度地减少为其分配默认优先级的项目数。  
  
```sql 
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/ClaimPriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = //Adventure-Works.com/Expenses/ClaimService,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 9);  
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/ApprovalPriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = //Adventure-Works.com/Expenses/ClaimService,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/BasePriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 3);  
```  
  
### <a name="d-creating-three-priority-levels-for-a-target-service-by-using-services"></a>D. 使用服务为目标服务创建三个优先级别  
 支持一个提供以下三个性能级别的系统：Gold（高）、Silver（中）和 Bronze（低）。 只有一个约定，但每个级别都有单独的发起方服务。 所有发起方服务都与一个中心目标服务通信。  
  
```sql  
CREATE BROKER PRIORITY GoldInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = GoldInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY GoldTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'GoldInitiatorService',  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY SilverInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = SilverInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY SilverTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'SilverInitiatorService',  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY BronzeInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = BronzeInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 2);  
CREATE BROKER PRIORITY BronzeTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'BronzeInitiatorService',  
         PRIORITY_LEVEL = 2);  
```  
  
### <a name="e-creating-three-priority-levels-for-multiple-services-using-contracts"></a>E. 使用约定为多个服务创建三个优先级别  
 支持一个提供以下三个性能级别的系统：Gold（高）、Silver（中）和 Bronze（低）。 每个级别都有一个单独的约定。 这些优先级适用于使用这些约定的会话所引用的任何服务。  
  
```sql  
CREATE BROKER PRIORITY GoldPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = GoldContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY SilverPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SilverContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY BronzePriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = BronzeContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 2);  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [CREATE CONTRACT (Transact-SQL)](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE QUEUE (Transact SQL)](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE SERVICE (Transact SQL)](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [GET CONVERSATION GROUP (Transact-SQL)](../../t-sql/statements/get-conversation-group-transact-sql.md)   
 [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md)   
 [SEND (Transact-SQL)](../../t-sql/statements/send-transact-sql.md)   
 [sys.conversation_priorities (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-priorities-transact-sql.md)  
  
  
