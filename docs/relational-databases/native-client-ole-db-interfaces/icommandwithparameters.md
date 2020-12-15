---
description: 'ICommandWithParameters (Native Client OLE DB 提供程序) '
title: ICommandWithParameters (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 66644c70-def7-46d8-8c47-b883292a0288
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1f23eeff801717661305dafac67939990545e43a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473438"
---
# <a name="icommandwithparameters-native-client-ole-db-provider"></a>ICommandWithParameters (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，数据库引擎中的改进功能允许 ICommandWithParameters::GetParameterInfo 获取关于预期结果的更准确描述。 这些更准确的结果可能与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以前版本中的 CommandWithParameters::GetParameterInfo 所返回的值有所不同。 有关详细信息，请参阅[元数据发现](../../relational-databases/native-client/features/metadata-discovery.md)。  
  
 同样从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，在调用 ICommandWithParameters::SetParameterInfo 时，传递给 pwszName 参数的值必须是有效的标识符  。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。  
  
## <a name="see-also"></a>另请参阅  
 [接口 (OLE DB)](./sql-server-native-client-ole-db-interfaces.md)  
  
