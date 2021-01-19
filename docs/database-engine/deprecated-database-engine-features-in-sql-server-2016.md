---
title: 弃用的数据库引擎功能 | Microsoft Docs
titleSuffix: SQL Server 2016
description: 了解弃用的数据库引擎功能，这些功能在 SQL Server 2016 (13.x) 中仍然可用，但不可在新的应用程序中使用。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [SQL Server]
- Database Engine [SQL Server], backward compatibility
- deprecation [SQL Server], feature list
ms.assetid: c10eeaa5-3d3c-49b4-a4bd-5dc4fb190142
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a036ba8c2b822d5663252b52beeb0ca2e526b969
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170549"
---
# <a name="deprecated-database-engine-features-in-sql-server-2016"></a>SQL Server 2016 中不推荐使用的数据库引擎功能
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  

本主题介绍 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 中仍然可用但不推荐使用的 [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]功能。 在新的应用程序中不应使用这些不推荐使用的功能。  
  
如果功能标记为已弃用，表示：
-  该功能仅处于维护模式。 无法进行新的更改，包括与新功能的互操作性有关的更改。
-  我们努力不从将来的版本中删除已弃用的功能，使升级更简单。 但是，在极少数情况下，如果该功能限制了将来的创新，我们可能选择从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中将其永久删除。
-  对于新的开发工作，不建议使用已弃用的功能。      

对于 [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]，请参阅 [SQL Server 2017 中弃用的数据库引擎功能](../database-engine/deprecated-database-engine-features-in-sql-server-2017.md)。

可以使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Deprecated Features Object 性能计数器监视不推荐使用的功能的使用情况并跟踪事件。 有关详细信息，请参阅 [使用 SQL Server 对象](../relational-databases/performance-monitor/use-sql-server-objects.md)。  
  
这些计数器的值也可通过执行以下语句而得：  
  
```sql  
SELECT * FROM sys.dm_os_performance_counters   
WHERE object_name = 'SQLServer:Deprecated Features';  
```  
  
## <a name="features-deprecated-in-the-next-version-of-sql-server"></a>SQL Server 的下一版本中弃用的功能
 下一版 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 将不再支持以下 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]功能。 请不要在新的开发工作中使用这些功能，并尽快修改当前还在使用这些功能的应用程序。 功能名称值在跟踪事件中显示为 ObjectName，而在性能计数器和 `sys.dm_os_performance_counters` 中显示为实例名称  。 **功能 ID** 值在跟踪事件中作为 ObjectId。  
  
