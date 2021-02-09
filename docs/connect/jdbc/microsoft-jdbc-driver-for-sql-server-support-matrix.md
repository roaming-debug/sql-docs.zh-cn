---
title: Microsoft JDBC Driver for SQL Server 支持矩阵
description: 本页介绍了 Microsoft JDBC Driver for SQL Server 的支持矩阵和支持生命周期策略。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: c5769e67-99f7-4bc1-a4fa-8941dad33d35
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 21b977b950babad552246925d78878c08cd36af3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163612"
---
# <a name="microsoft-jdbc-driver-for-sql-server-support-matrix"></a>Microsoft JDBC Driver for SQL Server 支持矩阵

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  本页包含 Microsoft SQL Server JDBC 驱动程序的支持矩阵和支持生命周期策略。  
  
## <a name="microsoft-jdbc-driver-support-lifecycle-matrix-and-policy"></a>Microsoft JDBC Driver 支持生命周期矩阵和策略  

Microsoft 支持生命周期 (MSL) 策略提供了与 Microsoft 产品的支持生命周期有关的可预测透明信息。 从驱动程序发布之日起，JDBC 驱动程序版本 4.x、6.x、7.x、8.x 和 9.x 就具有五年的主流支持。 主流支持在 Microsoft 支持生命周期网站上定义。  
  
Microsoft JDBC 驱动程序不提供扩展和自定义支持选项。  

支持以下 Microsoft JDBC 驱动程序，直到指定的支持结束日期。  
  
|驱动程序名称|驱动程序包版本|适用的 JAR|主要支持结束日期|
|-|-|-|-|  
|Microsoft JDBC Driver 9.2 for SQL Server|9.2|mssql-jdbc-9.2.0.jre15.jar<br> mssql-jdbc-9.2.0.jre11.jar<br> mssql-jdbc-9.2.0.jre8.jar|2026 年 1 月 29 日|
|Microsoft JDBC Driver 8.4 for SQL Server|8.4|mssql-jdbc-8.4.1.jre14.jar<br> mssql-jdbc-8.4.1.jre11.jar<br> mssql-jdbc-8.4.1.jre8.jar|2025 年 7 月 31 日|
|Microsoft JDBC Driver 8.2 for SQL Server|8.2|mssql-jdbc-8.2.2.jre13.jar<br> mssql-jdbc-8.2.2.jre11.jar<br> mssql-jdbc-8.2.2.jre8.jar|2025 年 1 月 31 日|
|Microsoft JDBC Driver 7.4 for SQL Server|7.4|mssql-jdbc-7.4.1.jre12.jar<br> mssql-jdbc-7.4.1.jre11.jar<br> mssql-jdbc-7.4.1.jre8.jar|2024 年 7 月 31 日|
|Microsoft JDBC Driver 7.2 for SQL Server|7.2|mssql-jdbc-7.2.2.jre11.jar<br> mssql-jdbc-7.2.2.jre8.jar|2024 年 1 月 31 日|
|Microsoft JDBC Driver 7.0 for SQL Server|7.0|mssql-jdbc-7.0.0.jre10.jar<br> mssql-jdbc-7.0.0.jre8.jar|2023 年 7 月 31 日|
|Microsoft JDBC Driver 6.4 for SQL Server|6.4|mssql-jdbc-6.4.0.jre9.jar<br> mssql-jdbc-6.4.0.jre8.jar<br> mssql-jdbc-6.4.0.jre7.jar|2023 年 2 月 27 日|
|Microsoft JDBC Driver 6.2 for SQL Server|6.2|mssql-jdbc-6.2.2.jre8.jar<br> mssql-jdbc-6.2.2.jre7.jar|2022 年 6 月 30 日|
|Microsoft JDBC Driver 6.0 for SQL Server|6.0|sqljdbc42.jar<br>sqljdbc41.jar|2021 年 7 月 14 日|
  
 以下 Microsoft JDBC 驱动程序不再受到支持。  

|驱动程序名称|驱动程序包版本|主要支持结束日期|  
|-|-|-|
|Microsoft SQL Server JDBC 驱动程序 4.2|4.2|2020 年 8 月 24 日|
|Microsoft SQL Server JDBC 驱动程序 4.1|4.1|2019 年 12 月 12 日|  
|Microsoft JDBC Driver 4.0 for SQL Server|4.0|2017 年 3 月 6 日|  
|Microsoft SQL Server JDBC Driver 3.0|3.0|2015 年 4 月 23 日|  
|Microsoft SQL Server JDBC 驱动程序 2.0|2.0|2012 年 12 月 31 日|  
|Microsoft SQL Server 2005 JDBC Driver 1.2|1.2|2011 年 6 月 25 日|  
|Microsoft SQL Server 2005 JDBC 驱动程序 1.1|1.1|2011 年 6 月 25 日|  
|Microsoft SQL Server 2005 JDBC 驱动程序 1.0|1.0|2011 年 6 月 25 日|  
|Microsoft SQL Server 2000 JDBC 驱动程序|2000|2010 年 7 月 9 日|  
  
## <a name="sql-version-compatibility"></a>SQL 版本兼容性  
  
