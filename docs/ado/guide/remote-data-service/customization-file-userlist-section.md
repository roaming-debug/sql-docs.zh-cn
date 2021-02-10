---
description: 自定义文件 UserList 部分
title: 自定义文件 UserList 部分 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- UserList section in rds [ADO]
- customization file in RDS [ADO]
ms.assetid: 42e8ec20-eaac-4a95-8cb8-4bba93a75bcb
author: rothja
ms.author: jroth
ms.openlocfilehash: 4ca0d2e0a2bff2e5f9a3ea057220ac8cda14c9af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036507"
---
# <a name="customization-file-userlist-section"></a>自定义文件 UserList 部分
**Userlist** 部分适用于具有相同节 *标识符* 参数的 **connect** 部分。  
  
 此部分可以包含一个 *用户访问条目*，该条目指定指定用户的访问权限，并覆盖匹配 **连接** 部分中的 *默认**访问条目*。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
 用户访问条目的格式为：  
  
 _用户名_**=**   
 **_accessRights_**  
  
|部分|说明|  
|----------|-----------------|  
|*userName*|采用此连接的人员的 *用户名* 。 有效用户名与 IIS **Service Manager** 对话框建立在一起。|  
|**_accessRights_**|以下访问权限之一：<br /><br /> -   **NoAccess** -用户无法访问数据源。<br />-   **ReadOnly** -用户可以读取数据源。<br />-   **ReadWrite** -用户可以读取或写入数据源。|  
  
## <a name="see-also"></a>另请参阅  
 [自定义文件连接部分](./customization-file-connect-section.md)   
 [自定义文件日志部分](./customization-file-logs-section.md)   
 [自定义文件 SQL 部分](./customization-file-sql-section.md)   
 [自定义 DataFactory](./datafactory-customization.md)   
 [必需的客户端设置](./required-client-settings.md)   
 [了解自定义文件](./understanding-the-customization-file.md)   
 [编写自己的自定义处理程序](./writing-your-own-customized-handler.md)