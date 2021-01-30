---
description: SQLRemoveDriverManager 函数
title: SQLRemoveDriverManager 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDriverManager
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDriverManager
helpviewer_keywords:
- SQLRemoveDriverManager function function [ODBC]
ms.assetid: 3a41511f-6603-4b81-a815-7883874023c4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 92359b7df67535f8bee5a5a3dfb4b7a7348f54f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192499"
---
# <a name="sqlremovedrivermanager-function"></a>SQLRemoveDriverManager 函数
**度**  
 引入的版本： ODBC 3.0：在 Windows XP Service Pack 2、Windows Server 2003 Service Pack 1 和更高版本的操作系统中已弃用。  
  
 **摘要**  
 **SQLRemoveDriverManager** 从系统信息中的 Odbcinst.ini 项更改或删除有关 ODBC core 组件的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLRemoveDriverManager(  
     LPDWORD     pdwUsageCount);  
```  
  
## <a name="arguments"></a>参数  
 *pdwUsageCount*  
 输出在调用此函数后，驱动程序管理器的使用计数。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。 如果调用此函数时，系统信息中不存在任何条目，则该函数将返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLRemoveDriverManager** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|在注册表中找不到组件|安装程序无法删除驱动程序管理器信息，因为它不存在于注册表中或在注册表中找不到。|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|无法递增或递减组件使用率计数|安装程序无法减少驱动程序管理器的使用计数。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 **SQLRemoveDriverManager** 补充了 **SQLInstallDriverManager** 函数，并更新了系统信息中的组件使用计数。 只应从安装应用程序调用此函数。  
  
 **SQLRemoveDriverManager** 会将核心组件使用量计数减少1。 如果组件使用率计数为0，则将删除输入系统信息。 核心组件条目在系统信息中的以下位置中的 "ODBC Core" 标题下：  
  
 `HKEY_LOCAL_MACHINE`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbcinst.ini`  
  
> [!CAUTION]  
>  当组件使用计数和文件使用计数都为零时，应用程序不应物理删除驱动程序管理器文件。  
  
 **SQLRemoveDriverManager** 不会实际删除任何文件。 调用程序负责删除文件并维护文件使用计数。 但是，当组件用量计数和文件使用计数都达到零时，不应删除驱动程序管理器文件，因为这些文件可能会被其他尚未增加文件使用计数的应用程序使用。  
  
 在卸载过程中，将调用 **SQLRemoveDriverManager** 。 ODBC core 组件 (包括驱动程序管理器、游标库、安装程序、语言库、管理员、thunk 文件等) 作为一个整体进行卸载。 在卸载过程中调用 **SQLRemoveDriverManager** 时，不会删除以下文件：  

:::row:::
    :::column:::
        ODBC32DLL  
        ODBCCR32.DLL  
        ODBCCU32.DLL  
        ODBCINT.DLL  
        ODBCTRAC.DLL  
        MSVCRT40.DLL  
        ODBCCP32.CPL  
    :::column-end:::
    :::column:::
        ODBCCP32.DLL  
        ODBC16GT.DLL  
        ODBC32GT.DLL  
        DS16GT.DLL  
        DS32GT.DLL  
        ODBCAD32.EXE  
    :::column-end:::
:::row-end:::

 **SQLRemoveDriverManager** 也作为升级过程的一部分被调用。 如果应用程序检测到它必须执行升级，并且它以前安装了驱动程序，则应删除该驱动程序，然后重新安装。  
  
 应该首先调用 **SQLRemoveDriverManager** 以减少组件使用计数。 然后，应调用 **SQLInstallDriverEx** 来递增组件使用计数。 应用程序安装程序必须用新文件替换旧的核心组件文件。 文件使用计数将保持不变，并且使用较旧版本核心组件文件的其他应用程序现在将使用较新版本的文件。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|安装驱动程序管理器|[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|
