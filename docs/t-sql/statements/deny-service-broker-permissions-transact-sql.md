---
description: DENY Service Broker 权限 (Transact-SQL)
title: DENY Service Broker 权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- denying permissions [Service Broker]
- routes [Service Broker], permissions
- Service Broker, permissions
- DENY statement, Service Broker
- remote service bindings [Service Broker], permissions
- permissions [Service Broker]
- message types [Service Broker], permissions
- denying permissions [SQL Server], Service Broker
- contracts [Service Broker], permissions
- services [Service Broker], permissions
ms.assetid: 7c6de71b-865c-41db-9413-ad9b3562e579
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 1a39e74300d5a522e1ebdb9f72c0b83e21c7a9df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200388"
---
# <a name="deny-service-broker-permissions-transact-sql"></a>DENY Service Broker 权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  拒绝授予 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 约定、消息类型、远程服务绑定、路由或服务的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DENY permission  [ ,...n ] ON  
    {    
       [ CONTRACT :: contract_name ]   
       | [ MESSAGE TYPE :: message_type_name ]    
       | [ REMOTE SERVICE BINDING :: remote_binding_name ]    
       | [ ROUTE :: route_name ]   
       | [ SERVICE :: service_name ]      
        }  
    TO database_principal [ ,...n ]   
    [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission  
 指定可拒绝授予 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 安全对象的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 CONTRACT ::contract_name  
 指定拒绝将其权限授权他人的约定。 需要使用作用域限定符 ::。  
  
 MESSAGE TYPE ::message_type_name  
 指定拒绝将其权限授予他人的消息类型。 需要使用作用域限定符 ::。  
  
 REMOTE SERVICE BINDING ::remote_binding_name  
 指定拒绝将其权限授予他人的远程服务绑定。 需要使用作用域限定符 ::。  
  
 ROUTE ::route_name  
 指定拒绝将其权限授予他人的路由。 需要使用作用域限定符 ::。  
  
 SERVICE ::message_type_name  
 指定拒绝将其权限授予他人的服务。 需要使用作用域限定符 ::。  
  
 database_principal  
 指定要对其拒绝权限的主体。 下列类型作之一：  
  
-   数据库用户  
-   数据库角色  
-   应用程序角色  
-   映射到 Windows 登录名的数据库用户  
-   映射到 Windows 组的数据库用户  
-   映射到证书的数据库用户  
-   映射到非对称密钥的数据库用户  
-   未映射到服务器主体的数据库用户  
  
CASCADE  
 指示要拒绝的权限也会被对此主体授予该权限的其他主体拒绝。  
  
denying_principal  
 指定一个主体，执行该查询的主体从该主体获得拒绝授予该权限的权利。 下列类型作之一：  
  
-   数据库用户  
-   数据库角色  
-   应用程序角色  
-   映射到 Windows 登录名的数据库用户  
-   映射到 Windows 组的数据库用户  
-   映射到证书的数据库用户  
-   映射到非对称密钥的数据库用户  
-   未映射到服务器主体的数据库用户  
  
## <a name="remarks"></a>备注  
  
## <a name="service-broker-contracts"></a>Service Broker 约定  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 约定是权限层次结构中其父级数据库包含的数据库级安全对象。 下表列出了可拒绝授予 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 约定的最特定和最受限的权限，以及隐含这些权限的更常用权限。  
  
|Service Broker 约定权限|Service Broker 约定权限隐含的权限|数据库权限隐含的权限|  
|----------------------------------------|---------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY CONTRACT|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-message-types"></a>Service Broker 消息类型  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 消息类型是权限层次结构中其父级数据库包含的数据库级安全对象。 下表列出了可拒绝授予 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 消息类型的最特定和最受限的权限，以及隐含这些权限的更常用权限。  
  
|Service Broker 消息类型权限|Service Broker 消息类型权限隐含的权限|数据库权限隐含的权限|  
|--------------------------------------------|-------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY MESSAGE TYPE|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-remote-service-bindings"></a>Service Broker 远程服务绑定  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 远程服务绑定是权限层次结构中其父级数据库包含的数据库级安全对象。 下表列出了可拒绝授予 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 远程服务绑定的最特定和最受限的权限，以及隐含这些权限的更常用权限。  
  
|Service Broker 远程服务绑定权限|Service Broker 远程服务绑定权限隐含的权限|数据库权限隐含的权限|  
|------------------------------------------------------|-----------------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY REMOTE SERVICE BINDING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-routes"></a>Service Broker 路由  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 路由是权限层次结构中其父级数据库包含的数据库级安全对象。 下表列出了可拒绝授予 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 路由的最特定和最受限的权限，以及隐含这些权限的更常用权限。  
  
|Service Broker 路由权限|Service Broker 路由权限隐含的权限|数据库权限隐含的权限|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROUTE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
### <a name="service-broker-services"></a>Service Broker 服务  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务是权限层次结构中其父级数据库包含的数据库级安全对象。 下表列出了可拒绝授予 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务的最特定和最受限的权限，以及隐含这些权限的更常用权限。  
  
|Service Broker 服务权限|Service Broker 服务权限隐含的权限|数据库权限隐含的权限|  
|---------------------------------------|--------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|SEND|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY SERVICE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 需要对 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 约定、消息类型、远程服务绑定、路由或服务具有 CONTROL 权限。 如果使用 AS 子句，那么指定主体必须拥有其权限被拒绝授予的安全对象。  
  
## <a name="see-also"></a>另请参阅  
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVOKE Service Broker 权限 (Transact-SQL)](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)  
  
  
