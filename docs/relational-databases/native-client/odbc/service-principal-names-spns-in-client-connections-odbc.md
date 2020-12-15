---
title: 'ODBC 客户端中的服务主体名称 (Spn) '
description: 了解支持 (Spn) 客户端应用程序中的服务主体名称的 ODBC 属性和函数。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 1d60cb30-4c46-49b2-89ab-701e77a330a2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a45b7cc233ac9a3f29859471a70eec24fc9756f7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475948"
---
# <a name="service-principal-names-spns-in-client-connections-odbc"></a>客户端连接中的服务主体名称 (SPN) (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题介绍支持客户端应用程序中的服务主体名称 (SPN) 的 ODBC 属性和函数。 有关客户端应用程序中的 Spn 的详细信息，请参阅 [客户端连接中的服务主体名称 &#40;spn&#41; 支持](../../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md) 和 [获取相互 Kerberos 身份验证](../../../relational-databases/native-client-odbc-how-to/get-mutual-kerberos-authentication.md)。  
  
## <a name="connection-string-keywords"></a>连接字符串关键字  
 客户端应用程序使用以下连接字符串关键字可指定 SPN。  
  
|关键字|值|  
|-------------|-----------|  
|**ServerSPN**|服务器的 SPN。 默认值是空字符串，这将导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用默认的驱动程序生成的 SPN。|  
|**FailoverPartnerSPN**|故障转移伙伴的 SPN。 默认值是空字符串，这将导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用默认的驱动程序生成的 SPN。|  
  
## <a name="connection-attributes"></a>连接属性  
 客户端应用程序使用以下连接属性可指定 SPN 和查询身份验证方法。  
  
|名称|类型|使用情况|  
|----------|----------|-----------|  
|SQL_COPT_SS_SERVER_SPN<br /><br /> SQL_COPT_SS_FAILOVER_PARTNER_SPN|SQLTCHAR，读/写|指定服务器的 SPN。 默认值是空字符串，这将导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用默认的驱动程序生成的 SPN。<br /><br /> 只有在以编程方式设置该属性或打开连接之后才能查询该属性。 如果试图对未打开的连接查询该属性，并且尚未以编程方式设置该属性，则返回 SQL_ERROR 并生成具有 SQLState 08003 和消息“连接未打开”的诊断记录。<br /><br /> 如果在连接打开时试图设置该属性，则返回 SQL_ERROR 并生成具有 SQLState HY011 和消息“操作此时无效”的诊断记录。|  
|SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD|SQLTCHAR，只读|返回用于连接的身份验证方法。 返回到应用程序的值是 Windows 返回到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 的值。 可能的值包括：<br /><br /> “NTLM”，使用 NTLM 身份验证打开连接时将返回该值。<br /><br /> “Kerberos”，使用 Kerberos 身份验证打开连接时将返回该值。<br /><br /> <br /><br /> 只能为使用 Windows 身份验证的打开的连接读取该属性。 如果试图在连接打开之前读取它，则返回 SQL_ERROR 并以 SQLState 08003 和消息“连接未打开”记录错误。<br /><br /> 如果对未使用 Windows 身份验证的连接查询该属性，则返回 SQL_ERROR 并以 SQLState HY092 和消息“属性/选项标识符无效(SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD 只适用于可信连接)”记录错误。<br /><br /> 如果无法确定身份验证方法，则返回 SQL_ERROR 并以 SQLState HY000 和消息“常规错误”记录错误。|  
|SQL_COPT_SS_MUTUALLY_AUTHENTICATED|SQLSMALLINT，只读|如果连接中的服务器相互验证，则返回 SQL_TRUE；否则，返回 SQL_FALSE。<br /><br /> 只能为打开的连接读取该属性。 如果试图在连接打开之前读取它，则返回 SQL_ERROR 并以 SQLState 08003 和消息“连接未打开”记录错误。<br /><br /> 如果为未使用 Windows 身份验证的连接查询该属性，则返回 SQL_FALSE。|  
  
## <a name="odbc-function-support-for-specifying-spns"></a>支持指定 SPN 的 ODBC 函数  
 以下 ODBC 函数支持客户端应用程序和 SPN：  
  
-   [SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)  
  
-   [SQLConnect](../../../relational-databases/native-client-odbc-api/sqlconnect.md)  
  
-   [SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)  
  
-   [SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md)  
  
-   [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
  
