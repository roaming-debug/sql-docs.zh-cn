---
title: 使用查询通知 | Microsoft Docs
description: 了解查询通知如何让应用程序在 OLE DB Driver for SQL Server 中的数据发生更改时收到通知。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], query notifications
- rowsets [SQL Server], notifications
- OLE DB Driver for SQL Server, query notifications
- notifications [OLE DB Driver for SQL Server]
- query notifications [SQL Server], OLE DB Driver for SQL Server
- canceling rowset changes
- IRowsetNotify interface
- MSOLEDBSQL, query notifications
- OLE DB Driver for SQL Server, query notifications
- consumer notification for rowset changes [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3b21f3a8b17c5174c4e9f8b037aa891bdfcc6d10
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751067"
---
# <a name="working-with-query-notifications"></a>使用查询通知

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

查询通知是在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和 OLE DB Driver for SQL Server 中引入的。 查询通知建立在 SQL Server 2005 (9.x) 中引入的 SQL Service Broker 基础结构之上，并允许在数据发生更改时向应用程序发送通知。 对提供数据库信息的缓存且需要在源数据发生更改时收到通知的应用程序（如 Web 应用程序）而言，以上功能特别有用。

使用查询通知，你可以在查询的基础数据发生更改时请求在指定的超时期限内发送通知。 请求指定通知选项，其中包括服务名称、消息正文和服务器的超时值。 通知是通过 Service Broker 队列传递的，应用程序可轮询该队列来获取可用通知。

查询通知选项字符串的语法为：

`service=<service-name>[;(local database=<database> | broker instance=<broker instance>)]`

 例如：

`service=mySSBService;local database=mydb`

通知订阅的生存期长于启动它们的进程。 这是因为应用程序可以创建通知订阅，然后终止。 订阅保持有效，如果数据发生更改，则在创建该订阅时指定的超时期限内发送通知。 通知是通过执行的查询、通知选项和消息文本标识的。 可以通过将其超时值设置为零来取消通知。

只发送一次通知。 若要持续收到数据更改的通知，请在处理每个通知之后重新执行查询来创建新的订阅。

OLE DB Driver for SQL Server 应用程序通常使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] [RECEIVE](../../../t-sql/statements/receive-transact-sql.md) 命令接收通知。 它使用此命令从与通知选项中指定的服务相关联的队列中读取通知。

> [!NOTE]
> 对于需要通知的查询，必须限定其中的表名。 例如，`dbo.myTable` 。 表名必须用包含两个部分的名称进行限定。 如果使用的名称由三个或四个部分组成，则订阅无效。

通知基础结构构建在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中引入的队列功能之上。 通常，服务器上生成的通知是通过这些队列发送的，以便稍后进行处理。

若要使用查询通知，服务器上必须存在队列和服务。 可以使用如下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 创建队列和服务：

```sql
CREATE QUEUE myQueue
CREATE SERVICE myService ON QUEUE myQueue
([https://schemas.microsoft.com/SQL/Notifications/PostQueryNotification])
```

> [!NOTE]
> 服务必须使用预定义约定，如上所示。

## <a name="ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序

OLE DB Driver for SQL Server 支持行集修改时的使用者通知。 使用者在行集修改的每个阶段以及在尝试执行任何更改时接收通知。

> [!NOTE]
> 使用 ICommand::Execute 向服务器传递通知查询是向适用于 SQL Server 的 OLE DB 驱动程序订阅查询通知的唯一有效方式  。

### <a name="dbpropset_sqlserverrowset-property-set"></a>DBPROPSET_SQLSERVERROWSET 属性集

为通过 OLE DB 支持查询通知，OLE DB Driver for SQL Server 向 `DBPROPSET_SQLSERVERROWSET` 属性集添加了以下新属性。

|名称|类型|说明|
|----------|----------|-----------------|
|SSPROP_QP_NOTIFICATION_TIMEOUT|VT_UI4|查询通知保持为活动状态的秒数。<br /><br /> 默认为 432000 秒（5 天）。 最小值为 1 秒，最大值为 2^31-1 秒。|
|SSPROP_QP_NOTIFICATION_MSGTEXT|VT_BSTR|通知的消息正文。 此消息是用户定义的，没有预定义格式。<br /><br /> 默认情况下，该字符串为空。 使用 1 到 2000 个字符指定一条消息。|
|SSPROP_QP_NOTIFICATION_OPTIONS|VT_BSTR|查询通知选项。 采用 name=value 语法以字符串形式指定这些内容。 用户负责创建服务并从队列中读取通知。<br /><br /> 默认值为空字符串。|

始终提交通知订阅。 无论语句是在用户事务中运行还是以自动提交模式运行，或者无论是提交还是回滚运行语句的事务，都将发生这种情况。 根据以下任一无效通知条件激发服务器通知：更改基础数据或架构，或者当达到超时期限时（以先发生者为准）。 

激发通知后将立即删除通知注册。 因此，在接收通知时，如果你想要获取进一步的更新，应用程序必须再次订阅通知。

其他连接或线程可以检查通知的目标队列。 例如：

```sql
WAITFOR (RECEIVE * FROM MyQueue); -- Where MyQueue is the queue name.
```

> [!NOTE]
> `SELECT *` 不会从队列中删除该项。 但 `RECEIVE * FROM` 会。 如果队列为空，该语句将停止服务器线程。 如果在调用时有队列项，它们将立即返回。 否则，调用将等待，直到创建队列项。

```sql
RECEIVE * FROM MyQueue
```

如果队列为空，该语句将立即返回空的结果集。 否则，它将返回所有队列通知。

如果 `SSPROP_QP_NOTIFICATION_MSGTEXT` 和 `SSPROP_QP_NOTIFICATION_OPTIONS` 为非 null 且非空，则会将包含上面定义的三个属性的查询通知 TDS 标头发送到服务器。 每次执行命令时都会发生这种情况。 如果其中任何一个属性为 null（或为空），则不会发送标头并引发 `DB_E_ERRORSOCCURRED`（如果属性同时标记为可选，则会引发 `DB_S_ERRORSOCCURRED`）。 然后，将状态值设置为 `DBPROPSTATUS_BADVALUE`。 在执行和准备期间进行验证。 类似地，当设置查询通知属性以连接到 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 之前的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本时，将引发 `DB_S_ERRORSOCCURED`。 在本例中，状态值为 `DBPROPSTATUS_NOTSUPPORTED`。

启动订阅不能保证将成功传递后续消息。 此外，不检查指定服务名称的有效性。

> [!NOTE]
> 准备语句不会导致订阅初始化。 只有执行语句才会实现初始化。 OLE DB 核心服务的使用不会影响查询通知。

有关 `DBPROPSET_SQLSERVERROWSET` 属性集的详细信息，请参阅[行集属性和行为](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md)。

## <a name="see-also"></a>另请参阅

[适用于 SQL Server 的 OLE DB 驱动程序功能](../../oledb/features/oledb-driver-for-sql-server-features.md)
