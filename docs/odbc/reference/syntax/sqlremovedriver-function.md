---
description: SQLRemoveDriver 函数
title: SQLRemoveDriver 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDriver
helpviewer_keywords:
- SQLRemoveDriver function [ODBC]
ms.assetid: 9a3b4f8b-982b-44b9-ade6-754ff026dc90
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ee0165f815b874844f072c36325fb7afb8046c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192509"
---
# <a name="sqlremovedriver-function"></a>SQLRemoveDriver 函数
**度**  
 引入的版本： ODBC 3。0  
  
 **摘要**  
 **SQLRemoveDriver** 从系统信息的 Odbcinst.ini 条目中更改或删除有关驱动程序的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLRemoveDriver(  
     LPCSTR   lpszDriver,  
     BOOL     fRemoveDSN,  
     LPDWORD  lpdwUsageCount);  
```  
  
## <a name="arguments"></a>参数  
 *lpszDriver*  
 送在系统信息的 Odbcinst.ini 密钥中注册的驱动程序的名称。  
  
 *fRemoveDSN*  
 送有效值为：  
  
 TRUE：删除与在 *lpszDriver* 中指定的驱动程序相关联的 dsn。 FALSE：不要删除与 *lpszDriver* 中指定的驱动程序相关联的 dsn。  
  
 *lpdwUsageCount*  
 输出调用此函数后驱动程序的使用计数。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。 如果调用此函数时，系统信息中不存在任何条目，则该函数将返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLRemoveDriver** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|在注册表中找不到组件|安装程序无法删除驱动程序信息，因为它不存在于注册表中或在注册表中找不到。|  
|ODBC_ERROR_INVALID_NAME|无效的驱动程序或转换器名称|*LpszDriver* 参数无效。|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|无法递增或递减组件使用率计数|安装程序无法减少驱动程序的使用计数。|  
|ODBC_ERROR_REQUEST_FAILED|申请失败。|*FRemoveDSN* 参数为 TRUE;但是，无法删除一个或多个 Dsn。 与 ODBC_REMOVE_DRIVER 请求的 **SQLConfigDriver** 调用失败。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 **SQLRemoveDriver** 补充了 [SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md) 函数并更新了系统信息中的组件使用计数。 只应从安装应用程序调用此函数。  
  
 **SQLRemoveDriver** 会将组件使用率计数值减少1。 如果组件使用率计数为0，则将发生以下情况：  
  
1.  将调用带有 ODBC_REMOVE_DRIVER 选项的 **SQLConfigDriver** 函数。 如果将 *fRemoveDSN* 选项设置为 TRUE，则 **ConfigDSN** 函数将调用 **SQLRemoveDSNFromIni** 以删除与 lpszDriver 中指定的驱动程序相关联的所有数据源 *。* 如果将 *fRemoveDSN* 选项设置为 FALSE，则不会删除数据源。  
  
2.  系统信息中的驱动程序项将被删除。 驱动程序条目位于以下系统信息位置，位于驱动程序名称下面：  
  
     `HKEY_LOCAL_MACHINE`  
  
     `SOFTWARE`  
  
     `ODBC`  
  
     `Odbcinst.ini`  
  
 **SQLRemoveDriver** 不会实际删除任何文件。 调用程序负责删除文件并维护文件使用计数。 仅在组件使用计数和文件使用计数达到零时，才会实际删除文件。 可以删除组件中的某些文件，而不会删除其他文件，这取决于文件是否由其他应用程序使用，这些应用程序已增加了文件使用次数。  
  
 **SQLRemoveDriver** 也作为升级过程的一部分被调用。 如果应用程序检测到它必须执行升级，并且它以前安装了驱动程序，则应删除该驱动程序，然后重新安装。 应该首先调用 **SQLRemoveDriver** 以减少组件使用计数，然后调用 **SQLInstallDriverEx** 来递增组件使用计数。 应用程序安装程序必须将旧文件替换为新文件。 文件使用计数将保持不变，并且使用较旧版本文件的其他应用程序现在将使用较新的版本。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|添加、修改或删除驱动程序|安装程序 DLL 中的[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md) () |  
|添加、修改或删除驱动程序|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|  
|安装驱动程序|[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|
