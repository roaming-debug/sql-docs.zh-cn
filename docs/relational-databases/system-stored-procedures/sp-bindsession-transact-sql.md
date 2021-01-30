---
description: sp_bindsession (Transact-SQL)
title: sp_bindsession (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_bindsession
- sp_bindsession_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindsession
ms.assetid: 1436fe21-ad00-4a98-aca1-1451a5e571d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 64f6b6b4134c5f29dea38d87e7d75e43910ca530
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206612"
---
# <a name="sp_bindsession-transact-sql"></a>sp_bindsession (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将会话绑定到同一个实例中的其他会话或解除绑定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。 绑定会话允许两个或更多的会话参与同一事务并共享锁，直到发出 ROLLBACK TRANSACTION 或 COMMIT TRANSACTION 命令。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用多个活动结果集 (MARS) 或分布式事务。 有关详细信息，请参阅[使用多个活动的结果集 (MARS)](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_bindsession { 'bind_token' | NULL }  
```  
  
## <a name="arguments"></a>参数  
 **"** *bind_token* **"**  
 标识最初使用 **sp_getbindtoken** 或开放式数据服务 **srv_getbindtoken** 函数获取的事务的标记。 *bind_token* 是 **varchar (255)**。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 绑定的两个会话只共享事务和锁。 每个会话均保留自身的隔离级别，在一个会话上设置新的隔离级别不会影响另一个会话的隔离级别。 每个会话仍由其安全帐户标识，且只能访问已授权该帐户访问的数据库资源。  
  
 **sp_bindsession** 使用绑定令牌绑定两个或更多个现有客户端会话。 这些客户端会话必须位于获得绑定令牌的同一[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中。 会话是执行命令的客户端。 绑定数据库会话共享事务和锁空间。  
  
 从一个[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例获得的绑定令牌不能用于连接到另一实例的客户端会话，甚至对 DTC 事务也是如此。 绑定令牌仅在每个实例的本地有效，不能由多个实例共享。 若要将客户端会话绑定到的另一个实例 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，则必须通过执行 **sp_getbindtoken** 获取不同的绑定令牌。  
  
 如果 **sp_bindsession** 将失败，并在使用非活动令牌时出现错误。  
  
 使用 **sp_bindsession** 取消绑定，无需指定 *bind_token* 或通过在 *bind_token* 中传递 NULL。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下示例将指定的绑定令牌绑定到当前会话。  
  
> [!NOTE]  
>  示例中显示的绑定令牌是通过在执行 **sp_bindsession** 之前执行 **sp_getbindtoken** 获取的。  
  
```  
USE master;  
GO  
EXEC sp_bindsession 'BP9---5---->KB?-V'<>1E:H-7U-]ANZ';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_getbindtoken (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-getbindtoken-transact-sql.md)   
 [扩展存储过程 API srv_getbindtoken &#40;&#41;](../../relational-databases/extended-stored-procedures-reference/srv-getbindtoken-extended-stored-procedure-api.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
