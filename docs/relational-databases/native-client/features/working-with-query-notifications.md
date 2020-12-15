---
title: 使用查询通知 |Microsoft Docs
description: 查询通知允许您在超时期内请求通知，SQL Server Native Client 中的基础数据发生更改时。
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- data access [SQL Server Native Client], query notifications
- rowsets [SQL Server], notifications
- SQL Server Native Client, query notifications
- notifications [SQL Server Native Client]
- query notifications [SQL Server], SQL Server Native Client
- canceling rowset changes
- IRowsetNotify interface
- SQLNCLI, query notifications
- SQL Server Native Client OLE DB provider, query notifications
- consumer notification for rowset changes [SQL Server Native Client]
ms.assetid: 2f906fff-5ed9-4527-9fd3-9c0d27c3dff7
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6182c18b9f85b2b11a5813a11131b7a5efb3bbbd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463358"
---
# <a name="working-with-query-notifications"></a>使用查询通知
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 中引入了查询通知。 查询通知建立在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中引入的 Service Broker 基础结构之上，并允许在数据发生更改时向应用程序发送通知。 对提供数据库信息的缓存且需要在源数据发生更改时收到通知的应用程序（如 Web 应用程序）而言，以上功能特别有用。  
  
 查询通知使您可以在查询的基础数据发生更改时请求在指定的超时期限内发送通知。 通知请求指定通知选项，其中包括服务名称、消息正文和服务器的超时值。 通知是通过 Service Broker 队列传递的，应用程序可以轮询该队列以获取可用通知。  
  
 查询通知选项字符串的语法为：  
  
 `service=<service-name>[;(local database=<database> | broker instance=<broker instance>)]`  
  
 例如：  
  
 `service=mySSBService;local database=mydb`  
  
 由于应用程序可能在创建通知订阅后终止，因此，通知订阅的生存期比启动它们的进程的生存期长。 订阅将保持有效，如果数据发生更改，则将在创建该订阅时指定的超时期限内发送通知。 通知是通过执行的查询、通知选项和消息正文标识的，将通知的超时值设置为零可以取消该通知。  
  
 只发送一次通知。 对于数据更改的持续通知，必须在处理每个通知之后重新执行查询来创建新的订阅。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 本机客户端应用程序通常使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] [receive](../../../t-sql/statements/receive-transact-sql.md) 命令接收通知，以从与通知选项中指定的服务关联的队列中读取通知。  
  
> [!NOTE]  
>  对于需要通知的查询，必须限定其中的表名，例如 `dbo.myTable`。 表名必须用包含两个部分的名称进行限定。 如果使用的名称由三个或四个部分组成，则订阅无效。  
  
 通知基础结构构建在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中引入的队列功能之上。 通常，在服务器上生成的通知是通过这些队列发送的，以便在稍后进行处理。  
  
 若要使用查询通知，服务器上必须存在队列和服务。 使用如下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 可以创建队列和服务：  
  
```  
CREATE QUEUE myQueue  
CREATE SERVICE myService ON QUEUE myQueue   
  
([https://schemas.microsoft.com/SQL/Notifications/PostQueryNotification])  
```  
  
> [!NOTE]  
>  服务必须使用预定义约定 `https://schemas.microsoft.com/SQL/Notifications/PostQueryNotification`，如上所示。  
  
## <a name="sql-server-native-client-ole-db-provider"></a>SQL Server Native Client OLE DB 访问接口  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序支持按行集修改的使用者通知。 使用者在行集修改的每个阶段以及在尝试执行任何更改时接收通知。  
  
> [!NOTE]  
>  使用 **ICommand：： Execute** 将通知查询传递给服务器，这是使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序订阅查询通知的唯一有效方式。  
  
### <a name="the-dbpropset_sqlserverrowset-property-set"></a>DBPROPSET_SQLSERVERROWSET 属性集  
 为了通过 OLE DB 支持查询通知， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将以下新属性添加到 DBPROPSET_SQLSERVERROWSET 属性集。  
  
|名称|类型|说明|  
|----------|----------|-----------------|  
|SSPROP_QP_NOTIFICATION_TIMEOUT|VT_UI4|查询通知保持为活动状态的秒数。<br /><br /> 默认为 432000 秒（5 天）。 最小值为 1 秒，最大值为 2^31-1 秒。|  
|SSPROP_QP_NOTIFICATION_MSGTEXT|VT_BSTR|通知的消息正文。 消息正文是用户定义的，没有预定义格式。<br /><br /> 默认情况下，该字符串为空。 您可以使用 1 至 2000 个字符指定消息。|  
|SSPROP_QP_NOTIFICATION_OPTIONS|VT_BSTR|查询通知选项。 采用 name=value 语法以字符串形式指定这些内容。 用户负责创建服务并从队列中读取通知。<br /><br /> 默认值为空字符串。|  
  
 无论语句是在用户事务中运行还是以自动提交模式运行，或者无论是提交还是回滚运行语句的事务，将始终提交通知订阅。 根据以下任一无效通知条件激发服务器通知：更改基础数据或架构，或者当达到超时期限时（以先发生者为准）。 激发通知后将立即删除通知注册。 因此，在接收通知时，应用程序必须再次订阅通知，以备进一步更新之用。  
  
 其他连接或线程可以检查通知的目标队列。 例如：  
  
