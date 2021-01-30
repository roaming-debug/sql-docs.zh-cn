---
description: ConfigDSN 函数
title: ConfigDSN 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ConfigDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigDSN
helpviewer_keywords:
- ConfigDSN [ODBC]
ms.assetid: 01ced74e-c575-4a25-83f5-bd7d918123f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42e85b0c77ccc71946836e3103953fc85206b6e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174731"
---
# <a name="configdsn-function"></a>ConfigDSN 函数
**度**  
 引入的版本： ODBC 1。0  
  
 **摘要**  
 **ConfigDSN** 添加、修改或删除系统信息中的数据源。 它可能会提示用户输入连接信息。 它可以位于驱动程序 DLL 或单独的安装程序 DLL 中。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL ConfigDSN(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszAttributes);  
```  
  
## <a name="arguments"></a>参数  
 *hwndParent*  
 送父窗口句柄。 如果句柄为 null，则该函数将不会显示任何对话框。  
  
 *fRequest*  
 送请求的类型。 *FRequest* 参数必须包含以下值之一：  
  
 ODBC_ADD_DSN：添加新的数据源。  
  
 ODBC_CONFIG_DSN：配置 (修改现有数据源) 。  
  
 ODBC_REMOVE_DSN：移除现有数据源。  
  
 *lpszDriver*  
 送驱动程序描述 (通常会呈现给用户的关联 DBMS) 名称，而不是物理驱动程序名称。  
  
 *lpszAttributes*  
 送以关键字-值对形式的双向、以 null 结尾的属性列表。 有关详细信息，请参阅 "注释"。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **ConfigDSN** 返回 FALSE 时，将通过调用 **SQLPostInstallerError** 将关联的 *\* pfErrorCode* 值发布到安装程序错误缓冲区，并可通过调用 **SQLInstallerError** 获取该值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|无效的窗口句柄|*HwndParent* 参数无效。|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|关键字-值对无效|*LpszAttributes* 参数包含语法错误。|  
|ODBC_ERROR_INVALID_NAME|无效的驱动程序或转换器名称|*LpszDriver* 参数无效。 在注册表中找不到它。|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|请求的类型无效|*FRequest* 参数不是以下项之一：<br /><br /> ODBC_ADD_DSN ODBC_CONFIG_DSN ODBC_REMOVE_DSN|  
|ODBC_ERROR_REQUEST_FAILED|*请求* 失败|无法执行 *fRequest* 参数请求的操作。|  
|ODBC_ERROR_DRIVER_SPECIFIC|驱动程序或转换器特定的错误|未定义 ODBC 安装程序错误的特定于驱动程序的错误。 对 **SQLPostInstallerError** 函数的调用中的 *SzError* 参数应包含特定于驱动程序的错误消息。|  
  
## <a name="comments"></a>注释  
 **ConfigDSN** 接收来自安装程序 DLL 的连接信息，作为关键字-值对形式的属性列表。 每对都以 null 字节结束，整个列表以 null 字节结束。  (即，两个 null 字节标记列表的末尾。不允许在关键字-值对中的等号前后使用 ) 空格。 **ConfigDSN** 可以接受不是有效关键字的关键字 **SQLBrowseConnect** 和 **SQLDriverConnect**。 **ConfigDSN** 不一定支持 **SQLBrowseConnect** 和 **SQLDriverConnect** 的有效关键字的所有关键字。  (**ConfigDSN** 不接受 **DRIVER** 关键字。 ) **ConfigDSN** 函数使用的关键字必须支持使用安装程序的自动安装功能重新创建数据源所需的所有选项。 当使用 **ConfigDSN** 值并且连接字符串值相同时，应使用相同的关键字。  
  
 与 **SQLBrowseConnect** 和 **SQLDriverConnect** 中一样，关键字及其值不应包含 **[] {} ( # A1，;？ \*=！ @** 个字符， **DSN** 关键字的值不能仅包含空格。 由于注册表语法的原因，关键字和数据源名称不能包含反斜杠 (\\) 字符。  
  
 **ConfigDSN** 应调用 **SQLValidDSN** 来检查数据源名称的长度，并验证名称中是否包含无效字符。 如果数据源名称的长度超过 SQL_MAX_DSN_LENGTH 或包含无效字符，则 **SQLValidDSN** 将返回错误，并且 **ConfigDSN** 将返回错误。 **SQLWriteDSNToIni** 还会检查数据源名称的长度。  
  
 例如，若要配置需要用户 ID、密码和数据库名称的数据源，安装应用程序可能会传递以下关键字-值对：  
  
```  
DSN=Personnel Data\0UID=Smith\0PWD=Sesame\0DATABASE=Personnel\0\0  
```  
  
 有关这些关键字的详细信息，请参阅 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) 和每个驱动程序的文档。  
  
 若要显示对话框， *hwndParent* 不能为 null。  
  
## <a name="adding-a-data-source"></a>添加数据源  
 如果将数据源名称传递到 *lpszAttributes* 中的 **ConfigDSN** ，则 **ConfigDSN** 会检查该名称是否有效。 如果数据源名称与现有数据源名称相匹配，并且 *hwndParent* 为 null，则 **ConfigDSN** 将覆盖现有名称。 如果它匹配现有名称并且 *hwndParent* 不为 null，则 **ConfigDSN** 会提示用户覆盖现有名称。  
  
 如果 *lpszAttributes* 包含足够的信息来连接到数据源，则 **ConfigDSN** 可以添加数据源或显示一个对话框，用户可以使用该对话框更改连接信息。 如果 *lpszAttributes* 未包含足够的信息来连接到数据源，则 **ConfigDSN** 必须确定所需的信息;如果 *hwndParent* 不为 null，则它将显示一个对话框，用于检索用户的信息。  
  
 如果 **ConfigDSN** 显示一个对话框，则必须在 *lpszAttributes* 中显示传递给它的任何连接信息。 特别是，如果向数据源名称传递了数据源名称，则 **ConfigDSN** 将显示该名称，但不允许用户对其进行更改。 **ConfigDSN** 可以提供未在 *lpszAttributes* 中传递给它的连接信息的默认值。  
  
 如果 **ConfigDSN** 无法获取数据源的完整连接信息，则返回 FALSE。  
  
 如果 **ConfigDSN** 可以获取数据源的完整连接信息，则它将在安装程序 DLL 中调用 **SQLWriteDSNToIni** ，以将新的数据源规范添加到 Odbc.ini 文件 (或注册表) 。 **SQLWriteDSNToIni** 将数据源名称添加到 [ODBC 数据源] 部分，创建数据源规范部分，并添加 **driver** 关键字，其中包含驱动程序说明作为其值。 **ConfigDSN** 调用安装程序 DLL 中的 **SQLWritePrivateProfileString** ，以添加驱动程序使用的任何其他关键字和值。  
  
## <a name="modifying-a-data-source"></a>修改数据源  
 若要修改数据源，必须将数据源名称传递到 *lpszAttributes* 中的 **ConfigDSN** 。 **ConfigDSN** 检查数据源名称 Odbc.ini 文件 (或注册表) 。  
  
 如果 *hwndParent* 为 null，则 **ConfigDSN** 将使用 *lpszAttributes* 中的信息来修改 Odbc.ini 文件中的信息 (或注册表) 。 如果 *hwndParent* 不为 null，则 **ConfigDSN** 将使用 *lpszAttributes* 中的信息显示一个对话框;对于不在 *lpszAttributes* 中的信息，它使用系统信息中的信息。 在 **ConfigDSN** 将信息存储在系统信息中之前，用户可以修改这些信息。  
  
 如果更改了数据源名称，则 **ConfigDSN** 首先会在安装程序 DLL 中调用 **SQLRemoveDSNFromIni** ，以从 Odbc.ini 文件 (或注册表) 删除现有的数据源规范。 然后，它按照上一部分中的步骤添加新的数据源规范。 如果数据源名称未发生更改，则 **ConfigDSN** 会在安装程序 DLL 中调用 **SQLWritePrivateProfileString** 来进行任何其他更改。 **ConfigDSN** 不能删除或更改 **Driver** 关键字的值。  
  
## <a name="deleting-a-data-source"></a>删除数据源  
 若要删除数据源，必须将数据源名称传递到 *lpszAttributes* 中的 **ConfigDSN** 。 **ConfigDSN** 检查数据源名称 Odbc.ini 文件 (或注册表) 。 然后，它调用安装程序 DLL 中的 **SQLRemoveDSNFromIni** 来删除数据源。  
  
## <a name="note"></a>备注
 如果编写此例程的 Unicode 版本，则必须使用 LPCWSTR 参数而不是 LPCSTR 来调用 **ConfigDSNW**。
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|添加、修改或删除数据源|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|从 Odbc.ini 文件或注册表获取值|[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|  
|删除默认数据源|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|  
|删除 Odbc.ini (或注册表中的数据源名称) |[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|  
|将数据源名称添加到 Odbc.ini (或注册表) |[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|  
|向 Odbc.ini 文件或注册表写入值|[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|
