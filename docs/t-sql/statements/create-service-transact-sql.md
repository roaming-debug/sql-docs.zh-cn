---
description: CREATE SERVICE (Transact-SQL)
title: CREATE SERVICE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SERVICE_TSQL
- SERVICE
- CREATE SERVICE
- SERVICE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- services [Service Broker], creating
- CREATE SERVICE statement
- contracts [Service Broker], service creation
ms.assetid: fb804fa2-48eb-4878-a12f-4e0d5f4bc9e3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3d7c8981e7b8d79f9819c6df4327037f0d2e930b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179156"
---
# <a name="create-service-transact-sql"></a>CREATE SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建一项新服务。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务是指某个特定任务或一组任务的名称。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 使用服务的名称路由消息、将消息传递到数据库中的正确队列，以及强制执行会话的约定。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE SERVICE service_name  
   [ AUTHORIZATION owner_name ]  
   ON QUEUE [ schema_name. ]queue_name  
   [ ( contract_name | [DEFAULT][ ,...n ] ) ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *service_name*  
 要创建的服务的名称。 新服务在当前数据库中创建，由 AUTHORIZATION 子句中指定的主体数据库所有。 不能指定服务器、数据库和架构名称。 service_name 必须是有效的 sysname。  
  
> [!NOTE]  
> 不要创建将关键字 ANY 用于 service_name 的服务。 在 `CREATE BROKER PRIORITY` 中为服务名称指定 `ANY` 时，优先级被视为针对所有服务。 此情况不限于名称为 ANY 的服务。  
  
 AUTHORIZATION owner_name   
 将服务所有者设置为指定的数据库用户或角色。 如果当前用户为 dbo 或 sa，则 owner_name 可能为任意有效用户或角色的名称。 否则，owner_name 必须是当前用户的名称，或者是当前用户对其有 IMPERSONATE 权限的用户的名称，或者是当前用户所属的角色的名称  。  
  
 ON QUEUE [ schema_name. ] queue_name  
 指定接收服务消息的队列。 该队列必须与服务在同一数据库中。 如果未提供 schema_name，则架构为执行该语句的用户的默认架构。  
  
 contract_name  
 指定针对此服务的约定。 服务程序使用指定的约定来启动与此服务的会话。 如果未指定约定，则该服务可能仅启动会话。  
  
 [DEFAULT]   
 指定该服务可作为遵循 DEFAULT 约定的会话的目标。 在此子句的上下文中，DEFAULT 不是关键字，必须作为标识符进行分隔。 DEFAULT 约定允许会话的双方发送 DEFAULT 类型的消息。 DEFAULT 消息类型使用 NONE 验证。  
  
## <a name="remarks"></a>备注  
 服务公开与其关联的约定提供的功能，以便其他服务可使用该功能。 `CREATE SERVICE` 语句指定针对此服务的约定。 一个服务只能是使用该服务指定的约定会话的目标。 未指定约定的服务不会向其他服务公开任何功能。  
  
 从此服务启动的会话可使用任何约定。 如果服务仅启动会话，则创建服务时可不指定约定。  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 从远程服务接受新会话时，目标服务的名称决定了 Broker 在会话中放入消息的队列。  
  
## <a name="permissions"></a>权限  
 默认情况下，`db_ddladmin` 或 `db_owner` 固定数据库角色和 `sysadmin` 固定服务器角色的成员拥有创建服务的权限。 执行 `CREATE SERVICE` 语句的用户必须对队列和指定的所有约定具有 `REFERENCES` 权限。  
  
 默认情况下，服务的所有者、`db_ddladmin` 或 `db_owner` 固定数据库角色的成员以及 `sysadmin` 固定服务器角色的成员拥有该服务的 `REFERENCES` 权限。 默认情况下，服务所有者、`db_owner` 固定数据库角色的成员以及 `sysadmin` 固定服务器角色的成员拥有服务的 `SEND` 权限。  
  
 服务不能是临时对象。 允许服务名称以 # 开头，但仅限于永久对象。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-service-with-one-contract"></a>A. 创建具有一个约定的服务  
 以下示例在 `//Adventure-Works.com/Expenses` 架构中的 `ExpenseQueue` 队列中创建服务 `dbo`。 以此服务为目标的对话必须遵循约定 `//Adventure-Works.com/Expenses/ExpenseSubmission`。  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses]  
    ON QUEUE [dbo].[ExpenseQueue]  
    ([//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
### <a name="b-creating-a-service-with-multiple-contracts"></a>B. 创建具有多个约定的服务  
 以下示例在 `//Adventure-Works.com/Expenses` 队列中创建服务 `ExpenseQueue`。 以此服务为目标的对话必须遵循约定 `//Adventure-Works.com/Expenses/ExpenseSubmission` 或约定 `//Adventure-Works.com/Expenses/ExpenseProcessing`。  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses] ON QUEUE ExpenseQueue  
    ([//Adventure-Works.com/Expenses/ExpenseSubmission],  
     [//Adventure-Works.com/Expenses/ExpenseProcessing]) ;  
```  
  
### <a name="c-creating-a-service-with-no-contracts"></a>C. 创建无约定的服务  
 以下示例创建 `//Adventure-Works.com/Expenses on the ExpenseQueue` 队列。 此服务没有约定信息。 因此，该服务只能启动对话。  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses] ON QUEUE ExpenseQueue ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER SERVICE (Transact-SQL)](../../t-sql/statements/alter-service-transact-sql.md)   
 [DROP SERVICE (Transact-SQL)](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
