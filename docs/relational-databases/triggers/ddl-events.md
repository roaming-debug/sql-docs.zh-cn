---
description: DDL 事件
title: DDL 事件 | Microsoft Docs
ms.custom: ''
ms.date: 11/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- DDL events
- DDL triggers, events
- events [SQL Server], DDL
ms.assetid: 62ef24b4-3553-4aed-b62a-670980bae501
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f33460a64292ba30d28a1d6cc6f51c8c10e8dc55
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97403174"
---
# <a name="ddl-events"></a>DDL 事件
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  下表列出了可用于激发 DDL 触发器或事件通知的 DDL 事件。 注意，每个事件都对应于一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或存储过程，并且语句语法修改为在关键字之间加入了一个下划线字符 (_)。  
  
> [!IMPORTANT]  
>  执行类似 DDL 的操作的系统存储过程也可以激发 DDL 触发器和事件通知。 请测试您的 DDL 触发器和事件通知以确定它们是否响应运行的系统存储过程。 例如，CREATE TYPE 语句和 **sp_addtype** 存储过程都将激发针对 CREATE_TYPE 事件创建的 DDL 触发器或事件通知。  
  
## <a name="ddl-statements-that-have-server-or-database-scope"></a>具有服务器或数据库作用域的 DDL 语句  
 可以创建 DDL 触发器或事件通知，以便在其中创建了它们的数据库发生以下事件（或服务器实例中的任何位置发生以下事件时）激发它们以做出响应。  
  
