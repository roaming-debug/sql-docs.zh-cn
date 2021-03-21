---
description: SQLBrowseConnect
title: SQLBrowseConnect |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLBrowseConnect function
ms.assetid: 57faf388-c7ca-4696-9845-34e0a10cc5f7
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ef7782ed2c0fb5f60d0ca51ff814f109116820d8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751367"
---
# <a name="sqlbrowseconnect"></a>SQLBrowseConnect
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLBrowseConnect** 使用可分类为三个级别的连接信息的关键字。 对于每个关键字，下表指示是否返回有效值列表以及该关键字是否可选。  
  
## <a name="level-1"></a>级别 1  
  
|关键字|是否返回列表？|是否可选？|说明|  
|-------------|--------------------|---------------|-----------------|  
|DSN|空值|否|**SQLDataSources** 返回的数据源的名称。 如果使用 DRIVER 关键字，则无法使用 DSN 关键字。|  
|DRIVER|空值|否|Microsoft® [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC driver name 为 { [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native client 11}。 如果使用 DSN 关键字，则无法使用 DRIVER 关键字。|  
  
## <a name="level-2"></a>级别 2  
  
|关键字|是否返回列表？|是否可选？|说明|  
|-------------|--------------------|---------------|-----------------|  
|SERVER|是|否|数据源所驻留网络上的服务器名称。 可以输入术语 "(local)" 作为服务器，在此情况下，即使此为非联网版本，也可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地副本。|  
|UID|否|是|用户登录 ID。|  
|PWD|否|是（取决于用户）|用户指定的密码。|  
|APP|否|是|调用 **SQLBrowseConnect** 的应用程序的名称。|  
|WSID|否|是|工作站 ID。 通常，这是运行应用程序的计算机的网络名称。|  
  
## <a name="level-3"></a>Level 3  
  
|关键字|是否返回列表？|是否可选？|说明|  
|-------------|--------------------|---------------|-----------------|  
|DATABASE|是|是|数据库的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|LANGUAGE|是|是|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用的区域语言。|  
  
 **SQLBrowseConnect** 忽略存储在 ODBC 数据源定义中的数据库和语言关键字的值。 如果传递到 **SQLBrowseConnect** 的连接字符串中指定的数据库或语言无效，则 **SQLBrowseConnect** 将返回 SQL_NEED_DATA 和3级连接属性。  
  
 以下属性（通过调用 [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)设置）确定由 **SQLBrowseConnect** 返回的结果集。  
  
|特性|说明|  
|---------------|-----------------|  
|SQL_COPT_SS_BROWSE_CONNECT|如果将其设置为 SQL_MORE_INFO_YES，则 **SQLBrowseConnect** 将返回服务器属性的扩展字符串。<br /><br /> 下面是 **SQLBrowseConnect** 返回的扩展字符串示例：<br /><br /> <br /><br /> `ServerName\InstanceName;Clustered:No;Version:8.00.131`<br /><br /> <br /><br /> 在此字符串中，分号用于分隔与服务器有关的各部分信息， 逗号用于分隔不同的服务器实例。|  
|SQL_COPT_SS_BROWSE_SERVER|如果指定了服务器名称，则 **SQLBrowseConnect** 将返回指定服务器的信息。 如果 SQL_COPT_SS_BROWSE_SERVER 设置为 NULL，则 **SQLBrowseConnect** 将返回域中所有服务器的信息。<br /><br /> <br /><br /> 请注意，由于网络问题， **SQLBrowseConnect** 可能无法及时接收来自所有服务器的响应。 因此，每个请求所返回的服务器列表都可能不同。|  
|SQL_COPT_SS_BROWSE_CACHE_DATA|如果 SQL_COPT_SS_BROWSE_CACHE_DATA 属性设置为 SQL_CACHE_DATA_YES，当缓冲区长度不足以容纳结果时，您可以提取块区中的数据。 此长度在 SQLBrowseConnect 的 BufferLength 参数中指定。<br /><br /> 当有更多的数据可用时，将返回 SQL_NEED_DATA。 如果检索不到更多的数据，将返回 SQL_SUCCESS。<br /><br /> 默认值是 SQL_CACHE_DATA_NO。|  
  
## <a name="sqlbrowseconnect-support-for-high-availability-disaster-recovery"></a>对高可用性、灾难恢复的 SQLBrowseConnect 支持  
 有关使用 **SQLBrowseConnect** 连接到群集的详细信息 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] ，请参阅 [高可用性和灾难恢复的 SQL Server Native Client 支持](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)。  
  
## <a name="sqlbrowseconnect-support-for-service-principal-names-spns"></a>对服务主体名称 (SPN) 的 SQLBrowseConnect 支持  
 当打开连接时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 将 SQL_COPT_SS_MUTUALLY_AUTHENTICATED 和 SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD 设置为用于打开此连接的身份验证方法。  
  
 有关 Spn 的详细信息，请参阅 [&#40;ODBC&#41;的客户端连接中的服务主体名称 &#40;spn&#41; ](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md)。  
  
## <a name="change-history"></a>更改历史记录  
  
|更新的内容|  
|---------------------|  
|介绍了 SQL_COPT_SS_BROWSE_CACHE_DATA。|  
  
## <a name="see-also"></a>另请参阅  
 [SQLBrowseConnect 函数](../../odbc/reference/syntax/sqlbrowseconnect-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
