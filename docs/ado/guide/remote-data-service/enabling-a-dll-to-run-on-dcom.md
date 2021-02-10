---
description: 允许 DCOM 在 DLL 上运行
title: 使 DLL 在 DCOM 上运行 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DLL on DCOM in RDS [ADO]
- DCOM in RDS [ADO]
- business objects in RDS [ADO]
ms.assetid: 5f1c2205-191c-4fb4-9bd9-84c878ea46ed
author: rothja
ms.author: jroth
ms.openlocfilehash: 31aa1fde6ddce21bc314bf14aea3ea563567d4a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031969"
---
# <a name="enabling-a-dll-to-run-on-dcom"></a>允许 DCOM 在 DLL 上运行
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 以下步骤概述了如何通过组件服务使业务对象 .dll 同时使用 DCOM 和 Microsoft Internet Information Services (HTTP) 。  
  
1.  在组件服务 MMC 管理单元中创建新的空包。  
  
     你将使用 "组件服务" MMC 管理单元来创建包，并将 DLL 添加到此包中。 这使得 .dll 可通过 DCOM 访问，但它会删除通过 IIS 的可访问性。  (如果签入 .dll 的注册表， **Inproc** 键现在为空，则为。设置激活属性（在本主题后面部分介绍）将在 **Inproc** 项中添加一个值。 )   
  
2.  将业务对象安装到包中。  
  
     或  
  
     将 [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) 对象导入到包中。  
  
3.  **在创建者的进程中** 将包的激活特性设置 (库应用程序) 。  
  
     若要使 .dll 可通过 DCOM 和 IIS 在同一台计算机上访问，必须在 "组件服务" MMC 管理单元中设置组件的 "激活" 属性。 **在创建者的进程中** 将属性设置为之后，你会注意到注册表中的 **Inproc** 服务器键已添加到 "组件服务" 代理项。  
  
 有关组件服务 (或 Microsoft Transaction Service 的详细信息，如果使用的是 Windows NT) 以及如何执行这些步骤，请访问 Microsoft Transaction Server 网站。