---
description: REVOKE Service Broker 权限 (Transact-SQL)
title: REVOKE Service Broker 权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- routes [Service Broker], permissions
- Service Broker, permissions
- remote service bindings [Service Broker], permissions
- permissions [Service Broker]
- message types [Service Broker], permissions
- contracts [Service Broker], permissions
- services [Service Broker], permissions
- REVOKE statement, Service Broker
ms.assetid: 70f1d938-97e2-48a4-9bc0-8be9f2f2c36d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2d018e710bb272d9daa9eda7099d201b80ea3c1d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88357113"
---
# <a name="revoke-service-broker-permissions-transact-sql"></a>REVOKE Service Broker 权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  撤消授予 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 约定、消息类型、远程服务绑定、路由或服务的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ] ON  
    {   
       [ CONTRACT :: contract_name ]   
       | [ MESSAGE TYPE :: message_type_name ]    
       | [ REMOTE SERVICE BINDING :: remote_binding_name ]    
       | [ ROUTE :: route_name ]   
       | [ SERVICE :: service_name ]      
        }  
    { TO | FROM } database_principal [ ,...n ]  
    [ CASCADE ]  
    [ AS revoking_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 GRANT OPTION FOR  
 指示要撤消用于向其他主体授予指定权限的权限。 不会撤消该权限本身。  
  
> [!IMPORTANT]  
>  如果主体具有不带 GRANT 选项的指定权限，则将撤消该权限本身。  
  
 permission  
 指定可对 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 安全对象撤消的权限。 有关这些权限的列表，请参阅本主题后面的“备注”部分。  
  
 CONTRACT ::contract_name  
 指定对其撤消权限的约定。 需要使用作用域限定符 ::。  
  
 MESSAGE TYPE ::message_type_name  
 指定对其撤消权限的消息类型。 需要使用作用域限定符 ::。  
  
 REMOTE SERVICE BINDING ::remote_binding_name  
 指定对其撤消权限的远程服务绑定。 需要使用作用域限定符 ::。  
  
 ROUTE ::route_name  
 指定对其撤消权限的路由。 需要使用作用域限定符 ::。  
  
 SERVICE ::message_type_name  
 指定对其撤消权限的服务。 需要使用作用域限定符 ::。  
  
 database_principal  
 指定要从中撤消权限的主体。 database_principal 可以为以下各项之一：  
  
-   数据库用户  
  
-   数据库角色  
  
-   应用程序角色  
  
-   映射到 Windows 登录名的数据库用户  
  
-   映射到 Windows 组的数据库用户  
  
-   映射到证书的数据库用户  
  
-   映射到非对称密钥的数据库用户  
  
-   未映射到服务器主体的数据库用户  
  
 CASCADE  
 指示要撤消的权限也会从此主体授予或拒绝该权限的其他主体中撤消。  
  
> [!CAUTION]  
>  如果对授予了 WITH GRANT OPTION 权限的权限执行级联撤消，将同时撤消该权限的 GRANT 和 DENY 权限。  
  
 AS revoking_principal  
 指定一个主体，执行该查询的主体从该主体获得撤消该权限的权利。 revoking_principal 可以为以下各项之一：  
  
-   数据库用户  
  
-   数据库角色  
  
-   应用程序角色  
  
-   映射到 Windows 登录名的数据库用户  
  
-   映射到 Windows 组的数据库用户  
  
-   映射到证书的数据库用户  
  
-   映射到非对称密钥的数据库用户  
  
-   未映射到服务器主体的数据库用户  
  
## <a name="remarks"></a>注解  
  
## <a name="service-broker-contracts"></a>Service Broker 约定  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 协定是权限层次结构中其父级数据库包含的数据库级安全对象。 下表列出了可撤消的对 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 协定最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|Service Broker 约定权限|Service Broker 约定权限隐含的权限|数据库权限隐含的权限|  
|----------------------------------------|---------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY CONTRACT|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-message-types"></a>Service Broker 消息类型  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 消息类型是权限层次结构中其父级数据库包含的数据库级安全对象。 下表列出了可撤消的对 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 消息类型最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|Service Broker 消息类型权限|Service Broker 消息类型权限隐含的权限|数据库权限隐含的权限|  
|--------------------------------------------|-------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY MESSAGE TYPE|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-remote-service-bindings"></a>Service Broker 远程服务绑定  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 远程服务绑定是权限层次结构中其父级数据库包含的数据库级安全对象。 下表列出了可撤消的对 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 远程服务绑定最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|Service Broker 远程服务绑定权限|Service Broker 远程服务绑定权限隐含的权限|数据库权限隐含的权限|  
|------------------------------------------------------|-----------------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY REMOTE SERVICE BINDING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-routes"></a>Service Broker 路由  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 路由是权限层次结构中其父级数据库包含的数据库级安全对象。 下表列出了可撤消的对 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 路由最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|Service Broker 路由权限|Service Broker 路由权限隐含的权限|数据库权限隐含的权限|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROUTE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
### <a name="service-broker-services"></a>Service Broker 服务  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务是权限层次结构中其父级数据库包含的数据库级安全对象。 下表列出了可撤消的对 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|Service Broker 服务权限|Service Broker 服务权限隐含的权限|数据库权限隐含的权限|  
|---------------------------------------|--------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|SEND|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY SERVICE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 需要对 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 约定、消息类型、远程服务绑定、路由或服务具有 CONTROL 权限  
  
## <a name="see-also"></a>另请参阅  
 [GRANT Service Broker 权限 (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)   
 [ Service Broker 权限 (Transact-SQL)](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
