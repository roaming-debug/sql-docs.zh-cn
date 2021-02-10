---
description: Internet 服务器错误：拒绝访问
title: Internet 服务器错误：访问被拒绝 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- access denied error in RDS [ADO]
ms.assetid: e5b43cfa-da8d-430d-a2ab-5443dda47a16
author: rothja
ms.author: jroth
ms.openlocfilehash: f0fa2a2e3ee7d28bab239980710440cbd375d52a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036477"
---
# <a name="internet-server-error-access-denied"></a>Internet 服务器错误：拒绝访问
如果收到此错误，则通常意味着 Microsoft Internet Information Services (IIS) 返回以下状态：  
  
 HTTP_STATUS_DENIED 401  
  
 请确保 IIS 访问的目录具有适当的权限。 RDS 可以与在以下三种密码身份验证模式之一中运行的 IIS Web 服务器进行通信：匿名、基本或 NT 质询/响应 (称为 Windows 2000) 中的集成 Windows 身份验证。 此外，如果 Web 服务器是 Windows NT/Windows 2000 计算机，则它必须具有对数据源计算机的权限。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="see-also"></a>另请参阅  
 [RDS 基础知识](./rds-fundamentals.md)