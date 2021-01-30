---
description: SQLRemoveTranslator 函数
title: SQLRemoveTranslator 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveTranslator
helpviewer_keywords:
- SQLRemoveTranslator function [ODBC]
ms.assetid: c6feda49-0359-4224-8de9-77125cf2397b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cc8d1c7340d548276f462512bf2b52a3deb37b79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192478"
---
# <a name="sqlremovetranslator-function"></a>SQLRemoveTranslator 函数
**度**  
 引入的版本： ODBC 3。0  
  
 **摘要**  
 **SQLRemoveTranslator** 从系统信息的 Odbcinst.ini 部分删除有关转换器的信息，并将转换器的组件使用计数递减1。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLRemoveTranslator(  
     LPCSTR    lpszTranslator,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>参数  
 *lpszTranslator*  
 送在系统信息的 Odbcinst.ini 密钥中注册的转换器的名称。  
  
 *lpdwUsageCount*  
 输出调用此函数后转换器的使用计数。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。 如果调用此函数时，系统信息中不存在任何条目，则该函数将返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLRemoveTranslator** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|在注册表中找不到组件|安装程序无法删除转换器信息，因为它不存在于注册表中或在注册表中找不到。|  
|ODBC_ERROR_INVALID_NAME|无效的驱动程序或转换器名称|*LpszTranslator* 参数无效。|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|无法递增或递减组件使用率计数|安装程序无法减少驱动程序的使用计数。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 **SQLRemoveTranslator** 补充了 [SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md) 函数并更新了系统信息中的组件使用计数。 只应从安装应用程序调用此函数。  
  
 **SQLRemoveTranslator** 会将组件使用计数递减1。 如果组件使用率计数为0，系统信息中的转换器项将被删除。 转换器条目在系统信息中的以下位置的 "转换器名称" 下：  
  
 `HKEY_LOCAL_MACHINE`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbcinst.ini`  
  
 **SQLRemoveTranslator** 不会实际删除任何文件。 调用程序负责删除文件，并维护文件使用计数。 仅在组件使用计数和文件使用计数达到零时，才会实际删除文件。 可以删除组件中的某些文件，而不会删除其他文件，这取决于文件是否由其他应用程序使用，这些应用程序已增加了文件使用次数。  
  
 **SQLRemoveTranslator** 也作为升级过程的一部分被调用。 如果应用程序检测到它必须执行升级，并且它以前安装了驱动程序，则应删除该驱动程序，然后重新安装。 应该首先调用 **SQLRemoveTranslator** 以减少组件使用计数，然后调用 **SQLInstallTranslatorEx** 来递增组件使用计数。 应用程序安装程序必须以物理方式将旧文件替换为新文件。 文件使用计数将保持不变，并且使用较旧版本文件的其他应用程序现在将使用较新的版本。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|安装转换器|[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|
