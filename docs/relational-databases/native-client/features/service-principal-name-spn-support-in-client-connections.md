---
title: 连接中的服务主体名称
description: 了解支持 SQL Server Native Client 中相互身份验证的服务主体名称。
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client, SPNs
- ODBC, SPNs
- OLE DB, SPNs
- SPNs [SQL Server]
ms.assetid: 96598c69-ce9a-4090-aacb-d546591e8af7
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 492ec9b3aad422ffe2c075bdeab6db932b14ff21
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462058"
---
# <a name="service-principal-name-spn-support-in-client-connections-in-sql-server-native-client"></a>SQL Server Native Client 的客户端连接中的服务主体名称 (SPN) 支持
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  从 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 开始，扩展了对服务主体名称 (SPN) 的支持，从而能够在所有协议中相互进行身份验证。 在先前版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中，如果已使用 Active Directory 注册 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的默认 SPN，则仅对使用 TCP 的 Kerberos 支持 SPN。  
  
 身份验证协议可使用 SPN 来确定运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例时使用的帐户。 如果实例帐户已知，则 Kerberos 身份验证可用于通过客户端和服务器提供相互身份验证。 如果实例帐户未知，则使用仅通过服务器提供客户端的身份验证的 NTLM 身份验证。 目前， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 执行身份验证查找，并从实例名和网络连接属性派生 SPN。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例将尝试在启动时注册 SPN，或者可手动对其进行注册。 但是，如果尝试注册 SPN 的帐户的访问权限不足，则注册将失败。  
  
 域和计算机帐户在 Active Directory 中自动注册。 这些帐户可以用作 SPN，管理员也可以定义自己的 SPN。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可通过允许客户端直接指定要使用的 SPN，从而使安全身份验证更易于管理且更可靠。  
  
> [!NOTE]  
>  只有在使用 Windows 集成安全性进行连接时，才使用客户端应用程序指定的 SPN。  
  
> [!TIP]  
>  **[!INCLUDE[msCoName](../../../includes/msconame-md.md)] Kerberos Configuration Manager for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** 是一款诊断工具，可帮助解决与 Kerberos Configuration Manager for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]相关的连接问题。 有关详细信息，请参阅 [Microsoft Kerberos Configuration Manager for SQL Server](https://www.microsoft.com/download/details.aspx?id=39046)。  
  
 有关 Kerberos 的详细信息，请参阅下列文章：  
  
-   [Kerberos Technical Supplement for Windows（针对 Windows 的 Kerberos 技术补充信息）](/previous-versions/msp-n-p/ff649429(v=pandp.10))  
  
-   [Microsoft Kerberos](/windows/win32/secauthn/microsoft-kerberos)  
  
## <a name="usage"></a>使用情况  
 下表介绍了客户端应用程序可启用安全身份验证的最常见应用场景。  
  
|方案|说明|  
|--------------|-----------------|  
|早期应用程序不指定 SPN。|该兼容应用场景可确保不会对针对先前版本 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]开发的应用程序的行为进行任何更改。 如果未指定 SPN，则应用程序使用已生成的 SPN，但不能识别使用哪个身份验证方法。|  
|使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 当前版本的客户端应用程序将连接字符串中的 SPN 指定为域用户或计算机帐户、特定于实例的 SPN 或用户定义的字符串。|在访问接口、初始化或连接字符串中可使用 **ServerSPN** 关键字进行以下操作：<br /><br /> -指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例用于连接的帐户。 这可简化对 Kerberos 身份验证的访问。 如果 Kerberos 密钥发行中心 (KDC) 存在且指定了正确的帐户，则使用 Kerberos 身份验证的可能性大于 NTLM。 KDC 通常与域控制器在同一台计算机上。<br /><br /> -指定 SPN 可查找 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的服务帐户。 对于每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，会生成两个可用于此目的的默认 SPN。 但是，不能保证 Active Directory 中存在这些密钥，因此这种情况下无法保证 Kerberos 身份验证。<br /><br /> -指定将用于查找 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的服务帐户的 SPN。 此 SPN 可以是任何映射到服务帐户的用户定义字符串。 这种情况下，必须手动在 KDC 中注册密钥，且密钥必须满足用户定义的 SPN 的规则。<br /><br /> **FailoverPartnerSPN** 关键字可用于为故障转移伙伴服务器指定 SPN。 帐户和 Active Directory 键值的范围与您可为主体服务器指定的值相同。|  
|ODBC 应用程序将 SPN 指定为主体服务器或故障转移伙伴服务器的连接属性。|连接属性 **SQL_COPT_SS_SERVER_SPN** 可用于为与主体服务器的连接指定 SPN。<br /><br /> 连接属性 **SQL_COPT_SS_FAILOVER_PARTNER_SPN** 可用于为故障转移伙伴服务器指定 SPN。|  
|OLE DB 应用程序将 SPN 指定为主体服务器或故障转移伙伴服务器的数据源初始化属性。|**SSPROP_INIT_SERVER_SPN** 属性集中的连接属性 **DBPROPSET_SQLSERVERDBINIT** 可用于为连接指定 SPN。<br /><br /> **SSPROP_INIT_FAILOVER_PARTNER_SPN** 中的连接属性 **DBPROPSET_SQLSERVERDBINIT** 可用于为故障转移伙伴服务器指定 SPN。|  
|用户在 ODBC 数据源名称 (DSN) 中为服务器或故障转移伙伴服务器指定 SPN。|可在 ODBC DSN 中通过 DSN 设置对话框指定 SPN。|  
|用户在 OLE DB 的 **“数据链接”** 或 **“登录”** 对话框中为服务器或故障转移伙伴服务器指定 SPN。|可在 **“数据链接”** 或 **“登录”** 对话框中指定 SPN。 **“登录”** 对话框可用于 ODBC 或 OLE DB。|  
|ODBC 应用程序确定用于建立连接的身份验证方法。|连接成功打开后，应用程序可查询连接属性 **SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD** ，从而确定使用了哪个身份验证方法。 值将包括但不限于 **NTLM** 和 **Kerberos**。|  
|OLE DB 应用程序确定用于建立连接的身份验证方法。|连接成功打开后，应用程序可查询 **SSPROP_AUTHENTICATION_METHOD** 属性集中的连接属性 **DBPROPSET_SQLSERVERDATASOURCEINFO** ，从而确定使用了哪个身份验证方法。 值将包括但不限于 **NTLM** 和 **Kerberos**。|  
  