|数据库版本&nbsp;&#8594;<br />&#8595; 驱动程序版本|Azure SQL Database|Azure Synapse Analytics|Azure SQL 托管实例|SQL Server 2019|SQL Server 2017|SQL Server 2016|SQL Server 2014|SQL Server 2012|PDW 2008R2 AU3<sup>4</sup>|SQL Server 2008 R2|SQL Server 2008|
|---|---|---|---|---|---|---|---|---|---|---|---|
|9.2|是|是|是|是|是|是|是|是|   |   |   |
|8.4|是|是|是|是|是|是|是|是|是|   |   |
|8.2|是|是|是|是|是|是|是|是|是|   |   |
|7.4|是|是|是|是|是|是|是|是|是|   |   |
|7.2|是|是|是|   |是|是|是|是|是|是|   |
|7.0|是|是|是|   |是|是|是|是|是|是|   |
|6.4|是|是|是|   |是|是|是|是|是|是|   |
|6.2|是|是|   |   |是|是|是|是|是|是|是|
|6.1|是|   |   |   |   |是|是|是|是|是|是|
|6.0|是|   |   |   |   |是|是|是|是|是|是|
|4.2|是|   |   |   |   |是|是|是|是|是|是|
|4.1|是|   |   |   |   |是|是|是|是|是|是|
|4.0|是|   |   |   |   |是|是|是|是|是|是|
|3.0|Yes<sup>2</sup>|   |   |   |   |   |是<sup>5</sup>|是<sup>1</sup>|   |是|是|
|2.0|   |   |   |   |   |   |   |   |   |是<sup>3</sup>|是<sup>3</sup>|
|1.2|   |   |   |   |   |   |   |   |   |   |是<sup>3</sup>|

 <sup>1</sup> Microsoft SQL Server JDBC Driver 3.0 版可作为下级客户端连接到 SQL Server 2012。  
  
 <sup>2</sup> 3.0 驱动程序中以修补程序的形式引入了 Azure SQL Database 的支持。 建议 Azure SQL Database 客户使用最新的驱动程序版本。  
  
 <sup>3</sup> Microsoft SQL Server JDBC Driver 2.0 版和 Microsoft SQL Server 2005 JDBC Driver 1.2 版可作为下级客户端连接到 SQL Server 2008。 当允许下级转换时，应用程序可以对新的 SQL Server 2008 数据类型执行查询和更新，如 time、date、datetime2、datetimeoffset 和 FILESTREAM。 有关如何将这些新数据类型用于 JDBC 驱动程序的详细信息，请参阅  [Working with SQL Server 2008 Date/Time Data Types using JDBC Driver（使用 JDBC 驱动程序处理 SQL Server 2008 日期/时间数据类型）](/archive/blogs/jdbcteam/) 和  [Working with SQL Server 2008 FileStream using JDBC Driver（使用 JDBC 驱动程序处理 SQL Server 2008 文件流）](/archive/blogs/jdbcteam/)。 有关这些新数据类型的下级兼容性的详细信息，请参阅 SQL Server 联机丛书中的  [Using Date and Time Data（使用日期和时间数据）](/previous-versions/sql/sql-server-2008-r2/ms180878(v=sql.105))和  [FILESTREAM Support（文件流支持）](../../relational-databases/native-client/features/filestream-support.md) 主题。  
  
 <sup>4</sup> Microsoft JDBC Driver 4.0 for SQL Server 和 Microsoft SQL Server 2008 R2 并行数据仓库设备更新 3 中首先引入了 Microsoft JDBC Driver 和并行数据仓库间的连接支持。  
  
 <sup>5</sup> Microsoft SQL Server JDBC Driver 3.0 版可作为下级客户端连接到 SQL Server 2014。  
  
## <a name="java-and-jdbc-specification-support"></a>Java 和 JDBC 规格支持
  
|JDBC 驱动程序版本|JRE 版本|JDBC API 版本|
|-|-|-|
|[9.2](release-notes-for-the-jdbc-driver.md#92)|1.8、11、15|4.2、4.3（部分）|
|[8.4](release-notes-for-the-jdbc-driver.md#84)|1.8、11、14|4.2、4.3（部分）|
|[8.2](release-notes-for-the-jdbc-driver.md#82)|1.8、11、13|4.2、4.3（部分）|
|[7.4](release-notes-for-the-jdbc-driver.md#74)|1.8、11、12|4.2、4.3（部分）|
|[7.2](release-notes-for-the-jdbc-driver.md#72)|1.8、11|4.2、4.3（部分）|
|[7.0](release-notes-for-the-jdbc-driver.md#70)|1.8、10|4.2、4.3（部分）|
|[6.4](release-notes-for-the-jdbc-driver.md#64)|1.7、1.8、9|4.1、4.2、4.3（部分）|
|[6.2](release-notes-for-the-jdbc-driver.md#62)|1.7、1.8|4.1、4.2|
|[6.1](release-notes-for-the-jdbc-driver.md#61)|1.7、1.8|4.1、4.2|
|[6.0](release-notes-for-the-jdbc-driver.md#60)|1.7、1.8|4.1、4.2|
|4.2|1.7、1.8|4.1、4.2|
|4.1|1.7|4.0|
|4.0|1.5、1.6、1.7|3.0、4.0|
|3.0|1.5、1.6、|3.0、4.0|
|2.0|1.5、1.6|3.0、4.0|
|1.2|1.4、1.5、1.6|3.0|
|1.1|1.4|3.0|
|1.0|1.4|3.0|
|2000|1.4|3.0|

## <a name="supported-operating-systems"></a>支持的操作系统

Microsoft JDBC 驱动程序可在任何支持使用 Java 虚拟机 (JVM) 的操作系统上工作。 一些常用的平台包括 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Server 2008 R2、Linux、Unix、AIX、macOS 等。  

JDBC 产品团队在 Windows、Sun Solaris、SUSE Linux、Ubuntu Linux、CentOS Linux 和 macOS 上测试了驱动程序。

## <a name="application-server-support"></a>应用程序服务器支持

针对各种应用程序服务器对 Microsoft SQL Server JDBC 驱动程序进行了测试。  请咨询应用程序服务器供应商了解有关与其产品兼容的驱动程序版本的其他详细信息。