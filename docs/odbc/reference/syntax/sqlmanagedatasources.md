---
description: SQLManageDataSources
title: SQLManageDataSources |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLManageDataSources
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLManageDataSources
helpviewer_keywords:
- SQLManageDataSources [ODBC]
ms.assetid: ac6d186f-b394-406c-94c4-c6331d1ca468
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4fbb34d2daafc4a135cbf51c2b0382d3965f9dfc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160809"
---
# <a name="sqlmanagedatasources"></a>SQLManageDataSources
**度**  
 引入的版本： ODBC 2。0  
  
 **摘要**  
 **SQLManageDataSources** 显示一个对话框，用户可在其中设置、添加和删除系统信息中的数据源。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLManageDataSources(  
     HWND     hwnd);  
```  
  
## <a name="arguments"></a>参数  
 *hwnd*  
 送父窗口句柄。  
  
## <a name="returns"></a>返回  
 如果 *hwnd* 不是有效的窗口句柄，则 **SQLManageDataSources** 返回 FALSE。 否则，返回 TRUE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLManageDataSources** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_REQUEST_FAILED|*请求* 失败|对 **ConfigDSN** 的调用失败。|  
|ODBC_ERROR_INVALID__HWND|无效的窗口句柄|*Hwnd* 参数无效或为 NULL。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="managing-data-sources"></a>管理数据源  
 **SQLManageDataSources** 最初显示 " **ODBC 数据源管理器** " 对话框，如下图所示。  
  
 ![“ODBC 数据源管理器”对话框](../../../odbc/reference/syntax/media/ch23e.gif "CH23E")  
  
 此对话框将显示在以下三个选项卡下的 "系统信息" 中列出的数据源： " **用户 dsn**"、" **系统 dsn**" 和 " **文件 dsn**"。 如果用户双击数据源或选择数据源并单击 " **配置**"，则 **SQLManageDataSources** 将使用 ODBC_CONFIG_DSN 选项在安装程序 DLL 中调用 **ConfigDSN** 。  
  
 如果用户单击 " **添加**"，则 **SQLManageDataSources** 将显示 " **创建新数据源** " 对话框，如下图所示。  
  
 ![“创建新数据源”对话框](../../../odbc/reference/syntax/media/ch23f.gif "CH23F")  
  
 此对话框显示已安装驱动程序的列表。 如果用户双击驱动程序或选择驱动程序，然后单击 **"确定"**，则 **SQLManageDataSources** 会在安装程序 DLL 中调用 **ConfigDSN** ，并向其传递 ODBC_ADD_DSN 选项。  
  
 如果用户选择数据源并单击 " **删除**"，则 **SQLManageDataSources** 会询问用户是否要删除数据源。 如果用户单击 **"是**"，则 **SQLManageDataSources** 会在安装 DLL 中用 ODBC_REMOVE_DSN 选项调用 **ConfigDSN** 。  
  
 " **创建新数据源** " 对话框用于添加或删除用户数据源、系统数据源或文件数据源。  
  
## <a name="user-dsns"></a>用户 Dsn  
 为单个用户创建的 Dsn 将称为用户 Dsn，以将其与系统 Dsn 区分开来。 用户 Dsn 在系统信息中注册如下：  
  
 `HKEY_CURRENT_USERS`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbc.ini`  
  
## <a name="system-dsns"></a>系统 Dsn  
 使用 " **创建新数据源** " 对话框，您可以将系统数据源添加到您的本地计算机上，也可以删除它，或者设置系统数据源的配置。  
  
 在同一台计算机上的多个用户可以使用系统数据源名称 (DSN) 设置的数据源。 它还可以由一种系统范围服务使用，即使没有用户登录到计算机，也可以访问数据源。  
  
 系统 DSN 是在系统信息中的 HKEY_LOCAL_MACHINE 条目中注册的，而不是在 HKEY_CURRENT_USER 条目中注册的。 它不会绑定到使用其特定的用户名和密码登录的用户，但可以由该计算机的任何用户或自动的系统范围服务使用。 但是，系统 DSN 绑定到一台计算机。 它不支持在计算机之间使用远程 Dsn。 系统 Dsn 在系统信息中注册如下：  
  
 HKEY_LOCAL_MACHINE SOFTWARE ODBC Odbc.ini  
  
## <a name="file-dsns"></a>文件 Dsn  
 文件数据源没有数据源名称（与计算机数据源相同），并且未注册到任何一个用户或计算机。 该数据源的连接信息包含在一个可复制到任何计算机的 .dsn 文件中。 文件数据源可以共享，在这种情况下，.dsn 文件位于网络上，并且网络上的多个用户可以同时使用该数据源，只要用户安装了相应的驱动程序即可。 文件数据源还可以 unshareable，在这种情况下，它只能在一台计算机上使用。  
  
 有关文件数据源的详细信息，请参阅 [使用文件数据源进行连接](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md)或参阅 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)。  
  
## <a name="managing-drivers"></a>管理驱动程序  
 如果用户单击 " **ODBC 数据源管理器**" 对话框中的 "**驱动程序**" 选项卡， **SQLManageDataSources** 将显示系统上安装的 ODBC 驱动程序的列表以及有关驱动程序的信息。 显示的日期是驱动程序的创建日期，如下图所示。  
  
 ![“ODBC 数据源管理器驱动程序”选项卡](../../../odbc/reference/syntax/media/ch23g.gif "ch23g")  
  
## <a name="tracing-options"></a>跟踪选项  
 如果用户单击 " **ODBC 数据源管理器**" 对话框中的 "**跟踪**" 选项卡， **SQLManageDataSources** 将显示跟踪选项，如下图所示。  
  
 ![“ODBC 数据源管理器跟踪”选项卡](../../../odbc/reference/syntax/media/ch23h.gif "Ch23h")  
  
 如果用户单击 " **立即开始跟踪** "，然后单击 **"确定"**， **SQLManageDataSources** 将对计算机上当前运行的所有应用程序手动启用跟踪。  
  
 如果用户在 " **日志文件路径** " 文本框中指定跟踪文件的名称，然后单击 **"确定**"，则 **SQLManageDataSources** 会将系统信息的 [ODBC] 部分中的 **TraceFile** 关键字设置为指定的名称。  
  
> [!IMPORTANT]  
>  从 Windows 8 (中开始，从 Windows 8 Visual Studio Analyzer 中删除了对 Visual Studio Analyzer 的支持，只包含在较早版本的 Visual Studio 中 ) 。 有关备用故障排除机制，请使用投标跟踪。  
  
 如果用户单击 " **开始 Visual Studio Analyzer** 然后单击 **" 确定 "**，则 Visual Studio Analyzer 处于启用状态。 它保持启用状态，直到单击 **停止 Visual Studio Analyzer** 。  
  
 有关跟踪的详细信息，请参阅 [跟踪](../../../odbc/reference/develop-app/tracing.md)。 有关 **Trace** 和 **TraceFile** 关键字的详细信息，请参阅 [ODBC 子项](../../../odbc/reference/install/odbc-subkey.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|创建数据源|[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|
