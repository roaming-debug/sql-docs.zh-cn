---
title: Linux 和 macOS 上的 mssql-tools 发行说明
description: 了解 Microsoft SQL Server Tools 的发布版本中的新增功能和已更新内容。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
author: v-zhangw
ms.author: v-zhangw
manager: kenvh
ms.openlocfilehash: 75f1144e84792b3c9361dcab74dcdbcb7b072268
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075919"
---
# <a name="release-notes-for-the-microsoft-sql-server-tools-on-linux-and-macos"></a>Linux 和 macOS 上的 Microsoft SQL Server Tools 的发行说明

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

本文列出并介绍了 Linux 和 macOS 上的 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] SQL Server Tools 的版本控制版本中的新增功能。

## <a name="17711-january-2021"></a>17.7.1.1，2021 年 1 月

| 新增功能 | 详细信息 |
| :------------ | :------ |
| Sqlcmd Bug 修复 | 修复了输入重定向 bug 和空行导致重复执行的问题。 |
| Sqlcmd Bug 修复 | 修复了某些格式设置下 r、p、X 和 k 选项的错误报告。 |
| Sqlcmd -z/-Z“Password”选项 | 现在受支持。 |
| &nbsp; | &nbsp; |

## <a name="17611-july-2020"></a>17.6.1.1，2020 年 7 月

| 新增功能 | 详细信息 |
| :------------ | :------ |
| Sqlcmd 命令行分析程序已更新 | 修复了在按不同顺序使用某些选项时出现意外行为的 bug。 |
| 已更新 Sqlcmd 错误消息 | 修正了 sqlcmd 中返回错误的各种不一致性。 |
| Sqlcmd -Y 选项已修复 | 修复了 -Y 选项无效的问题 |
| Sqlcmd 列名称截断已修复 | 修复了列名称被错误截断的问题 |
| Sqlcmd Linux 退出代码 | 修复了 Linux 上缺少进程退出代码的问题 |
| &nbsp; | &nbsp; |

## <a name="next-steps"></a>后续步骤

详细了解如何连接 [BCP](connecting-with-bcp.md) 和 [SQLCMD](connecting-with-sqlcmd.md)！
