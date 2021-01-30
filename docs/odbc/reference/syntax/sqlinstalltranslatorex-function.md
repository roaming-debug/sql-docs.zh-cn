---
description: SQLInstallTranslatorEx 函数
title: SQLInstallTranslatorEx 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallTranslatorEx
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallTranslatorEx
helpviewer_keywords:
- SQLInstallTranslatorEx function [ODBC]
ms.assetid: a0630602-53c1-4db0-98ce-70d160aedf8d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e055f3f3565c6240134bf43b4b4f8951059e2725
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189651"
---
# <a name="sqlinstalltranslatorex-function"></a>SQLInstallTranslatorEx 函数
**度**  
 引入的版本： ODBC 3。0  
  
 **摘要**  
 **SQLInstallTranslatorEx** 将有关转换器的信息添加到系统信息的 Odbcinst.ini 部分 ( # A1\ODBC 翻译程序注册表项) 。  
  
 还可以 [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md)访问 **SQLInstallTranslatorEx** 功能。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLInstallTranslatorEx(  
     LPCSTR    lpszTranslator,  
     LPCSTR    lpszPathIn,  
     LPSTR     lpszPathOut,  
     WORD      cbPathOutMax,  
     WORD *    pcbPathOut,  
     WORD      fRequest,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>参数  
 *lpszTranslator*  
 送这必须包含用于描述转换器的关键字/值对的双向结尾列表。 有关关键字-值对语法的详细信息，请参阅 [转换器规范子项](../../../odbc/reference/install/translator-specification-subkeys.md)。  
  
 *LpszTranslator* 字符串中必须包含 **转换器** 和 **安装** 关键字。 翻译 DLL 与 **translator** 关键字一起列出，转换器设置 Dll 与 **setup** 关键字一起列出。 每对都以 NULL 字节结束，整个列表以 NULL 字节结束。  (即，两个 NULL 字节标记列表的末尾。 ) *LpszTranslator* 的格式如下所示：  
  
 \0Translator =*转换器-dll-filename*\ 0 [setup =*setup.exe-filename*\ 0] \ 0  
  
 *lpszPathIn*  
 送要安装转换器的完整路径或 null 指针。 如果 *lpszPath* 为 null 指针，则将在系统目录中安装转换器。  
  
 *lpszPathOut*  
 输出应安装转换器的目标目录的路径。 如果从未安装过转换器，则 *lpszPathOut* 与 *lpszPathIn* 相同。 如果以前安装了转换器，则 *lpszPathOut* 是之前安装的路径。  
  
 *cbPathOutMax*  
 送LpszPathOut 的长度 *。*  
  
 *pcbPathOut*  
 输出可在 *lpszPathOut* 中返回的总字节数。 如果可返回的字节数大于或等于 *cbPathOutMax*，则 *lpszPathOut* 中的输出路径将截断为 *pcbPathOutMax* 减 null 终止字符。 *PcbPathOut* 参数可以为 null 指针。  
  
 *fRequest*  
 送请求的类型。 *fRequest* 必须包含下列值之一：  
  
 ODBC_INSTALL_INQUIRY：查询可在何处安装转换器。  
  
 ODBC_INSTALL_COMPLETE：完成安装请求。  
  
 *lpdwUsageCount*  
 输出调用此函数后转换器的使用计数。  
  
 应用程序不应设置使用计数。 ODBC 将维护此计数。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLInstallTranslatorEx** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_INVALID_BUFF_LEN|缓冲区长度无效|*LpszPathOut* 参数不够大，无法包含输出路径。 缓冲区包含截断的路径。<br /><br /> *CbPathOutMax* 参数为0，并且 *fRequest* 参数 ODBC_INSTALL_COMPLETE。|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|请求的类型无效|*FRequest* 参数不是以下项之一：<br /><br /> ODBC_INSTALL_INQUIRY ODBC_INSTALL_COMPLETE|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|关键字-值对无效|*LpszTranslator* 参数包含语法错误。|  
|ODBC_ERROR_INVALID_PATH|安装路径无效|*LpszPathIn* 参数包含无效路径。|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|参数序列无效|*LpszTranslator* 参数不包含关键字/值对的列表。|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|不能增加或减少注册表的组件使用计数|安装程序未能递增转换器的使用计数。|  
  
## <a name="comments"></a>注释  
 **SQLInstallTranslatorEx** 提供一种机制来仅安装转换器。 此函数不会实际复制任何文件。 调用程序负责复制转换器文件。  
  
 **SQLInstallTranslatorEx** 将安装的转换器的组件使用计数增加1。 如果转换器的某个版本已存在，但该转换器的组件使用计数不存在，则新的 "组件使用计数" 值将设置为2。  
  
 应用程序安装程序负责物理复制转换器文件并维护文件使用计数。 如果以前未安装转换器文件，则应用程序安装程序必须复制该文件或文件，并创建文件或文件使用计数。 如果以前安装过该文件，则安装程序只会递增文件使用计数。  
  
 如果应用程序以前安装了较旧版本的转换器，则应该卸载并重新安装该转换器，以便转换器组件的使用计数有效。 应调用 **SQLRemoveTranslator** 来减小组件使用计数，然后调用 **SQLInstallTranslatorEx** 来递增组件使用计数。 应用程序安装程序必须将旧文件替换为新文件。 文件使用计数将保持不变，并且使用较旧版本文件的其他应用程序现在将使用较新的版本。  
  
 **SQLInstallTranslatorEx** 的 *lpszPathOut* 中的路径长度允许两阶段安装过程，因此应用程序可以通过使用 ODBC_INSTALL_INQUIRY 模式的 *FRequest* 调用 **SQLInstallTranslatorEx** 来确定 *cbPathOutMax* 。 这将返回 *pcbPathOut* 缓冲区中可用的总字节数。 然后，可以使用 ODBC_INSTALL_COMPLETE 的 *fRequest* 调用 **SQLInstallTranslatorEx** ，并将 *cbPathOutMax* 参数设置为 *pcbPathOut* 缓冲区中的值加上 null 终止字符。  
  
 如果选择不使用 **SQLInstallTranslatorEx** 的两阶段模型，则必须将 *cbPathOutMax* 定义为目标目录的路径的存储大小，以在 stdlib.h 中定义的值 _MAX_PATH，以防止截断。  
  
 ODBC_INSTALL_COMPLETE *fRequest* 时， **SQLInstallTranslatorEx** 不允许 *LpszPathOut* 为 NULL (或 *cbPathOutMax*) 。 如果 *fRequest* ODBC_INSTALL_COMPLETE，则当可返回的字节数大于或等于 *cbPathOutMax* 时，将返回 FALSE，并将发生截断。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|返回默认转换选项|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|  
|选择转换器|[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|  
|删除转换器|[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|
