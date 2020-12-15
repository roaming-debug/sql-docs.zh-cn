---
description: 'ICommand (Native Client OLE DB 提供程序) '
title: ICommand (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ICommand [SQL Server Native Client]
ms.assetid: 5e24b3a0-0658-44fc-b653-f4c52f9eb328
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f261d8fa00650be277f145c43c13af0ab7c03fa9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483188"
---
# <a name="icommand-native-client-ole-db-provider"></a>ICommand (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题讨论特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的 OLE DB 行为。  
  
## <a name="icommandexecute"></a>ICommand::Execute  
 如果插入的数据大于列的大小，通常会导致错误。 但是，可能会出现将返回 S_OK、但 dwStatus 将设置为 DBSTATUS_S_TRUNCATED 的情况  。 当用参数插入数据时，通常会发生这种情况，列的大小不足以容纳数据，并且 **ICommandWithParameters：： SetParameterInfo** 尚未调用。  
  
## <a name="see-also"></a>另请参阅  
 [接口 (OLE DB)](./sql-server-native-client-ole-db-interfaces.md)  
  
