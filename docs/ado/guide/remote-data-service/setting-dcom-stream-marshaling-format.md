---
description: 设置 DCOM 流封送格式
title: 设置 DCOM 流封送处理格式 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- dcom stream marshaling format in rds [ADO]
ms.assetid: 46664ac5-d6e6-4457-8bae-3a98300f2a41
author: rothja
ms.author: jroth
ms.openlocfilehash: 8286ae626f4c505daa4b74e31dcc72a49e12d677
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036367"
---
# <a name="setting-dcom-stream-marshaling-format"></a>设置 DCOM 流封送格式
使用 RDS 1.5 或更低版本中的组件的客户端计算机与使用 RDS 2.0 或更高版本中的组件的服务器不兼容。 使用 DCOM 作为基础协议时，对 RDS 2.0 或更高版本的支持在传输 [Recordset](../../reference/ado-api/recordset-object-ado.md) 对象时更有效。 如果客户端运行的是 RDS 1.5 或更早版本中的组件，则可以将服务器设置为与以前的 RDS 支持 (（称为 RDS 1.0) ）或更高版本的 RDS 支持 (称为 RDS 2.0 或更高版本) 。 设置以下注册表项之一：  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
```console
[HKEY_CLASSES_ROOT]  
\CLSID\[58ECEE30-E715-11CF-B0E3-00AA003F000F}\ADTGOptions]"MarshalFormat"="RDS10"  
```  
  
 或  
  
```console
[HKEY_CLASSES_ROOT]  
\CLSID\[58ECEE30-E715-11CF-B0E3-00AA003F000F}\ADTGOptions]"MarshalFormat"="RDS20"  
```