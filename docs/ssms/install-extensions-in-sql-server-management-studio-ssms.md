---
description: 在 SQL Server Management Studio (SSMS) 中安装扩展
title: 在 SQL Server Management Studio (SSMS) 中安装扩展
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
keywords:
- 扩展
- vsix
- 安装扩展
- 安装 vsix
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 07/29/2020
ms.openlocfilehash: d9fe47b283eb95ed624b68bc9c22abd9258bad15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344709"
---
# <a name="install-extensions-in-sql-server-management-studio-ssms"></a>在 SQL Server Management Studio (SSMS) 中安装扩展

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

SQL Server Management Studio (SSMS) 扩展是使用 C# 通过 Visual Studio 中的“Visual Studio 扩展开发”工作负载创建的。 SSMS 18.x 是在 Visual Studio 2017 shell 上构建的，并且受该环境的限制。

可以通过使用 Visual Studio 或独立的托管包安装程序部署 VSIX 来实现 SSMS 18.x 中的扩展安装。  Visual Studio 部署如下所述。

> [!NOTE]
> 无法在 SSMS 18.x 下通过 VSIXInstaller 安装 SQL Server Management Studio 扩展。
  
## <a name="visual-studio-deployment-of-an-extension-for-ssms-18x"></a>SSMS 18.x 扩展的 Visual Studio 部署

手动扩展安装是通过将关联的扩展文件 (VSIX) 复制到默认 SSMS 扩展文件夹来完成的。  SSMS 在启动时自动检查此文件夹中的扩展。  VSIX 部署可在项目生成时由 Visual Studio 完成。 

  
1.  找到 SSMS 安装和默认扩展文件夹。  如果使用的是默认 SSMS 安装设置，则文件夹位置为 ```C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\Extensions\```。  


2. 以管理员身份启动 Visual Studio。

3.  通过在项目的“属性”窗口的“VSIX”选项卡中选中“将 VSIX 内容复制到以下位置”复选框，文件复制过程可在生成时由 Visual Studio 完成。 在复选框下方的文本框中，输入以上文件夹位置并附加此扩展的文件夹。  例如：```C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\Extensions\SampleExtension```
  
![具有 3 个复选框和 1 个文本框的“项目属性”窗口 VSIX 设置](./media/install-extensions/vsix_ssms.png)

4. 生成扩展项目，成功的生成会将扩展文件传输到 SSMS 扩展文件夹。

5.  启动 SSMS 并测试扩展功能。
  
