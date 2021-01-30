---
description: SQLCreateDataSource 函数
title: SQLCreateDataSource 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCreateDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLCreateDataSource
helpviewer_keywords:
- SQLCreateDataSource function [ODBC]
ms.assetid: 76ee851a-dca9-40cc-8e9e-eb3f74e560ee
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: feca3e6c135e4dba441a20b1fa458f990b437fde
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206108"
---
# <a name="sqlcreatedatasource-function"></a>SQLCreateDataSource 函数
**度**  
 引入的版本： ODBC 2。0  
  
 **摘要**  
 **SQLCreateDataSource** 显示一个对话框，用户可在其中添加数据源。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLCreateDataSource(  
     HWND    hwnd,  
     LPSTR   lpszDS);  
```  
  
## <a name="arguments"></a>参数  
 *hwnd*  
 送父窗口句柄。  
  
 *lpszDS*  
 送数据源名称。 *lpszDS* 可以是 null 指针，也可以是空字符串。  
  
## <a name="returns"></a>返回  
 如果创建了数据源，则 **SQLCreateDataSource** 返回 TRUE。 否则，返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLCreateDataSource** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_INVALID_HWND|无效的窗口句柄|*Hwnd* 参数无效或为 NULL。|  
|ODBC_ERROR_INVALID_DSN|DSN 无效|*LpszDS* 参数包含对于 DSN 无效的字符串。|  
|ODBC_ERROR_REQUEST_FAILED|*请求* 失败|与 ODBC_ADD_DSN 选项的 **ConfigDSN** 调用失败。|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|无法加载驱动程序或转换器安装程序库|无法加载驱动程序安装库。|  
|ODBC_ERROR_USER_CANCELED|用户取消了操作|用户已取消创建新的数据源。|  
|ODBC_ERROR_CREATE_DSN_FAILED|无法创建请求的 DSN|无法连接到数据库;针对文件 DSN 的 **SQLDriverConnect** 调用未返回成功的连接。<br /><br /> 无法写入文件。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 如果 *hwnd* 为 null，则 **SQLCreateDataSource** 返回 FALSE。 否则，它将显示 " **创建新数据源** " 对话框，其中包含用于选择要设置的数据源类型的向导页，如下图所示。  
  
 ![“创建新数据源”对话框：选择类型](../../../odbc/reference/syntax/media/ch23a.gif "CH23A")  
  
 默认选项为 " **文件数据源**"。 选择数据源并单击 " **下一步** " 后，将显示包含已安装驱动程序列表的下列向导页面。  
  
 ![“创建新数据源”对话框：选择驱动程序](../../../odbc/reference/syntax/media/ch23b.gif "CH23B")  
  
 如果单击 " **取消** "，则对话框将消失， **SQLCREATEDATASOURCE** 将返回 FALSE，并返回 ODBC_ERROR_USER_CANCELED 的错误代码。 如果选择了 " **用户数据源** " 或 " **系统数据源** " 选项，则 " **高级** " 按钮将不可用。  
  
 单击 " **下一步** " 按钮时，将发生下列情况之一，具体取决于所选的数据源类型：  
  
-   如果选择了 " **文件数据源** "，则会显示一个向导页面，用户可在其中输入文件名。  
  
-   如果选择了 " **用户数据源** " 或 " **系统数据源** "，则会显示显示数据源和驱动程序类型的向导页，并且在单击 " **完成** " 时，将设置数据源。  
  
 如果在 "创建新数据源" 向导页中单击 " **高级** "，则会显示一个向导页面，供用户输入特定于驱动程序的信息。 在此对话框的文本框中，键入由返回分隔的驱动程序和关键字，如下图所示。  
  
 ![“高级文件 DSN 创建设置”对话框](../../../odbc/reference/syntax/media/ch23c.gif "CH23C")  
  
 在 **SQLDriverConnect** 的 "描述" 下可以找到其他特定于驱动程序的关键字。 允许除 **DSN** 之外的所有情况。  
  
 **Verify 此连接** 选项的默认值为 TRUE。 此默认值适用于是否激活此向导页。 如果单击 **"确定"** ，则在文本框中指定的字符串和 " **验证此连接** 选项" 值将被缓存。  (如果单击 " **关闭** " 按钮或 " **取消** "，则任何新输入的特定于驱动程序的信息都将丢失，因为在文本框中指定的字符串和 " **验证此连接** 选项" 值未缓存。 )   
  
 如果在第一个向导页中选择了 " **文件数据源** "，则在选择驱动程序并在 "高级向导" 页中输入关键字值之后，系统会提示用户输入文件名。 单击 " **浏览** " 搜索文件名，在这种情况下，" **浏览** " 框中的默认目录由 "CommonFileDir" 指定的路径和 "ODBC\DataSources" HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion 中指定的路径的组合指定。  (如果 CommonFileDir 为 "C:\Program Files\Common Files"，则默认目录为 "C:\Program Files\Common Files\ODBC\Data 源"。 )   
  
 如果已输入文件名，然后单击 " **下一步** "，则根据操作系统的标准文件命名规则检查输入的文件名是否有效。 如果文件名无效，则会出现一个错误消息框，通知用户输入了无效的文件名。 用户确认消息框后，焦点将返回到在其中输入文件名的向导页面。 如果文件名有效，将显示一个显示所选关键字-值对的向导页，如下图所示。  
  
 ![“创建新数据源”对话框：检查](../../../odbc/reference/syntax/media/ch23d.gif "CH23D")  
  
 如果单击 " **完成** "，并且选择了 " **文件数据源** " 作为数据源类型，并且如果 " **验证此连接** " 选项为 TRUE，则将调用 **SQLDriverConnect** 和 **SAVEFILE** 和 **DRIVER** 关键字。 *DriverCompletion* 参数设置为 SQL_DRIVER_COMPLETE。 **SAVEFILE** 关键字的文件名是输入或选择的名称， **driver** 关键字的驱动程序名称是所选的名称。 如果在 "高级向导" 页中指定了驱动程序特定的连接字符串，则该字符串将追加到 **driver** 关键字后面。  
  
 如果 **SQLDriverConnect** 返回 SQL_SUCCESS，则驱动程序管理器已创建文件 DSN。 **SQLCreateDataSource** 返回 TRUE。 如果 **SQLDriverConnect** 未返回 SQL_SUCCESS，则会出现一个警告消息框，指示无法与数据源建立连接。 仍可创建具有最小连接信息的 DSN。 此消息框允许用户取消或继续创建文件 DSN。  
  
 如果用户选择继续创建 DSN，此过程将继续，就好像将 **此连接** 选项设置为 FALSE。 如果用户选择取消，则会为 **SQLCreateDataSource** 返回 FALSE，并返回错误代码 ODBC_ERROR_CREATE_DSN_FAILED。  
  
 如果选择 " **文件数据源** " 作为数据源类型，并且 " **验证此连接** " 选项为 "FALSE"，则会创建一个文件 DSN，其中包含 **DRIVER** 关键字和用户指定的连接字符串 (如果任何) 从高级向导页面。 如果文件创建成功，则为 **SQLCreateDataSource** 返回 TRUE。 如果文件创建未成功，则会出现一个错误消息框，通知用户从操作系统返回的任何错误。 为 **SQLCreateDataSource** 返回 FALSE，错误代码为 ODBC_ERROR_CREATE_DSN_FAILED。 有关文件数据源的详细信息，请参阅 [使用文件数据源进行连接](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md)或参阅 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)。  
  
 如果选择 "**用户**" 或 "**系统数据源**" 作为数据源类型，则将 ODBC_ADD_DSN *fRequest* 调用驱动程序安装库中的 **ConfigDSN** 。 有关详细信息，请参阅 [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|管理数据源|[SQLManageDataSources](../../../odbc/reference/syntax/sqlmanagedatasources.md)|
