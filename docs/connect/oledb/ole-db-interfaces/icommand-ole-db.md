---
title: ICommand（OLE DB 驱动程序）| Microsoft Docs
description: 了解特定于 OLE DB Driver for SQL Server 的 ICommand::Execute 方法的行为。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ICommand [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 085f529c05c5ed095f88fb523ea5dbec00859c6f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752607"
---
# <a name="icommand-ole-db"></a>ICommand (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  本文介绍特定于 OLE DB Driver for SQL Server 的 OLE DB 行为。  
  
## <a name="icommandexecute"></a>ICommand::Execute  
 如果插入的数据大于列的大小，通常会导致错误。 但是，可能会出现返回了 S_OK 而 `dwStatus` 设置为 DBSTATUS_S_TRUNCATED 的情况。 此错误通常发生在下面的几种情况下：

- 插入了带参数的数据  
- 列不够大，无法容纳数据  
- 未调用 `ICommandWithParameters::SetParameterInfo`  
  
## <a name="see-also"></a>另请参阅  
 [接口 (OLE DB)](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)
  
  
