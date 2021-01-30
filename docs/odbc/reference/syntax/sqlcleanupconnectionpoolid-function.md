---
description: SQLCleanupConnectionPoolID 函数
title: SQLCleanupConnectionPoolID 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLCleanupConnectionPoolID function [ODBC]
ms.assetid: 1fc61908-e003-4587-b91a-32f40569fb99
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ce2a6dc5a0a368312a46cb038cb56407f977b2ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181018"
---
# <a name="sqlcleanupconnectionpoolid-function"></a>SQLCleanupConnectionPoolID 函数
**度**  
 引入的版本： ODBC 3.81 标准符合性： ODBC  
  
 **摘要**  
 **SQLCleanupConnectionPoolID** 通知驱动程序池 ID 已超时。如果与该池 ID 相关联的池中的所有连接超时，则池 ID 可能会超时。有关连接超时的详细信息，请参阅 [Microsoft 数据访问组件中的池](/previous-versions/ms810829(v=msdn.10)) 。  
  
## <a name="syntax"></a>语法  
  
```cpp
  
SQLRETURN  SQLCleanupConnectionPoolID (  
                SQLHENV    EnvironmentHandle  
                SQLPOOLID  PoolID );  
```  
  
## <a name="arguments"></a>参数  
 *EnvironmentHandle*  
 送池的环境句柄。  
  
 *PoolID*  
 送与已超时的池 ID 相关联的池。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 驱动程序管理器将不处理从 **SQLCleanupConnectionPoolID** 返回的诊断信息。  
  
 应用程序无法接收驱动程序返回的错误消息。  
  
## <a name="remarks"></a>备注  
 可以随时调用 **SQLCleanupConnectionPoolID** ，但驱动程序管理器保证没有其他线程同时调用 **SQLGetPoolID** ，并且没有其他线程同时调用 **SQLRateConnection** 和 **SQLPOOLCONNECT** ，并使用使用该池 ID 分配的连接信息令牌。 因此，驱动程序必须确保此函数是线程安全的。  
  
 驱动程序可以清理与池 ID 关联的资源。  
  
 应用程序不应直接调用此函数。 支持驱动程序感知连接池的 ODBC 驱动程序必须实现此功能。  
  
 包括用于 ODBC 驱动程序开发的 sqlspi。  
  
## <a name="see-also"></a>另请参阅  
 [开发 ODBC 驱动程序](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [识别驱动程序的连接池](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [在 ODBC 驱动程序中开发连接池感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)