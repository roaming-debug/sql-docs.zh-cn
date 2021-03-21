---
title: SQL Server 和 Azure SQL 数据库的安全文档
description: 提供 SQL Server 和 Azure SQL 数据库的安全与保护相关内容参考。
ms.custom: seo-lt-2019
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- SQL Server, security
- security [SQL Server]
- database security [SQL Server]
- databases [SQL Server], security
ms.assetid: dfb39d16-722a-4734-94bb-98e61e014ee7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 799f00bfd7ddf0763212cefb00d8fea72a7dce27
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739917"
---
# <a name="security-center-for-sql-server-database-engine-and-azure-sql-database"></a>SQL Server 数据库引擎和 Azure SQL Database 的安全中心
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本页提供的链接可帮助你找到有关 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 和 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]中的安全性和保护的必要信息。  
  
 **图例**  
  
 ![说明功能可用性图标的图例的屏幕截图。](../performance/media/security-center-legend.PNG "security-center-legend")  
  
##  <a name="authentication-who-are-you"></a><a name="Who"></a> 身份验证：你是谁？  
  
|||  
|-|-|  
|**谁进行身份验证？**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Windows 身份验证<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Azure Active Directory|谁进行身份验证？ （Windows 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）<br /><br /> [选择身份验证模式](../../relational-databases/security/choose-an-authentication-mode.md)<br /><br /> [使用 Azure Active Directory 身份验证连接到 SQL 数据库](/azure/azure-sql/database/authentication-aad-overview)|  
|**在哪里进行身份验证？**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 在 master 数据库：登录名和 DB 用户<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 在用户数据库：包含的 DB 用户|在 master 数据库进行身份验证（登录名和数据库用户）<br /><br /> [创建 SQL Server 登录名](../../relational-databases/security/authentication-access/create-a-login.md)<br /><br /> [在 Azure SQL 数据库中管理数据库和登录名](/previous-versions/azure/ee336235(v=azure.100))<br /><br /> [创建数据库用户](../../relational-databases/security/authentication-access/create-a-database-user.md)<br /><br /> <br /><br /> 在用户数据库进行身份验证<br /><br /> [包含的数据库用户 - 使数据库可移植](../../relational-databases/security/contained-database-users-making-your-database-portable.md)|  
|**使用其他标识**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 凭据<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: 以其他登录身份执行<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 以其他数据库用户身份执行|[凭据（数据库引擎）](../../relational-databases/security/authentication-access/credentials-database-engine.md)<br /><br /> [作为另一个登录名执行](../../t-sql/statements/execute-as-transact-sql.md)<br /><br /> [作为另一个数据库用户执行](../../t-sql/statements/execute-as-transact-sql.md)|  
  
##  <a name="authorization-what-can-you-do"></a><a name="What"></a> 授权：你可以做什么？  
  
|||  
|-|-|  
|**授予、撤消和拒绝权限**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 安全对象类<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: 具体服务器权限<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 具体数据库权限|[权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)<br /><br /> [权限](../../relational-databases/security/permissions-database-engine.md)<br /><br /> [安全对象](../../relational-databases/security/securables.md)<br /><br /> [数据库引擎权限入门](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)|  
|**按角色分类的安全性**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: 服务器级别角色<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 数据库级别角色|[服务器级角色](../../relational-databases/security/authentication-access/server-level-roles.md)<br /><br /> [数据库级别的角色](../../relational-databases/security/authentication-access/database-level-roles.md)|  
|**限制对所选数据元素的数据访问**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 使用视图/过程限制数据访问<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 行级安全性<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 动态数据掩码<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 签名的对象|使用 [视图](../../relational-databases/views/views.md) 和 [过程](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)限制数据访问<br /><br /> [行级别安全性 (SQL Server)](../../relational-databases/security/row-level-security.md)<br /><br /> [行级别安全性 (Azure SQL Database)](./row-level-security.md)<br /><br /> [动态数据掩码 (SQL Server)](../../relational-databases/security/dynamic-data-masking.md)<br /><br /> [动态数据掩码 (Azure SQL Database)](/azure/azure-sql/database/dynamic-data-masking-overview)<br /><br /> [签名的对象](../../t-sql/statements/add-signature-transact-sql.md)|  
  
##  <a name="encryption-storing-secret-data"></a><a name="Encrypt"></a> 加密：存储机密数据  
  
