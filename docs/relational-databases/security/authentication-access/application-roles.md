---
title: 应用程序角色 | Microsoft Docs
description: 使用应用程序角色，在 SQL Server 中只允许通过特定应用程序进行连接的用户访问数据。
ms.custom: ''
ms.date: 08/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- application roles [SQL Server], about application roles
- principals [SQL Server], application roles
- credentials [SQL Server], roles
- application roles [SQL Server]
- roles [SQL Server], application
- permissions [SQL Server], roles
- users [SQL Server], application roles
- authentication [SQL Server], roles
- groups [SQL Server], roles
ms.assetid: dca18b8a-ca03-4b7f-9a46-8474d5b66f76
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 340ea589ac9032b1aa85de02b0056f82805a147e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468568"
---
# <a name="application-roles"></a>应用程序角色
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  应用程序角色是一个数据库主体，它使应用程序能够用其自身的、类似用户的权限来运行。 使用应用程序角色，可以只允许通过特定应用程序连接的用户访问特定数据。 与数据库角色不同的是，应用程序角色默认情况下不包含任何成员，而且是非活动的。 可以使用 **sp_setapprole** 启用应用程序角色，该过程需要密码。 因为应用程序角色是数据库级主体，所以它们只能通过其他数据库中为 **guest** 授予的权限来访问这些数据库。 因此，其他数据库中的应用程序角色将无法访问任何已禁用 **guest** 的数据库。  
  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中，应用程序角色无法访问服务器级元数据，因为它们不与服务器级主体关联。 若要禁用此限制，从而允许应用程序角色访问服务器级元数据，请设置全局标志 4616。 有关详细信息，请参阅[跟踪标志 (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 和 [DBCC TRACEON (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)。  
  
## <a name="connecting-with-an-application-role"></a>连接应用程序角色  
 应用程序角色切换安全上下文的过程包括下列步骤：  
  
1.  用户执行客户端应用程序。  
  
2.  客户端应用程序作为用户连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
3.  然后应用程序用一个只有它才知道的密码执行 **sp_setapprole** 存储过程。  
  
4.  如果应用程序角色名称和密码都有效，则启用应用程序角色。  
  
5.  此时，连接将失去用户权限，而获得应用程序角色权限。  

 通过应用程序角色获得的权限在连接期间始终有效。  
  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的早期版本中，用户若要在启动应用程序角色后重新获取其原始安全上下文，唯一的方法就是断开 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]连接，然后再重新连接。 从 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]开始， **sp_setapprole** 有了一个可创建 Cookie 的选项。 Cookie 包含启用应用程序角色之前的上下文信息。 **sp_unsetapprole** 可以使用此 Cookie 将会话还原到其原始上下文。 有关此新选项和示例的信息，请参阅 [sp_setapprole (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)授予的权限来访问这些数据库。  
  
> [!IMPORTANT]  
>  **SqlClient** 不支持 ODBC **encrypt** 选项。 通过网络传输机密信息时，请使用传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”）或 IPSec 对通道进行加密。 如果必须使凭据在客户端应用程序中持久化，请使用加密 API 函数来加密凭据。 在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 及更高版本中，参数 *password* 将作为单向哈希进行存储。  
  
## <a name="related-tasks"></a>Related Tasks  
  
| 任务 | 类型 |
| ---- | ---- |
|创建应用程序角色。|[创建应用程序角色](../../../relational-databases/security/authentication-access/create-an-application-role.md)和 [CREATE APPLICATION ROLE (Transact-SQL)](../../../t-sql/statements/create-application-role-transact-sql.md)|  
|更改应用程序角色。|[ALTER APPLICATION ROLE (Transact-SQL)](../../../t-sql/statements/alter-application-role-transact-sql.md)|  
|删除应用程序角色。|[DROP APPLICATION ROLE (Transact-SQL)](../../../t-sql/statements/drop-application-role-transact-sql.md)|  
|使用应用程序角色。|[sp_setapprole (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)|  
  
## <a name="see-also"></a>另请参阅  
 [保护 SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
  
