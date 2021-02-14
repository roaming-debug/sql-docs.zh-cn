---
description: 驱动程序管理器连接池
title: 驱动程序管理器连接池 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connection pooling [ODBC]
- pooled connections [ODBC]
- connecting to driver [ODBC], connection pooling
- connecting to data source [ODBC], connection pooling
ms.assetid: ee95ffdb-5aa1-49a3-beb2-7695b27c3df9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8876696412360275fcf443a5543634c9ac537df6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070332"
---
# <a name="driver-manager-connection-pooling"></a>驱动程序管理器连接池
使用连接池，应用程序可以使用连接池的连接，无需在每次使用时重新建立连接。 创建连接并将其放置到池中后，应用程序可以重新使用该连接，而无需执行完整的连接过程。  
  
 使用共用连接可以显著提高性能，因为应用程序可以节省建立连接所需的系统开销。 对于通过网络连接的中间层应用程序或重复连接和断开连接的应用程序（如 Internet 应用程序），这可能特别重要。  
  
 除了提高性能之外，连接池体系结构还允许单个进程中的多个组件使用某个环境及其关联的连接。 这意味着，同一进程中的独立组件可以彼此交互，而无需彼此了解。 连接池中的连接可以由多个组件重复使用。  
  
> [!NOTE]
>  ODBC 应用程序可以使用连接池。*x* 行为，前提是应用程序可以调用 *SQLSetEnvAttr*。 使用连接池时，应用程序不能执行更改数据库或数据库上下文的 SQL 语句，例如更改 \<*database name*> ，这将更改数据源所使用的目录。  


 ODBC 驱动程序必须完全是线程安全的，并且连接不能具有线程关联以支持连接池。 这意味着，驱动程序可以随时处理对任何线程的调用，并且能够连接到一个线程，在另一个线程上使用连接，并在第三个线程上断开连接。  
  
 连接池由驱动程序管理器维护。 当应用程序调用 **SQLConnect** 或 **SQLDriverConnect** 时，将从池中提取连接，并在应用程序调用 **SQLDisconnect** 时返回到池中。 池的大小根据请求的资源分配动态增长。 它基于非活动超时进行收缩：如果连接在一段时间内处于非活动状态 (未在连接) 中使用该连接，则会将其从池中删除。 池的大小仅受服务器上的内存限制和限制的限制。  
  
 驱动程序管理器根据在 **SQLConnect** 或 **SQLDriverConnect** 中传递的参数以及在分配连接后设置的连接属性，确定是否应使用池中的特定连接。  
  
 当驱动程序管理器建立池连接时，它需要能够确定连接是否仍在运行，然后再处理连接。 否则，每当发生暂时性网络故障时，驱动程序管理器就会继续处理到应用程序的连接。 ODBC 2.x 中定义了一个新的连接属性 *： SQL_ATTR_CONNECTION_DEAD*。 这是一个只读的连接属性，它返回 SQL_CD_TRUE 或 SQL_CD_FALSE。 值 SQL_CD_TRUE 表示连接已丢失，而值 SQL_CD_FALSE 表示连接仍处于活动状态。 与早期版本的 ODBC 相容 (驱动程序还可以支持此属性。 )   
  
 驱动程序必须有效地实现此选项，否则它会削弱连接池的性能。 具体而言，调用 get 此连接属性不会导致与服务器之间的往返。 相反，驱动程序应该只返回最近已知的连接状态。 如果最后一次到服务器的行程失败，连接将会失效，如果上次行程成功，则不会失效。  
  
## <a name="remarks"></a>备注  
 如果 (通过 SQL_ATTR_CONNECTION_DEAD) 报告的连接丢失，ODBC 驱动程序管理器会通过在驱动程序中调用 SQLDisconnect 来销毁该连接。 新的连接请求可能在池中找不到可用的连接。 最终，驱动程序管理器可能会建立新的连接，假设池为空。  
  
 若要使用连接池，应用程序需要执行以下步骤：  
  
