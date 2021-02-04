---
title: IRowsetFastLoad::Commit（OLE DB 驱动程序）| Microsoft Docs
description: 了解 IRowsetFastLoad::Commit 方法如何标记一批插入的行的末尾，并将它们写入 OLE DB Driver for SQL Server 中的 SQL Server 表。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IRowsetFastLoad::Commit (OLE DB)
apitype: COM
helpviewer_keywords:
- Commit method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e2b2d1b909732b6436346adb31db2d4c34b7f698
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183867"
---
# <a name="irowsetfastloadcommit-ole-db"></a>IRowsetFastLoad::Commit (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  标记一批插入的行的末尾并将这些行写入 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表。 有关示例，请参阅[使用 IRowsetFastLoad 大容量复制数据 (OLE DB)](../../oledb/ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md) 和[使用 IROWSETFASTLOAD 和 ISEQUENTIALSTREAM 将 BLOB 数据发送到 SQL Server (OLE DB)](../../oledb/ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT Commit(  
      BOOL fDone);  
```  
  
## <a name="arguments"></a>参数  
  fDone[in]  
 如果是 FALSE，则行集保持有效性，并且可由使用者用于插入其他行。 如果是 TRUE，则行集失去有效性，并且使用者无法执行进一步的插入。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 方法成功，并且所有插入的数据已写入 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表。  
  
 E_FAIL  
 发生了特定于访问接口的错误。 从访问接口检索特定错误文本的错误信息。  
  
 E_UNEXPECTED  
 对以前被 **IRowsetFastLoad::Commit** 方法作废的大容量复制行集调用了该方法。  
  
## <a name="remarks"></a>备注  
 OLE DB Driver for SQL Server 大容量复制行集的行为与延迟更新模式的行集相同。 当用户通过行集插入行数据时，对插入行的处理方式与在支持 IRowsetUpdate 的行集上挂起插入相同  。  
  
 使用者必须对大容量复制行集调用 Commit 方法，才能以与使用 IRowsetUpdate::Update 方法将挂起行提交到 SQL Server 实例相同的方式将插入行写入 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表。  
  
 如果使用者释放其对大容量复制行集的引用，而不调用 Commit 方法，则以前未写入的所有插入行将丢失  。  
  
 通过在将 fDone 参数设置为 FALSE 的情况下调用 Commit 方法，使用者可以成批插入行   。 当 fDone 设置为 TRUE 时，行集变为无效  。 无效的大容量复制行集仅支持 ISupportErrorInfo 接口和 IRowsetFastLoad::Release 方法   。  
  
## <a name="see-also"></a>另请参阅  
 [IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md)  
  
  
