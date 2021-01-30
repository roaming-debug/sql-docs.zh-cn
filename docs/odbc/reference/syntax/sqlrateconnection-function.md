---
description: SQLRateConnection 函数
title: SQLRateConnection 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLRateConnection function [ODBC]
ms.assetid: e8da2ffb-d6ef-4ca7-824f-57afd29585d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 40aec3459e43d2684bd80f0ced55171314551d92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192552"
---
# <a name="sqlrateconnection-function"></a>SQLRateConnection 函数
**度**  
 引入的版本： ODBC 3.81 标准符合性： ODBC  
  
 **摘要**  
 **SQLRateConnection** 确定驱动程序是否可以重复使用连接池中的现有连接。  
  
## <a name="syntax"></a>语法  
  
```cpp
  
SQLRETURN  SQLRateConnection(  
                SQLHDBC_INFO_TOKEN   hRequest,  
                SQLHDBC              hCandidateConnection,  
                BOOL                 fRequiredTransactionEnlistment,  
                TRANSID              transId,  
                DWORD *              pRating );  
```  
  
## <a name="arguments"></a>参数  
 *hRequest*  
 送表示新应用程序连接请求的标记句柄。  
  
 *hCandidateConnection*  
 送连接池中的现有连接。 连接必须处于打开状态。  
  
 *fRequiredTransactionEnlistment*  
 送如果 *为 TRUE，则为新* 的连接请求重复使用现有连接 (*hRequest*) 需要额外的登记。  
  
 *transId*  
 送如果 *fRequiredTransactionEnlistment* 为 TRUE，则 *transId* 表示请求将登记的 DTC 事务。 如果 *fRequiredTransactionEnlistment* 为 FALSE，则将忽略 *transId* 。  
  
 *pRating*  
 输出 *hCandidateConnection* 对 *hRequest* 的重复使用评级。 此级别将介于0到100之间 (包含) 之间。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 驱动程序管理器不会处理从此函数返回的诊断信息。  
  
## <a name="remarks"></a>备注  
 **SQLRateConnection** 生成0到100之间的分数 (包含) 指示现有连接与请求的匹配程度。  
  
|分数|返回 SQL_SUCCESS 时的意义 () |  
|-----------|-----------------------------------------------|  
|0|*hRequest* 不能重复使用 *hCandidateConnection* 。|  
|1到98之间的任何值 (包含) |分数越高， *hCandidateConnection* 与 *hRequest* 匹配越接近。|  
|99|只有无意义的属性中存在不匹配的情况。  驱动程序管理器应停止评级循环。|  
|100|完全匹配。  驱动程序管理器应停止评级循环。|  
|任何其他大于100的值|*hCandidateConnection* 被标记为 "死"，即使在未来的连接请求中也不会重复使用。|  
  
 如果返回代码不是 SQL_SUCCESS (包括 SQL_SUCCESS_WITH_INFO) 或分级大于100，驱动程序管理器会将连接标记为 "死"。 即使在未来的连接请求) ，也不会重复使用该死连接 (，并最终会在 CPTimeout 通过后超时。 驱动程序管理器将继续从池中找到另一个连接以进行评级。  
  
 如果驱动程序管理器重复使用某个连接，其分数严格小于 100 (包括 99) ，则驱动程序管理器将调用 SQLSetConnectAttr (SQL_ATTR_DBC_INFO_TOKEN) ，以将该连接重置回应用程序所请求的状态。 在此函数调用中，驱动程序不应重置连接。  
  
 如果 *fRequiredTransactionEnlistment* 为 TRUE，则重复使用 *hCandidateConnection* 需要额外的登记 (*transId* ！ = null) 或 unenlistment (*transId* = = null) 。 这表示重新使用连接的成本，以及驱动程序是否应在该连接再次使用连接的情况下登记或取消登记连接。 如果 *fRequireTransactionEnlistment* 为 FALSE，则驱动程序应忽略 *transId* 的值。  
  
 驱动程序管理器保证 *hRequest* 和 *HCANDIDATECONNECTION* 的父 HENV 句柄相同。 驱动程序管理器保证与 *hRequest* 和 *hCandidateConnection* 相关联的池 ID 是相同的。  
  
 应用程序不应直接调用此函数。 支持驱动程序感知连接池的 ODBC 驱动程序必须实现此功能。  
  
 包括用于 ODBC 驱动程序开发的 sqlspi。  
  
## <a name="see-also"></a>另请参阅  
 [开发 ODBC 驱动程序](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [识别驱动程序的连接池](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [在 ODBC 驱动程序中开发连接池感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