## <a name="failover"></a>故障转移  
 SPN 未存储在故障转移缓存中，因此不能在连接之间传递 SPN。 在连接字符串或连接属性中指定时，SPN 将用于对主体和伙伴的所有连接尝试。  
  
## <a name="connection-pooling"></a>连接池  
 应用程序应注意在部分而不是所有连接字符串中指定 SPN 可能会导致池碎片。  
  
 应用程序可用编程方式指定 SPN 作为连接属性，而不是指定连接字符串关键字。 这有助于管理连接池碎片。  
  
 应用程序应注意可通过设置相应的连接属性覆盖连接字符串中的 SPN，但连接池使用的连接字符串将连接字符串值用于池。  
  
## <a name="down-level-server-behavior"></a>下级服务器行为  
 新的连接行为由客户端实现，因此这种行为不特定于某个版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。  
  
## <a name="linked-servers-and-delegation"></a>链接服务器和委托  
 创建链接服务器时，[sp_addlinkedserver](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) 的 \@provstr 参数可用于指定服务器和故障转移合作伙伴 SPN。 这样做的好处与在客户端连接字符串中指定 SPN 相同：建立使用 Kerberos 身份验证的连接更简单、更可靠。  
  
 使用链接服务器的委托要求 Kerberos 身份验证。  
  
## <a name="management-aspects-of-spns-specified-by-applications"></a>应用程序指定的 SPN 的管理方面  
 选择在应用程序中通过连接字符串还是以编程方式通过连接属性指定 SPN（而不是依赖于默认访问接口生成的 SPN）时，请考虑以下因素：  
  
-   安全性：指定的 SPN 是否会泄露受保护的信息？  
  
-   可靠性：若要能够使用默认 SPN，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例运行时使用的服务帐户必须具有足够的特权才能对 KDC 更新 Active Directory。  
  
-   方便性和位置透明性：如果应用程序的数据库移到其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，则将如何影响该应用程序的 SPN？ 如果使用数据库镜像，则这种情况适用于主体服务器及其故障转移伙伴。 如果服务器更改意味着必须更改 SPN，则这种情况将如何影响应用程序？ 是否将管理所有更改？  
  
## <a name="specifying-the-spn"></a>指定 SPN  
 您可以使用对话框和代码指定 SPN。 本节论述了可以指定 SPN 的方式。  
  
 SPN 的最大长度是 260 个字符。  
  
 以下是 SPN 在连接字符串或连接属性中使用的语法：  
  
|语法|说明|  
|------------|-----------------|  
|MSSQLSvc/*fqdn*|使用除 TCP 之外的协议时访问接口生成的用于默认实例的默认 SPN。<br /><br /> *fqdn* 为完全限定的域名。|  
|MSSQLSvc/*fqdn*:*port*|使用 TCP 时访问接口生成的默认 SPN。<br /><br /> *port* 是 TCP 端口号。|  
|MSSQLSvc/*fqdn*:*InstanceName*|使用除 TCP 之外的协议时访问接口生成的用于命名实例的默认 SPN。<br /><br /> InstanceName 为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例名。|  
|HOST/*fqdn*<br /><br /> HOST/*MachineName*|映射到内置计算机帐户的 SPN，这些内置计算机帐户由 Windows 自动注册。|  
|*Username*@*Domain*|域帐户的直接规范。<br /><br /> *Username* 为 Windows 用户帐户名。<br /><br /> *Domain* 为 Windows 域名或完全限定的域名。|  
|*MachineName*$@*Domain*|计算机帐户的直接规范。<br /><br /> （如果要连接到的服务器正在 LOCAL SYSTEM 或 NETWORK SERVICE 帐户下运行，则若要获取 Kerberos 身份验证， **ServerSPN** 可以使用 *MachineName*$@*Domain* 格式）。|  
|*KDCKey*/*MachineName*|用户指定的 SPN。<br /><br /> *KDCKey* 为符合 KDC 密钥的规则的字母数字字符串。|  
  
## <a name="odbc-and-ole-db-syntax-supporting-spns"></a>支持 SPN 的 ODBC 和 OLE DB 语法  
 有关特定于语法的信息，请参阅以下主题：  
  
-   [客户端连接中的服务主体名称 (SPN) (ODBC)](../../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md)  
  
-   [客户端连接中的服务主体名称 (SPN) (OLE DB)](../../../relational-databases/native-client/ole-db/service-principal-names-spns-in-client-connections-ole-db.md)  
  
 有关演示此功能的示例应用程序的信息，请参阅 [SQL Server 数据编程示例](https://msftdpprodsamples.codeplex.com/)。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
[为 Kerberos 连接注册服务主体名称](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)  
