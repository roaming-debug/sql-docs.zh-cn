---
description: 了解如何使用 sendTimeAsDatetime 连接选项配置 java.sql.Time 值发送到服务器的方式。
title: 配置如何将 java.sql.Time 值发送到服务器
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 07eb00dd-621a-46f9-a5a5-8cab4d6058b5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3982d18398a77a28113292f3a8c8300a94a5de47
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793064"
---
# <a name="configuring-how-javasqltime-values-are-sent-to-the-server"></a>配置如何将 java.sql.Time 值发送到服务器

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

如果使用 java.sql.Time 对象或 java.sql.Types.TIME JDBC 类型来设置参数，可以配置如何将 java.sql.Time 值发送到服务器，即是作为  time[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  类型发送，还是作为 datetime 类型发送。

使用以下方法之一时适用此方案：

- [SQLServerCallableStatement.registerOutParameter(int, int)](reference/registeroutparameter-method-int-int.md)
- [SQLServerCallableStatement.registerOutParameter(int, int, int)](reference/registeroutparameter-method-int-int-int.md)
- [SQLServerCallableStatement.setTime](reference/settime-method-sqlservercallablestatement.md)
- [SQLServerPreparedStatement.setTime](reference/settime-method-sqlserverpreparedstatement.md)
- [SQLServerCallableStatement.setObject](reference/setobject-method-sqlservercallablestatement.md)
- [SQLServerPreparedStatement.setObject](reference/setobject-method-sqlserverpreparedstatement.md)

可以通过使用 sendTimeAsDatetime 连接属性配置如何发送 java.sql.Time 值。 有关详细信息，请参阅[设置连接属性](setting-the-connection-properties.md)。

可以通过编程使用 [SQLServerDataSource.setSendTimeAsDatetime](reference/setsendtimeasdatetime-method-sqlserverdatasource.md) 修改 sendTimeAsDatetime 连接属性的值。

低于 [!INCLUDE[ssKatmai](../../includes/sskatmai_md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本不支持 time 数据类型，因此使用 java.sql.Time 的应用程序通常会将 java.sql.Time 值作为 datetime 或 smalldatetime [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型进行存储。

若要在处理 java.sql.Time 值时使用 datetime 和 smalldatetime[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型，应将 sendTimeAsDatetime 连接属性设置为 true。 若要在处理 java.sql.Time 值时使用 time [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型，应将 sendTimeAsDatetime 连接属性设置为 false。

在将 java.sql.Time 值发送给其数据类型还存储日期的参数时，默认日期会有所不同，具体取决于 java.sql.Time 值是作为 datetime (1/1/1970) 还是 time (1/1/1900) 值发送 。 有关将数据发送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时的数据转换的详细信息，请参阅[使用日期和时间数据](/previous-versions/sql/sql-server-2008-r2/ms180878(v=sql.105))。

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] JDBC 驱动程序 3.0 中，sendTimeAsDatetime 默认设为 true。 在将来的发行版中，默认情况下可以将 sendTimeAsDatetime 连接属性设置为 false。

为了确保无论 sendTimeAsDatetime 连接属性的默认值为什么，应用程序都能正常工作，可以：

- 在使用 time[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型时使用 java.sql.Time。
- 处理 datetime、smalldatetime 和 datetime2[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型时，请使用 java.sql.Timestamp。

SendTimeAsDatetime 必须对加密列设为 false，因为加密列不支持从 time 转换为 datetime。 自 Microsoft JDBC Driver 6.0 for SQL Server 起，SQLServerConnection 类包含以下两种方法，可用于设置/获取 sendTimeAsDatetime 属性的值。

```java
  public boolean getSendTimeAsDatetime()
  public void setSendTimeAsDatetime(boolean sendTimeAsDateTimeValue)
```

## <a name="see-also"></a>另请参阅

[了解 JDBC 驱动程序数据类型](understanding-the-jdbc-driver-data-types.md)
