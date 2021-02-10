---
description: 通讯簿应用程序的系统要求
title: 通讯簿应用程序的系统要求 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: da385405-1c9a-478b-9bf6-fba70015324c
author: rothja
ms.author: jroth
ms.openlocfilehash: 0fa1eb879a536a2958184942c5d779ebb1c998b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036077"
---
# <a name="system-requirements-for-the-address-book-application"></a>通讯簿应用程序的系统要求
若要设置通讯簿示例应用程序，需要满足以下软件和数据库要求：  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="software-requirements"></a>软件要求  
 运行此 Web 应用程序的服务器计算机软件要求包括：  
  
-   Microsoft Windows NT® Server 4.0 Service Pack 3 或更高版本，或者 Microsoft Windows®2000服务器。  
  
-   Microsoft Internet Information Services (IIS) 版本3.0 或更高版本，以及 Active Server Pages。  
  
 运行此 Web 应用程序的客户端计算机软件要求包括：  
  
-   Microsoft Internet Explorer 4.0 或更高版本。  
  
-   Microsoft Windows NT 4.0 工作站或服务器、Windows 2000 或 Microsoft Windows 98。  
  
## <a name="database-requirements"></a>数据库要求  
 若要使用此示例，必须具有：  
  
-   Microsoft® SQL Server 版本6.5 或更高版本的数据库服务器。  
  
-   用于创建数据库并在其中填充示例数据的权限。  
  
-   通过企业管理器或 ISQL 实用程序在 SQL Server 7.0) 中 (名为 Query Analyzer 的已填充数据的验证。  
  
 如果你没有权限，则数据库管理员可能需要设置系统，并向你授予对数据库的访问权限，或者为你设置数据库。  
  
## <a name="see-also"></a>另请参阅  
 [运行通讯簿 SQL 脚本](./running-the-address-book-sql-script.md)   
 [DataControl 对象 (RDS) ](../../reference/rds-api/datacontrol-object-rds.md)   
 [运行通讯簿示例应用程序](./running-the-address-book-sample-application.md)