:::row:::
    :::column:::
        CREATE_APPLICATION_ROLE（适用于 CREATE APPLICATION ROLE 语句和 **sp_addapprole**。 如果创建新架构，则此事件还会触发 CREATE_SCHEMA 事件）。
    :::column-end:::
    :::column:::
        ALTER_APPLICATION_ROLE（适用于 ALTER APPLICATION ROLE 语句和 **sp_approlepassword**）。
    :::column-end:::
    :::column:::
        DROP_APPLICATION_ROLE（适用于 DROP APPLICATION ROLE 语句和 **sp_dropapprole**）。
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_ASSEMBLY
    :::column-end:::
    :::column:::
        ALTER_ASSEMBLY
    :::column-end:::
    :::column:::
        DROP_ASSEMBLY
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_ASYMMETRIC_KEY
    :::column-end:::
    :::column:::
        ALTER_ASYMMETRIC_KEY
    :::column-end:::
    :::column:::
        DROP_ASYMMETRIC_KEY
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER_AUTHORIZATION
    :::column-end:::
    :::column:::
        ALTER_AUTHORIZATION_DATABASE（当指定 ON DATABASE 时，适用于 ALTER AUTHORIZATION 语句和 **sp_changedbowner**。）
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_BROKER_PRIORITY
    :::column-end:::
    :::column:::
        CREATE_BROKER_PRIORITY
    :::column-end:::
    :::column:::
        CREATE_BROKER_PRIORITY
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_CERTIFICATE
    :::column-end:::
    :::column:::
        ALTER_CERTIFICATE
    :::column-end:::
    :::column:::
        DROP_CERTIFICATE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_CONTRACT
    :::column-end:::
    :::column:::
        DROP_CONTRACT
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_CREDENTIAL
    :::column-end:::
    :::column:::
        ALTER_CREDENTIAL
    :::column-end:::
    :::column:::
        DROP_CREDENTIAL
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        GRANT_DATABASE
    :::column-end:::
    :::column:::
        DENY_DATABASE
    :::column-end:::
    :::column:::
        REVOKE_DATABASE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_DATABASE_AUDIT_SPEFICIATION
    :::column-end:::
    :::column:::
        ALTER_DATABASE_AUDIT_SPEFICIATION
    :::column-end:::
    :::column:::
        DENY_DATABASE_AUDIT_SPEFICIATION
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_DATABASE_ENCRYPTION_KEY
    :::column-end:::
    :::column:::
        ALTER_DATABASE_ENCRYPTION_KEY
    :::column-end:::
    :::column:::
        DROP_DATABASE_ENCRYPTION_KEY
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_DEFAULT
    :::column-end:::
    :::column:::
        DROP_DEFAULT
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        BIND_DEFAULT（适用于 **sp_bindefault**。）
    :::column-end:::
    :::column:::
        UNBIND_DEFAULT（适用于 **sp_unbindefault**。）
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_EVENT_NOTIFICATION
    :::column-end:::
    :::column:::
        DROP_EVENT_NOTIFICATION
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_EXTENDED_PROPERTY（适用于 **sp_addextendedproperty**。）
    :::column-end:::
    :::column:::
        ALTER_EXTENDED_PROPERTY（适用于 **sp_updateextendedproperty**。）
    :::column-end:::
    :::column:::
        DROP_EXTENDED_PROPERTY（适用于 **sp_dropextendedproperty**。）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_FULLTEXT_CATALOG（当指定 create 时适用于 CREATE FULLTEXT CATALOG 语句和 **sp_fulltextcatalog**。）
    :::column-end:::
    :::column:::
        ALTER_FULLTEXT_CATALOG（当指定 start_incremental、start_full、Stop 或 Rebuild 时，适用于 ALTER FULLTEXT CATALOG 语句 **sp_fulltextcatalog**，当指定 enable 时，适用于 **sp_fulltext_database**。）
    :::column-end:::
    :::column:::
        DROP_FULLTEXT_CATALOG（当指定 drop 时，适用于 DROP FULLTEXT CATALOG 语句和 **sp_fulltextcatalog**。）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_FULLTEXT_INDEX（当指定 create 时，适用于 CREATE FULLTEXT INDEX 语句和 **sp_fulltexttable**。）
    :::column-end:::
    :::column:::
        ALTER_FULLTEXT_INDEX（当指定 start_full、start_incremental 或stop 时，适用于 ALTER FULLTEXT INDEX 语句和 **sp_fulltextcatalog**，当指定除了 create 或 drop 操作之外时，适用于 **sp_fulltext_column** 和 **sp_fulltext_table**。）
    :::column-end:::
    :::column:::
        DROP_FULLTEXT_INDEX（当指定 drop 时，适用于 DROP FULLTEXT INDEX 语句和 **sp_fulltexttable**。）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_FULLTEXT_STOPLIST
    :::column-end:::
    :::column:::
        ALTER_FULLTEXT_STOPLIST
    :::column-end:::
    :::column:::
        DROP_FULLTEXT_STOPLIST
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_FUNCTION
    :::column-end:::
    :::column:::
        ALTER_FUNCTION
    :::column-end:::
    :::column:::
        DROP_FUNCTION
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_INDEX
    :::column-end:::
    :::column:::
        ALTER_INDEX（适用于ALTER INDEX 语句和 **sp_indexoption**。）
    :::column-end:::
    :::column:::
        DROP_INDEX
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_MASTER_KEY
    :::column-end:::
    :::column:::
        ALTER_MASTER_KEY
    :::column-end:::
    :::column:::
        DROP_MASTER_KEY
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_MESSAGE_TYPE
    :::column-end:::
    :::column:::
        ALTER_MESSAGE_TYPE
    :::column-end:::
    :::column:::
        DROP_MESSAGE_TYPE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_PARTITION_FUNCTION
    :::column-end:::
    :::column:::
        ALTER_PARTITION_FUNCTION
    :::column-end:::
    :::column:::
        DROP_PARTITION_FUNCTION
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_PARTITION_SCHEME
    :::column-end:::
    :::column:::
        ALTER_PARTITION_SCHEME
    :::column-end:::
    :::column:::
        DROP_PARTITION_SCHEME
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_PLAN_GUIDE（适用于 **sp_create_plan_guide**。）
    :::column-end:::
    :::column:::
        ALTER_PLAN_GUIDE（当指定 ENABLE、ENABLE ALL、DISABLE 或 DISABLE ALL 时适用于 **sp_control_plan_guide** 。）
    :::column-end:::
    :::column:::
        DROP_PLAN_GUIDE（当指定 DROP 或 DROP ALL 时适用于 **sp_control_plan_guide** 。）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_PROCEDURE
    :::column-end:::
    :::column:::
        ALTER_PROCEDURE（适用于 ALTER PROCEDURE 语句和 **sp_procoption**。）
    :::column-end:::
    :::column:::
        DROP_PROCEDURE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_QUEUE
    :::column-end:::
    :::column:::
        ALTER_QUEUE
    :::column-end:::
    :::column:::
        DROP_QUEUE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_REMOTE_SERVICE_BINDING
    :::column-end:::
    :::column:::
        ALTER_REMOTE_SERVICE_BINDING
    :::column-end:::
    :::column:::
        DROP_REMOTE_SERVICE_BINDING
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SPATIAL_INDEX
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        RENAME（适用于 **sp_rename**）
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_ROLE（适用于 CREATE ROLE 语句、 **sp_addrole** 和 **sp_addgroup**。）
    :::column-end:::
    :::column:::
        ALTER_ROLE
    :::column-end:::
    :::column:::
        DROP_ROLE（适用于 DROP ROLE 语句、 **sp_droprole** 和 **sp_dropgroup**。）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ADD_ROLE_MEMBER
    :::column-end:::
    :::column:::
        DROP_ROLE_MEMBER
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_ROUTE
    :::column-end:::
    :::column:::
        ALTER_ROUTE
    :::column-end:::
    :::column:::
        DROP_ROUTE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_RULE
    :::column-end:::
    :::column:::
        DROP_RULE
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        BIND_RULE（适用于 **sp_bindrule**。）
    :::column-end:::
    :::column:::
        UNBIND_RULE（适用于 **sp_unbindrule**。）
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SCHEMA（适用于 CREATE SCHEMA 语句、**sp_addrole****sp_adduser**、**sp_addgroup** 和 **sp_grantdbaccess**。）
    :::column-end:::
    :::column:::
        ALTER_SCHEMA（适用于 ALTER SCHEMA 语句和 **sp_changeobjectowner**）。
    :::column-end:::
    :::column:::
        DROP_SCHEMA
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SEARCH_PROPERTY_LIST
    :::column-end:::
    :::column:::
        ALTER_SEARCH_PROPERTY_LIST
    :::column-end:::
    :::column:::
        DROP_SEARCH_PROPERTY_LIST
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SEQUENCE_EVENTS
    :::column-end:::
    :::column:::
        CREATE_SEQUENCE_EVENTS
    :::column-end:::
    :::column:::
        CREATE_SEQUENCE_EVENTS
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SERVER_ROLE
    :::column-end:::
    :::column:::
        ALTER_SERVER_ROLE
    :::column-end:::
    :::column:::
        DROP_SERVER_ROLE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SERVICE
    :::column-end:::
    :::column:::
        ALTER_SERVICE
    :::column-end:::
    :::column:::
        DROP_SERVICE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER_SERVICE_MASTER_KEY
    :::column-end:::
    :::column:::
        BACKUP_SERVICE_MASTER_KEY
    :::column-end:::
    :::column:::
        RESTORE_SERVICE_MASTER_KEY
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ADD_SIGNATURE（用于对非架构范围的对象的签名操作；数据库，程序集，触发器）
    :::column-end:::
    :::column:::
        DROP_SIGNATURE
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ADD_SIGNATURE_SCHEMA_OBJECT（用于架构范围的对象；存储过程，函数）
    :::column-end:::
    :::column:::
        DROP_SIGNATURE_SCHEMA_OBJECT
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SPATIAL_INDEX
    :::column-end:::
    :::column:::
        ALTER_INDEX 可用于空间索引。
    :::column-end:::
    :::column:::
        DROP_INDEX 可用于空间索引。
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_STATISTICS
    :::column-end:::
    :::column:::
        DROP_STATISTICS
    :::column-end:::
    :::column:::
        UPDATE_STATISTICS
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SYMMETRIC_KEY
    :::column-end:::
    :::column:::
        ALTER_SYMMETRIC_KEY
    :::column-end:::
    :::column:::
        DROP_SYMMETRIC_KEY
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SYNONYM
    :::column-end:::
    :::column:::
        DROP_SYNONYM
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_TABLE
    :::column-end:::
    :::column:::
        ALTER_TABLE（适用于 ALTER TABLE 语句和 **sp_tableoption**。）
    :::column-end:::
    :::column:::
        DROP_TABLE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_TRIGGER
    :::column-end:::
    :::column:::
        ALTER_TRIGGER（适用于 ALTER TRIGGER 语句和 **sp_settriggerorder**。）
    :::column-end:::
    :::column:::
        DROP_TRIGGER
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_TYPE（适用于 CREATE TYPE 语句和 **sp_addtype**）
    :::column-end:::
    :::column:::
        DROP_TYPE（适用于 DROP TYPE 语句和 **sp_droptype**。）
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_USER（适用于 CREATE USER 语句、 **sp_adduser** 和 **sp_grantdbaccess**）
    :::column-end:::
    :::column:::
        ALTER_USER（应用于 ALTER USER 语句和 **sp_change_users_login**。）
    :::column-end:::
    :::column:::
        DROP_USER（适用于 DROP USER 语句、 **sp_dropuser** 和 **sp_revokedbaccess**。）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_VIEW
    :::column-end:::
    :::column:::
        ALTER_VIEW
    :::column-end:::
    :::column:::
        DROP_VIEW
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_XML_INDEX
    :::column-end:::
    :::column:::
        ALTER_INDEX 可用于 XML 索引。
    :::column-end:::
    :::column:::
        DROP_INDEX 可用于 XML 索引。
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_XML_SCHEMA_COLLECTION
    :::column-end:::
    :::column:::
        ALTER_XML_SCHEMA_COLLECTION
    :::column-end:::
    :::column:::
        DROP_XML_SCHEMA_COLLECTION
    :::column-end:::
