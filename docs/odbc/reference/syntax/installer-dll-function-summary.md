---
description: 安装程序 DLL 函数摘要
title: 安装程序 DLL 函数摘要 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], installer DLL functions
- installer DLL [ODBC]
ms.assetid: 666c09d3-1e10-4d89-9b42-eda2957a87f0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f5cce294142a716180b66544ebe12638e83f792e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174667"
---
# <a name="installer-dll-function-summary"></a>安装程序 DLL 函数摘要
下表介绍了安装程序 DLL 中的函数。 有关每个函数的语法和语义的详细信息，请参阅 [安装程序 DLL API 参考](../../../odbc/reference/syntax/installer-dll-api-reference-function.md)。  
  
|任务|函数名称|用途|  
|----------|-------------------|-------------|  
|安装 ODBC|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|加载特定于驱动程序的安装 DLL。|  
||[SQLGetInstalledDrivers](../../../odbc/reference/syntax/sqlgetinstalleddrivers-function.md)|返回已安装的驱动程序的列表。|  
||[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|将驱动程序添加到系统信息。|  
||[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|返回驱动程序管理器的目标目录。|  
||[SQLInstallerError](../../../odbc/reference/syntax/sqlinstallererror-function.md)|返回安装程序函数的错误或状态信息。|  
||[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|向系统信息添加转换器。|  
||[SQLPostInstallerError](../../../odbc/reference/syntax/sqlpostinstallererror-function.md)|允许驱动程序或转换器安装库报告错误。|  
||[SQLRemoveDriver](../../../odbc/reference/syntax/sqlremovedriver-function.md)|从系统信息中删除驱动程序。|  
||[SQLRemoveDriverManager](../../../odbc/reference/syntax/sqlremovedrivermanager-function.md)|从系统信息中删除 ODBC core 组件。|  
||[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|从系统信息中删除转换器。|  
|配置数据源|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|调用驱动程序特定的安装 DLL。|  
||[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|显示一个对话框，用于添加数据源。|  
||[SQLGetConfigMode](../../../odbc/reference/syntax/sqlgetconfigmode-function.md)|检索配置模式，该配置模式指示 Odbc.ini 项列出 DSN 值在系统信息中的位置。|  
||[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|将一个值写入系统信息。|  
||[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|显示一个对话框，用于选择转换器。|  
||[SQLManageDataSources](../../../odbc/reference/syntax/sqlmanagedatasources.md)|显示一个对话框，用于配置数据源和驱动程序。|  
||[SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md)|读取文件 Dsn 中的信息。|  
||[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|删除默认数据源。|  
||[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|删除数据源。|  
||[SQLSetConfigMode](../../../odbc/reference/syntax/sqlsetconfigmode-function.md)|设置指示 Odbc.ini 条目列出 DSN 值的位置在系统信息中的配置模式。|  
||[SQLValidDSN](../../../odbc/reference/syntax/sqlvaliddsn-function.md)|检查数据源名称的长度和有效性。|  
||[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|添加数据源。|  
||[SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md)|将信息写入文件 Dsn。|  
||[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|获取系统信息中的值。|
