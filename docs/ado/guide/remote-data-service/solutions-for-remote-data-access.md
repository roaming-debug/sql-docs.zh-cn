---
description: 远程数据访问的解决方案
title: 远程数据访问的解决方案 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS [ADO]
ms.assetid: d311cc67-7db7-4c43-9590-d465564695e4
author: rothja
ms.author: jroth
ms.openlocfilehash: 01516d04ce20fc1590ca41985078bcd47f47e47e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031769"
---
# <a name="solutions-for-remote-data-access"></a>远程数据访问的解决方案
## <a name="the-issue"></a>问题  
 使用 ADO，你的应用程序可以直接访问和修改数据源， (有时称为两层系统) 。 例如，如果连接到包含数据的数据源，则这是双层系统中的直接连接。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 但是，你可能希望通过诸如 Microsoft® Internet Information Services (IIS) ，间接访问数据源。 这种安排有时称为三层系统。 IIS 是一个客户端/服务器系统，它为本地应用程序或客户端应用程序通过 Internet 或 intranet 调用远程或服务器程序提供了一种有效的方式。 服务器程序可获得对数据源的访问权限，还可以选择处理获得的数据。  
  
 例如，intranet 网页包含以 Microsoft® Visual Basic Scripting Edition (VBScript) （用于连接到 IIS）编写的应用程序。 IIS 反过来连接到实际数据源、检索数据、以某种方式处理数据，然后将处理的信息返回给应用程序。  
  
 在此示例中，应用程序永远不会直接连接到数据源;IIS 确实如此。 和 IIS 通过 ADO 访问数据。  
  
> [!NOTE]
>  客户端/服务器应用程序不一定基于 Internet 或 intranet (，这是基于 Web 的) -它可能只包含本地局域网上的编译程序。 不过，通常情况下，这是一个基于 Web 的应用程序。  
  
 由于某些可视控件（如网格、复选框或列表）可能会使用返回的信息，因此视觉对象控件必须可以轻松地使用返回的信息。  
  
 你需要一个简单而有效的应用程序编程接口，该接口支持三层系统，并像在两层系统上检索信息一样轻松地返回信息。 远程数据服务 (RDS) 此接口。  
  
## <a name="the-solution"></a>解决方案  
 RDS 定义一种编程模型，该模型是获取和更新数据源所需的活动序列-为了通过中间 (IIS) Internet Information Services 获取对数据的访问权限。 编程模型汇总了 RDS 的全部功能。  
  
## <a name="see-also"></a>另请参阅  
 [基本 RDS 编程模型](./basic-rds-programming-model.md)   
 [RDS 方案](./rds-scenario.md)   
 [RDS 教程](./rds-tutorial.md)   
 [RDS 使用情况和安全性](./rds-usage-and-security.md)