:::row-end:::
 
## <a name="ddl-statements-that-have-server-scope"></a>具有服务器作用域的 DDL 语句  
 可以创建当服务器实例中的任何位置发生以下事件时被激发以响应这些事件的 DDL 触发器或事件通知。  
  
:::row:::
    :::column:::
        ALTER_AUTHORIZATION_SERVER
    :::column-end:::
    :::column:::
        ALTER_SERVER_CONFIGURATION
    :::column-end:::
    :::column:::
        ALTER_INSTANCE（当指定了本地服务器实例时，适用于 **sp_configure** 和 **sp_addserver** 。）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_AVAILABILITY_GROUP
    :::column-end:::
    :::column:::
        ALTER_AVAILABILITY_GROUP
    :::column-end:::
    :::column:::
        DROP_AVAILABILITY_GROUP
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_CREDENTIAL
    :::column-end:::
    :::column:::
        ALTER_CREDENTIAL
    :::column-end:::
    :::column:::
        DROP_CREDENTIAL
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_CRYPTOGRAPHIC_PROVIDER
    :::column-end:::
    :::column:::
        ALTER_CRYPTOGRAPHIC_PROVIDER
    :::column-end:::
    :::column:::
        DROP_CRYPTOGRAPHIC_PROVIDER
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_DATABASE
    :::column-end:::
    :::column:::
        ALTER_DATABASE（适用于 ALTER DATABASE 语句和 **sp_fulltext_database**。）
    :::column-end:::
    :::column:::
        DROP_DATABASE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_ENDPOINT
    :::column-end:::
    :::column:::
        ALTER_ENDPOINT
    :::column-end:::
    :::column:::
        DROP_ENDPOINT
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_EVENT_SESSION
    :::column-end:::
    :::column:::
        ALTER_EVENT_SESSION
    :::column-end:::
    :::column:::
        DROP_EVENT_SESSION
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_EXTENDED_PROCEDURE（适用于 **sp_addextendedproc**。）
    :::column-end:::
    :::column:::
        DROP_EXTENDED_PROCEDURE（适用于 **sp_dropextendedproc**。）
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_LINKED_SERVER（适用于 **sp_addlinkedserver**。）
    :::column-end:::
    :::column:::
        ALTER_LINKED_SERVER（适用于 **sp_serveroption**。）
    :::column-end:::
    :::column:::
        DROP_LINKED_SERVER（当指定了链接服务器时，适用于 **sp_dropserver** 。）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_LINKED_SERVER_LOGIN（适用于 **sp_addlinkedsrvlogin**。）
    :::column-end:::
    :::column:::
        DROP_LINKED_SERVER_LOGIN（适用于 **sp_droplinkedsrvlogin**。）
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_LOGIN（当使用必须隐式创建的不存在登录名时，适用于 CREATE LOGIN 语句、**sp_addlogin****sp_grantlogin****xp_grantlogin** 和 **sp_denylogin**。）
    :::column-end:::
    :::column:::
        ALTER_LOGIN（当指定 Auto_Fix 时，适用于 **sp_defaultdb**、**sp_defaultlanguage**、**sp_password** 和 **sp_change_users_login**。）
    :::column-end:::
    :::column:::
        DROP_LOGIN（适用于 DROP LOGIN 语句、 **sp_droplogin**、 **sp_revokelogin** 和 **xp_revokelogin**）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_MESSAGE（适用于 **sp_addmessage**。）
    :::column-end:::
    :::column:::
        ALTER_MESSAGE（适用于 **sp_altermessage**。）
    :::column-end:::
    :::column:::
        DROP_MESSAGE（适用于 **sp_dropmessage**。）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_REMOTE_SERVER（适用于 **sp_addserver**。）
    :::column-end:::
    :::column:::
        ALTER_REMOTE_SERVER（适用于 **sp_setnetname**。）
    :::column-end:::
    :::column:::
        DROP_REMOTE_SERVER（当指定了远程服务器时，适用于 **sp_dropserver** 。）
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_RESOURCE_POOL
    :::column-end:::
    :::column:::
        ALTER_RESOURCE_POOL
    :::column-end:::
    :::column:::
        DROP_RESOURCE_POOL
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        GRANT_SERVER
    :::column-end:::
    :::column:::
        DENY_SERVER
    :::column-end:::
    :::column:::
        REVOKE_SERVER
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ADD_SERVER_ROLE_MEMBER
    :::column-end:::
    :::column:::
        DROP_SERVER_ROLE_MEMBER
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SERVER_AUDIT
    :::column-end:::
    :::column:::
        ALTER_SERVER_AUDIT
    :::column-end:::
    :::column:::
        DROP_SERVER_AUDIT
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_SERVER_AUDIT_SPECIFICATION
    :::column-end:::
    :::column:::
        ALTER_SERVER_AUDIT_SPECIFICATION
    :::column-end:::
    :::column:::
        DROP_SERVER_AUDIT_SPECIFICATION
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE_WORKLOAD_GROUP
    :::column-end:::
    :::column:::
        ALTER_WORKLOAD_GROUP
    :::column-end:::
    :::column:::
        DROP_WORKLOAD_GROUP
    :::column-end:::
:::row-end:::
 
## <a name="see-also"></a>另请参阅  
 [DDL 触发器](../../relational-databases/triggers/ddl-triggers.md)   
 [事件通知](../../relational-databases/service-broker/event-notifications.md)   
 [DDL 事件组](../../relational-databases/triggers/ddl-event-groups.md)  
  
  
