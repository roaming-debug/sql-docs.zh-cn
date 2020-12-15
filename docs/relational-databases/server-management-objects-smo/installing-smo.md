---
description: 安装 SMO
title: 安装 SMO |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.reviewer: ''
ms.prod_service: database-engine
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- installing SMO
- SMO [SQL Server], installing
- SQL Server Management Objects, installing
ms.assetid: 140e9971-4940-4866-89b9-5cec938e2a16
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0b182a2c4993206d302ce951234e709d0954dc5c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462978"
---
# <a name="installing-smo"></a>安装 SMO

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

本页提供有关如何安装应用程序使用的 SMO 以及使用 SMO 的系统要求的信息。

## <a name="smo-nuget-package"></a>SMO NuGet 包

从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2017 SMO 开始，作为 [SqlManagementObjects](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) NuGet 包分发，使用户能够用 SMO 开发应用程序。

这是 SharedManagementObjects.msi 的替代，以前作为 SQL Server 的每个版本的 SQL 功能包的一部分发布。 应将使用 SMO 的应用程序更新为使用 NuGet 包，并负责确保二进制文件与正在开发的应用程序一起安装。

>>[!Important]
>>如 " [文件和版本号](files-and-version-numbers.md) " 页中所述，不应将 SMO 程序集安装到 GAC 中。 这样做可能会导致其他应用程序出现问题，这些应用程序也使用这些版本的 SMO (例如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio) 。

## <a name="installing-the-package"></a>安装包

有关安装和使用 NuGet 包的说明和示例，请参阅 [nuget 快速入门-使用包](/nuget/quickstart/use-a-package) 。 
  
## <a name="system-requirements"></a>系统要求
  
 SMO 需要 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.0 或 .Net Core 2.0 才能运行，因此使用它的任何应用程序都必须确保客户端计算机安装了该版本或更高版本。 随 NetFx SMO 库一起安装的某些本机二进制文件还需要安装 VC 2013 运行时;该包中不包含该运行时。 你可以从中下载适用于你的目标体系结构的可再发行的 https://www.microsoft.com/download/details.aspx?id=40784
