---
title: 处理结果（OLE DB 驱动程序）
description: 了解 OLE DB Driver for SQL Server 使用者如何获取和访问由命令或访问接口生成的行集中的数据。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, results processing
- OLE DB, processing results
- rowsets [SQL Server], results processing
- results [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 99c6875a48ee85f13a5197689e8f74361469a3fc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746337"
---
# <a name="processing-results"></a>处理结果
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  如果行集对象是由执行命令或直接从访问接口生成的，则使用者需要检索和访问行集中的数据。  
  
 行集是使 OLE DB Driver for SQL Server 能够以表格格式提供数据的核心对象。 从概念上说，行集是指其中的每行都拥有列数据的行的集合。 行集对象可提供如下接口：IRowset（包含按顺序从行集提取行的方法）、IAccessor（允许定义一组列绑定来说明将表格格式数据绑定到使用者程序变量的方式）、IColumnsInfo（提供有关行集中列的信息）以及 IRowsetInfo（提供有关行集的信息）     。  
  
 使用者可以调用 IRowset::GetData 方法将行集中的一行数据检索到缓冲区中  。 在调用 GetData 之前，使用者使用一组 DBBINDING 结构来描述缓冲区  。 每个绑定都说明了行集中的列在使用者缓冲区中的存储方式并包含以下内容：  
  
-   应用绑定的列（或参数）的序号。  
  
-   有关绑定内容的信息（例如，数据值、数据长度及其绑定状态）。  
  
-   有关缓冲区中到这些部分中每个部分的偏移量的信息。  
  
-   数据值存在于使用者缓冲区中时的长度和类型。  
  
 获取数据时，访问接口将使用每个绑定中的信息来确定从使用者缓冲区的何处以及如何检索数据。 设置使用者缓冲区中的数据时，访问接口将使用每个绑定中的信息来确定在使用者缓冲区的何处以及如何返回数据。  
  
 指定 DBBINDING 结构后，将创建取值函数 (IAccessor::CreateAccessor)  。 取值函数是一个绑定集合，可用于获取或设置使用者缓冲区中的数据。  
  
## <a name="see-also"></a>另请参阅  
 [创建适用于 SQL Server 的 OLE DB 驱动程序应用程序](../../oledb/ole-db-driver/creating-a-oledb-driver-for-sql-server-application.md)   
 [OLE DB 操作指南主题](../../oledb/ole-db-how-to/ole-db-how-to-topics.md)  
  
  
