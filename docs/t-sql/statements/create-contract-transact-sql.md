---
description: CREATE CONTRACT (Transact-SQL)
title: CREATE CONTRACT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONTRACT_TSQL
- CREATE_CONTRACT_TSQL
- CREATE CONTRACT
- CONTRACT
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE CONTRACT statement
- contracts [Service Broker], creating
- message types [Service Broker], contracts
ms.assetid: 494cbfa6-8e93-4161-a64d-90d681915211
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7cf2fa8811bad953b14810e3119a4841c9ec8f1f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192758"
---
# <a name="create-contract-transact-sql"></a>CREATE CONTRACT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建新约定。 约定用于定义在 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会话中所使用的消息类型，还用于确定会话的哪一端可以发送该类型的消息。 每个会话都要遵循一个约定。 当会话开始时，启动服务为会话指定约定。 目标服务指定该目标服务将接受其会话的约定。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
CREATE CONTRACT contract_name  
   [ AUTHORIZATION owner_name ]  
      (  {   { message_type_name | [ DEFAULT ] }  
          SENT BY { INITIATOR | TARGET | ANY }   
       } [ ,...n] )   
[ ; ]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 contract_name  
 要创建的约定的名称。 新约定创建于当前数据库中，并且由 AUTHORIZATION 子句中所指定的主体拥有。 不能指定服务器、数据库和架构名称。 contract_name 最多可具有 128 个字符。  
  
> [!NOTE]  
>  不要创建将关键字 ANY 用于 contract_name 的约定。 在 CREATE BROKER PRIORITY 中为约定名称指定 ANY 时，优先级将被视为针对所有约定。 此情况不限于名称为 ANY 的约定。  
  
 AUTHORIZATION owner_name   
 将约定的所有者设置为指定的数据库用户或角色。 如果当前用户为 dbo 或 sa，则 owner_name 可以为任意有效用户或角色的名称。 否则，owner_name 必须是当前用户的名称，或者是当前用户对其有模拟权限的用户的名称，或者是当前用户所属的角色的名称。 如果省略此子句，则约定属于当前用户。  
  
 *message_type_name*  
 要作为约定的一部分所包括的消息类型的名称。  
  
 SENT BY  
 指定哪个端点可以发送所指示的消息类型的消息。 约定将记录服务可以用来拥有特定会话的消息。 每个会话都有两个端点：发起程序端点（启动会话的服务）和目标端点（发起程序要联系的服务）。  
  
 INITIATOR  
 指示只有会话的发起方才能发送指定消息类型的消息。 启动会话的服务称为会话的发起程序。  
  
 TARGET  
 指示只有会话的目标才能发送指定消息类型的消息。 接受由另一个服务启动的会话的服务称为会话的目标。  
  
 ANY  
 指示发起方和目标都可以发送此类型的消息。  
  
 [ DEFAULT ]  
 指示此约定支持默认消息类型的消息。 默认情况下，所有数据库都包含名为 DEFAULT 的消息类型。 此消息类型使用的验证为 NONE。 在此子句的上下文中，DEFAULT 不是关键字，必须作为标识符进行分隔。 Microsoft SQL Server 还提供了 DEFAULT 约定，该约定指定 DEFAULT 消息类型。  
  
## <a name="remarks"></a>备注  
 在约定中消息类型的顺序并不重要。 目标收到第一条消息之后，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 允许会话的任一端在任何时候发送允许该会话端发送的任何消息。 例如，如果会话发起程序可以发送消息类型 //Adventure-Works.com/Expenses/SubmitExpense，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 允许发起程序在会话过程中发送任意数量的 SubmitExpense 消息 。  
  
 无法更改约定中的消息类型和方向。 若要更改约定的 AUTHORIZATION，请使用 ALTER AUTHORIZATION 语句。  
  
 约定必须允许发起方发送消息。 如果约定未包含至少一个 SENT BY ANY 或 SENT BY INITIATOR 消息类型，CREATE CONTRACT 语句将失败。  
  
 服务始终可以接收消息类型 `https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer`、`https://schemas.microsoft.com/SQL/ServiceBroker/Error` 和 `https://schemas.microsoft.com/SQL/ServiceBroker/EndDialog`，而无需考虑约定。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将这些消息类型用于发送到应用程序的系统消息。  
  
 约定不能是临时对象。 约定名称允许以 # 开头，但应当是永久对象。  
  
## <a name="permissions"></a>权限  
 默认情况下，db_ddladmin 或 db_owner 固定数据库角色的成员以及 sysadmin 固定服务器角色的成员可以创建约定。  
  
 默认情况下，约定的所有者、db_ddladmin 或 db_owner 固定数据库角色的成员以及 sysadmin 固定服务器角色的成员对约定拥有 REFERENCES 权限。  
  
 执行 CREATE CONTRACT 语句的用户必须对所指定的所有消息类型拥有 REFERENCES 权限。  
  
## <a name="examples"></a>示例  
 **A. 创建约定**  
  
 以下示例基于三个消息类型创建费用归还约定。  
  
```sql  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]           
    VALIDATION = WELL_FORMED_XML ;           
  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/ExpenseApprovedOrDenied]           
    VALIDATION = WELL_FORMED_XML ;           
  
CREATE MESSAGE TYPE           
    [//Adventure-Works.com/Expenses/ExpenseReimbursed]           
    VALIDATION= WELL_FORMED_XML ;           
  
CREATE CONTRACT            
    [//Adventure-Works.com/Expenses/ExpenseSubmission]           
    ( [//Adventure-Works.com/Expenses/SubmitExpense]           
          SENT BY INITIATOR,           
      [//Adventure-Works.com/Expenses/ExpenseApprovedOrDenied]           
          SENT BY TARGET,           
      [//Adventure-Works.com/Expenses/ExpenseReimbursed]           
          SENT BY TARGET           
    ) ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [DROP CONTRACT (Transact-SQL)](../../t-sql/statements/drop-contract-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
