---
description: 了解如何在数据源向导中定义高级选项，以创建与 SQL Server 的新 ODBC 连接。
title: 数据源向导屏幕 4 (ODBC Driver for SQL Server)
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5152012e385e5e28ebccc967eb30af42852cf640
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673331"
---
# <a name="data-source-wizard-screen-4"></a>数据源向导屏幕 4

指定用于 SQL Server 消息的语言、字符集转换，以及适用于 SQL Server 的 ODBC 驱动程序是否应使用区域设置。 还可以控制长时间运行查询的日志记录和驱动程序统计信息设置。

## <a name="options"></a>选项

### <a name="change-the-language-of-sql-server-system-messages-to"></a>将 SQL Server 系统消息的语言更改为

SQL Server 的每个实例都有多组系统消息，每组消息使用不同的语言（例如，英语、西班牙语、法语等等）。 如果对具有多组系统消息的服务器定义一个数据源，您可以指定要为系统消息使用哪种语言。 从列表中选择语言。 如果 SQL Server 中仅安装了一种语言，则此选项不可用。

### <a name="use-strong-encryption-for-data"></a>对数据使用强加密

选中此选项后，将对通过使用此 DSN 建立的连接传输的数据加密。 默认情况下，即使清除此复选框，也将对登录名加密。

### <a name="trust-server-certificate"></a>信任服务器证书

仅当启用“对数据使用强加密”时，此选项才适用。 选中后，将不会验证服务器的证书是否有服务器的正确主机名，以及是否由受信任的证书颁发机构颁发。

### <a name="perform-translation-for-character-data"></a>执行字符数据翻译

选中此复选框后，适用于 SQL Server 的 ODBC 驱动程序使用 Unicode 对客户端计算机和 SQL Server 之间发送的 ANSI 字符串进行转换。 ODBC 驱动程序有时会在 SQL Server 代码页和客户端计算机中的 Unicode 之间进行转换。 此选项要求 SQL Server 使用的代码页为客户端计算机中可用的代码页之一。

清除此复选框后，当在客户端应用程序和服务器之间发送 ANSI 字符串时，将不会对 ANSI 字符串中的扩展字符进行翻译。 如果客户端计算机使用不同于 SQL Server 代码页的 ANSI 代码页 (ACP)，则可能会错误地解释 ANSI 字符串中的扩展字符。 如果客户端计算机对 SQL Server 使用的 ACP 使用同一代码页，则会正确地解释扩展字符。

### <a name="use-regional-settings-when-outputting-currency-numbers-dates-and-times"></a>在输出货币、数字、日期和时间时使用区域设置

指定驱动程序使用客户端计算机的区域设置对字符输出字符串中的货币、数字、日期和时间进行格式设置。 驱动程序使用通过数据源连接的用户的 Windows 登录帐户的默认区域设置。 请对仅显示数据的应用程序选择此选项，而对处理数据的应用程序则不要选择此选项。

### <a name="save-long-running-queries-to-the-log-file"></a>将长时间运行的查询保存到日志文件

指定驱动程序记录任何时间超出“长查询时间”值的查询。 长时间运行的查询将记录到指定文件中。 若要指定一个日志文件，可以在此框中键入完整的路径和文件名，也可以选择“浏览”，在现有文件目录中导航选择一个日志文件。

### <a name="long-query-time-milliseconds"></a>长查询时间(毫秒)

为长时间运行的查询日志记录指定一个阈值（毫秒）。 将记录任何运行时间超过该毫秒数的查询。

### <a name="log-odbc-driver-statistics-to-the-log-file"></a>将 ODBC 驱动程序统计信息记录到日志文件

指定记录的统计信息。 统计信息将记录到指定的文件中。 若要指定一个日志文件，可以在此框中键入完整的路径和文件名，也可以选择“浏览”，在现有文件目录中导航选择一个日志文件。

统计信息日志是一个制表符分隔文件，可以在 Microsoft Excel 或支持制表符分隔文件的任何其他应用程序中进行分析。

### <a name="connect-retry-count"></a>连接重试计数

指定失败连接尝试的重试次数。

### <a name="connect-retry-interval-seconds"></a>连接重试时间间隔(秒)

指定每次连接重试尝试之间的秒数。 有关运行此选项和“连接重试计数”选项的详细信息，请参阅 [Windows ODBC 驱动程序中的连接弹性](connection-resiliency-in-the-windows-odbc-driver.md)。

### <a name="back"></a>返回

选择此按钮可返回到向导的上一页。

### <a name="finish"></a>完成

如果此屏幕上指定的信息为全部信息，则可以选择“完成”。 DSN 是使用在向导的此屏幕和其他屏幕上指定的所有属性创建的，你将有机会测试新创建的 DSN。

## <a name="next-steps"></a>后续步骤

[数据源向导屏幕 3](dsn-wizard-3.md)
