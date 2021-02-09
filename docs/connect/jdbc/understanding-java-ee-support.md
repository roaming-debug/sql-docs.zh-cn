---
description: 了解 Java EE 支持
title: 了解 Java EE 支持 | Microsoft Docs
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a9448b80-b7a3-49cf-8bb4-322c73676005
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6cdc82e200609706981894ea22194de6baa7f51b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187599"
---
# <a name="understanding-java-ee-support"></a>了解 Java EE 支持

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

以下各部分介绍 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 如何为 Java Platform、Enterprise Edition (Java EE) 和 JDBC 3.0 可选 API 功能提供支持。 本“帮助”系统中提供的源代码示例提供了很好的参考资料，供您开始使用这些功能。  
  
首先，确保您的 Java 环境（JDK、JRE）包含 javax.sql 包。 这是使用可选 API 的任何 JDBC 应用程序所必需的包。 JDK 1.5 和更高版本已包含此包，因此不需要单独安装它。  
  
## <a name="driver-name"></a>驱动程序名称

驱动程序类名称为 com.microsoft.sqlserver.jdbc.SQLServerDriver。 对于 JDBC Driver 4.1、4.2 和 6.0，驱动程序包含在 sqljdbc.jar、sqljdbc4.jar、sqljdbc41.jar 或 sqljdbc42.jar 文件中。

对于 JDBC Driver 6.2，驱动程序包含在 mssql-jdbc-6.2.2.jre7.jar 或 mssql-jdbc-6.2.2.jre8.jar 中。

对于 JDBC Driver 6.4，驱动程序包含在 mssql-jdbc-6.4.0.jre7.jar、mssql-jdbc-6.4.0.jre8.jar 或 mssql-jdbc-6.4.0.jre9.jar 中。

对于 JDBC Driver 7.0，驱动程序包含在 mssql-jdbc-7.0.0.jre8.jar 或 mssql-jdbc-7.0.0.jre10.jar 中。

对于 JDBC Driver 7.2，驱动程序包含在 mssql-jdbc-7.2.2.jre8.jar 或 mssql-jdbc-7.2.2.jre11.jar 中。

对于 JDBC Driver 7.4，驱动程序包含在 mssql-jdbc-7.4.1.jre8.jar、mssql-jdbc-7.4.1.jre11.jar 或 mssql-jdbc-7.4.1.jre12.jar 中。

对于 JDBC Driver 8.2，驱动程序包含在 mssql-jdbc-8.2.2.jre8.jar、mssql-jdbc-8.2.2.jre11.jar 或 mssql-jdbc-8.2.2.jre13.jar 中  。

对于 JDBC Driver 8.4，驱动程序包含在 mssql-jdbc-8.4.1.jre8.jar、mssql-jdbc-8.4.1.jre11.jar 或 mssql-jdbc-8.4.1.jre14.jar 中。

对于 JDBC Driver 9.2，驱动程序包含在 mssql-jdbc-9.2.0.jre8.jar、mssql-jdbc-9.2.0.jre11.jar 或 mssql-jdbc-9.2.0.jre15.jar 中  。

只要你使用 JDBC DriverManager 类加载驱动程序，只要你在任何驱动程序配置中指定驱动程序的类名，就会使用此类名。 例如，配置 Java EE 应用程序服务器内的数据源可能要求输入驱动程序类名称。  
  
## <a name="data-sources"></a>数据源

JDBC 驱动程序为 Java EE / JDBC 3.0 数据源提供支持。 JDBC 驱动程序 [SQLServerXADataSource](../../connect/jdbc/reference/sqlserverxadatasource-class.md) 类由 `com.microsoft.sqlserver.jdbc.SQLServerXADataSource` 实现。  
  
### <a name="datasource-names"></a>数据源名称

可以使用数据源来建立数据库连接。 下表中描述了可用于 JDBC 驱动程序的数据源：  
  
|数据源类型|类名和说明|  
|---------------|--------------------------|  
|数据源|`com.microsoft.sqlserver.jdbc.SQLServerDataSource` <br/> <br/> 非连接池数据源。|  
|ConnectionPoolDataSource|`com.microsoft.sqlserver.jdbc.SQLServerConnectionPoolDataSource` <br/> <br/> 用于配置 JAVA EE 应用程序服务器连接池的数据源。 通常当应用程序在 JAVA EE 应用程序服务器中运行时使用。|  
|XADataSource|`com.microsoft.sqlserver.jdbc.SQLServerXADataSource` <br/> <br/> 用于配置 JAVA EE XA 数据源的数据源。 通常当应用程序在 JAVA EE 应用程序服务器和 XA 事务管理器中运行时使用。|  
  
### <a name="data-source-properties"></a>数据源属性

所有数据源均支持设置和获取与基础驱动程序的属性集关联的任何属性的功能。  
  
示例：  
  
`setServerName("localhost");`  
`setDatabaseName("AdventureWorks");`  
  
下面的内容说明应用程序如何使用数据源进行连接：  

```java
//initialize JNDI ..  
Context ctx = new InitialContext(System.getProperties());
...
DataSource ds = (DataSource) ctx.lookup("MyDataSource");
Connection c = ds.getConnection("user", "pwd");  
```

若要详细了解数据源属性，请参阅[设置数据源属性](../../connect/jdbc/setting-the-data-source-properties.md)。  
  
## <a name="see-also"></a>另请参阅

[JDBC 驱动程序概述](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
