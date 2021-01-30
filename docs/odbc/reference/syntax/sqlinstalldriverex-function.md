---
description: SQLInstallDriverEx 函数
title: SQLInstallDriverEx 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallDriverEx
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallDriverEx
helpviewer_keywords:
- SQLInstallDriverEx function [ODBC]
ms.assetid: 1dd74544-f4e9-46e1-9b5f-c11d84fdab4c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3ebdfa732ba9c5a4a539009600749563daff4a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189707"
---
# <a name="sqlinstalldriverex-function"></a>SQLInstallDriverEx 函数
**度**  
 引入的版本： ODBC 3。0  
  
 **摘要**  
 **SQLInstallDriverEx** 将有关驱动程序的信息添加到系统信息中的 Odbcinst.ini 条目，并将驱动程序的 *UsageCount* 递增1。 但是，如果驱动程序的某个版本已存在，但该驱动程序的 *UsageCount* 值不存在，则新的 *UsageCount* 值将设置为2。  
  
 此函数不会实际复制任何文件。 调用程序负责正确地将驱动程序的文件复制到目标目录。  
  
 还可以 [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md)访问 **SQLInstallDriverEx** 功能。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLInstallDriverEx(  
     LPCSTR    lpszDriver,  
     LPCSTR    lpszPathIn,  
     LPSTR     lpszPathOut,  
     WORD      cbPathOutMax,  
     WORD *    pcbPathOut,  
     WORD      fRequest,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>参数  
 *lpszDriver*  
 送驱动程序描述通常 (向用户显示的关联 DBMS) 的名称，而不是物理驱动程序名称。 *LpszDriver* 参数必须包含描述驱动程序的关键字值对的双向以 null 结尾的列表。 有关关键字/值对的详细信息，请参阅 [驱动程序规范子项](../../../odbc/reference/install/driver-specification-subkeys.md)。 有关双向以 null 结尾的字符串的详细信息，请参阅 [ConfigDSN 函数](../../../odbc/reference/syntax/configdsn-function.md)。  
  
 *lpszPathIn*  
 送安装的目标目录的完整路径，或 null 指针。 如果 *lpszPathIn* 为 null 指针，则驱动程序将安装到系统目录中。  
  
 *lpszPathOut*  
 输出应安装驱动程序的目标目录的路径。 如果先前未安装驱动程序， *lpszPathOut* 应与 *lpszPathIn* 相同。 如果先前安装了驱动程序，则 *lpszPathOut* 是之前安装的路径。  
  
 *cbPathOutMax*  
 送 *LpszPathOut* 的长度。  
  
 *pcbPathOut*  
 输出 (不包括 null 终止字符) 可在 *lpszPathOut* 中返回的总字节数。 如果可返回的字节数大于或等于 *cbPathOutMax*，则 *lpszPathOut* 中的输出路径将截断为 *cbPathOutMax* 减 null 终止字符。 *PcbPathOut* 参数可以为 null 指针。  
  
 *fRequest*  
 送请求的类型。 *FRequest* 参数必须包含以下值之一：  
  
 ODBC_INSTALL_INQUIRY：查询可安装驱动程序的位置。  
  
 ODBC_INSTALL_COMPLETE：完成安装请求。  
  
 *lpdwUsageCount*  
 输出调用此函数后驱动程序的使用计数。  
  
 应用程序不应设置使用计数。 ODBC 将维护此计数。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLInstallDriverEx** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_INVALID_BUFF_LEN|缓冲区长度无效|*LpszPathOut* 参数不够大，无法包含输出路径。 缓冲区包含截断的路径。<br /><br /> *CbPathOutMax* 参数为0， *fRequest* 为 ODBC_INSTALL_COMPLETE。|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|请求的类型无效|*FRequest* 参数不是以下项之一：<br /><br /> ODBC_INSTALL_INQUIRY ODBC_INSTALL_COMPLETE|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|关键字-值对无效|*LpszDriver* 参数包含语法错误。|  
|ODBC_ERROR_INVALID_PATH|安装路径无效|*LpszPathIn* 参数包含无效路径。|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|无法加载驱动程序或转换器安装程序库|无法加载驱动程序安装库。|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|参数序列无效|*LpszDriver* 参数不包含关键字/值对的列表。|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|无法递增或递减组件使用率计数|安装程序未能递增驱动程序的使用计数。|  
  
## <a name="comments"></a>注释  
 *LpszDriver* 参数是一个以关键字-值对形式列出的属性列表。 每对都以 null 字节结束，整个列表以 null 字节结束。  (即，两个 null 字节标记列表的末尾。 ) 此列表的格式如下所示：  
  
 _驱动程序-desc_ **\\** 0Driver **=** _驱动程序-dll-文件名_ **\\** 0 [安装 **=** _程序-dll-filename_ <b>\\</b> 0]  
  
 [_keyword1_ **=**_value1_ <b>\\</b>0] [_keyword2_ **=** _value2_ <b>\\</b> 0] ... <b>\\</b>0  
  
 其中，\ 0 是一个 null 字节，而 *keywordn* 是任何驱动程序属性关键字。 关键字必须按指定顺序显示。 例如，假设带格式文本文件的驱动程序具有独立的驱动程序和设置 Dll，并且可以使用扩展名为 .txt 和 .csv 的文件。 此驱动程序的 *lpszDriver* 参数可能如下所示：  
  
```  
Text\0Driver=TEXT.DLL\0Setup=TXTSETUP.DLL\0FileUsage=1\0  
FileExtns=*.txt,*.csv\0\0  
```  
  
 假设 SQL Server 的驱动程序没有单独的安装程序 DLL，并且没有任何驱动程序属性关键字。 此驱动程序的 *lpszDriver* 参数可能如下所示：  
  
```  
SQL Server\0Driver=SQLSRVR.DLL\0\0  
```  
  
 **SQLInstallDriverEx** 从 *lpszDriver* 参数中检索有关驱动程序的信息后，它会将驱动程序说明添加到系统信息中 Odbcinst.ini 条目的 [ODBC driver] 部分。 然后，它创建一个标题为驱动程序说明的部分，并添加驱动程序 DLL 和安装程序 DLL 的完整路径。 最后，它返回安装的目标目录的路径，但不会将驱动程序文件复制到该目录。 调用程序必须实际将驱动程序文件复制到目标目录。  
  
 **SQLInstallDriverEx** 将安装的驱动程序的组件使用计数增加1。 如果驱动程序的某个版本已存在，但该驱动程序的组件使用计数不存在，则新的 "组件使用计数" 值将设置为2。  
  
 应用程序安装程序负责对驱动程序文件进行物理复制并维护文件使用计数。 如果先前未安装驱动程序文件，则应用程序安装程序必须复制 *lpszPathIn* 路径中的文件，并创建文件使用计数。 如果以前安装过该文件，安装程序只会递增文件使用计数，并在 *lpszPathOut* 参数中返回以前安装的路径。  
  
> [!NOTE]  
>  有关组件使用计数和文件使用计数的详细信息，请参阅 [用量计数](../../../odbc/reference/install/usage-counting.md)。  
  
 如果应用程序以前安装了旧版本的驱动程序文件，则应卸载该驱动程序，然后重新安装该驱动程序，以便驱动程序组件的使用计数有效。 应该首先调用 ODBC_REMOVE_DRIVER *) 的* **SQLConfigDriver** (，然后调用 **SQLRemoveDriver** 来减小组件使用计数。 然后，应调用 **SQLInstallDriverEx** 来重新安装驱动程序，并递增组件用量计数。 应用程序安装程序必须用新文件替换旧文件。 文件使用计数将保持不变，并且使用较旧版本文件的任何其他应用程序现在将使用较新的版本。  
  
> [!NOTE]  
>  如果以前安装了驱动程序，并且调用了 **SQLInstallDriverEx** 来将驱动程序安装在另一个目录中，则该函数将返回 TRUE，但 *lpszPathOut* 将包含已安装驱动程序的目录。 它不会包含在 *lpszDriver* 参数中输入的目录。  
  
 **SQLInstallDriverEx** 的 *lpszPathOut* 中的路径长度允许两阶段安装过程，因此应用程序可以通过使用 ODBC_INSTALL_INQUIRY 模式的 *FRequest* 调用 **SQLInstallDriverEx** 来确定 *cbPathOutMax* 。 这将返回 *pcbPathOut* 缓冲区中可用的总字节数。 然后，可以使用 ODBC_INSTALL_COMPLETE 的 *fRequest* 调用 **SQLInstallDriverEx** ，并将 *cbPathOutMax* 参数设置为 *pcbPathOut* 缓冲区中的值加上 null 终止字符。  
  
 如果选择不使用 **SQLInstallDriverEx** 的两阶段模型，则必须将 *cbPathOutMax* 定义为目标目录的路径的存储大小，以在 stdlib.h 中定义的值 _MAX_PATH，以防止截断。  
  
 ODBC_INSTALL_COMPLETE *fRequest* 时， **SQLInstallDriverEx** 不允许 *LpszPathOut* 为 NULL (或 *cbPathOutMax*) 。 如果 *fRequest* ODBC_INSTALL_COMPLETE，则当可返回的字节数大于或等于 *cbPathOutMax* 时，将返回 FALSE，并将发生截断。  
  
 在调用 **SQLInstallDriverEx** 之后，应用程序安装程序已复制驱动程序文件 (如有必要) ，驱动程序安装程序 DLL 必须调用 **SQLConfigDriver** 来设置驱动程序的配置。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|安装驱动程序管理器|[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|