|||  
|-|-|  
|**加密文件**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: BitLocker 加密（驱动器级别）<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: NTFS 加密（文件夹级别）<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 透明数据加密（文件级别）<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 备份加密（文件级别）|[BitLocker（驱动器级别）](https://support.microsoft.com/kb/2855131)<br /><br /> [NTFS 加密（文件夹级别）](/previous-versions/tn-archive/dd163562(v=technet.10))<br /><br /> [透明数据加密（文件级别）](../../relational-databases/security/encryption/transparent-data-encryption.md)<br /><br /> [备份加密（文件级别）](../../relational-databases/backup-restore/backup-encryption.md)|  
|**加密源**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: 可扩展密钥管理模块<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Azure Key Vault 中存储的密钥<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Always Encrypted|[可扩展密钥管理模块](../../relational-databases/security/encryption/extensible-key-management-ekm.md)<br /><br /> [Azure 密钥保管库中存储的密钥](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)<br /><br /> [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)|  
|**列、数据和密钥加密**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 使用证书进行加密<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 使用对称密钥进行加密<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 使用非对称密钥进行加密<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 使用密码进行加密|[使用证书进行加密](../../t-sql/functions/encryptbycert-transact-sql.md)<br /><br /> [使用非对称密钥进行加密](../../t-sql/functions/encryptbyasymkey-transact-sql.md)<br /><br /> [使用对称密钥进行加密](../../t-sql/functions/encryptbykey-transact-sql.md)<br /><br /> [使用密码进行加密](../../t-sql/functions/encryptbypassphrase-transact-sql.md)<br /><br /> [加密数据列](../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
##  <a name="connection-security-restricting-and-securing"></a><a name="Connect"></a> 连接安全性：限制和保护  
  
|||  
|-|-|  
|**防火墙保护**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Windows 防火墙设置<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Azure 服务防火墙设置<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: 数据库防火墙设置|[为数据库引擎访问配置 Windows 防火墙](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)<br /><br /> [Azure SQL 数据库防火墙设置](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)<br /><br /> [Azure 服务防火墙设置](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)|  
|**在传输过程中加密数据**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: 强制的 SSL 连接<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: 可选的 SSL 连接|[启用数据库引擎的加密连接](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)<br /><br /> [启用与数据库引擎建立加密连接](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)、[网络安全](/azure/sql-database/sql-database-security-best-practice#network-security) <br /><br /> [针对 Microsoft SQL Server 的 TLS 1.2 支持](https://support.microsoft.com/kb/3135244)|  
  
##  <a name="auditing-recording-access"></a><a name="Audit"></a> 审核：记录访问  
  
|||  
|-|-|  
|**自动审核**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 审核（服务器和 DB 级别）<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 审核（数据库级别）<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: 检测威胁| <br /><br /> [SQL Server Audit（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)<br /><br /> [SQL 数据库审核](/azure/azure-sql/database/auditing-overview)<br /><br /> [SQL 数据库高级威胁防护入门](/azure/azure-sql/database/threat-detection-configure) <br /><br /> [SQL 数据库漏洞评估](/azure/sql-database/sql-vulnerability-assessment) |  
|**自定义审核**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: 触发器|自定义审核实现：创建 [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md) 和 [DML Triggers](../../relational-databases/triggers/dml-triggers.md)|  
|**遵从性**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: 合规性|SQL Server：<br />                        [通用准则](https://go.microsoft.com/fwlink/?LinkId=616319)<br /><br /> SQL 数据库：<br />                        [Microsoft Azure 信任中心：符合性（按功能）](https://azure.microsoft.com/support/trust-center/services/)|  
  
##  <a name="sql-injection"></a><a name="SQLInjection"></a> SQL 注入  
 SQL 注入是一种攻击方式，可将恶意代码插入字符串中，稍后这些字符串会传递到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 进行分析和执行。 任何构成 SQL 语句的过程都应进行注入漏洞检查，因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将执行其接收到的所有语法有效的查询。 所有的数据库系统都会面临一些受到 SQL 注入攻击的风险，并会受到正在查询 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的应用程序中引入的许多安全漏洞的威胁。 你可以防止受到 SQL 注入攻击，具体方法为使用存储过程和参数化命令，避免使用动态 SQL，并限制所有用户的权限。  有关详细信息，请参阅 [SQL Injection](../../relational-databases/security/sql-injection.md)。  
  
 向应用程序程序员提供的其他链接：  
  
-   [SQL Server 中的应用程序安全方案](/dotnet/framework/data/adonet/sql/application-security-scenarios-in-sql-server)  
  
-   [在 SQL Server 中编写安全动态 SQL](/dotnet/framework/data/adonet/sql/writing-secure-dynamic-sql-in-sql-server)  
  
-   [如何：在 ASP.NET 内避免 SQL 注入](/previous-versions/msp-n-p/ff648339(v=pandp.10))  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎权限入门](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)   
 [保护 SQL Server](../../relational-databases/security/securing-sql-server.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [SQL Server 证书和非对称密钥](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)   
 [SQL Server 加密](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [外围应用配置器](../../relational-databases/security/surface-area-configuration.md)   
 [强密码](../../relational-databases/security/strong-passwords.md)   
 [TRUSTWORTHY 数据库属性](../../relational-databases/security/trustworthy-database-property.md)   
 [数据库引擎功能和任务](../../sql-server/what-s-new-in-sql-server-ver15.md)  
 [保护 SQL Server 知识产权](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
  
[!INCLUDE[get-help-security](../../includes/get-help-security.md)]