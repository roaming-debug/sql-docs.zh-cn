---
description: 配置用于安全或不受限制模式的 DataFactory
title: 为安全或不受限制的模式配置 DataFactory |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory configuration in RDS [ADO]
ms.assetid: 8ff24805-dc7a-42ae-b600-5bad0e3f51b8
author: rothja
ms.author: jroth
ms.openlocfilehash: a1727608476b8522f0e9ec6f09ce471de4ffb419
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032041"
---
# <a name="configuring-datafactory-for-safe-or-unrestricted-modes"></a>配置用于安全或不受限制模式的 DataFactory
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 默认情况下，使用 "safe" [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) 配置安装 ADO。 RDS Server 组件的安全模式意味着满足以下条件：  
  
1.  需要使用 RDSServer. DataFactory 的处理程序 (这是注册表项设置) 所规定的。  
  
2.  默认处理程序 msdfmap 已注册，存在于安全处理程序列表中，并标记为默认处理程序。  
  
3.  Msdfmap.ini 文件安装在 Windows 目录中。 在三层模式下使用 RDS 之前，必须根据需要配置此文件。  
  
 还可以配置无限制的 **DataFactory** 安装。 无需自定义处理程序即可直接使用 **DataFactory** 。 用户仍可以通过修改连接字符串来使用自定义处理程序，但这不是必需的。 有关使用 DataFactory 对象的影响的详细信息，请参阅 [保护 RDS 应用程序](./securing-rds-applications.md) **RDSServer** 。  
  
 提供了注册表文件 handsafe，用于为安全配置设置处理程序注册表项。 若要在安全模式下运行，请运行 handsafe。  
  
 运行 handsafe 之后，必须在命令提示符窗口中键入以下命令，在 Web 服务器上停止并重新启动 World Wide Web 发布服务： "NET STOP W3SVC" 和 "NET START W3SVC"。  
  
## <a name="see-also"></a>另请参阅  
 [自定义 DataFactory](./datafactory-customization.md)   
 [RDS 基础知识](./rds-fundamentals.md)