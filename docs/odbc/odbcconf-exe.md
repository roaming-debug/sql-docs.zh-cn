---
description: ODBCCONF.EXE
title: ODBCCONF.EXE |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- odbcconf.exe
ms.assetid: 3bf2be83-61f9-4183-836b-85204ac7116a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e62fec06a10316bb3b773cf599f11c23ba376cfd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075043"
---
# <a name="odbcconfexe"></a>ODBCCONF.EXE
ODBCCONF.exe 是一种命令行工具，可用于配置 ODBC 驱动程序和数据源的名称。  
  
> [!NOTE]  
>  Windows 数据访问组件的未来版本中将删除 ODBCCONF.exe。 避免使用此功能，并计划修改当前使用此功能的应用程序。 可以使用 PowerShell 命令来管理驱动程序和数据源。 有关这些 PowerShell 命令的详细信息，请参阅 [Windows 数据访问组件 cmdlet](/powershell/module/wdac)。  
  
## <a name="syntax"></a>语法  
  
```console  
ODBCCONF [switches] action  
```  
  
## <a name="arguments"></a>参数  
 *交换器*  
 零个或多个开关选项。 有关可用开关的列表，请参阅本主题后面的 "备注" 部分。  
  
 *action*  
 要执行的一项操作。 有关可用选项的列表，请参阅 "备注" 部分。  
  
## <a name="remarks"></a>备注  
 以下开关可用：  
  
|开关|说明|  
|------------|-----------------|  
|/A {*操作*}|指定操作。<br /><br /> 如果只指定了一个操作，则/a 是可选的。|  
|/?|显示 ODBCCONF.EXE 的使用情况。|  
|/C|如果操作失败，处理将继续进行。|  
|/E|处理完成后，清除用/F 指定的响应文件。|  
|/F|使用响应文件，如 `odbcconf /F my.rsp` 。<br /><br /> rsp 可能如下所示： `REGSVR c:\my.dll`<br /><br /> /A 不用于响应文件。|  
|/H|显示用法 (帮助) 。 此开关与/？相同。|  
|/L [*mode*] *filename*|以三种模式之一将程序输出发送到文件：正常 (n) 、verbose (v) ，并调试 (d) 。 调试模式记录 odbcconf.exe 加载的 Dll。<br /><br /> 如果在不使用模式的情况下指定/L，则日志文件将为空。<br /><br /> 例如， **/Lv log.txt**。|  
|/R|操作将在重新启动后执行。|  
|/S|静默模式。 不显示错误消息。|  
  
 提供了以下选项：  
  
|操作|说明|  
|------------|-----------------|  
|CONFIGDRIVER *driver_name * * 驱动程序特定的配置参数*|加载相应的驱动程序安装程序 DLL 并调用 **ConfigDriver** 函数。<br /><br /> 等效于 [SQLConfigDriver 函数](../odbc/reference/syntax/sqlconfigdriver-function.md)。<br /><br /> 例如：<br /><br /> /A {CONFIGDRIVER "Driver Name" "CPTimeout = 60"}<br /><br /> /A {CONFIGDRIVER "Driver Name" "DriverODBCVer = 03.80"}|  
|CONFIGDSN *driver_name* DSN =*name* &#124; *属性*|添加或修改系统数据源。<br /><br /> 等效于 [SQLConfigDataSource 函数](../odbc/reference/syntax/sqlconfigdatasource-function.md)。<br /><br /> 例如：<br /><br /> /A {CONFIGDSN "SQL Server" "DSN = name &#124; Server = srv"}|  
|CONFIGSYSDSN *driver_name* DSN =*name* &#124; *属性*|添加或修改系统数据源。<br /><br /> 等效于 [SQLConfigDataSource 函数](../odbc/reference/syntax/sqlconfigdatasource-function.md)。<br /><br /> 例如：<br /><br /> /A {CONFIGSYSDSN "SQL Server" "DSN = name &#124; Server = srv"}|  
|INSTALLDRIVER|等效于 [SQLInstallDriverEx 函数](../odbc/reference/syntax/sqlinstalldriverex-function.md)。<br /><br /> 有关传递到 INSTALLDRIVER 的关键字值对语法的信息，请参阅 [驱动程序规范子项](../odbc/reference/install/driver-specification-subkeys.md)。<br /><br /> 例如：<br /><br /> /A {INSTALLDRIVER "Driver &#124; Driver =c:\your.dll &#124; Setup =c:\your.dll &#124; APILevel = 2 &#124; ConnectFunctions = YYY &#124; DriverODBCVer = 03.50 &#124; FileUsage = 0 &#124; SQLLevel = 1"}|  
|INSTALLTRANSLATOR *转换器配置 * * 驱动程序路径*|将有关转换器的信息添加到 **HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI \Odbc 转换器** 注册表项。<br /><br /> 等效于 [SQLInstallTranslatorEx 函数](../odbc/reference/syntax/sqlinstalltranslatorex-function.md)。<br /><br /> 有关传递到 INSTALLDRIVER 的关键字/值对语法的信息，请参阅 [转换器规范子项](../odbc/reference/install/translator-specification-subkeys.md)。<br /><br /> 例如：<br /><br /> /A {INSTALLTRANSLATOR "我的翻译 &#124; Translator =c:\my.dll &#124; 安装程序 =c:\my.dll"}|  
|REGSVR *dll*|注册 DLL。<br /><br /> 等效于 regsvr32.exe。<br /><br /> 例如：<br /><br /> /A {REGSVR c:\my.dll}|  
|SETFILEDSNDIR|如果不存在 HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC.INI \ODBC 文件 DSN\DefaultDSNDir，则 SETFILEDSNDIR 操作会创建该文件，并为其分配 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\CommonFilesDir 中追加了 \ODBC\Data 源的值。<br /><br /> 在创建基于文件的数据源时，HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC.INI \ODBC 文件 DSN\DefaultDSNDir 的值指定 ODBC 数据源管理器使用的默认位置。<br /><br /> 例如：<br /><br /> /A {SETFILEDSNDIR}|  
  
## <a name="see-also"></a>另请参阅  
 [Microsoft 开放式数据库连接 (ODBC)](../odbc/microsoft-open-database-connectivity-odbc.md)
