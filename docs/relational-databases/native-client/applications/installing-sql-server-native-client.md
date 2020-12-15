---
title: 安装
description: SQL Server Native Client 11.0 随 SQL Server 2016 一起安装。 了解组件的安装位置。 还有一个可再发行的安装程序。
ms.custom: ''
ms.date: 07/15/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
author: markingmyname
ms.author: maghan
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client, uninstalling
- SQLNCLI, installing
- SQLNCLI, uninstalling
- Setup [SQL Server Native Client]
- uninstalling SQL Server Native Client
- data access [SQL Server Native Client], uninstalling SQL Server Native Client
- installing SQL Server Native Client
- SQL Server Native Client, installing
- data access [SQL Server Native Client], installing SQL Server Native Client
- removing SQL Server Native Client
ms.assetid: c6abeab2-0052-49c9-be79-cfbc50bff5c1
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e713a0e7867f79fc4e0317151acf9f564f112a33
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469238"
---
# <a name="installing-sql-server-native-client"></a>安装 SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


  在安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]时，将同时安装 Microsoft [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)] Native Client 11.0。 
 
 没有 SQL Server 2016 Native Client。 有关详细信息，请参阅 [SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client.md)。 
 
你还可以从 SQL Server 2012 功能包网页获取 sqlncli.msi。 若要下载最新版本的 SQL Server Native Client，请参阅 [Microsoft® SQL Server®2012功能包](https://www.microsoft.com/download/details.aspx?id=56041)。 如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 计算机上还安装了早于 SQL Server 2012 以前版本的 Native client，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] native client 11.0 将与早期版本并行安装。  
  
  Native Client 文件（sqlncli11.dll、sqlnclir11.rll 和 s11ch_sqlncli.chm）将安装到以下位置：  
  
 `%SYSTEMROOT%\system32\`  
  
> [!NOTE]  
>   Native Client OLE DB 访问接口和  Native Client ODBC 驱动程序的所有相应注册表设置都将在安装过程中完成。  
  
  Native Client 头文件和库文件（sqlncli.h 和 sqlncli11.lib）安装在以下位置：  
  
 `%PROGRAMFILES%\Microsoft SQL Server\110\SDK`  
  
 除了作为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装过程的一部分安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 以外，还可以在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装磁盘的以下位置找到名为 sqlncli.msi 的可再发行安装程序：`%CD%\Setup\`。  
  
 您可以通过 sqlncli.msi 分发 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client。 在您部署某一应用程序时，可能需要安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client。 安装多个包（对于用户而言就像是一次安装）的一种方法就是使用链接器和引导程序技术。 有关详细信息，请参阅[为 Visual Studio 2005 创作自定义引导程序包](/previous-versions/aa730839(v=vs.80))和[添加自定义系统必备](/visualstudio/deployment/creating-bootstrapper-packages)。  
  
 sqlncli.msi 的 x64 和 Itanium 版本也会安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 的 32 位版。 如果您的应用程序所针对的目标平台并非其开发时所使用的平台，则可以从 Microsoft 下载中心下载针对 x64、Itanium 和 x86 的 sqlncli.msi 版本。  
  
 在调用 sqlncli.msi 时，默认情况下只会安装客户端组件。 客户端组件是支持运行使用 Native Client 开发的应用程序的文件 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 若还要安装 SDK 组件，请在命令行中指定 `ADDLOCAL=All`。 例如：  
  
 `msiexec /i sqlncli.msi ADDLOCAL=ALL APPGUID={0CC618CE-F36A-415E-84B4-FB1BFF6967E1}`  
  
## <a name="silent-install"></a>无提示安装  
 如果您将 /passive、/qn、/qb 或 /qr 选项与 msiexec 一起使用，则必须还指定 IACCEPTSQLNCLILICENSETERMS=YES，以便显式指示您接受最终用户许可协议条款。 必须以全大写字母指定此选项。  
  
## <a name="uninstalling-sql-server-native-client"></a>卸载 SQL Server Native Client  
 由于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务器和工具等应用程序 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 依赖于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native client，因此在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 卸载所有从属应用程序之前，不要卸载 Native client。 若要向用户提供应用程序依赖于 Native Client 的警告， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 请使用 MSI 中的 APPGUID 安装选项，如下所示：  
  
 `msiexec /i sqlncli.msi APPGUID={0CC618CE-F36A-415E-84B4-FB1BFF6967E1}`  
  
 传递给 APPGUID 的值是您的特定产品代码。 当使用 Microsoft Installer 捆绑应用程序安装程序时，必须创建产品代码。  
  
## <a name="see-also"></a>另请参阅  
 [构建具有 SQL Server Native Client 的应用程序](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md)   
 [安装操作指南主题](/previous-versions/sql/)  
  
