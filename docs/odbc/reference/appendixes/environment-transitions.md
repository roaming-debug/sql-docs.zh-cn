---
description: 环境转换
title: 环境转换 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- environment transitions [ODBC]
- transitioning states [ODBC], environment
- state transitions [ODBC], environment
ms.assetid: 9d11b1ab-f4c8-48ca-9812-8c04303f939d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: daef0bc276831358cd3d82dd10a1dd44082214af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194836"
---
# <a name="environment-transitions"></a>环境转换
ODBC 环境具有以下三种状态。  
  
|状态|说明|  
|-----------|-----------------|  
|步进|未分配环境|  
|E1|已分配环境，未分配连接|  
|E2|已分配环境，已分配连接|  
  
 下表显示了每个 ODBC 函数如何影响环境状态。  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|步进<br /><br /> 未分配|E1<br /><br /> 已分配|E2<br /><br /> 连接|  
|------------------------|----------------------|-----------------------|  
|E1 [1]|--[4]|--[4]|  
| (IH) [2]|E2 [5]<br /> (HY010) [6]|--[4]|  
| (IH) [3]| (IH) |--[4]|  
  
 [1] 此行显示 SQL_HANDLE_ENV *HandleType* 时的转换。  
  
 [2] 在 SQL_HANDLE_DBC *HandleType* 时，该行显示转换。  
  
 [3] 当 *HandleType* 为 SQL_HANDLE_STMT 或 SQL_HANDLE_DESC 时，该行显示转换。  
  
 [4] 调用 **SQLAllocHandle** 时，如果 *OutputHandlePtr* 指向有效的句柄，则会覆盖该句柄。 这可能是应用程序编程错误。  
  
 [5] 已在环境中设置 SQL_ATTR_ODBC_VERSION 环境属性。  
  
 [6] 未在环境中设置 SQL_ATTR_ODBC_VERSION 环境属性。  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources 和 SQLDrivers  
  
|步进<br /><br /> 未分配|E1<br /><br /> 已分配|E2<br /><br /> 连接|  
|------------------------|----------------------|-----------------------|  
| (IH) |--[1]<br /> (HY010) [2]|--[1]<br /> (HY010) [2]|  
  
 [1] 已在环境中设置 SQL_ATTR_ODBC_VERSION 环境属性。  
  
 [2] 未在环境中设置 SQL_ATTR_ODBC_VERSION 环境属性。  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|步进<br /><br /> 未分配|E1<br /><br /> 已分配|E2<br /><br /> 连接|  
|------------------------|----------------------|-----------------------|  
| (IH) [1]|--[3]<br /> (HY010) [4]|--[3]<br /> (HY010) [4]|  
| (IH) [2]| (IH) |--|  
  
 [1] 此行显示 SQL_HANDLE_ENV *HandleType* 时的转换。  
  
 [2] 在 SQL_HANDLE_DBC *HandleType* 时，该行显示转换。  
  
 [3] 已在环境中设置 SQL_ATTR_ODBC_VERSION 环境属性。  
  
 [4] 未在环境中设置 SQL_ATTR_ODBC_VERSION 环境属性。  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|步进<br /><br /> 未分配|E1<br /><br /> 已分配|E2<br /><br /> 连接|  
|------------------------|----------------------|-----------------------|  
| (IH) [1]|步进| (HY010) |  
| (IH) [2]| (IH) |--[4]<br />E1 [5]|  
| (IH) [3]| (IH) |--|  
  
 [1] 此行显示 SQL_HANDLE_ENV *HandleType* 时的转换。  
  
 [2] 在 SQL_HANDLE_DBC *HandleType* 时，该行显示转换。  
  
 [3] 当 *HandleType* 为 SQL_HANDLE_STMT 或 SQL_HANDLE_DESC 时，该行显示转换。  
  
 [4] 有其他已分配的连接句柄。  
  
 [5] *句* 柄中指定的连接句柄是唯一分配的连接句柄。  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField 和 SQLGetDiagRec  
  
|步进<br /><br /> 未分配|E1<br /><br /> 已分配|E2<br /><br /> 连接|  
|------------------------|----------------------|-----------------------|  
| (IH) [1]|--|--|  
| (IH) [2]| (IH) |--|  
  
 [1] 此行显示 SQL_HANDLE_ENV *HandleType* 时的转换。  
  
 [2] 当 *HandleType* 为 SQL_HANDLE_DBC、SQL_HANDLE_STMT 或 SQL_HANDLE_DESC 时，该行显示转换。  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|步进<br /><br /> 未分配|E1<br /><br /> 已分配|E2<br /><br /> 连接|  
|------------------------|----------------------|-----------------------|  
| (IH) |--[1]<br /> (HY010) [2]|--|  
  
 [1] 已在环境中设置 SQL_ATTR_ODBC_VERSION 环境属性。  
  
 [2] 未在环境中设置 SQL_ATTR_ODBC_VERSION 环境属性。  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|步进<br /><br /> 未分配|E1<br /><br /> 已分配|E2<br /><br /> 连接|  
|------------------------|----------------------|-----------------------|  
| (IH) |--[1]<br /> (HY010) [2]| (HY011) |  
  
 [1] 已在环境中设置 SQL_ATTR_ODBC_VERSION 环境属性。  
  
 [2] 找不到 *属性* 参数 SQL_ATTR_ODBC_VERSION，环境中尚未设置 SQL_ATTR_ODBC_VERSION 环境属性。  
  
## <a name="all-other-odbc-functions"></a>所有其他 ODBC 函数  
  
|步进<br /><br /> 未分配|E1<br /><br /> 已分配|E2<br /><br /> 连接|  
|------------------------|----------------------|-----------------------|  
| (IH) | (IH) |--|
