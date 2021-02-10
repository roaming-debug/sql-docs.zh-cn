---
description: 配置 RDS
title: 配置 RDS |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS configuration [ADO]
ms.assetid: 5dd48483-858a-48c2-98ce-f2359abe1f59
author: rothja
ms.author: jroth
ms.openlocfilehash: 1af0a8c1fd4c54f73ad0751e4ac4e93b45a04783
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032029"
---
# <a name="configuring-rds"></a>配置 RDS
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 若要高效地实现 RDS，请确保熟悉可用的各种配置。 本部分包含有关实现 RDS 的安全性和可伸缩性的重要信息。 有关将计算机配置为使用 RDS 的信息，请参阅以下主题。  
  
-   [授予 Web 服务器计算机来宾特权](./granting-guest-privileges-to-a-web-server-computer.md)  
  
-   [注册自定义业务对象](./registering-a-custom-business-object.md)  
  
-   [将业务对象标记为“可安全编写脚本”](./marking-business-objects-as-safe-for-scripting.md)  
  
-   [在用于 DCOM 的客户端中注册业务对象](./registering-business-objects-on-the-client-for-use-with-dcom.md)  
  
-   [设置 DCOM 流封送格式](./setting-dcom-stream-marshaling-format.md)  
  
-   [允许 DCOM 在 DLL 上运行](./enabling-a-dll-to-run-on-dcom.md)  
  
-   [在 IIS 上配置虚拟服务器](./configuring-virtual-servers-on-iis.md)  
  
-   [保证 RDS 应用程序的安全](./securing-rds-applications.md)  
  
-   [配置用于安全或不受限制模式的 DataFactory](./configuring-datafactory-for-safe-or-unrestricted-modes.md)  
  
## <a name="see-also"></a>另请参阅  
 [使用 RDS 的相关技术](./using-related-technologies-with-rds.md)   
 [自定义 DataFactory](./datafactory-customization.md)   
 [RDS 故障排除](./troubleshooting-rds.md)