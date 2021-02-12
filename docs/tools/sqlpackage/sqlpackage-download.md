---
title: 下载并安装 sqlpackage
description: 下载并安装适用于 Windows、macOS 或 Linux 的 sqlpackage
ms.custom: tools|sos
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 10/02/2020
ms.openlocfilehash: d664c9b5d87e8a9358fc63418f3223a062e5d7bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061032"
---
# <a name="download-and-install-sqlpackage"></a>下载并安装 sqlpackage

sqlpackage 在 Windows、macOS 和 Linux 上运行。

下载并安装最新的 .NET Framework 版本以及 macOS 和 Linux 预览版：

|平台|下载|发布日期|版本|构建
|:---|:---|:---|:---|:---|
|Windows|[MSI 安装程序](https://go.microsoft.com/fwlink/?linkid=2143544)|2020 年 9 月 18 日| 18.6 | 15.0.4897.1 |
|[macOS .NET Core](#get-sqlpackage-net-core-for-macos) |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2143659)|2020 年 9 月 18 日| 18.6| 15.0.4897.1 |
|[Linux .NET Core](#get-sqlpackage-net-core-for-linux) |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2143497)|2020 年 9 月 18 日| 18.6| 15.0.4897.1 |
|[Windows .NET Core](#get-sqlpackage-net-core-for-windows) |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2143496)|2020 年 9 月 18 日| 18.6| 15.0.4897.1 |

有关最新版本的详细信息，请参阅[发行说明](release-notes-sqlpackage.md)。 若要下载其他语言，请参阅[可用语言](#available-languages)部分。


提供一个长期有效链接 ([https://aka.ms/sqlpackage-linux](https://aka.ms/sqlpackage-linux))，该链接指向适用于 [Linux .NET Core](#get-sqlpackage-net-core-for-linux) 的 sqlpackage 的当前版本（可以在具有最新版 sqlpackage 的自动化环境中使用）。

## <a name="dacfx"></a>DacFx
SqlPackage 是 DacFx 框架的命令行接口，公开了一些公共 DacFx API。 DacServices ([Microsoft.SqlServer.Dac](/dotnet/api/microsoft.sqlserver.dac.dacservices)) 是一种用于将数据库部署集成到应用程序管道的相关机制。  DacServices API 可通过 NuGet 在包 [Microsoft.SqlServer.DACFx](https://www.NuGet.org/packages/Microsoft.SqlServer.DACFx) 中获得。  当前 DacFx 版本为 150.4897.1。

通过 .NET CLI 安装 NuGet 包时，可通过以下命令完成：

```cmd
dotnet add package Microsoft.SqlServer.DACFx
```

>[!NOTE]
> 其他的 NuGet 包以 DacFx 名称“Microsoft.SqlServer.DacFx.x64”和“Microsoft.SqlServer.DacFx.x86”发布。 “Microsoft.SqlServer.DACFx”包涵盖了对这两个平台的支持。 应对此包（而不是 x64 或 x86 变体）进行新的引用。

## <a name="get-sqlpackage-for-windows"></a>获取适用于 Windows 的 sqlpackage

此版本的 sqlpackage 包括标准 Windows 安装程序体验和 .zip： 

1. 下载并运行[适用于 Windows 的 DacFramework.msi 安装程序](https://go.microsoft.com/fwlink/?linkid=2143544)。
2. 打开一个新的命令提示符窗口，并运行 sqlpackage.exe
    - sqlpackage 会安装到 ```C:\Program Files\Microsoft SQL Server\150\DAC\bin``` 文件夹

## <a name="get-sqlpackage-net-core-for-windows"></a>获取适用于 Windows 的 sqlpackage .NET Core

1. 下载[适用于 Windows 的 sqlpackage](https://go.microsoft.com/fwlink/?linkid=2143496)。
2. 若要提取文件，请在 Windows 资源管理器中右键单击文件，然后依次选择“全部提取...”和目标目录。
3. 打开新的终端窗口，并键入 cd，以转到 sqlpackage 的提取位置：

   ```cmd
   > sqlpackage
   ```

## <a name="get-sqlpackage-net-core-for-macos"></a>获取适用于 macOS 的 sqlpackage .NET Core

1. 下载[适用于 macOS 的 sqlpackage](https://go.microsoft.com/fwlink/?linkid=2143659)。
2. 若要提取文件并启动 sqlpackage，请打开一个新的终端窗口并键入以下命令：

   ```bash
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-osx-<version string>.zip -d ~/sqlpackage 
   $ echo 'export PATH="$PATH:~/sqlpackage"' >> ~/.bash_profile
   $ source ~/.bash_profile
   $ sqlpackage
   ```

   > [!NOTE]
   > 可能需要修改安全设置，才能在 macOS 上运行 sqlpackage。 使用以下命令从命令行与 Gatekeeper 进行交互。

   执行 sqlpackage 前：
   ```bash
   $ sudo spctl --master-disable
   ```

   执行 sqlpackage 后：
   ```bash
   $ sudo spctl --master-enable
   ```

## <a name="get-sqlpackage-net-core-for-linux"></a>获取适用于 Linux 的 sqlpackage .NET Core

1. 通过使用安装程序之一或 tar.gz 存档来下载[适用于 Linux 的 sqlpackage](https://go.microsoft.com/fwlink/?linkid=2143497)。
2. 若要提取文件并启动 sqlpackage，请打开一个新的终端窗口并键入以下命令：

   ```bash
   $ cd ~
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-linux-<version string>.zip -d ~/sqlpackage 
   $ echo "export PATH=\"\$PATH:$HOME/sqlpackage\"" >> ~/.bashrc
   $ chmod a+x ~/sqlpackage/sqlpackage
   $ source ~/.bashrc
   $ sqlpackage
   ```

   > [!NOTE]
   > 在 Debian、Redhat 和 Ubuntu 上，可能缺少依赖项。 使用以下命令安装这些依赖项，具体取决于你的 Linux 版本：

   **Debian：**

   ```bash
   $ sudo apt-get install libunwind8
   ```

   **Redhat：**

   ```bash
   $ yum install libunwind
   $ yum install libicu
   ```

   **Ubuntu：**

   ```bash
   $ sudo apt-get install libunwind8

   # install the libicu library based on the Ubuntu version
   $ sudo apt-get install libicu52      # for 14.x
   $ sudo apt-get install libicu55      # for 16.x
   $ sudo apt-get install libicu57      # for 17.x
   $ sudo apt-get install libicu60      # for 18.x
   ```

## <a name="uninstall-sqlpackage"></a>卸载 sqlpackage

如果使用 Windows 安装程序安装了 sqlpackage，则按照删除任何 Windows 应用程序的方式进行卸载。

如果使用 .zip 或其他存档安装了 sqlpackage，则删除文件。

## <a name="supported-operating-systems"></a>受支持的操作系统

sqlpackage 在 Windows、macOS 和 Linux 上运行，并使用 .NET Core 3.1 生成。  [.NET Core 3.1 OS 要求](https://github.com/dotnet/core/blob/master/release-notes/3.1/3.1-supported-os.md) 适用于 sqlpackage。

### <a name="windows-x64"></a>Windows (x64)

- Windows 10 (1607+)
- Windows 8.1
- Windows 7 SP1
- Windows Server 核心
- Windows Server 2012 R2
- Windows Server 2016
- Windows Server 2019

### <a name="macos"></a>macOS

- macOS 10.15“Catalina”
- macOS 10.14“Mojave”
- macOS 10.13“High Sierra”

### <a name="linux-x64"></a>Linux (x64)

- Red Hat Enterprise Linux 7+
- SUSE Linux Enterprise Server v12 SP2+
- Ubuntu 16.04+

## <a name="available-languages"></a>可用语言

可采用以下语言安装此版本的 sqlpackage：

sqlpackage Windows：  
[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x40a)

sqlpackage .NET Core Windows：  
[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x40a)

sqlpackage .NET Core macOS：  
[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x40a)

sqlpackage .NET Core Linux：  
[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x40a)


## <a name="next-steps"></a>后续步骤

- 了解有关 [sqlpackage](sqlpackage.md) 的详细信息

[Microsoft 隐私声明](https://go.microsoft.com/fwlink/?LinkId=521839)