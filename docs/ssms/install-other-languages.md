---
title: 安装非英语版本
description: 安装非英语版 SQL Server Management Studio (SSMS)。 本文适用于 SSMS 17.x。
ms.prod: sql
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 04/25/2019
ms.openlocfilehash: 7637c21ce048c4ffc0277a73c4403c5d16484a21
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597179"
---
# <a name="install-non-english-language-versions-of-sql-server-management-studio-ssms"></a>安装非英语语言版本的 SQL Server Management Studio (SSMS)

SSMS 提供了多种语言，但如果系统区域设置不匹配 SSMS 语言，SSMS 安装程序将阻止在计算机上安装。

> [!NOTE]
> 本文适用于 SSMS 17.x。 对于 SSMS 18.x，已取消阻止混合语言设置，例如，现在可以在法语版 Windows 上安装 SSMS 德语版。 如果 OS 语言与 SSMS 语言不匹配，请在“工具” > “选项” > “国际设置”下设置所需语言，否则 SSMS 将显示英语用户界面  。

以下说明因 Windows 版本而异。 下面的说明适用于 Windows 10。

## <a name="install-non-english-ssms-on-a-computer-running-an-english-operating-system-os"></a>在运行英语操作系统 (OS) 的计算机上安装非英语 SSMS

1. 安装 SSMS 要使用的语言的 Windows 语言包：
   - “设置” > “时间和语言” > “区域和语言” > “添加语言”
2. 现在，单击刚安装的语言，然后选择“设为默认值”，来设置系统区域设置，以使用在上一步中安装的语言包。 （在安装 SSMS 后，可以将系统区域设置改回英语。）
3. 操作系统以所需语言运行后，请安装所需的 SSMS 语言。 第一次安装新的 SSMS 语言时，请使用完整包。 对于后续安装，可以使用升级包。
4. 运行 SSMS，它应显示在上一步中安装的语言。
5. 将计算机的系统区域设置改回英语。

## <a name="install-ssms-in-a-language-other-than-the-language-of-the-installed-os"></a>使用不同于已安装 OS 的语言安装 SSMS

1. 安装 SSMS 要使用的语言的 Windows 语言包：
   - “设置” > “时间和语言” > “区域和语言” > “添加语言”
2. 现在，单击刚安装的语言，然后选择“设为默认值”，来设置系统区域设置，以使用在上一步中安装的语言包。
3. 操作系统以所需语言运行后，请安装所需的 SSMS 语言。 第一次安装新的 SSMS 语言时，请使用完整包。 对于后续安装，可以使用升级包。
4. 对于每种你想要安装的语言，如果与已安装的 SSMS 第一个版本的语言不匹配，请安装相应的 Visual Studio 2015 Shell（独立）语言包：
   - 浏览到 [https://connect.microsoft.com/VisualStudio/ExtendVS](https://connect.microsoft.com/VisualStudio/ExtendVS)（你可能需要登录并完成 Connect 注册过程）。
   - 下载并安装所需的 Visual Studio 2015 Shell（独立）语言包。

   > [!IMPORTANT]
   > 使用前述步骤来安装 Visual Studio 2015 Shell（独立）语言包，请勿使用“工具” | “选项” | “区域设置”上的“获取其他语言”链接。

5. 运行 SSMS 并通过以下菜单选择你想要使用的语言：
   - “工具” | “选项” | “区域设置”
6. 关闭并重新启动 SSMS。

## <a name="next-steps"></a>后续步骤

- [教程：SQL Server Management Studio](./quickstarts/ssms-connect-query-sql-server.md)