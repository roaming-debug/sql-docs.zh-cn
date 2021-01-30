---
title: sys.sp_rda_test_connection (Transact-sql) |Microsoft Docs
description: 了解如何使用 sys.sp_rda_test_connection 测试从 SQL Server 到远程 Azure 服务器的连接，并报告可能阻止数据迁移的问题。
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_test_connection
- sys.sp_rda_test_connection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_test_connection stored procedure
ms.assetid: e2ba050c-d7e3-4f33-8281-c9b525b4edb4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e584604a0229e3e2e0c213b70d8fd4bca0321c2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211713"
---
# <a name="syssp_rda_test_connection-transact-sql"></a>sys.sp_rda_test_connection (Transact-sql) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  测试从 SQL Server 到远程 Azure 服务器的连接，并报告可能阻止数据迁移的问题。  
  
## <a name="syntax"></a>语法  
  
```  
  
EXECUTE sys.sp_rda_test_connection  
   @database_name = N'db_name',   
   @server_address = N'azure_server_fully_qualified_address',  
   @azure_username = N'azure_username',   
   @azure_password = N'azure_password',  
   @credential_name = N'credential_name'  
  
```  
  
## <a name="arguments"></a>参数  
 @database_name = N '*db_name*'  
 已启用延伸的 SQL Server 数据库的名称。 此参数是可选的。  
  
 @server_address = N '*azure_server_fully_qualified_address*'  
 Azure 服务器的完全限定的地址。  
  
-   如果为 **\@ database_name** 提供了一个值，但指定的数据库未启用 Stretch，则必须为 **\@ server_address** 提供一个值。  
  
-   如果为 **\@ database_name** 提供了一个值，并且指定的数据库已启用 Stretch，则无需为 **\@ server_address** 提供值。 如果为 **\@ server_address** 提供值，存储过程将忽略它，并使用已与已启用延伸的数据库相关联的现有 Azure 服务器。  
  
 @azure_username = N '*azure_username*  
 远程 Azure 服务器的用户名。  
  
 @azure_password = N '*azure_password*'  
 远程 Azure 服务器的密码。  
  
 @credential_name = N '*credential_name*'  
 您可以提供已启用 Stretch 的数据库中存储的凭据的名称，而不是提供用户名和密码。  
  
## <a name="return-code-values"></a>返回代码值  
 如果 **成功**，sp_rda_test_connection 将返回错误 14855 (STRETCH_MAJOR，STRETCH_CONNECTION_TEST_PROC_SUCCEEDED 包含严重性) 的 EX_INFO 和成功返回代码。  
  
 如果 **失败**，sp_rda_test_connection 将返回错误 14856 (STRETCH_MAJOR，STRETCH_CONNECTION_TEST_PROC_FAILED 包含严重性) 的 EX_USER 和错误返回代码。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|link_state|int|以下值之一，对应于 **link_state_desc** 的值。<br /><br /> -0<br />-1<br />-2<br />-3<br />-4|  
|link_state_desc|varchar(32)|以下值之一，对应于 **link_state** 前面的值。<br /><br /> -正常<br />     SQL Server 与远程 Azure 服务器之间的状况良好。<br />-ERROR_AZURE_FIREWALL<br />     Azure 防火墙阻止 SQL Server 与远程 Azure 服务器之间的链接。<br />-ERROR_NO_CONNECTION<br />     SQL Server 无法建立与远程 Azure 服务器的连接。<br />-ERROR_AUTH_FAILURE<br />     身份验证失败会阻止 SQL Server 与远程 Azure 服务器之间的链接。<br />-错误<br />     不是身份验证问题、连接问题或防火墙问题的错误正在阻止 SQL Server 与远程 Azure 服务器之间的链接。|  
|error_number|int|错误号。 如果没有错误，则此字段为 NULL。|  
|error_message|nvarchar(1024)|错误消息。 如果没有错误，则此字段为 NULL。|  
  
## <a name="permissions"></a>权限  
 需要 db_owner 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="check-the-connection-from-sql-server-to-the-remote-azure-server"></a>检查从 SQL Server 到远程 Azure 服务器的连接  
  
```sql  
EXECUTE sys.sp_rda_test_connection @database_name = N'<Stretch-enabled database>'  
GO  
  
```  
  
 结果显示 SQL Server 无法连接到远程 Azure 服务器。  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|2|ERROR_NO_CONNECTION|*\<connection-related error number>*|*\<connection-related error message>*|  
  
### <a name="check-the-azure-firewall"></a>检查 Azure 防火墙  
  
```sql  
USE <Stretch-enabled database>  
GO  
EXECUTE sys.sp_rda_test_connection  
GO  
  
```  
  
 结果显示，Azure 防火墙阻止 SQL Server 与远程 Azure 服务器之间的链接。  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|1|ERROR_AZURE_FIREWALL|*\<firewall-related error number>*|*\<firewall-related error message>*|  
  
### <a name="check-authentication-credentials"></a>检查身份验证凭据  
  
```sql  
USE <Stretch-enabled database>  
GO  
EXECUTE sys.sp_rda_test_connection  
GO  
  
```  
  
 结果显示身份验证失败阻止 SQL Server 与远程 Azure 服务器之间的链接。  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|3|ERROR_AUTH_FAILURE|*\<authentication-related error number>*|*\<authentication-related error message>*|  
  
### <a name="check-the-status-of-the-remote-azure-server"></a>检查远程 Azure 服务器的状态  
  
```sql  
USE <SQL Server database>  
GO  
EXECUTE sys.sp_rda_test_connection   
    @server_address = N'<server name>.database.windows.net',   
    @azure_username = N'<user name>',   
    @azure_password = N'<password>'  
GO  
  
```  
  
 结果显示连接正常，并且你可以为指定的数据库启用 Stretch Database。  
  
|link_state|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|0|HEALTHY|Null|Null|  
  
  
