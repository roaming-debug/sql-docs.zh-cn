---
description: 将业务对象标记为“可安全编写脚本”
title: 将业务对象标记为安全的脚本 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- business objects in RDS [ADO]
ms.assetid: 0be98d1a-ab3d-4dce-a166-dacda10d154a
author: rothja
ms.author: jroth
ms.openlocfilehash: 1866d72f438e263ec6cdb1f66cccb247d460a270
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031919"
---
# <a name="marking-business-objects-as-safe-for-scripting"></a>将业务对象标记为“可安全编写脚本”
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 若要帮助确保安全的 Internet 环境，需要标记使用 RDS 实例化的任何业务对象 [。空间](../../reference/rds-api/dataspace-object-rds.md) 对象的 [CreateObject](../../reference/rds-api/createobject-method-rds.md) 方法为 "安全编写脚本"。 你需要确保在系统注册表的 "许可证" 区域中将它们标记为这样，然后才能在 DCOM 中使用它们。  
  
> [!NOTE]
>  标记为 "可安全执行脚本" 的业务对象或用于初始化的安全对象可以通过网络上的任何人进行实例化和初始化。 将业务对象标记为 "可安全执行脚本" 并不安全。 至关重要的一点是，确保使用最高的安全性对业务对象进行编码，以确保此类对象不会为敏感数据提供未受保护的访问点。  
  
 若要将业务对象手动标记为可安全编写脚本，请创建一个扩展名为 .reg 的文本文件，该文件包含以下文本。 在此示例中， \<*MyActiveXGUID*> 是业务对象的十六进制 GUID 编号。 以下两个数字启用了 "安全脚本编写" 功能：  
  
```console
[HKEY_CLASSES_ROOT\CLSID\<MyActiveXGUID>\Implemented   
Categories\{7DD95801-9882-11CF-9FA9-00AA006C42C4}]  
[HKEY_CLASSES_ROOT\CLSID\<MyActiveXGUID>\Implemented   
Categories\{7DD95802-9882-11CF-9FA9-00AA006C42C4}]  
```  
  
 保存该文件，并使用注册表编辑器或在 Windows 资源管理器中双击 .reg 文件，将其合并到注册表中。  
  
 通过包和部署向导，可以将在 Microsoft Visual Basic 中创建的业务对象自动标记为 "可安全执行脚本"。 当向导要求你指定安全设置时，选择 " **安全" 进行初始化** ，并选择 " **安全" 进行脚本编写**。  
  
 在最后一个步骤中，应用程序安装向导将创建一个 .htm 和一个 .cab 文件。 然后，你可以将这两个文件复制到目标计算机，然后双击 .htm 文件以加载页面并正确注册服务器。  
  
 因为默认情况下会在 Windows\System32\Occache 目录中安装业务对象，请将其移动到 Windows\System32 目录，并更改 **HKEY_CLASSES_ROOT\CLSID\\** \<*MyActiveXGUID*> \\ **InprocServer32** 注册表项以匹配正确的路径。