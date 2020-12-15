---
description: Dbmslpcn.dll 共享内存中的 ConnectionValidSharedMemory 函数
title: ConnectionValidSharedMemory dbmslpcn.dll
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 6ae35826-7d75-4542-b686-5f79316b6157
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a487eb380ea4e7b0fe246efdb9281cbab63a1303
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475988"
---
# <a name="connectionvalidsharedmemory-function-in-dbmslpcndll-shared-memory"></a>Dbmslpcn.dll 共享内存中的 ConnectionValidSharedMemory 函数
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  函数确定是否已安装并激活 SQL Server 共享内存。  
  
## <a name="syntax"></a>语法  
  
```cpp  
BOOL ConnectionValidSharedMemory(char * szServerName);  
```  
  
## <a name="parameters"></a>parameters  
 *szServerName*  
  
-   类型： **char \** _  
  
-   SQL server 的名称。  
  
## <a name="return-value"></a>返回值  
 类型： _ *BOOL**  
  
 如果无效，则返回 0;否则返回非零值。  
  
  
