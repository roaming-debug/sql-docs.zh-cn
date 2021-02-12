---
description: 获取完整内存转储
title: 获取用于排查 SSMS 故障的完整内存转储
Description: 从 SQL Server Management Studio (SSMS) 中捕获诊断信息，以便可以对崩溃或无响应系统进行故障排除。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, sstein
ms.custom: seo-lt-2019
ms.date: 05/17/2019
ms.openlocfilehash: 38e711e9857aea6601878f712c9c5de8cf908e03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017379"
---
# <a name="get-full-memory-dump"></a>获取完整内存转储

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

本文将介绍如何捕获诊断信息来排查你在QL Server Management Studio (SSMS) 中遇到的系统故障或无响应的问题。

若要捕获诊断信息来排除故障，请按照以下步骤操作。

1. 下载 [ProcDump](/sysinternals/downloads/procdump)。

2. 将下载内容解压缩到文件夹中。

3. 打开命令提示符（如 `cmd.exe`），并运行以下命令。

    ```console
    <PathToProcDumpFolder>\procdump.exe -e -h -ma -w ssms.exe
    ```

    它应该会提示你接受许可协议，请选择“同意”  。

4. 启动 SQL Server Management Studio (SSMS)（如果尚未启动的话）。

5. 重现遇到的问题。

6. 关于写入转储文件的命令提示符中应该会出现文本，请等待此操作完成。

7. 新建文件夹，并将写入操作输出的 *.dmp 文件复制此文件夹中。

8. 将下列文件复制到同一文件夹中。

    * “C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll”
    * “C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll”
    * “C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll”

9. 压缩此文件夹。

## <a name="outofmemoryexception"></a>OutOfMemoryException

还可以在 SSMS 抛出 OutOfMemoryException（可以是任意托管异常）时获取 SSMS 的完整内存转储。

若要捕获诊断信息来排查 SSMS 抛出的 OutOfMemoryException，请按照以下步骤操作。

1. 下载 [ProcDump](/sysinternals/downloads/procdump)。

2. 将下载内容解压缩到文件夹中。

3. 打开命令提示符，并运行以下命令。

    ```cmd
    <PathToProcDumpFolder>\procdump.exe -e 1 -f System.OutOfMemoryException -ma -w ssms.exe
    ```

    它应该会提示你接受许可协议，请选择“同意”  。

4. 启动 SQL Server Management Studio（如果尚未启动的话）。

5. 重现遇到的问题。

6. 关于写入转储文件的命令提示符中应该会出现文本，请等待此操作完成。

7. 新建文件夹，并将写入操作输出的 *.dmp 文件复制此文件夹中。

8. 将下列文件复制到同一文件夹中。

    * “C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll”
    * “C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll”
    * “C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll”

9. 压缩此文件夹。

## <a name="share-the-information"></a>共享信息

1. 若要与 SSMS 团队共享信息，请在 https://aka.ms/sqlfeedback 处记录问题。

2. 然后将收集的内存转储文件共享到可以收集文件的 OneDrive（或等效位置）。

    > [!Important]
    > 内存转储文件可能包含敏感信息。

## <a name="next-steps"></a>后续步骤

[SQL Server Management Studio](../sql-server-management-studio-ssms.md)