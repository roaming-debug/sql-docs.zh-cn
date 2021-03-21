---
title: 安装适用于 SQL Server 的 OLE DB 驱动程序 | Microsoft 文档
description: 安装和卸载 OLE DB Driver for SQL Server。 必须有 msoledbsql.msi 安装程序，才能安装 OLE DB Driver for SQL Server。
ms.custom: ''
ms.date: 02/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, uninstalling
- MSOLEDBSQL, installing
- MSOLEDBSQL, uninstalling
- Setup [OLE DB Driver for SQL Server]
- uninstalling OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server], uninstalling OLE DB Driver for SQL Server
- installing OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server, installing
- data access [OLE DB Driver for SQL Server], installing OLE DB Driver for SQL Server
- removing OLE DB Driver for SQL Server
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f3fa22e03525119a5aa1b66fa81f11c6dc6e9023
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746367"
---
# <a name="installing-ole-db-driver-for-sql-server"></a>安装适用于 SQL Server 的 OLE DB 驱动程序
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

必须有 msoledbsql.msi 安装程序，才能安装 OLE DB Driver for SQL Server。
运行安装程序并选择偏好的选项。 OLE DB Driver for SQL Server 可以与旧版 Microsoft OLE DB 提供程序并行安装。

OLE DB Driver for SQL Server 文件（msoledbsql.dll、msoledbsqlr.rll）安装在 `%SYSTEMROOT%\system32\`。 此外，x64 msoledbsql.msi 将 32 位二进制文件安装在 `%SYSTEMROOT%\SysWOW64\`。

> [!NOTE]  
> OLE DB Driver for SQL Server 的所有相应注册表设置都是在安装过程中进行。  

OLE DB Driver for SQL Server 头文件和库文件（msoledbsql.h 和 msoledbsql.lib）安装在 `%PROGRAMFILES%\Microsoft SQL Server\Client SDK\OLEDB\182\SDK`。 此外，x64 msoledbsql.msi 将相同的文件安装在 `%PROGRAMFILES(x86)%\Microsoft SQL Server\Client SDK\OLEDB\182\SDK`。  

可以通过 msoledbsql.msi 分发 OLE DB Driver for SQL Server。 你可能需要在部署应用程序时安装用于 SQL Server 的 OLE DB 驱动程序。 安装多个包（对于用户而言就像是一次安装）的一种方法就是使用链接器和引导程序技术。 有关详细信息，请参阅[为 Visual Studio 2005 创作自定义引导程序包](/previous-versions/aa730839(v=vs.80))和[添加自定义系统必备](/visualstudio/deployment/creating-bootstrapper-packages)。  
  
x64 msoledbsql.msi 也安装 32 位版本的 OLE DB Driver for SQL Server。 如果应用程序的目标平台不是开发它的平台，可以下载 x64 和 x86 版本 msoledbsql.msi。

在调用 msoledbsql.msi 时，默认情况下仅安装客户端组件。 这些客户端组件即为那些支持运行使用适用于 SQL Server 安装的 OLE DB 驱动程序开发的应用程序的文件。 若还要安装 SDK 组件，请在命令行中指定 `ADDLOCAL=All`。 例如：  

`msiexec /i msoledbsql.msi ADDLOCAL=ALL`  

## <a name="silent-install"></a>无提示安装  
 如果将 /passive、/qn、/qb 或 /qr 选项与 msiexec 一起使用，则必须还指定 IACCEPTMSOLEDBSQLLICENSETERMS=YES，以便显式指示接受最终用户许可协议条款。 必须以全大写字母指定此选项。  

## <a name="installing-ole-db-driver-for-sql-server-as-a-dependency"></a>安装 OLE DB Driver for SQL Server 作为依赖项  
请务必注意，除非卸载所有依赖应用程序，否则不能卸载 OLE DB Driver for SQL Server。 若要向用户显示应用程序依赖 OLE DB Driver for SQL Server 的警告，请使用 MSI 中的 APPGUID 安装选项，如下所示：  

 `msiexec /i msoledbsql.msi APPGUID={0CC618CE-F36A-415E-84B4-FB1BFF6967E1}`  

传递给 APPGUID 的值是您的特定产品代码。 当使用 Microsoft Installer 捆绑应用程序安装程序时，必须创建产品代码。
APPGUID 选项要求，必须从提升的命令提示符运行安装程序。

## <a name="see-also"></a>另请参阅  
 [使用适用于 SQL Server 的 OLE DB 驱动程序生成应用程序](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)