|类别|不推荐使用的功能|替代功能|功能名称|功能 ID|  
|--------------|------------------------|-----------------|------------------|----------------|  
|备份和还原|仍弃用 RESTORE { DATABASE &#124; LOG } WITH [MEDIA]PASSWORD。 停止使用 BACKUP { DATABASE &#124; LOG } WITH PASSWORD 和 BACKUP { DATABASE &#124; LOG } WITH MEDIAPASSWORD。|无|BACKUP DATABASE 或 LOG WITH PASSWORD<br /><br /> BACKUP DATABASE 或 LOG WITH MEDIAPASSWORD|104<br /><br /> 103|  
|兼容级别|从版本 100（[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]）升级。|如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本结束[支持](/lifecycle/products/?products=sql-server)，相关数据库兼容性级别将标记为已弃用。 但是，我们将继续支持在任何支持的数据库兼容性级别上认证的应用程序尽可能长的时间，使升级更简单。 有关兼容性级别的详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。|数据库兼容性级别 100|108|  
|数据库对象|从触发器返回结果集的功能|无|从触发器返回结果|12|  
|加密|不推荐使用通过 RC4 或 RC4_128 进行加密，并计划在下一个版本中删除这种加密方法。 不推荐使用 RC4 和 RC4_128 解密。|请使用其他加密算法，例如 AES。|不推荐使用的加密算法|253|  
|哈希算法|使用 MD2、MD4、MD5、SHA 和 SHA1 已弃用。|请改用 SHA2_256 或 SHA2_512。 旧算法将继续起作用，但会抛出弃用事件。|弃用的哈希算法|无|  
|远程服务器|sp_addremotelogin<br /><br /> sp_addserver<br /><br /> sp_dropremotelogin<br /><br /> sp_helpremotelogin<br /><br /> sp_remoteoption|用链接服务器替代远程服务器。 sp_addserver 仅可与本地选项一起使用。|sp_addremotelogin<br /><br /> sp_addserver<br /><br /> sp_dropremotelogin<br /><br /> sp_helpremotelogin<br /><br /> sp_remoteoption|70<br /><br /> 69<br /><br /> 71<br /><br /> 72<br /><br /> 73|  
|远程服务器|\@\@remserver|用链接服务器替代远程服务器。|无|无|  
|远程服务器|SET REMOTE_PROC_TRANSACTIONS|用链接服务器替代远程服务器。|SET REMOTE_PROC_TRANSACTIONS|110|  
|表提示|不带括号的 HOLDLOCK 表提示。|使用 HOLDLOCK 以及括号。|不带括号的 HOLDLOCK 表提示|167|  
  
## <a name="features-deprecated-in-a-future-version-of-sql-server"></a>SQL Server 未来版本中弃用的功能  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的下一版本仍支持以下 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 功能，但以后的版本将弃用这些功能。 具体是哪一 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本现在还未确定。  
  
|类别|不推荐使用的功能|替代功能|功能名称|功能 ID|  
|--------------|------------------------|-----------------|------------------|----------------|  
|兼容级别|sp_dbcmptlevel|ALTER DATABASE ...SET COMPATIBILITY_LEVEL。 有关详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。|sp_dbcmptlevel|80|  
|兼容级别|数据库兼容性级别 110 和 120。|计划为未来版本升级数据库和应用程序。 但是，我们将继续支持在任何支持的数据库兼容性级别上认证的应用程序尽可能长的时间，使升级更简单。 有关兼容性级别的详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。|数据库兼容性级别 110<br /><br /> 数据库兼容性级别 120||  
|XML|内联 XDR 架构生成|不推荐使用 FOR XML 选项的 XMLDATA 指令。 如果是 RAW 和 AUTO 模式，请使用 XSD 生成。 在 EXPLICT 模式下，没有可以代替 XMLDATA 指令的项。|XMLDATA|181|  
|备份和还原|BACKUP { DATABASE &#124; LOG } TO TAPE<br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_tape*|BACKUP { DATABASE &#124; LOG } TO DISK<br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_disk*|BACKUP DATABASE 或 LOG TO TAPE|235|  
|备份和还原|sp_addumpdevice'**tape**'|sp_addumpdevice'**disk**'|ADDING TAPE DEVICE|236|  
|备份和还原|sp_helpdevice|sys.backup_devices|sp_helpdevice|100|  
|排序规则|Korean_Wansung_Unicode<br /><br /> Lithuanian_Classic<br /><br /> SQL_AltDiction_CP1253_CS_AS|无。 [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]中存在这些排序规则，但 fn_helpcollations 并不将它们显示出来。|Korean_Wansung_Unicode<br /><br /> Lithuanian_Classic<br /><br /> SQL_AltDiction_CP1253_CS_AS|191<br /><br /> 192<br /><br /> 194|  
|排序规则|Hindi<br /><br /> 马其顿语|[!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] 及更高版本中存在这些排序规则，但 fn_helpcollations 并不将它们显示出来。 请改用 Macedonian_FYROM_90 和 Indic_General_90。|Hindi<br /><br /> 马其顿语|190<br /><br /> 193|  
|排序规则|Azeri_Latin_90<br /><br /> Azeri_Cyrilllic_90|Azeri_Latin_100<br /><br /> Azeri_Cyrilllic_100|Azeri_Latin_90<br /><br /> Azeri_Cyrilllic_90|232<br /><br /> 233|  
|配置|SET ANSI_NULLS OFF 和 ANSI_NULLS OFF 数据库选项<br /><br /> SET ANSI_PADDING OFF 和 ANSI_PADDING OFF 数据库选项<br /><br /> SET CONCAT_NULL_YIELDS_NULL OFF 和 CONCAT_NULL_YIELDS_NULL OFF 数据库选项<br /><br /> SET OFFSETS|无。<br /><br /> ANSI_NULLS、ANSI_PADDING 和 CONCAT_NULLS_YIELDS_NULL 这三个选项在任何情况下始终设置为 ON。 SET OFFSETS 将不可用。|SET ANSI_NULLS OFF<br /><br /> SET ANSI_PADDING OFF<br /><br /> SET CONCAT_NULL_YIELDS_NULL OFF<br /><br /> SET OFFSETS<br /><br /> ALTER DATABASE SET ANSI_NULLS OFF<br /><br /> ALTER DATABASE SET ANSI_PADDING OFF<br /><br /> ALTER DATABASE SET CONCAT_NULL_YIELDS_NULL OFF|111<br /><br /> 113<br /><br /> 112<br /><br /> 36<br /><br /> 111<br /><br /> 113<br /><br /> 112|  
|数据类型|sp_addtype<br /><br /> sp_droptype|CREATE TYPE<br /><br /> DROP TYPE|sp_addtype<br /><br /> sp_droptype|62<br /><br /> 63|  
|数据类型|**timestamp** 数据类型的 **rowversion** 语法|**rowversion** 数据类型语法|TIMESTAMP|158|  
|数据类型|在 **timestamp** 列中插入 null 值的功能。|请改用 DEFAULT。|将 NULL 插入 TIMESTAMP 列|179|  
|数据类型|“text in row”表选项|使用 **varchar(max)** 、**nvarchar(max)** 和 **varbinary(max)** 数据类型。 有关详细信息，请参阅 [sp_tableoption (Transact-SQL)](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)。|text in row 表选项|9|  
|数据类型|数据类型：<br /><br /> **text**<br /><br /> **ntext**<br /><br /> **图像**|使用 **varchar(max)** 、**nvarchar(max)** 和 **varbinary(max)** 数据类型。|数据类型： **text**、 **ntext** 或 **image**|4|  
|数据库管理|sp_attach_db<br /><br /> sp_attach_single_file_db|使用 FOR ATTACH 选项的 CREATE DATABASE 语句。 若要在一个或多个日志文件有新位置的情况下重新生成这些日志文件，请使用 FOR ATTACH_REBUILD_LOG 选项。|sp_attach_db<br /><br /> sp_attach_single_file_db|81<br /><br /> 82|  
|数据库对象|CREATE DEFAULT<br /><br /> DROP DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault|CREATE TABLE 和 ALTER TABLE 中的 DEFAULT 关键字|CREATE_DROP_DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault|162<br /><br /> 64<br /><br /> 65|  
|数据库对象|CREATE RULE<br /><br /> DROP RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule|CREATE TABLE 和 ALTER TABLE 中的 CHECK 关键字|CREATE_DROP_RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule|161<br /><br /> 66<br /><br /> 67|  
|数据库对象|sp_change_users_login|使用 ALTER USER。|sp_change_users_login|231|  
|数据库对象|sp_depends|sys.dm_sql_referenced_entities 和 sys.dm_sql_referenced_entities|sp_depends|19|  
|数据库对象|sp_renamedb|ALTER DATABASE 中的 MODIFY NAME|sp_renamedb|79|  
|数据库对象|sp_getbindtoken|使用 MARS 或分布式事务。|sp_getbindtoken|98|  
|数据库选项|sp_bindsession|使用 MARS 或分布式事务。|sp_bindsession|97|  
|数据库选项|sp_resetstatus|ALTER DATABASE SET { ONLINE &#124; EMERGENCY }|sp_resetstatus|83|  
|数据库选项|ALTER DATABASE 的 TORN_PAGE_DETECTION 选项|ALTER DATABASE 的 PAGE_VERIFY TORN_PAGE_DETECTION 选项|ALTER DATABASE WITH TORN_PAGE_DETECTION|102|  
|DBCC|DBCC DBREINDEX|ALTER INDEX 的 REBUILD 选项。|DBCC DBREINDEX|11|  
|DBCC|DBCC INDEXDEFRAG|ALTER INDEX 的 REORGANIZE 选项|DBCC INDEXDEFRAG|18|  
|DBCC|DBCC SHOWCONTIG|sys.dm_db_index_physical_stats|DBCC SHOWCONTIG|10|  
|DBCC|DBCC PINTABLE<br /><br /> DBCC UNPINTABLE|无效。|DBCC [UN]PINTABLE|189|  
|扩展属性|用 Level0type = 'type' 和 Level0type = 'USER' 向 1 级或 2 级类型对象添加扩展属性。|用 Level0type = 'USER' 只可直接向用户或角色添加扩展属性。<br /><br /> 用 Level0type = 'SCHEMA' 向 1 级类型（如 TABLE 或 VIEW）或 2 级类型（如 COLUMN 或 TRIGGER）添加扩展属性。 有关详细信息，请参阅 [sp_addextendedproperty (Transact-SQL)](../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)。|EXTPROP_LEVEL0TYPE<br /><br /> EXTPROP_LEVEL0USER|13<br /><br /> 14|  
|扩展存储过程编程|srv_alloc<br /><br /> srv_convert<br /><br /> srv_describe<br /><br /> srv_getbindtoken<br /><br /> srv_got_attention<br /><br /> srv_message_handler<br /><br /> srv_paramdata<br /><br /> srv_paraminfo<br /><br /> srv_paramlen<br /><br /> srv_parammaxlen<br /><br /> srv_paramname<br /><br /> srv_paramnumber<br /><br /> srv_paramset<br /><br /> srv_paramsetoutput<br /><br /> srv_paramstatus<br /><br /> srv_paramtype<br /><br /> srv_pfield<br /><br /> srv_pfieldex<br /><br /> srv_rpcdb<br /><br /> srv_rpcname<br /><br /> srv_rpcnumber<br /><br /> srv_rpcoptions<br /><br /> srv_rpcowner<br /><br /> srv_rpcparams<br /><br /> srv_senddone<br /><br /> srv_sendmsg<br /><br /> srv_sendrow<br /><br /> srv_setcoldata<br /><br /> srv_setcollen<br /><br /> srv_setutype<br /><br /> srv_willconvert<br /><br /> srv_wsendmsg|请改用 CLR 集成。|XP_API|20|  
|扩展存储过程编程|sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc|请改用 CLR 集成。|sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc|94<br /><br /> 95<br /><br /> 96|  
|扩展的存储过程|xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|使用 CREATE LOGIN<br /><br /> 使用 SERVERPROPERTY 的 DROP LOGIN IsIntegratedSecurityOnly 参数|xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|44<br /><br /> 45<br /><br /> 59|  
|函数|fn_get_sql|sys.dm_exec_sql_text|fn_get_sql|151|  
|高可用性|数据库镜像|[!INCLUDE[ssHADR](../includes/sshadr-md.md)]<br /><br /> 如果您的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本不支持 [!INCLUDE[ssHADR](../includes/sshadr-md.md)]，请使用日志传送。|DATABASE_MIRRORING|267|  
|索引选项|sp_indexoption|ALTER INDEX|sp_indexoption|78|  
|索引选项|选项两侧没有括号的 CREATE TABLE、ALTER TABLE 或 CREATE INDEX 语法。|请重写语句以使用当前语法。|INDEX_OPTION|33|  
|实例选项|sp_configure option 'allow updates'|系统表不再可更新。 其设置不起作用。|sp_configure 'allow updates'|173|  
|实例选项|sp_configure 选项：<br /><br /> 'locks'<br /><br /> 'open objects'<br /><br /> 'set working set size'|这些选项目前为自动配置。 其设置不起作用。|sp_configure 'locks'<br /><br /> sp_configure 'open objects'<br /><br /> sp_configure 'set working set size'|174<br /><br /> 175<br /><br /> 176|  
|实例选项|sp_configure option 'priority boost'|系统表不再可更新。 其设置不起作用。 请改用 Windows start /high … program.exe 选项。|sp_configure 'priority boost'|199|  
|实例选项|sp_configure option 'remote proc trans'|系统表不再可更新。 其设置不起作用。|sp_configure 'remote proc trans'|37|  
|链接服务器|对于链接服务器，指定 SQLOLEDB 访问接口。|SQL Server Native Client (SQLNCLI)|对于链接服务器使用 SQLOLEDDB|19|  
|锁定|sp_lock|sys.dm_tran_locks|sp_lock|99|  
|元数据|FILE_ID<br /><br /> INDEXKEY_PROPERTY|FILE_IDEX<br /><br /> sys.index_columns|FILE_ID<br /><br /> INDEXKEY_PROPERTY|15<br /><br /> 17|  
|本机 XML Web 服务|带有 FOR SOAP 选项的 CREATE ENDPOINT 或 ALTER ENDPOINT 语句。<br /><br /> sys.endpoint_webmethods<br /><br /> sys.soap_endpoints|请改用 Windows Communications Foundation (WCF) 或 ASP.NET。|CREATE/ALTER ENDPOINT<br /><br /> sys.endpoint_webmethods<br /><br /> EXT_soap_endpoints<br /><br /> sys.soap_endpoints|21<br /><br /> 22<br /><br /> 23|  
|可删除的数据库|sp_certify_removable<br /><br /> sp_create_removable|sp_detach_db|sp_certify_removable<br /><br /> sp_create_removable|74<br /><br /> 75|  
|可删除的数据库|sp_dbremove|DROP DATABASE|sp_dbremove|76|  
|安全性|ALTER LOGIN WITH SET CREDENTIAL 语法|由新的 ALTER LOGIN ADD 和 DROP CREDENTIAL 语法取代|ALTER LOGIN WITH SET CREDENTIAL|230|  
|安全性|sp_addapprole<br /><br /> sp_dropapprole|CREATE APPLICATION ROLE<br /><br /> DROP APPLICATION ROLE|sp_addapprole<br /><br /> sp_dropapprole|53<br /><br /> 54|  
|安全性|sp_addlogin<br /><br /> sp_droplogin|CREATE LOGIN<br /><br /> DROP LOGIN|sp_addlogin<br /><br /> sp_droplogin|39<br /><br /> 40|  
|安全性|sp_adduser<br /><br /> sp_dropuser|CREATE USER<br /><br /> DROP USER|sp_adduser<br /><br /> sp_dropuser|49<br /><br /> 50|  
|安全性|sp_grantdbaccess<br /><br /> sp_revokedbaccess|CREATE USER<br /><br /> DROP USER|sp_grantdbaccess<br /><br /> sp_revokedbaccess|51<br /><br /> 52|  
|安全性|sp_addrole<br /><br /> sp_droprole|CREATE ROLE<br /><br /> DROP ROLE|sp_addrole<br /><br /> sp_droprole|56<br /><br /> 57|  
|安全性|sp_approlepassword<br /><br /> sp_password|ALTER APPLICATION ROLE<br /><br /> ALTER LOGIN|sp_approlepassword<br /><br /> sp_password|55<br /><br /> 46|  
|安全性|sp_changeobjectowner|ALTER SCHEMA 或 ALTER AUTHORIZATION|sp_changeobjectowner|58|  
|安全性|sp_control_dbmasterkey_password|主密钥必须存在，并且密码必须是正确的。|sp_control_dbmasterkey_password|274|  
|安全性|sp_defaultdb<br /><br /> sp_defaultlanguage|ALTER LOGIN|sp_defaultdb<br /><br /> sp_defaultlanguage|47<br /><br /> 48|  
|安全性|sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin|ALTER LOGIN DISABLE<br /><br /> CREATE LOGIN<br /><br /> DROP LOGIN|sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin|42<br /><br /> 41<br /><br /> 43|  
|安全性|USER_ID|DATABASE_PRINCIPAL_ID|USER_ID|16|  
|安全性|sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission|这些存储过程返回在 [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]中是正确的信息。 该输出不反映在 [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]中实现的权限层次结构的更改。 有关详细信息，请参阅 [固定服务器角色的权限](https://msdn.microsoft.com/library/ms175892\(SQL.100\).aspx)。|sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission|61<br /><br /> 60|  
|安全性|GRANT ALL<br /><br /> DENY ALL<br /><br /> REVOKE ALL|GRANT、DENY 和 REVOKE 特定权限。|ALL 权限|35|  
|安全性|PERMISSIONS 内部函数|请改为查询 sys.fn_my_permissions。|PERMISSIONS|170|  
|安全性|SETUSER|EXECUTE AS|SETUSER|165|  
|安全性|RC4 和 DESX 加密算法|请使用其他算法，如 AES。|DESX 算法|238|  
|SET 选项|SET FMTONLY|[sys.dm_exec_describe_first_result_set (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)、[sys.dm_exec_describe_first_result_set_for_object (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)、[sp_describe_first_result_set (Transact-SQL)](../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) 和 [sp_describe_undeclared_parameters (Transact-SQL)](../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)。|SET FMTONLY|250|  
|服务器配置选项|c2 审核选项<br /><br /> default trace enabled 选项|[启用了通用准则合规性的服务器配置选项](../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md)<br /><br /> [扩展事件](../relational-databases/extended-events/extended-events.md)|sp_configure 'c2 audit mode'<br /><br /> sp_configure 'default trace enabled'|252<br /><br /> 253|  
|SMO 类|**Microsoft.SQLServer.Management.Smo.Information** 类<br /><br /> **Microsoft.SQLServer.Management.Smo.Settings** 类<br /><br /> **Microsoft.SQLServer.Management.Smo.DatabaseOptions** 类<br /><br /> **Microsoft.SqlServer.Management.Smo.DatabaseDdlTrigger.NotForReplication** 属性|**Microsoft.SqlServer.Management.Smo.Server** 类<br /><br /> **Microsoft.SqlServer.Management.Smo.Server** 类<br /><br /> **Microsoft.SqlServer.Management.Smo.Database** 类<br /><br /> 无|无|无|  
|SQL Server 代理|**net send** 通知<br /><br /> 寻呼通知|电子邮件通知<br /><br /> 电子邮件通知 |无|无|  
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|解决方案资源管理器集成到 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]||无|无|  
|系统存储过程|sp_db_increased_partitions|无。 默认情况下， [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]提供了对增加的分区的支持。|sp_db_increased_partitions|253|  
|系统表|sysaltfiles<br /><br /> syscacheobjects<br /><br /> syscolumns<br /><br /> syscomments<br /><br /> sysconfigures<br /><br /> sysconstraints<br /><br /> syscurconfigs<br /><br /> sysdatabases<br /><br /> sysdepends<br /><br /> sysdevices<br /><br /> sysfilegroups<br /><br /> sysfiles<br /><br /> sysforeignkeys<br /><br /> sysfulltextcatalogs<br /><br /> sysindexes<br /><br /> sysindexkeys<br /><br /> syslockinfo<br /><br /> syslogins<br /><br /> sysmembers<br /><br /> sysmessages<br /><br /> sysobjects<br /><br /> sysoledbusers<br /><br /> sysopentapes<br /><br /> sysperfinfo<br /><br /> syspermissions<br /><br /> sysprocesses<br /><br /> sysprotects<br /><br /> sysreferences<br /><br /> sysremotelogins<br /><br /> sysservers<br /><br /> systypes<br /><br /> sysusers|兼容性视图。 有关详细信息，请参阅[兼容性视图 (Transact-SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)。<br /><br /> **重要提示：** 兼容性视图不公开 [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] 中引入功能的元数据。 建议将应用程序升级为使用目录视图。 有关详细信息，请参阅[目录视图 (Transact-SQL)](../relational-databases/system-catalog-views/catalog-views-transact-sql.md)。|sysaltfiles<br /><br /> syscacheobjects<br /><br /> syscolumns<br /><br /> syscomments<br /><br /> sysconfigures<br /><br /> sysconstraints<br /><br /> syscurconfigs<br /><br /> sysdatabases<br /><br /> sysdepends<br /><br /> sysdevices<br /><br /> sysfilegroups<br /><br /> sysfiles<br /><br /> sysforeignkeys<br /><br /> sysfulltextcatalogs<br /><br /> sysindexes<br /><br /> sysindexkeys<br /><br /> syslockinfo<br /><br /> syslogins<br /><br /> sysmembers<br /><br /> sysmessages<br /><br /> sysobjects<br /><br /> sysoledbusers<br /><br /> sysopentapes<br /><br /> sysperfinfo<br /><br /> syspermissions<br /><br /> sysprocesses<br /><br /> sysprotects<br /><br /> sysreferences<br /><br /> sysremotelogins<br /><br /> sysservers<br /><br /> systypes<br /><br /> sysusers|141<br /><br /> 无<br /><br /> 133<br /><br /> 126<br /><br /> 146<br /><br /> 131<br /><br /> 147<br /><br /> 142<br /><br /> 123<br /><br /> 144<br /><br /> 128<br /><br /> 127<br /><br /> 130<br /><br /> 122<br /><br /> 132<br /><br /> 134<br /><br /> 143<br /><br /> 140<br /><br /> 119<br /><br /> 137<br /><br /> 125<br /><br /> 139<br /><br /> 145<br /><br /> 157<br /><br /> 121<br /><br /> 153<br /><br /> 120<br /><br /> 129<br /><br /> 138<br /><br /> 136<br /><br /> 135<br /><br /> 124|  
|系统表|sys.numbered_procedures<br /><br /> sys.numbered_procedure_parameters|无|numbered_procedures<br /><br /> numbered_procedure_parameters|148<br /><br /> 149|  
|系统函数|fn_virtualservernodes<br /><br /> fn_servershareddrives|sys.dm_os_cluster_nodes<br /><br /> sys.dm_io_cluster_shared_drives|fn_virtualservernodes<br /><br /> fn_servershareddrives|155<br /><br /> 156|  
|系统视图|sys.sql_dependencies|sys.sql_expression_dependencies|sys.sql_dependencies|198|  
|表压缩|vardecimal 存储格式的使用。|不推荐使用 Vardecimal 存储格式。 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] 提供的数据压缩功能可以压缩十进制值和其他数据类型的值。 建议您使用数据压缩，而不使用 vardecimal 存储格式。|vardecimal 存储格式|200|  
|表压缩|sp_db_vardecimal_storage_format 过程的使用。|不推荐使用 Vardecimal 存储格式。 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] 提供的数据压缩功能可以压缩十进制值和其他数据类型的值。 建议您使用数据压缩，而不使用 vardecimal 存储格式。|sp_db_vardecimal_storage_format|201|  
|表压缩|sp_estimated_rowsize_reduction_for_vardecimal 过程的使用。|请改用数据压缩和 sp_estimate_data_compression_savings 过程。|sp_estimated_rowsize_reduction_for_vardecimal|202|  
|表提示|在 UPDATE 或 DELETE 语句的 FROM 子句中指定 NOLOCK 或 READUNCOMMITTED。|请从 FROM 子句中删除 NOLOCK 或 READUNCOMMITTED 表提示。|UPDATE 或 DELETE 中的 NOLOCK 或 READUNCOMMITTED|1|  
|表提示|不借助 WITH 关键字指定表提示。|使用 WITH。|不带 WITH 的表提示|8|  
|表提示|INSERT_HINTS||INSERT_HINTS|34|  
|Textpointers|WRITETEXT<br /><br /> UPDATETEXT<br /><br /> READTEXT|无|UPDATETEXT 或 WRITETEXT<br /><br /> READTEXT|115<br /><br /> 114|  
|Textpointers|TEXTPTR()<br /><br /> TEXTVALID()|无|TEXTPTR<br /><br /> TEXTVALID|5<br /><br /> 6|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|:: 函数调用序列|替换为 SELECT column_list FROM sys.\<*function_name*>()。<br /><br /> 例如，将 `SELECT * FROM ::fn_virtualfilestats(2,1)`替换为 `SELECT * FROM sys.fn_virtualfilestats(2,1)`。|“::”函数调用语法|166|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|由三部分和四部分组成的列引用。|由两部分组成的名称是符合标准的行为。|两个以上的部分构成的列名称|3|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|用引号引起来的字符串用作 SELECT 列表中表达式的列别名：<br /><br /> '*string_alias*' = *expression*|*expression* [AS] *column_alias*<br /><br /> *expression* [AS] [*column_alias*]<br /><br /> *expression* [AS] "*column_alias*"<br /><br /> *expression* [AS] '*column_alias*'<br /><br /> *column_alias* = *expression*|字符串文字作为列别名|184|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|编号过程|无。 请勿使用。|ProcNums|160|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|在 DROP INDEX 中使用 *table_name.index_name* 语法|在 DROP INDEX 中使用 *index_name* ON *table_name* 语法。|DROP INDEX 具有两部分构成的名称|163|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|不使用分号结束 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句。|使用分号 ( ; ) 结束 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句。|无|无|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|GROUP BY ALL|将自定义逐案例解决方案与 UNION 或派生表配合使用。|GROUP BY ALL|169|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|ROWGUIDCOL 在 DML 语句中用作列名。|请使用 $rowguid。|ROWGUIDCOL|182|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|IDENTITYCOL 在 DML 语句中用作列名。|请使用 $identity。|IDENTITYCOL|183|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|将 #、## 用作临时表和临时存储过程名称。|请至少使用一个其他字符。|“#”和“##”作为临时表和存储过程的名称|185|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|将 \@、\@\@ 或 \@\@ 用作 [!INCLUDE[tsql](../includes/tsql-md.md)] 标识符。|请勿使用 \@ 或 \@\@ 或以 \@\@ 开头的名称作为标识符。|“\@”和以“\@\@”开头的名称作为 [!INCLUDE[tsql](../includes/tsql-md.md)] 标识符|186.|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|将 DEFAULT 关键字用作默认值。|不要将单词 DEFAULT 用作默认值。|DEFAULT 关键字作为默认值|187|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|将空格用作表提示之间的分隔符。|使用逗号分隔各个表提示。|没有逗号的多个表提示|168|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|聚合索引视图的选择列表在 90 兼容模式下必须包含 COUNT_BIG (\*)|请使用 COUNT_BIG (\*)。|不包含 COUNT_BIG(\*) 的索引视图选择列表|2|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|表提示通过视图间接应用于多语句表值函数 (TVF) 的调用。|无。|间接 TVF 提示|7|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|ALTER DATABASE 语法:<br /><br /> MODIFY FILEGROUP READONLY<br /><br /> MODIFY FILEGROUP READWRITE|MODIFY FILEGROUP READ_ONLY<br /><br /> MODIFY FILEGROUP READ_WRITE|MODIFY FILEGROUP READONLY<br /><br /> MODIFY FILEGROUP READWRITE|195<br /><br /> 196|  
|其他|DB-Library<br /><br /> 用于 C 语言的嵌入式 SQL|尽管 [!INCLUDE[ssDE](../includes/ssde-md.md)] 仍然支持来自使用 DB-Library 和嵌入式 SQL API 的现有应用程序的连接，但它不包括在使用这些 API 的应用程序上进行编程工作所需的文件或文档。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 的未来版本将不再支持来自 DB-Library 或嵌入式 SQL 应用程序的连接。 请不要使用 DB-Library 或嵌入式 SQL 来开发新的应用程序。 修改现有应用程序时，请删除 DB-Library 或嵌入式 SQL 的任何依赖项。 请使用 SQLClient 命名空间或诸如 ODBC 的 API，而不使用这些 API。 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] 不包含运行这些应用程序所需的 DB-Library DLL。 若要运行 DB-Library 或嵌入式 SQL 应用程序，必须有 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 6.5 版、 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 7.0 版或 [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]提供的 DB-Library DLL。|无|无|  
|工具|SQL Server Profiler for Trace Capture|使用 SQL Server Management Studio 中嵌入的扩展事件探查器。|SQL Server Profiler|无|  
|工具|SQL Server Profiler for Trace Replay|[SQL Server 分布式重播](../tools/distributed-replay/sql-server-distributed-replay.md)|SQL Server Profiler|无|  
|跟踪管理对象|Microsoft.SqlServer.Management.Trace 命名空间（包含用于 SQL Server 跟踪和重播对象的 API）|跟踪配置： <xref:Microsoft.SqlServer.Management.XEvent><br /><br /> 跟踪读取： <xref:Microsoft.SqlServer.XEvent.Linq><br /><br /> 跟踪重播：无|||  
|SQL 跟踪存储过程、函数和目录视图|sp_trace_create<br /><br /> sp_trace_setevent<br /><br /> sp_trace_setfilter<br /><br /> sp_trace_setstatus<br /><br /> fn_trace_geteventinfo<br /><br /> fn_trace_getfilterinfo<br /><br /> fn_trace_getinfo<br /><br /> fn_trace_gettable<br /><br /> sys.traces<br /><br /> sys.trace_events<br /><br /> sys.trace_event_bindings<br /><br /> sys.trace_categories<br /><br /> sys.trace_columns<br /><br /> sys.trace_subclass_values|[扩展事件](../relational-databases/extended-events/extended-events.md)|sp_trace_create<br /><br /> sp_trace_setevent<br /><br /> sp_trace_setfilter<br /><br /> sp_trace_setstatus<br /><br /> fn_trace_geteventinfo<br /><br /> fn_trace_getfilterinfo<br /><br /> fn_trace_getinfo<br /><br /> fn_trace_gettable<br /><br /> sys.traces<br /><br /> sys.trace_events<br /><br /> sys.trace_event_bindings<br /><br /> sys.trace_categories<br /><br /> sys.trace_columns<br /><br /> sys.trace_subclass_values|258<br /><br /> 260<br /><br /> 261<br /><br /> 259<br /><br /> 256<br /><br /> 257|
|SET 选项|适用于 **SET ROWCOUNT** 、 **INSERT**, **UPDATE** 语句的 **DELETE**|TOP 关键字|SET ROWCOUNT|109|  

  
> [!NOTE]  
> **sp_setapprole** 的 cookie **OUTPUT** 参数现记载为 **varbinary(8000)** ，这是正确的最大长度。 但是，目前执行返回 **varbinary(50)** 。 如果开发人员已分配 **varbinary(50)** ，当 cookie 在将来的版本中返回大小增量时，应用程序可能需要更改。 尽管这不是不推荐使用的问题，但本主题中提到了，因为应用程序调整都是类似的。 有关详细信息，请参阅 [sp_setapprole (Transact-SQL)](../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 2016 中废止的数据库引擎功能](./discontinued-database-engine-functionality-in-sql-server.md)     
 [SQL Server 2017 中弃用的数据库引擎功能](../database-engine/deprecated-database-engine-features-in-sql-server-2017.md)