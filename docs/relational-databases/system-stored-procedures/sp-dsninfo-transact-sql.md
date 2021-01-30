---
description: sp_dsninfo (Transact-SQL)
title: sp_dsninfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dsninfo
- sp_dsninfo_TSQL
helpviewer_keywords:
- sp_dsninfo
ms.assetid: 34648615-814b-42bc-95a3-50e86b42ec4d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 65a3f12555d1dbb0e26702229fdcc47664cc4c82
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186976"
---
# <a name="sp_dsninfo-transact-sql"></a>sp_dsninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从与当前服务器关联的分发服务器中返回 ODBC 或 OLE DB 数据源信息。 此存储过程在分发服务器上的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dsninfo [ @dsn =] 'dsn'   
    [ , [ @infotype =] 'info_type']   
    [ , [ @login =] 'login']   
    [ , [ @password =] 'password']  
    [ , [ @dso_type=] dso_type]  
```  
  
## <a name="arguments"></a>参数  
`[ @dsn = ] 'dsn'` ODBC DSN 或 OLE DB 链接服务器的名称。 *dsn* 是 **varchar (128)**，无默认值。  
  
`[ @infotype = ] 'info_type'` 要返回的信息的类型。 如果未指定 *info_type* 或指定 NULL，则返回所有信息类型。 *info_type* 是 **varchar (128)**，默认值为 NULL，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**DBMS_NAME**|指定数据源供应商名称。|  
|**DBMS_VERSION**|指定数据源版本。|  
|**DATABASE_NAME**|指定数据库名。|  
|**SQL_SUBSCRIBER**|指定数据源可以是订阅服务器。|  
  
`[ @login = ] 'login'` 数据源的登录名。 如果数据源包括登录名，则指定 NULL 或忽略该参数。 *login* 是 **varchar (128)**，默认值为 NULL。  
  
`[ @password = ] 'password'` 登录名的密码。 如果数据源包括登录名，则指定 NULL 或忽略该参数。 *密码* 为 **varchar (128)**，默认值为 NULL。  
  
`[ @dso_type = ] dso_type` 数据源类型。 *dso_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1** （默认值）|ODBC 数据源|  
|**3**|OLE DB 数据源|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**信息类型**|**nvarchar (64)**|信息类型，如 DBMS_NAME、DBMS_VERSION、DATABASE_NAME 和 SQL_SUBSCRIBER。|  
|值|**nvarchar(512)**|关联的信息类型的值。|  
  
## <a name="remarks"></a>备注  
 **sp_dsninfo** 在所有类型的复制中使用。  
  
 **sp_dsninfo** 检索 ODBC 或 OLE DB 数据源信息，该信息显示数据库是否可用于复制或查询。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_dsninfo** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [sp_enumdsn &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-enumdsn-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
