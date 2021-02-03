---
description: SET REMOTE_PROC_TRANSACTIONS (Transact-SQL)
title: SET REMOTE_PROC_TRANSACTIONS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REMOTE_PROC_TRANSACTIONS_TSQL
- SET REMOTE_PROC_TRANSACTIONS
- REMOTE_PROC_TRANSACTIONS
- SET_REMOTE_PROC_TRANSACTIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote stored procedures [SQL Server]
- SET REMOTE_PROC_TRANSACTIONS statement
- distributed transactions [SQL Server], starting
- REMOTE_PROC_TRANSACTIONS option
- active transactions
ms.assetid: 4d284ae9-3f5f-465a-b0dd-1328a4832a03
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6164638072a8cc41c07bb46e0337a0d3be5498cc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206972"
---
# <a name="set-remote_proc_transactions-transact-sql"></a>SET REMOTE_PROC_TRANSACTIONS (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  指定当本地事务处于活动状态时，如果执行远程存储过程，将启动由 [!INCLUDE[tsql](../../includes/tsql-md.md)] 分布式事务处理协调器 (MS DTC) 管理的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] 提供此选项的目的在于为使用远程存储过程的应用程序提供向后兼容性。 不发出远程存储过程调用，而是使用引用链接服务器的分布式查询。 这些服务器是使用 [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) 定义的。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SET REMOTE_PROC_TRANSACTIONS { ON | OFF }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 ON | OFF  
 设置为 ON 时，从本地事务执行远程存储过程时将启动 [!INCLUDE[tsql](../../includes/tsql-md.md)] 分布式事务。 设置为 OFF 时，从本地事务调用远程存储过程将不启动 [!INCLUDE[tsql](../../includes/tsql-md.md)] 分布式事务。  
  
## <a name="remarks"></a>备注  
 当 REMOTE_PROC_TRANSACTIONS 设置为 ON 时，调用远程存储过程将启动分布式事务，并用 MS DTC 登记该事务。 调用远程存储过程的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是事务创建者，负责控制事务的完成。 当为连接发出后续 COMMIT TRANSACTION 或 ROLLBACK TRANSACTION 语句时，主控实例请求 MS DTC 在所涉及的计算机间管理分布式事务的完成。  
  
 在启动 [!INCLUDE[tsql](../../includes/tsql-md.md)] 分布式事务后，可以对已定义为远程服务器的其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例调用远程存储过程。 远程服务器全部登记在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 分布式事务中，而 MS DTC 确保在每台远程服务器上完成该事务。  
  
 REMOTE_PROC_TRANSACTIONS 是可用于覆盖实例级 **sp_configure remote proc trans** 选项的连接级设置。  
  
 当 REMOTE_PROC_TRANSACTIONS 为 OFF 时，远程存储过程调用不能成为本地事务的一部分。 远程存储过程所做的修改将在存储过程完成时提交或回滚。 由调用远程存储过程的连接发出的后续 COMMIT TRANSACTION 或 ROLLBACK TRANSACTION 语句对该过程所做的处理无效。  
  
 REMOTE_PROC_TRANSACTIONS 选项是一个兼容性选项，只影响对使用 **sp_addserver** 定义为远程服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例所进行的远程存储过程调用。 该选项不适用于在使用 **sp_addlinkedserver** 定义为链接服务器的实例上执行存储过程的分布式查询。  
  
 SET REMOTE_PROC_TRANSACTIONS 的设置是在执行或运行时设置，而不是在分析时设置。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
