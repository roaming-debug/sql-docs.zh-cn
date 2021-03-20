---
description: " (Native Client OLE DB 提供程序的处理结果) "
title: " (Native Client OLE DB 提供程序的处理结果) "
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, results processing
- OLE DB, processing results
- rowsets [SQL Server], results processing
- results [SQL Server Native Client]
ms.assetid: 20887ac4-f649-4e7f-92e6-f929e2e70952
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f479b8015248276e87c226c974be1344d06e27f9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753637"
---
# <a name="processing-results-native-client-ole-db-provider"></a> (Native Client OLE DB 提供程序的处理结果) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  如果行集对象是由执行命令或直接从访问接口生成的，则使用者需要检索和访问行集中的数据。  
  
 行集是使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口能够以表格格式提供数据的核心对象。 从概念上说，行集是指其中的每行都拥有列数据的行的集合。 行集对象可提供如下接口：IRowset（包含按顺序从行集提取行的方法）、IAccessor（允许定义一组列绑定来说明将表格格式数据绑定到使用者程序变量的方式）、IColumnsInfo（提供有关行集中列的信息）以及 IRowsetInfo（提供有关行集的信息）     。  
  
 使用者可以调用 IRowset::GetData 方法将行集中的一行数据检索到缓冲区中  。 在调用 GetData 之前，使用者使用一组 DBBINDING 结构来描述缓冲区  。 每个绑定都说明了行集中的列在使用者缓冲区中的存储方式并包含以下内容：  
  
-   应用绑定的列（或参数）的序号。  
  
-   有关绑定内容的信息（例如，数据值、数据长度及其绑定状态）。  
  
-   有关缓冲区中到这些部分中每个部分的偏移量的信息。  
  
-   数据值存在于使用者缓冲区中时的长度和类型。  
  
 获取数据时，访问接口将使用每个绑定中的信息来确定从使用者缓冲区的何处以及如何检索数据。 设置使用者缓冲区中的数据时，访问接口将使用每个绑定中的信息来确定在使用者缓冲区的何处以及如何返回数据。  
  
 指定 DBBINDING 结构后，将创建取值函数 (IAccessor::CreateAccessor)  。 取值函数是一个绑定集合，可用于获取或设置使用者缓冲区中的数据。  
  
## <a name="see-also"></a>另请参阅  
 [创建 SQL Server Native Client OLE DB 提供程序应用程序](../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)   
 [OLE DB 操作指南主题](../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
  
