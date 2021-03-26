---
title: 从 SQL Server 2005 Native Client 更新应用程序 | Microsoft Docs
description: 了解自 SQL Server 2005 (9.x) 中的 SQL Server Native Client 以来在 OLE DB Driver for SQL Server 中的重大更改。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, updating applications
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8f53c33d04e76c362737b7c7bdc3ad803846cf9
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793135"
---
# <a name="updating-applications-from-sql-server-2005-native-client"></a>从 SQL Server 2005 Native Client 更新应用程序

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

本文介绍自 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 以来在 OLE DB Driver for SQL Server 中的重大更改。

从 Microsoft 数据访问组件 (MDAC) 升级到适用于 SQL Server 的 OLE DB 驱动程序时，也会看到一些行为差异。 有关详细信息，请参阅[将应用程序从 MDAC 更新到 OLE DB Driver for SQL Server](updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md)。

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 随 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 附带。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 随 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 附带。  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.5 随 [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] 附带。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 11.0 随 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 和 [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] 附带。

|与 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] Native Client 相比，OLE DB Driver for SQL Server 中更改的行为|说明|
|------------------------------------------------------------------------------------|-----------------|
|OLE DB 仅填充到定义的小数位数。|对于要将转换的数据发送到服务器的转换操作，OLE DB Driver for SQL Server 在填充数据中的尾随零时，最多仅填充到 datetime  值的最大长度。 SQL Server Native Client 9.0 则填充到 9 位数。|
|验证 ICommandWithParameter::SetParameterInfo 的 DBTYPE_DBTIMESTAMP。|OLE DB Driver for SQL Server 实现了 ICommandWithParameter::SetParameterInfo 中 bScale  的 OLE DB 要求，设置为 DBTYPE_DBTIMESTAMP 的分数秒精度。|
|sp_columns  存储过程现在返回“否”  ，而不是为 IS_NULLABLE 列返回“否”  。|在 OLE DB Driver for SQL Server 中，sp_columns  存储过程现在返回“否”  ，而不是为 IS_NULLABLE 列返回“否”  。|
|日期超出范围时返回其他错误。|对于 datetime  类型，OLE DB Driver for SQL Server 将对超出范围的日期返回与更早版本返回错误号不同的错误号。<br /><br /> 具体来说，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 对字符串转换到 datetime  时所有超出范围的年份值均返回 22007；而 OLE DB Driver for SQL Server 则在日期处于 datetime2  支持的范围但不处于 datetime  或 smalldatetime  支持的范围时返回 22008。|
|如果舍入将更改日期，则 datetime 值将截断秒的小数部分，并且不舍入  。|在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 之前，对于发送到服务器的 datetime 值的客户端行为是将它们舍入到最接近 1/300 秒的值  。 在 OLE DB Driver for SQL Server 中，如果舍入会更改日期，则该方案会导致截断秒的小数部分。|
|可能对 datetime 值的秒数执行的截断。|如果绑定到某一 datetime 列并且其类型标识符为 DBTYPE_DBTIMESTAMP (OLE DB) 或 SQL_TIMESTAMP (ODBC)、小数位数为 0，则使用连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2005 服务器的适用于 SQL Server 的 OLE DB 驱动程序生成的应用程序将截断要发送到服务器的时间部分中的秒和秒的小数部分。<br /><br /> 例如：<br /><br /> 输入数据：1994-08-21 21:21:36.000<br /><br /> 插入的数据：1994-08-21 21:21:00.000|
|从 DBTYPE_DBTIME 到 DBTYPE_DATE 的 OLE DB 数据转换不会再导致日期发生更改。|在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 之前，如果 DBTYPE_DATE 的时间部分是在距离午夜的半秒内，则 OLE DB 转换代码将导致日期发生更改。 在 OLE DB Driver for SQL Server 中，日期不会更改（秒的小数部分将截断但不舍入）。|
|IBCPSession::BCColFmt 转换更改。|在 OLE DB Driver for SQL Server 中，使用 IBCPSession::BCOColFmt 将 SQLDATETIME 或 SQLDATETIME 转换为字符串类型时，将导出小数值。 例如，将类型 SQLDATETIME 转换为类型 SQLNVARCHARMAX 时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 之前的版本返回<br /> 1989-02-01 00:00:00。<br />适用于 SQL Server 的 OLE DB 驱动程序返回 <br />1989-02-01 00:00:00.0000000。|
|使用 BCP API 的自定义应用程序现在可以看到警告。|对于所有类型，如果数据长度超过某字段的指定长度，则 BCP API 将生成警告消息。 以前，该警告仅针对字符类型，而不会针对所有类型发出。|
|将空字符串插入到绑定为日期/时间类型的 sql_variant 中会生成错误  。|在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 中，将空字符串插入到绑定为日期/时间类型的 sql_variant 中不会生成错误  。 OLE DB Driver for SQL Server 在这种情况下则会正确地生成错误。|
|当有触发器运行时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可能会返回不同的结果。|[!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 中引入的变化可能会导致应用程序得到从 NOCOUNT OFF 有效时导致触发器运行的语句返回的不同的结果  。 在这种情况下，您的应用程序可能会生成错误。 若要解决此错误，请在触发器中设置 NOCOUNT ON  。|

## <a name="see-also"></a>另请参阅

[适用于 SQL Server 的 OLE DB 驱动程序](../oledb-driver-for-sql-server.md)
