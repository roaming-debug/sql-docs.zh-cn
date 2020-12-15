---
description: 'IRowsetFastLoad (Native Client OLE DB 提供程序) '
title: IRowsetFastLoad (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: COM
helpviewer_keywords:
- IRowsetFastLoad interface
ms.assetid: d19a7097-48d9-409a-aff9-277891b7aca7
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 36558ac50fdfd5e563c0851fad7cd4720a14f1bf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462148"
---
# <a name="irowsetfastload-native-client-ole-db-provider"></a>IRowsetFastLoad (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  IRowsetFastLoad 接口公开了对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 基于内存的大容量复制操作的支持。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者使用接口将数据快速添加到现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。  
  
 如果将会话的 SSPROP_ENABLEFASTLOAD 设置为 VARIANT_TRUE，则无法读取后续从该会话返回的行集中的数据。 将 SSPROP_ENABLEFASTLOAD 设置为 VARIANT_TRUE 时，在会话上创建的所有行集将属于 IRowsetFastLoad 类型。 IRowsetFastLoad 行集不支持行集提取功能，因此无法读取这些行集中的数据。  
  
## <a name="in-this-section"></a>本节内容  
  
|方法|说明|  
|------------|-----------------|  
|[IRowsetFastLoad::Commit &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-commit-ole-db.md)|标记一批插入的行的末尾并将这些行写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。|  
|[IRowsetFastLoad::InsertRow &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-insertrow-ole-db.md)|将行添加到大容量复制行集中。|  
  
## <a name="see-also"></a>另请参阅  
 [接口 &#40;OLE DB&#41;](./sql-server-native-client-ole-db-interfaces.md)   
 [使用 IRowsetFastLoad 大容量复制数据 (OLE DB)](../../relational-databases/native-client-ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md)   
 [使用 IROWSETFASTLOAD 和 ISEQUENTIALSTREAM 将 BLOB 数据发送到 SQL SERVER (OLE DB)](../../relational-databases/native-client-ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md)  
  
