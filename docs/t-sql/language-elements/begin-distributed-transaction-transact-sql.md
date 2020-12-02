---
description: BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)
title: BEGIN DISTRIBUTED TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/29/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DISTRIBUTED
- BEGIN_DISTRIBUTED_TRANSACTION_TSQL
- DISTRIBUTED_TSQL
- BEGIN DISTRIBUTED TRANSACTION
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN DISTRIBUTED TRANSACTION statement
- distributed transactions [SQL Server], starting
- MS DTC, transaction management
- distributed transactions [SQL Server], BEGIN DISTRIBUTED TRANSACTION statement
- servers [SQL Server], distributed transactions
- starting distributed transactions
- remote servers [SQL Server], distributed transactions
- starting transactions
ms.assetid: c3bc2716-39d3-4061-8c6a-8734899231ac
author: rothja
ms.author: jroth
ms.openlocfilehash: 757e838efe95130f745cf8966789cab4432bc0d5
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124528"
---
# <a name="begin-distributed-transaction-transact-sql"></a>BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  指定一个由 [!INCLUDE[tsql](../../includes/tsql-md.md)] 分布式事务处理协调器 (MS DTC) 管理的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务的起点。  
    
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
BEGIN DISTRIBUTED { TRAN | TRANSACTION }   
     [ transaction_name | @tran_name_variable ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 transaction_name  
 用户定义的事务名，用于跟踪 MS DTC 实用工具中的分布式事务。 transaction_name 必须符合标识符规则，字符数必须 \<= 32。  
  
 @*tran_name_variable*  
 用户定义的一个变量名，它含有一个事务名，该事务名用于跟踪 MS DTC 实用工具中的分布式事务。 必须使用 char、varchar、nchar 或 nvarchar 数据类型声明该变量   。  
  
## <a name="remarks"></a>注解  
 执行 BEGIN DISTRIBUTED TRANSACTION 语句的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例是事务创建者，并控制事务的完成。 当为会话发出后续 COMMIT TRANSACTION 或 ROLLBACK TRANSACTION 语句时，控制实例请求 MS DTC 在所涉及的所有实例间管理分布式事务的完成。  
  
 事务级别的快照隔离不支持分布式事务。  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的远程实例登记到分布式事务中的主要方法是当已在分布式事务中登记的会话执行引用链接服务器的分布式查询时。  
  
 例如，如果在服务器 A 上发出 BEGIN DISTRIBUTED TRANSACTION，则该会话将调用服务器 B 上的一个存储过程和服务器 C 上的另一个存储过程。 服务器 C 上的存储过程执行针对服务器 D 的分布式查询，这样该分布式事务将涉及所有四台计算机。 服务器 A 上的[!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例是该事务的初始控制实例。  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 分布式事务涉及的会话并不获取可以传递给另一个会话的事务对象，从而也不能将其显式登记在分布式事务中。 远程服务器登记到事务中的唯一方法是成为分布式查询或远程存储过程调用的目标。  
  
 在本地事务中执行分布式查询时，如果目标 OLE DB 数据源支持 ITransactionLocal，则该事务被自动提升为分布式事务。 如果目标 OLE DB 数据源不支持 ITransactionLocal，则只允许在分布式查询中执行只读操作。  
  
 已在分布式事务中登记的会话执行一个引用远程服务器的远程存储过程调用。  
  
 sp_configure remote proc trans 选项控制对本地事务中的远程存储过程调用是否自动使本地事务被提升为由 MS DTC 管理的分布式事务。 连接级别 SET 选项 REMOTE_PROC_TRANSACTIONS 可用于覆盖由 sp_configure remote proc trans 建立的实例默认值。启用此选项后，远程存储过程调用会导致一个本地事务提升为分布式事务。 创建 MS DTC 事务的连接成为该事务的创建者。 COMMIT TRANSACTION 初始化一个 MS DTC 协调的提交。 如果启用了 sp_configure remote proc trans 选项，本地事务中的远程存储过程调用将被自动保护，成为分布式事务的一部分，而不需要重写应用程序以便专门发出 BEGIN DISTRIBUTED TRANSACTION 而不是 BEGIN TRANSACTION。  
  
 有关分布式事务环境和处理的详细信息，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器文档。  
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples"></a>示例  
 该示例从[!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]的本地实例和远程服务器的实例上的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 数据库中同时删除候选项。 本地和远程数据库都将提交或回滚本事务。  
  
> [!NOTE]  
>  除非正在运行[!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例的计算机中当前安装了 MS DTC，否则本示例会产生错误消息。 有关安装 MS DTC 的详细信息，请参见 Microsoft 分布式事务处理协调器文档。  
  
```sql  
USE AdventureWorks2012;  
GO  
BEGIN DISTRIBUTED TRANSACTION;  
-- Delete candidate from local instance.  
DELETE AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
-- Delete candidate from remote instance.  
DELETE RemoteServer.AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT TRANSACTION;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION (Transact-SQL)](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK (Transact-SQL)](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION (Transact-SQL)](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK (Transact-SQL)](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION (Transact-SQL)](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
