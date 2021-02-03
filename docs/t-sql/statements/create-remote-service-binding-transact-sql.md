---
description: CREATE REMOTE SERVICE BINDING (Transact-SQL)
title: CREATE REMOTE SERVICE BINDING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE REMOTE SERVICE BINDING
- SERVICE_BINDING_TSQL
- CREATE REMOTE SERVICE
- REMOTE_TSQL
- CREATE_REMOTE_SERVICE_BINDING_TSQL
- CREATE_REMOTE_SERVICE_TSQL
- BINDING
- SERVICE BINDING
- BINDING_TSQL
- CREATE_REMOTE_TSQL
- REMOTE_SERVICE_TSQL
- CREATE REMOTE
- REMOTE SERVICE
- REMOTE_SERVICE_BINDING_TSQL
- REMOTE
- REMOTE SERVICE BINDING
dev_langs:
- TSQL
helpviewer_keywords:
- binding remote service [Service Broker]
- dialog security [Service Broker]
- end-to-end security [Service Broker]
- security [Service Broker], remote service bindings
- CREATE REMOTE SERVICE BINDING statement
- conversation security [Service Broker]
- remote service bindings [Service Broker], creating
ms.assetid: 4165c404-4d50-4063-9a6e-6e267d309376
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 13abf5a16b800bc856d482fbc525a833b9d7e872
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188520"
---
# <a name="create-remote-service-binding-transact-sql"></a>CREATE REMOTE SERVICE BINDING (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建一个绑定，以用于定义在启动与远程服务的会话时要使用的安全凭据。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE REMOTE SERVICE BINDING binding_name   
   [ AUTHORIZATION owner_name ]   
   TO SERVICE 'service_name'   
   WITH  USER = user_name [ , ANONYMOUS = { ON | OFF } ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 binding_name  
 要创建的远程服务绑定的名称。 不能指定服务器、数据库和架构名称。 binding_name 必须是有效的 sysname。  
  
 AUTHORIZATION owner_name   
 将绑定的所有者设置为指定的数据库用户或角色。 如果当前用户为 dbo 或 sa，则 owner_name 可以为任意有效用户或角色的名称。 否则，owner_name 必须是当前用户的名称，或者是当前用户对其有 IMPERSONATE 权限的用户的名称，或者是当前用户所属的角色的名称。  
  
 TO SERVICE 'service_name'  
 指定要绑定到在 WITH USER 子句中标识的用户的远程服务。  
  
 USER = user_name  
 指定拥有与 TO SERVICE 子句所标识远程服务关联的证书的数据库主体。 使用该证书对与远程服务交换的消息进行加密和身份验证。  
  
 ANONYMOUS  
 指定在与远程服务进行通信时是否使用匿名身份验证。 如果 ANONYMOUS 为 ON，则使用匿名身份验证，并且作为 public 固定数据库角色的成员执行远程数据库中的操作。 如果 ANONYMOUS 为 OFF，则作为该数据库中的特定用户执行远程数据库中的操作。 如果没有指定该子句，则默认为 OFF。  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 使用远程服务绑定来定位要用于新会话的证书。 与 user_name 关联的证书中的公钥用于对发送到远程服务的消息进行身份验证，并对会话密钥进行加密，然后使用加密的会话密钥对会话进行加密。 user_name 的证书必须对应于承载远程服务的数据库中的用户的证书。  
  
 只有在启动与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例以外的目标服务通信的服务时，才需要远程服务绑定。 承载启动服务的数据库必须包含可用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例以外的任何目标服务的远程服务绑定。 承载目标服务的数据库不需要包含用于与目标服务通信的启动服务的远程服务绑定。 当发起方服务与目标服务位于相同的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中时，无需远程服务绑定。 但是，如果存在远程服务绑定，并且其中为 TO SERVICE 指定的 service_name 与本地服务的名称匹配，则 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将使用该绑定。  
  
 ANONYMOUS = ON 时，启动服务将作为 public 固定数据库角色的成员连接到目标服务。 默认情况下，该角色的成员无权连接到数据库。 若要成功发送消息，目标数据库必须将数据库的 CONNECT 权限和目标服务的 SEND 权限授予 public 角色。  
  
 如果用户拥有多个证书，则 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将从当前有效的并标记为 AVAILABLE FOR BEGIN_DIALOG 的证书中选择过期日期最晚的证书。  
  
## <a name="permissions"></a>权限  
 下列用户默认拥有创建远程服务绑定的权限：在 USER 子句中指定的用户、db_owner 固定数据库角色的成员、db_ddladmin 固定数据库角色的成员以及 sysadmin 固定服务器角色的成员。  
  
 执行 CREATE REMOTE SERVICE BINDING 语句的用户必须对该语句中指定的主体数据库拥有模拟权限。  
  
 远程服务绑定可能不是临时对象。 远程服务绑定名称可以以 # 开头，但它们是永久对象。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-remote-service-binding"></a>A. 创建远程服务绑定  
 下面的示例为服务 `//Adventure-Works.com/services/AccountsPayable` 创建了绑定。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 使用 `APUser` 数据库主体所拥有的证书向远程服务验证身份，并与远程服务交换会话加密密钥。  
  
```sql  
CREATE REMOTE SERVICE BINDING APBinding  
    TO SERVICE '//Adventure-Works.com/services/AccountsPayable'  
    WITH USER = APUser ;  
```  
  
### <a name="b-creating-a-remote-service-binding-using-anonymous-authentication"></a>B. 使用匿名身份验证创建远程服务绑定  
 下面的示例为服务 `//Adventure-Works.com/services/AccountsPayable` 创建了绑定。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 使用 `APUser` 数据库主体所拥有的证书与远程服务交换会话加密密钥。 Broker 不向远程服务进行身份验证。 在承载远程服务的数据库中，作为 guest 用户传递消息。  
  
```sql  
CREATE REMOTE SERVICE BINDING APBinding  
    TO SERVICE '//Adventure-Works.com/services/AccountsPayable'  
    WITH USER = APUser, ANONYMOUS=ON ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER REMOTE SERVICE BINDING (Transact-SQL)](../../t-sql/statements/alter-remote-service-binding-transact-sql.md)   
 [DROP REMOTE SERVICE BINDING (Transact-SQL)](../../t-sql/statements/drop-remote-service-binding-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
