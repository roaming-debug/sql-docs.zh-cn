---
description: 注册自定义业务对象
title: 注册自定义业务对象 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- custom business object in RDS [ADO]
- registering custom business objects in RDS [ADO]
- business objects in RDS [ADO]
ms.assetid: e9032ad8-d14c-42e3-ba13-cb5f00084a79
author: rothja
ms.author: jroth
ms.openlocfilehash: 76f3311a622abbd249aa942e1ebdbda33ad06966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036417"
---
# <a name="registering-a-custom-business-object"></a>注册自定义业务对象
若要成功地通过 Web 服务器启动自定义业务对象 () ，必须在注册表中输入业务对象的 ProgID，如本过程中所述。 此 RDS 功能通过仅运行批准可执行文件来保护 Web 服务器的安全性。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
> [!NOTE]
>  对于 MDAC 2.0 和更高版本以及 Windows DAC，默认的业务对象 [RDSServer （DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)）在 MDAC/Windows DAC 安装期间默认情况下未注册。 但是，如果在安装之前将 **RDSServer** 注册为在计算机上执行的安全，则会保留该注册表项用于新的安装。  
  
### <a name="to-register-a-custom-business-object"></a>注册自定义业务对象：  
  
1.  单击 " **开始** "，然后单击 " **运行**"。  
  
2.  键入 **RegEdit** ，然后单击 **"确定"**。  
  
3.  在注册表编辑器中，导航到 **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\W3SVC\Parameters\ADCLaunch** 注册表项。  
  
4.  选择 **ADCLaunch** 项，然后在 " **编辑**" 菜单中，指向 " **新建** "，然后单击 " **项**"。  
  
5.  键入自定义业务对象的 ProgID，并单击 " **Enter**"。 将 **值** 项保留为空。