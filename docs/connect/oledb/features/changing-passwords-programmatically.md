---
title: 以编程方式更改密码 | Microsoft Docs
description: OLE DB Driver for SQL Server 支持通过 OLE DB Driver 和“SQL Server 登录”对话框以编程方式处理过期密码。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], password expiration
- OLE DB Driver for SQL Server, passwords
- passwords [SQL Server], expiration
- MSOLEDBSQL, password expiration
- expiration [OLE DB Driver for SQL Server]
- passwords [SQL Server], modifying
- expired passwords [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, password expiration
- modifying passwords
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ca8bfec9c9404f2912d288cb434aa064200fea45
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742847"
---
# <a name="changing-passwords-programmatically"></a>以编程方式更改密码
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 之前，如果用户的密码过期，则只有管理员能对其进行重置。 从 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 开始，OLE DB Driver for SQL Server 支持通过 OLE DB Driver 和对“SQL Server 登录”  对话框进行更改以编程方式处理过期密码。  
  
> [!NOTE]  
>  如果可能，请在运行时提示用户输入他们的凭据，并避免用持久化格式存储他们的凭据。 如果必须保留其凭据，应使用 [Win32 加密 API](/windows/win32/seccrypto/cryptography-reference) 来加密这些凭据。 有关密码使用的详细信息，请参阅[强密码](../../../relational-databases/security/strong-passwords.md)。  
  
## <a name="sql-server-login-error-codes"></a>SQL Server 登录错误代码  
 当由于身份验证问题而导致无法连接时，以下 SQL Server 错误代码之一可供应用程序使用，以帮助诊断和恢复。  
  
|SQL Server 错误代码|错误消息|  
|---------------------------|-------------------|  
|15113|用户 '%.*ls' 登录失败。原因: 密码验证失败。 帐户已锁定。|  
|18463|用户 "%.*ls" 登录失败。 原因: 密码更改失败。 此时无法使用密码。|  
|18464|用户 "%.*ls" 登录失败。 原因: 密码更改失败。 该密码太短，不符合策略要求。|  
|18465|用户 "%.*ls" 登录失败。 原因: 密码更改失败。 密码太长，不符合策略要求。|  
|18466|用户 "%.*ls" 登录失败。 原因: 密码更改失败。 该密码不够复杂，不符合策略要求。|  
|18467|用户 "%.*ls" 登录失败。 原因: 密码更改失败。 该密码不符合密码筛选器 DLL 的要求。|  
|18468|用户 "%.*ls" 登录失败。 原因: 密码更改失败。 在密码验证过程中出错。|  
|18487|用户 "%.*ls" 登录失败。 原因: 该帐户的密码已过期。|  
|18488|用户 "%.*ls" 登录失败。 原因: 该帐户的密码必须更改。|  
  
## <a name="ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序  
 OLE DB Driver for SQL Server 支持通过用户界面以编程方式处理过期密码。  
  
### <a name="ole-db-user-interface-password-expiration"></a>OLE DB 用户界面密码过期  
 适用于 SQL Server 的 OLE DB 驱动程序支持通过更改“SQL Server 登录”对话框来处理密码过期的情况  。 如果将 DBPROP_INIT_PROMPT 的值设置为 DBPROMPT_NOPROMPT，则在密码已过期的情况下初始连接尝试将失败。  
  
 如果 DBPROP_INIT_PROMPT 已设置为任何其他值，则无论密码是否已过期，用户都会看到“SQL Server 登录”对话框  。 用户可单击“选项”按钮，再选中“更改密码”进行更改   。  
  
 如果用户单击“确定”且密码已过期，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 会提示用户使用“更改 SQL Server 密码”对话框输入并确认新密码  。  
  
#### <a name="ole-db-prompt-behavior-and-locked-accounts"></a>OLE DB 提示行为和锁定的帐户  
 连接尝试可能会由于帐户被锁定而失败。 如果在显示“SQL Server 登录”对话框后发生此情况，则向用户显示服务器错误消息，且连接尝试中止  。 如果在显示“更改 SQL Server 密码”对话框后用户输入错误的旧密码值，也会出现这种情况  。 在这种情况下，将显示相同的错误消息，并且连接尝试将中止。  
  
#### <a name="ole-db-connection-pooling-password-expiration-and-locked-accounts"></a>OLE DB 连接池、密码过期和锁定的帐户  
 当连接在连接池中仍处于活动状态时，帐户可能被锁定或其密码可能已过期。 服务器会在以下两种情况下检查密码是否已过期以及帐户是否已锁定。 第一种情况是首次创建连接时。 第二种情况是从相应池中获取相应连接以重置连接时。  
  
 如果重置尝试失败，则将从相应池中删除相应连接且返回一个错误。  
  
### <a name="ole-db-programmatic-password-expiration"></a>OLE DB 编程密码过期  
 适用于 SQL Server 的 OLE DB 驱动程序支持通过添加已添至 DBPROPSET_SQLSERVERDBINIT 属性集的 SSPROP_AUTH_OLD_PASSWORD（类型 VT_BSTR）属性来处理密码过期情况。  
  
 现有“密码”属性是指 DBPROP_AUTH_PASSWORD，用于存储新密码。  
  
> [!NOTE]  
>  在连接字符串中，“旧密码”属性会设置 SSPROP_AUTH_OLD_PASSWORD，它是当前（有可能是过期的）密码，通过提供程序字符串属性无法得到该密码。  
  
 访问接口不保留此属性的值。 如果设置此属性，则访问接口不使用连接池进行首次连接，原因是将进行新连接。 如果密码更改成功，则不能重新使用当前连接，原因是当前连接仍包含旧密码，这些密码在密码更改后失效。 此外，如果登录成功，则访问接口将清除此属性。 如果随后尝试检索旧密码，则将返回 VT_EMPTY。  
  
> [!NOTE]  
>  不得保留 SSPROP_AUTH_OLD_PASSWORD，因为只有在密码过期时才使用它。  
  
 请注意，只要设置“旧密码”属性，访问接口就会假定正在尝试更改密码，除非还指定了 Windows 身份验证，这种情况下 Windows 身份验证始终优先。  
  
 如果使用 Windows 身份验证，则指定旧密码会产生 DB_E_ERRORSOCCURRED 或 DB_S_ERRORSOCCURRED（具体取决于是将旧密码指定为 REQUIRED 还是 OPTIONAL），并且在 dwStatus 中返回 DBPROPSTATUS_CONFLICTINGBADVALUE 的状态值  。 在调用 IDBInitialize::Initialize 时进行检测  。  
  
 如果更改密码的尝试意外失败，则服务器将返回错误代码 18468。 将从连接尝试返回标准的 OLEDB 错误。  
  
 若要详细了解 DBPROPSET_SQLSERVERDBINIT 属性集，请参阅[初始化和授权属性](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md)。  

  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序功能](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
