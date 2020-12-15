---
title: 组件
description: 了解 SQL Server Native Client 的组件，如 sqlncli11.dll、msmdsrv.rll、sqlncli.msi 和 sqlncli11。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, about SQL Server Native Client ODBC driver
- data access [SQL Server Native Client], components
- components [SQL Server Native Client]
- SQLNCLI, about SQL Server Native Client
ms.assetid: 65f932d5-daa1-4eff-b6df-ee633fcf2a7c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6908497c7d296c5138b95c0f41d560123b75d943
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462078"
---
# <a name="components-of-sql-server-native-client"></a>SQL Server Native Client 的组件
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 包含下列组件：  
  
|组件|说明|  
|---------------|-----------------|  
|sqlncli11.dll|包含所有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 功能的动态链接库 (DLL) 文件。 它包括 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序。|  
|sqlnclir11.rll|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 库附带的资源文件。|   
|sqlncli.h|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件，其中包含使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 时所需的所有新定义。 该头文件取代了 odbcss.h 和 sqloledb.h 头文件。<br /><br /> 注意：不能在同一程序中引用 sqlncli.msi 和 odbcss.h，但只要先定义 sqloledb，就可以在同一程序中引用 sqlncli.msi 和 sqloledb。|  
|sqlncli11.lib|直接调用作为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序的一部分的 bcp 实用工具函数所需的库文件。<br /><br /> 注意：如果确实要在编程代码中引用 sqlncli11 文件，则需要确保 sqlncli11.dll 文件位于您的系统路径中，并且在使用您的应用程序的用户的系统路径中。|  
  
## <a name="see-also"></a>另请参阅  
 [使用 SQL Server Native Client 生成应用程序](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
  
  
