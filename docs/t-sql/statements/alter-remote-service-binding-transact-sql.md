---
description: ALTER REMOTE SERVICE BINDING (Transact-SQL)
title: ALTER REMOTE SERVICE BINDING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER REMOTE SERVICE BINDING
- ALTER_REMOTE_SERVICE_BINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote service bindings [Service Broker], modifying
- ALTER REMOTE SERVICE BINDING statement
- modifying remote service bindings
ms.assetid: ee620b4a-9375-4eaa-a016-69916c9e1e68
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f7f8c056a12c2ae18bad5c5fc733a95d290c3b2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204078"
---
# <a name="alter-remote-service-binding-transact-sql"></a>ALTER REMOTE SERVICE BINDING (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改与远程服务绑定相关联的用户，或更改绑定的匿名身份验证设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER REMOTE SERVICE BINDING binding_name   
   WITH [ USER = <user_name> ] [ , ANONYMOUS = { ON | OFF } ]   
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 binding_name  
 要更改的远程服务绑定的名称。 不能指定服务器、数据库和架构名称。  
  
 WITH USER = \<*user_name>*  
 指定数据库用户，该用户持有与此绑定的远程服务相关联的证书。 此证书的公钥用于对与远程服务交换的消息进行加密和身份验证。  
  
 ANONYMOUS  
 指定在与远程服务进行通信时是否使用匿名身份验证。 如果 ANONYMOUS = ON，则使用匿名身份验证，且不会将本地用户的凭据传输给远程服务。 如果 ANONYMOUS = OFF，则传输用户凭据。 如果没有指定该子句，则默认为 OFF。  
  
## <a name="remarks"></a>备注  
 与 user_name 关联的证书中的公钥用于对发送到远程服务的消息进行身份验证，并对会话密钥进行加密，然后使用加密的会话密钥对会话进行加密。 user_name 的证书必须与承载远程服务的数据库登录证书相对应。  
  
## <a name="permissions"></a>权限  
 默认情况下，远程服务绑定所有者、db_owner 固定数据库角色成员以及 sysadmin 固定服务器角色成员具有更改远程服务绑定的权限 。  
  
 执行 ALTER REMOTE SERVICE BINDING 语句的用户必须具有该语句中所指定用户的模拟权限。  
  
 若要更改远程服务绑定的 AUTHORIZATION，请使用 ALTER AUTHORIZATION 语句。  
  
## <a name="examples"></a>示例  
 下面的示例使用帐户 `APBinding` 的证书将远程服务绑定 `SecurityAccount` 更改为加密消息。  
  
```sql  
ALTER REMOTE SERVICE BINDING APBinding  
    WITH USER = SecurityAccount ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE REMOTE SERVICE BINDING (Transact-SQL)](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [DROP REMOTE SERVICE BINDING (Transact-SQL)](../../t-sql/statements/drop-remote-service-binding-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
