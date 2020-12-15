---
description: 'IRowsetFastLoad：： InsertRow (Native Client OLE DB 提供程序) '
title: IRowsetFastLoad：： InsertRow (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IRowsetFastLoad::InsertRow (OLE DB)
apitype: COM
helpviewer_keywords:
- InsertRow method
ms.assetid: 594d3461-34d2-41e7-8ad4-bd2753601ab6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 633e9d77a30dae3b140a4b4cf6f8ff36daf402a6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462158"
---
# <a name="irowsetfastloadinsertrow-native-client-ole-db-provider"></a>IRowsetFastLoad：： InsertRow (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将行添加到大容量复制行集中。 有关示例，请参阅[使用 IRowsetFastLoad 大容量复制数据 (OLE DB)](../../relational-databases/native-client-ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md) 和[使用 IROWSETFASTLOAD 和 ISEQUENTIALSTREAM 将 BLOB 数据发送到 SQL Server (OLE DB)](../../relational-databases/native-client-ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT InsertRow(  
      HACCESSOR hAccessor,  
      void* pData);  
```  
  
## <a name="arguments"></a>参数  
  hAccessor[in]  
 定义大容量复制的行数据的取值函数句柄。 引用的取值函数为行取值函数，将绑定包含数据值的使用者拥有的内存。  
  
  pData[in]  
 指向包含数据值的使用者所拥有内存的指针。 有关详细信息，请参阅 [DBBINDING 结构](/previous-versions/windows/desktop/ms716845(v=vs.85))。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 方法成功。 所有列的任何绑定状态值都具有值 DBSTATUS_S_OK 或 DBSTATUS_S_NULL。  
  
 E_FAIL  
 出现了错误。 从行集的错误接口中发出错误信息。  
  
 E_INVALIDARG  
 pData 参数设置为 NULL 指针  。  
  
 E_OUTOFMEMORY  
 SQLNCLI11 无法分配足够的内存来完成请求。  
  
 E_UNEXPECTED  
 对以前被 [IRowsetFastLoad::Commit](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-commit-ole-db.md) 方法作废的大容量复制行集调用了该方法。  
  
 DB_E_BADACCESSORHANDLE  
 使用者提供的 hAccessor 参数无效  。  
  
 DB_E_BADACCESSORTYPE  
 指定的取值函数不是行取值函数，或者未指定使用者拥有的内存。  
  
## <a name="remarks"></a>备注  
 将使用者数据转换为列的数据类型时出现错误将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导致从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序返回 E_FAIL。 可以通过任何 InsertRow 方法或只通过 Commit 方法将数据传输到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]   。 在使用者应用程序收到存在数据类型转换错误的通知之前，它可以用错误数据多次调用 InsertRow 方法  。 因为 Commit 方法可确保使用者正确指定所有数据，所以使用者可在必要时使用 Commit 方法适当地验证数据   。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序大容量复制行集是只写的。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序不公开允许使用者查询行集的方法。 若要终止处理，使用者可以在不调用 Commit 方法的情况下释放其对 [IRowsetFastLoad](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md) 接口的引用  。 没有用于进行以下操作的设备：访问行集中使用者插入的行、更改其值或从行集中逐一删除该行。  
  
 大容量复制行在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的服务器上进行了格式化。 可能已针对连接或会话（例如 ANSI_PADDING）设置的任何选项均会影响行格式。 默认情况下，此选项默认设置为通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序进行的任何连接。  
  
## <a name="see-also"></a>另请参阅  
 [IRowsetFastLoad &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md)  
  
