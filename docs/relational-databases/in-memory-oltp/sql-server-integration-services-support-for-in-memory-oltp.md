---
title: SSIS 对内存中 OLTP 的支持
description: 了解如何使用本机编译的存储过程作为 SQL Server Integration Services 包中的源组件和目标组件。
ms.custom: seo-dt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: ea82a9b9-e9ed-4d6f-b3fd-917f6c687ae3
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3f638de7d98cd8e322174172ce918152cbe4623a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485189"
---
# <a name="sql-server-integration-services-support-for-in-memory-oltp"></a>对内存中 OLTP 的 SQL Server Integration Services 支持
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  你可以使用内存优化的表、引用内存优化表的视图或本机编译的存储过程作为你的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (SSIS) 包的源或目标。 你可以在 SSIS 包的数据流中使用 [ADO NET 源](../../integration-services/data-flow/ado-net-source.md)、 [OLE DB 源](../../integration-services/data-flow/ole-db-source.md)或 [ODBC 源](../../integration-services/data-flow/odbc-source.md) 并配置源组组件以便从内存优化的表或视图检索数据，或指定一个 SQL 语句来执行本机编译的存储过程。 同样，你可以使用 [ADO NET 目标](../../integration-services/data-flow/ado-net-destination.md)、 [OLE DB 目标](../../integration-services/data-flow/ole-db-destination.md)或 [ODBC 目标](../../integration-services/data-flow/odbc-destination.md) 将数据加载到内存优化的表或视图，或指定一个 SQL 语句来执行本机编译的存储过程。  
  
 您可以在 SSIS 包中配置上述的源和目标组件以从内存优化的表和视图中读取或写入它们，所用的方法与从其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表和视图中读取或写入它们的方法相同。 但是，在使用本机编译的存储过程时，需要注意下一节中的几个重要事项。  
  
## <a name="invoking-a-natively-compiled-stored-procedure-from-an-ssis-package"></a>从 SSIS 包调用本机编译的存储过程  
 若要从 SSIS 包调用本机编译的存储过程，建议使用 ODBC 源或 ODBC 目标并使用以下格式的 SQL 语句：\<procedure name>，不带 EXEC 关键字。 如果在 SQL 语句中使用 EXEC 关键字，您将看到错误消息，因为 ODBC 连接管理器将 SQL 命令文本解释为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句而非存储过程并使用游标，而执行本机编译的存储过程时不支持游标。 连接管理器将没有 EXEC 关键字的 SQL 语句作为存储过程调用处理，将不使用游标。  
  
 还可以使用 ADO .NET 源和 OLE DB 源来调用本机编译的存储过程，但是我们建议您使用 ODBC 源。 如果您配置 ADO .NET 源以执行本机编译的存储过程，将看到错误消息，因为 ADO .NET 源默认使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient) 的数据访问接口不支持执行本机编译的存储过程。 您可以配置 ADO .NET 源以使用 ODBC 数据访问接口、用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的 OLE DB 访问接口或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client。 但是，请注意 ODBC 源的性能好于使用 ODBC 数据访问接口的 ADO .NET 源。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 对内存中 OLTP 的支持](./transact-sql-support-for-in-memory-oltp.md)  
  
