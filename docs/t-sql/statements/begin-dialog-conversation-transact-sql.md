---
description: BEGIN DIALOG CONVERSATION (Transact-SQL)
title: BEGIN DIALOG CONVERSATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DIALOG CONVERSATION
- DIALOG
- BEGIN_DIALOG_TSQL
- BEGIN_DIALOG_CONVERSATION_TSQL
- DIALOG_CONVERSATION_TSQL
- DIALOG_TSQL
- BEGIN DIALOG
- BEGIN DIALOG CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker]
- beginning dialogs
- dialogs [Service Broker], beginning
- BEGIN DIALOG CONVERSATION statement
- cryptography [SQL Server], conversations
- opening conversations
- encryption [SQL Server], conversations
- starting conversations
ms.assetid: 8e814f9d-77c1-4906-b8e4-668a86fc94ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fdf0f646d2df5c8b0c2fc748973378426a0b96b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184464"
---
# <a name="begin-dialog-conversation-transact-sql"></a>BEGIN DIALOG CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  启动从一个服务到另一个服务的对话。 所谓对话，就是让两个服务能够进行一次顺序消息传递。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
BEGIN DIALOG [ CONVERSATION ] @dialog_handle  
   FROM SERVICE initiator_service_name  
   TO SERVICE 'target_service_name'  
       [ , { 'service_broker_guid' | 'CURRENT DATABASE' }]   
   [ ON CONTRACT contract_name ]  
   [ WITH  
   [  { RELATED_CONVERSATION = related_conversation_handle   
      | RELATED_CONVERSATION_GROUP = related_conversation_group_id } ]   
   [ [ , ] LIFETIME = dialog_lifetime ]   
   [ [ , ] ENCRYPTION = { ON | OFF }  ] ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **@** _dialog_handle_  
 一个变量，用于为 BEGIN DIALOG CONVERSATION 语句返回的新对话存储系统生成的对话句柄。 该变量的类型必须为 uniqueidentifier。  
  
 FROM SERVICE *initiator_service_name*  
 指定启动对话的服务。 指定的名称必须是当前数据库中的服务的名称。 为发起方服务指定的队列将接收由目标服务返回的消息，以及 Service Broker 为此会话创建的消息。  
  
 TO SERVICE 'target_service_name'  
 指定启动对话时的目标服务。 *target_service_name* 的类型为 **nvarchar(256)**。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会逐字节进行比较以便与 target_service_name 字符串匹配。 换言之，比较时将区分大小写，且不考虑当前的排序规则。  
  
 *service_broker_guid*  
 指定承载目标服务的数据库。 如果有多个数据库承载目标服务实例，则可通过提供 *service_broker_guid* 与特定数据库通信。  
  
 *service_broker_guid* 的类型为 **nvarchar(128)**。 若要查找数据库的 *service_broker_guid*，请在数据库中运行以下查询：  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID() ;  
```  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
 **'** CURRENT DATABASE **'**  
 指定会话使用当前数据库的 *service_broker_guid*。  
  
 ON CONTRACT *contract_name*  
 指定此会话遵循的约定。 当前数据库中必须有该约定。 如果目标服务不接受遵循指定约定的新会话，则 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将返回针对该会话的错误消息。 如果省略此子句，则会话会遵循名为 **DEFAULT** 的约定。  
  
 RELATED_CONVERSATION =related_conversation_handle  
 指定将新对话添加到的现有会话组。 如果使用该子句，则新对话与 *related_conversation_handle* 指定的对话属于同一个会话组。 *related_conversation_handle* 的类型必须可隐式转换为类型 **uniqueidentifier**。 如果 *related_conversation_handle* 不引用现有的对话，则该语句将失败。  
  
 RELATED_CONVERSATION_GROUP =related_conversation_group_id  
 指定将新对话添加到的现有会话组。 如果使用该子句，则新对话将添加到 *related_conversation_group_id* 指定的会话组中。 *related_conversation_group_id* 的类型必须可隐式转换为类型 **uniqueidentifier**。 如果 *related_conversation_group_id* 不引用现有的会话组，Service Broker 会使用指定的 *related_conversation_group_id* 创建一个新的会话组，并将新对话与该会话组关联。  
  
 LIFETIME =dialog_lifetime  
 指定对话将保持打开状态的最长时间。 为使对话成功完成，两个端点都必须在生存期内显式结束对话。 *dialog_lifetime* 的值必须以秒表示。 生存期的类型为 **int**。如果未指定 LIFETIME 子句，则对话的生存期为 **int** 数据类型的最大值。  
  
 ENCRYPTION  
 指定在将此对话发送和接收的消息发送到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例外部时是否必须进行加密。 必须加密的对话是 *安全对话*。 如果 ENCRYPTION = ON，但未配置支持加密所需的证书，则 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将返回针对该会话的错误消息。 ENCRYPTION = OFF 时，如果为 *target_service_name* 配置了远程服务绑定，则使用加密；否则，发送消息时不加密。 如果未使用此子句，则默认值为 ON。  
  
> [!NOTE]  
>  在任何情况下，都不对同一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的服务间交换的消息加密。 但是，如果用于会话的服务位于不同的数据库，则使用加密的会话仍然需要数据库主密钥和加密证书。 这样，在会话进行的过程中，如果将一个数据库移到其他实例，会话仍可继续进行。  
  
## <a name="remarks"></a>注解  
 所有消息都是会话的一部分。 因此，发起方服务必须在发送消息到目标服务之前启动与目标服务的会话。 BEGIN DIALOG CONVERSATION 语句中指定的信息类似于信函上的地址；[!INCLUDE[ssSB](../../includes/sssb-md.md)] 使用此信息将消息传递到正确的服务。 TO SERVICE 子句中指定的服务是消息发送到的地址。 FROM SERVICE 子句中指定的服务是用于答复消息的返回地址。  
  
 会话目标不需要调用 BEGIN DIALOG CONVERSATION。 当会话中来自发起方的第一条消息到达时，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 将在目标数据库中创建一个会话。  
  
 启动一个对话将在发起服务的数据库中创建一个会话端点，但不创建与承载目标服务的实例的网络连接。 在发送第一条消息之前，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 不会建立与对话目标的通信。  
  
 如果 BEGIN DIALOG CONVERSATION 语句未指定相关的会话或相关的会话组，则 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将为新的会话创建一个新的会话组。  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 不允许将会话任意分组。 会话组中的所有会话都必须有 FROM 子句中指定的服务作为会话的发起方或目标。  
  
 BEGIN DIALOG CONVERSATION 命令将锁定包含返回的 *dialog_handle* 的会话组。 如果该命令包含 RELATED_CONVERSATION_GROUP 子句，则 *dialog_handle* 的会话组为 *related_conversation_group_id* 参数中指定的会话组。 如果该命令包含 RELATED_CONVERSATION 子句，则 *dialog_handle* 的会话组为与指定的 *related_conversation_handle* 关联的会话组。  
  
 BEGIN DIALOG CONVERSATION 在用户定义函数中无效。  
  
## <a name="permissions"></a>权限  
 若要启动对话，则当前的用户对于在该命令的 FROM 子句中指定的服务的队列必须有 RECEIVE 权限，对于指定的约定有 REFERENCES 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-beginning-a-dialog"></a>A. 启动对话  
 以下实例会启动一个对话会话，并将对话的标识符存储于 `@dialog_handle.`。`//Adventure-Works.com/ExpenseClient` 服务是对话的发起方，`//Adventure-Works.com/Expenses` 服务是对话的目标。 对话遵循约定 `//Adventure-Works.com/Expenses/ExpenseSubmission`。  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
### <a name="b-beginning-a-dialog-with-an-explicit-lifetime"></a>B. 启动具有显式生存期的对话  
 以下实例将启动一个对话会话，并将对话的标识符存储于 `@dialog_handle`。 `//Adventure-Works.com/ExpenseClient` 服务是对话的发起方，`//Adventure-Works.com/Expenses` 服务是对话的目标。 对话遵循约定 `//Adventure-Works.com/Expenses/ExpenseSubmission`。 如果 END CONVERSATION 命令在 `60` 秒内未关闭此对话，则 Broker 将结束此对话并返回错误。  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH LIFETIME = 60 ;  
```  
  
### <a name="c-beginning-a-dialog-with-a-specific-broker-instance"></a>C. 启动具有特定 Broker 实例的对话  
 以下实例将启动一个对话会话，并将对话的标识符存储于 `@dialog_handle`。 `//Adventure-Works.com/ExpenseClient` 服务是对话的发起方，`//Adventure-Works.com/Expenses` 服务是对话的目标。 对话遵循约定 `//Adventure-Works.com/Expenses/ExpenseSubmission`。 Broker 将此对话的消息路由到由 GUID `a326e034-d4cf-4e8b-8d98-4d7e1926c904.` 标识的 Broker。  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses',   
              'a326e034-d4cf-4e8b-8d98-4d7e1926c904'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
### <a name="d-beginning-a-dialog-and-relating-it-to-an-existing-conversation-group"></a>D. 启动一个对话，并将其关联到现有的会话组  
 以下实例将启动一个对话会话，并将对话的标识符存储于 `@dialog_handle`。 `//Adventure-Works.com/ExpenseClient` 服务是对话的发起方，`//Adventure-Works.com/Expenses` 服务是对话的目标。 对话遵循约定 `//Adventure-Works.com/Expenses/ExpenseSubmission`。 Broker 将此对话与由 `@conversation_group_id` 标识的会话组关联，而不创建新的会话组。  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_group_id = <retrieve conversation group ID from database>  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH RELATED_CONVERSATION_GROUP = @conversation_group_id ;  
```  
  
### <a name="e-beginning-a-dialog-with-an-explicit-lifetime-and-relating-the-dialog-to-an-existing-conversation"></a>E. 启动一个具有显式生存期的对话，并将其关联到现有会话  
 以下实例将启动一个对话会话，并将对话的标识符存储于 `@dialog_handle`。 `//Adventure-Works.com/ExpenseClient` 服务是对话的发起方，`//Adventure-Works.com/Expenses` 服务是对话的目标。 对话遵循约定 `//Adventure-Works.com/Expenses/ExpenseSubmission`。 新对话与 `@existing_conversation_handle` 属于同一个会话组。 如果 END CONVERSATION 命令在 `600` 秒内未关闭此对话，则 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将结束此对话并返回错误。  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER  
DECLARE @existing_conversation_handle UNIQUEIDENTIFIER  
  
SET @existing_conversation_handle = <retrieve conversation handle from database>  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH RELATED_CONVERSATION = @existing_conversation_handle  
   LIFETIME = 600 ;  
```  
  
### <a name="f-beginning-a-dialog-with-optional-encryption"></a>F. 启动具有可选加密的对话  
 以下实例将启动一个对话，并将对话的标识符存储于 `@dialog_handle`。 `//Adventure-Works.com/ExpenseClient` 服务是对话的发起方，`//Adventure-Works.com/Expenses` 服务是对话的目标。 对话遵循约定 `//Adventure-Works.com/Expenses/ExpenseSubmission`。 如果加密不可用，则此示例中的会话允许消息在不加密的状态下通过网络传输。  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH ENCRYPTION = OFF ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN CONVERSATION TIMER (Transact-SQL)](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [END CONVERSATION (Transact-SQL)](../../t-sql/statements/end-conversation-transact-sql.md)   
 [MOVE CONVERSATION (Transact-SQL)](../../t-sql/statements/move-conversation-transact-sql.md)   
 [sys.conversation_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
