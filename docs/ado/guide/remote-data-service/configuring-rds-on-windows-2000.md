---
description: 在 Windows 2000 上配置 RDS
title: 在 Windows 2000 上配置 RDS |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS configuration [ADO], Windows 2000
ms.assetid: ef37e858-c05f-4f52-a65f-3ce6037e0d03
author: rothja
ms.author: jroth
ms.openlocfilehash: caa8fe846651f65e4316c81f29f2f18078628658
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036567"
---
# <a name="configuring-rds-on-windows-2000"></a>在 Windows 2000 上配置 RDS
如果在升级到 Windows 2000 后，如果遇到使 RDS 正常运行的问题，请按照以下步骤解决此问题：  
  
1.  通过使用 Internet Explorer 导航到 https://服务器，确保 World Wide Web 发布服务正在运行。 如果无法以这种方式访问 Web 服务器，请打开命令提示符并输入以下命令 "NET START W3SVC"。  
  
2.  在 "开始" 菜单中，选择 "运行"。 键入 msdfmap.ini 然后单击 "确定" 以在记事本中打开 msdfmap.ini 文件。 选中 [连接默认值] 部分，如果访问参数设置为 NOACCESS，请将其更改为 READONLY。  
  
3.  使用 RegEdit 实用工具导航到 "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DataFactory\HandlerInfo"，并确保 **HandlerRequired** 设置为0， **DefaultHandler** 为 "" (Null 字符串) 。  
  
     **注意** 如果对注册表的此部分进行了任何更改，则必须通过在命令提示符处输入以下命令来停止并重新启动 World Wide Web 发布服务： "NET STOP W3SVC" 和 "NET START W3SVC"。  
  
4.  使用 RegEdit 实用工具，在注册表中导航到 "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC\Parameters\ADCLaunch"，并验证是否存在名为 **RDSServer. Datafactory** 的键。 如果没有，则创建它。  
  
5.  使用 Internet 服务管理器，找到默认网站并查看 MSADC 虚拟根的属性。 检查目录安全/IP 地址和域名限制。 如果选中 "拒绝访问"，则选择 "已授予"。  
  
 如果最初没有解决问题，请务必尝试重新启动服务器。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件。 将使用 RDS 的应用程序迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="see-also"></a>另请参阅  
 [RDS 基础知识](./rds-fundamentals.md)