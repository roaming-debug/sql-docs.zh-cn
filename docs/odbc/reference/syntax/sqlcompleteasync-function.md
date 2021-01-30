---
description: SQLCompleteAsync 函数
title: SQLCompleteAsync 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
f1_keywords:
- SQLCompleteAsync
helpviewer_keywords:
- SQLCompleteAsync function [ODBC]
ms.assetid: 1b97c46a-d2e5-4540-8239-9d975e5321c6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ad9a84bcc5f8ae716394925d4935c384d29794c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207106"
---
# <a name="sqlcompleteasync-function"></a>SQLCompleteAsync 函数
**度**  
 引入的版本： ODBC 3.8 标准符合性：无  
  
 **摘要**  
 **SQLCompleteAsync** 可用于通过基于通知或轮询的处理来确定异步函数的完成时间。 有关异步操作的详细信息，请参阅 [异步执行](../../../odbc/reference/develop-app/asynchronous-execution.md)。  
  
 **SQLCompleteAsync** 仅在 ODBC 驱动程序管理器中实现。  
  
 在基于通知的异步处理模式下，驱动程序管理器引发用于通知的事件对象之后，必须调用 **SQLCompleteAsync** 。 **SQLCompleteAsync** 完成异步处理，并且异步函数将生成一个返回代码。  
  
 在基于轮询的异步处理模式下， **SQLCompleteAsync** 是调用原始异步函数的替代方法，无需在原始异步函数调用中指定参数。 无论是否启用 ODBC 游标库，都可以使用 **SQLCompleteAsync** 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLCompleteAsync(  
      SQLSMALLINT HandleType,  
      SQLHANDLE   Handle,  
      RETCODE *   AsyncRetCodePtr);  
```  
  
## <a name="arguments"></a>参数  
 *HandleType*  
 送要完成异步处理的句柄的类型。 有效值为 SQL_HANDLE_DBC 或 SQL_HANDLE_STMT。  
  
 *Handle*  
 送要完成异步处理的句柄。 如果 *句柄* 不是 *HandleType* 指定的类型的有效句柄，则 **SQLCompleteAsync** 将返回 SQL_INVALID_HANDLE。  
  
 如果 *句柄* 不是 *HandleType* 指定的类型的有效句柄，则 **SQLCompleteAsync** 将返回 SQL_INVALID_HANDLE。  
  
 *AsyncRetCodePtr*  
 输出指向将包含异步 API 的返回代码的缓冲区的指针。 如果 *AsyncRetCodePtr* 为 NULL，则 **SQLCompleteAsync** 将返回 SQL_ERROR。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_ERROR、SQL_NO_DATA 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 如果 **SQLCompleteAsync** 返回 SQL_SUCCESS，应用程序应从 *AsyncRetCodePtr* 指向的缓冲区中获取异步函数的返回代码。 关联的 SQLSTATE （ **如果有）可以通过使用 SQL_HANDLE_STMT** 的 *HandleType* 、语句句柄或 *HandleType* SQL_HANDLE_DBC 和连接句柄来获得。 这些诊断记录与异步函数关联，而不是与此 **SQLCompleteAsync** 函数相关联。  
  
 **SQLCompleteAsync** 返回 SQL_SUCCESS 以外的代码，以指示未正确调用 **SQLCompleteAsync** 。 在这种情况下， **SQLCompleteAsync** 不会发布任何诊断记录。 可能的返回代码是：  
  
-   SQL_INVALID_HANDLE： *HandleType* 和 *句柄* 指示的句柄不是有效的句柄。  
  
-   SQL_ERROR： *AsyncRetCodePtr* 为 NULL 或句柄上未启用异步处理。  
  
-   SQL_NO_DATA：在通知模式下，异步操作未进行或驱动程序管理器未向应用程序发出通知。 在轮询模式下，异步操作未进行。  
  
## <a name="comments"></a>注释  
 在基于轮询的异步处理模式下，当 **SQLCompleteAsync** 返回 SQL_SUCCESS 时， *AsyncRetCodePtr* 可能 SQL_STILL_EXECUTING。 应用程序应始终进行轮询，直到不 SQL_STILL_EXECUTING *AsyncRetCodePtr* 。 在基于通知的异步处理模式下， *AsyncRetCodePtr* 将永远不会 SQL_STILL_EXECUTING。  
  
## <a name="see-also"></a>另请参阅  
 [异步执行（轮询方法）](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