```  
WAITFOR (RECEIVE * FROM MyQueue);   // Where MyQueue is the queue name.   
```  
  
 请注意，SELECT * 不会删除 Queue 中的项，但 RECEIVE \* FROM 将删除相应项。 如果队列为空，该语句将停止服务器线程。 如果调用时存在队列项，则会立即返回这些队列项；否则调用将一直等待，直到生成队列项为止。  
  
```  
RECEIVE * FROM MyQueue  
```  
  
 如果队列为空，该语句将立即返回空的结果集；否则会返回所有队列通知。  
  
 如果 SSPROP_QP_NOTIFICATION_MSGTEXT 和 SSPROP_QP_NOTIFICATION_OPTIONS 为非 NULL 和非空，当执行其中的每个命令时，将向服务器发送包含上面定义的三个属性的查询通知 TDS 头。 如果其中任一属性为 Null（或空），则不会发送头，并引发 DB_E_ERRORSOCCURRED（或者如果两个属性均标记为可选，则引发 DB_S_ERRORSOCCURRED），同时将状态值设置为 DBPROPSTATUS_BADVALUE。 在执行/准备期间进行验证。 类似地，当设置查询通知属性以便连接到 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 之前的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本时，将引发 DB_S_ERRORSOCCURED。 在这种情况下，状态值为 DBPROPSTATUS_NOTSUPPORTED。  
  
 启动订阅不能保证将成功传递后续消息。 此外，不会检查指定服务名称的有效性。  
  
> [!NOTE]  
>  准备语句永远不会启动订阅；只有执行语句才能实现此目的，并且 OLE DB 核心服务的使用不会影响查询通知。  
  
 有关 DBPROPSET_SQLSERVERROWSET 属性集的详细信息，请参阅 [行集属性和行为](../../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)。  
  
## <a name="sql-server-native-client-odbc-driver"></a>SQL Server Native Client ODBC 驱动程序  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序通过向[SQLGetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md)和[SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)函数添加三个新属性，支持查询通知：  
  
-   SQL_SOPT_SS_QUERYNOTIFICATION_MSGTEXT  
  
-   SQL_SOPT_SS_QUERYNOTIFICATION_OPTIONS  
  
-   SQL_SOPT_SS_QUERYNOTIFICATION_TIMEOUT  
  
 如果 SQL_SOPT_SS_QUERYNOTIFICATION_MSGTEXT 和 SQL_SOPT_SS_QUERYNOTIFICATION_OPTIONS 不为 NULL，每次执行命令时，则不会向服务器发送包含上面定义的三个属性的查询通知 TDS 头。 如果上述两个属性中的任一属性为 Null，则不会发送标头，并返回 SQL_SUCCESS_WITH_INFO。 验证在 [SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md)、 **SqlExecDirect** 和 **SqlExecute** 上进行，如果特性无效，则所有这些操作都将失败。 类似地，当针对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之前的 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 版本设置这些查询通知属性时，执行将失败，并返回 SQL_SUCCESS_WITH_INFO。  
  
> [!NOTE]  
>  准备语句永远不会启动订阅；可以通过执行语句启动订阅。  
  
## <a name="special-cases-and-restrictions"></a>特殊事例和限制  
 通知不支持以下数据类型：  
  
-   **text**  
  
-   **ntext**  
  
-   **图像**  
  
 如果针对返回以上任一类型的查询执行通知请求，则立即激发通知，并指明不可能实现通知订阅。  
  
 如果发出批处理或存储过程订阅请求，则会针对批处理或存储过程内执行的每个语句发出单独的订阅请求。 EXECUTE 语句不会注册通知，但是会将通知请求发送到已执行的命令。 如果为批处理，则上下文将适用于已执行的语句，并且应用上述相同规则。  
  
 提交通知的查询，该查询由同一用户在同一数据库上下文中提交，并且具有相同的模板、相同的参数值、相同的通知 ID 以及与现有活动订阅相同的送达位置，将续订现有订阅，并重置新指定的超时值。这意味着，如果针对相同的查询请求通知，则仅发送一条通知。 这将适用于批处理中的重复查询，也适用于存储过程中调用多次的查询。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
