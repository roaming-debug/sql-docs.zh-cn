---
title: IBCPSession (OLE DB driver) | Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 如何使用 IBCPSession 来支持 SQL Server 基于文件的大容量复制操作，并了解其成员。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- IBCPSession interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7ff67f06b977600506a0add9eb74a716c42e70ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186838"
---
# <a name="ibcpsession-ole-db"></a>IBCPSession (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  IBCPSession 接口公开了对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 基于文件的大容量复制操作的支持。 IBCPSession  接口在 OLE DB Driver for SQL Server 中公开，与会话位于同一级别。 在适用于 SQL Server 的 OLE DB 驱动程序中，数据源对象是会话对象的工厂，且大容量复制操作在连接属性 SSPROP_ENABLEBULKCOPY 中指定。 此外，SSPROP_ENABLEFASTLOAD 属性应当设置为 True。  
  
 调用 IDBCreateSession::CreateSession 方法随后将创建 BulkCopySession 对象   。 然后，可针对此 IBCPSession 对象的 IBCPSession 接口，采用极为相似的签名来调用通过 IBCPSession 对象公开的所有基于文件的大容量复制方法    。  
  
> [!NOTE]  
>  适用于 SQL Server 的 OLE DB 驱动程序支持通过 [IRowsetFastLoad](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md) 接口进行基于内存的大容量复制操作。  
  
 有关使用 OLE DB Driver for SQL Server 进行大容量复制操作的详细信息，请参阅[执行大容量复制操作](../../oledb/features/performing-bulk-copy-operations.md)。  
  
 有关演示如何使用 IBCPSession  接口的示例，请参阅 [IBCPSession::BCPDone (OLE DB)](../../oledb/ole-db-interfaces/ibcpsession-bcpdone-ole-db.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
|方法|说明|  
|------------|-----------------|  
|[IBCPSession::BCPColFmt &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md)|在程序变量与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 列之间创建绑定。|  
|[IBCPSession::BCPColumns &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md)|设置绑定到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表中列的字段数。|  
|[IBCPSession::BCPControl &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md)|设置大容量复制操作的选项。|  
|[IBCPSession::BCPDone &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-bcpdone-ole-db.md)|提交要发送到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的剩余行。|  
|[IBCPSession::BCPExec &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-bcpexec-ole-db.md)|执行大容量复制操作。|  
|[IBCPSession::BCPInit &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md)|初始化大容量复制结构，执行某些错误检查，验证数据和格式化文件名是否正确，然后打开文件。|  
|[IBCPSession::BCPReadFmt &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-bcpreadfmt-ole-db.md)|从格式化文件中读取每一列的格式信息。|  
|[IBCPSession::BCPWriteFmt &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md)|将每一列的格式信息写入格式化文件。|  
  
## <a name="see-also"></a>另请参阅  
 [接口 (OLE DB)](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)  
  
  
