---
description: 连接到 SQL Server (OracleToSQL)
title: 连接到 SQL Server (OracleToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4ef384ea-5f3e-4f70-ad7c-b62d7b0da628
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: db938314a0c0700c72ac23580d36a5fc0f0c6633
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059340"
---
# <a name="connect-to-sql-server--oracletosql"></a>连接到 SQL Server (OracleToSQL)
使用 " **连接到 SQL Server** " 对话框连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要迁移到的实例。 若要访问 " **连接到 SQL Server** " 对话框，请在 " **文件** " 菜单上，单击 " **连接到 SQL Server**"。  
  
## <a name="options"></a>选项  
**服务器名称**  
输入或选择要连接到 SQL Server 的实例。 默认情况下，将显示您最近连接的实例。  
  
-   如果要连接到本地计算机上的默认实例，则可以输入 **localhost** 或点 (**。**) 。  
  
-   如果要连接到另一台计算机上的默认实例，请输入计算机的名称。  
  
-   如果要连接到另一台计算机上的命名实例，请输入计算机名称、反斜杠和实例名称，例如 *MyServer* \\ *MyInstance*。  
  
**服务器端口**  
如果的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 未配置为接受默认端口 (1433) 上的连接，请输入端口号。 否则，请将此值留空。  
  
**Database**  
指定要将对象和数据迁移到的数据库。 当重新连接到时，此选项不可用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
**身份验证**  
选择用于连接的身份验证方法 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 若要使用当前的 Windows 帐户，请选择 "Windows 身份验证"。 若要指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和密码，请选择 " [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证"。  
  
**用户名**  
如果使用的是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，请输入该实例的登录名 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果使用的是 Windows 身份验证，则此选项不可用。  
  
**密码**  
如果使用的是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，请输入该实例上的登录名的密码 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果使用的是 Windows 身份验证，则此选项不可用。  
  
**加密连接**  
如果希望安全地连接到 SQL Server，请通过选中 " **加密连接** " 复选框来使用加密连接。  
  
**信任服务器证书**  
如果要使用此选项，请选中 " **信任服务器证书** " 复选框。  
  
> [!NOTE]  
> 若要启用 **信任服务器证书**，必须将 "加密" 设置为 **True**。  
  