1.  通过调用 **SQLSetEnvAttr** 来启用连接池，以将 SQL_ATTR_CONNECTION_POOLING 环境特性设置为 SQL_CP_ONE_PER_DRIVER 或 SQL_CP_ONE_PER_HENV。 在应用程序分配要为其启用连接池的共享环境之前，必须执行此调用。 应将对 **SQLSetEnvAttr** 的调用中的环境句柄设置为 null，这使得 SQL_ATTR_CONNECTION_POOLING 进程级特性。 如果该属性设置为 SQL_CP_ONE_PER_DRIVER，则每个驱动程序都支持单个连接池。 如果某个应用程序使用多个驱动程序和很少的环境，这可能会更有效，因为可能需要较少的比较。 如果设置为 SQL_CP_ONE_PER_HENV，则每个环境都支持单个连接池。 如果应用程序适用于许多环境和很少的驱动程序，这可能会更有效，因为可能需要较少的比较。 通过将 SQL_ATTR_CONNECTION_POOLING 设置为 SQL_CP_OFF 来禁用连接池。  
  
2.  通过调用 **SQLAllocHandle** 并将 *HandleType* 参数设置为 SQL_HANDLE_ENV 来分配环境。 由于已启用连接池，因此此调用分配的环境将为隐式共享环境。 但不确定要使用的环境，而是在此环境中调用具有 SQL_HANDLE_DBC 的 *HandleType* 的 **SQLAllocHandle** 。  
  
3.  通过调用 **SQLAllocHandle** 并将 *将 inputhandle* 设置为 SQL_HANDLE_DBC 来分配连接，并将 *将 inputhandle* 设置为为连接池分配的环境句柄。 驱动程序管理器将尝试查找与应用程序设置的环境特性相匹配的现有环境。 如果不存在这样的环境，则将创建一个，并在驱动程序管理器) 为 1 (维护引用计数。 如果找到匹配的共享环境，则将环境返回到应用程序，并递增其引用计数。  (在调用 **SQLConnect** 或 **SQLDriverConnect** 之前，驱动程序管理器不会确定要使用的实际连接。 )   
  
4.  调用 **SQLConnect** 或 **SQLDriverConnect** 建立连接。 驱动程序管理器使用对 **SQLConnect** 的调用中的连接选项 (或) **SQLDriverConnect** 调用中的连接关键字，在连接分配后设置连接属性，以确定应使用池中的连接。  
  
    > [!NOTE]  
    >  请求的连接与池连接的匹配方式由 SQL_ATTR_CP_MATCH 环境属性确定。 有关详细信息，请参阅 [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)。  
  
     使用连接池的 ODBC 应用程序应在应用程序初始化期间调用 [CoInitializeEx](/windows/win32/api/combaseapi/nf-combaseapi-coinitializeex) ，并在应用程序关闭时调用 [CoUninitialize](/windows/win32/api/combaseapi/nf-combaseapi-couninitialize) 。  
  
5.  完成连接后，调用 **SQLDisconnect** 。 连接将返回到连接池，并可供重复使用。  
  
 有关详细讨论，请参阅 [Microsoft 数据访问组件中的池](/previous-versions/ms810829(v=msdn.10))。  
  
## <a name="connection-pooling-considerations"></a>连接池注意事项  
 使用 SQL 命令 (而不是通过 ODBC API) 执行以下任何操作可能会影响连接的状态，并在连接池处于活动状态时导致意外问题：  
  
-   打开连接并更改默认数据库。  
  
-   使用 SET 语句更改任何可配置的选项 (包括设置行计数、ANSI_NULL、IMPLICIT_TRANSACTIONS、显示计划、统计信息、TEXTSIZE 和 DATEFORMAT) 。  
  
-   创建临时表和存储过程。  
  
 如果这些操作中的任何一个在 ODBC API 之外执行，则使用该连接的下一个用户将自动继承以前的设置、表或过程。  
  
> [!NOTE]  
>  不希望某些设置出现在连接状态中。 应始终在应用程序中设置连接状态，并确保应用程序删除任何未使用的连接池设置。  
  
## <a name="driver-aware-connection-pooling"></a>识别驱动程序的连接池  
 从 Windows 8 开始，ODBC 驱动程序可以更有效地使用池中的连接。 有关详细信息，请参阅 [识别驱动程序的连接池](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)。  
  
## <a name="see-also"></a>另请参阅  
 [连接到数据源或驱动程序](../../../odbc/reference/develop-app/connecting-to-a-data-source-or-driver.md)   
 [开发 ODBC 驱动程序](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Microsoft 数据访问组件中的池](/previous-versions/ms810829(v=msdn.10))