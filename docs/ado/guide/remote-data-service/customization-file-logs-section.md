---
description: 自定义文件 Logs 部分
title: 自定义文件日志部分 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- logs section in RDS [ADO]
- customization file in RDS [ADO]
ms.assetid: a368e264-865c-41ee-be00-d9097255c2ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 34485c91a20b8c5df668e5eddd11e442418844a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036527"
---
# <a name="customization-file-logs-section"></a>自定义文件 Logs 部分
" **日志** " 部分包含一个日志文件条目，该条目指定在 **DataFactory** 操作过程中记录错误的文件的名称。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
 日志文件条目的格式如下：  
  
```console
  
err=  
FileName  
  
```  
  
## <a name="remarks"></a>备注  
  
|部分|说明|  
|----------|-----------------|  
|**err**|指示这是一个日志文件项的文字字符串。|  
|*FileName*|完整的路径和文件名。 典型文件名为 **c:\msdfmap.log**。|  
  
 日志文件将包含每个错误的用户名、HRESULT、日期和时间。  
  
## <a name="see-also"></a>另请参阅  
 [自定义文件连接部分](./customization-file-connect-section.md)   
 [自定义文件 SQL 部分](./customization-file-sql-section.md)   
 [自定义文件 UserList 部分](./customization-file-userlist-section.md)   
 [自定义 DataFactory](./datafactory-customization.md)   
 [必需的客户端设置](./required-client-settings.md)   
 [了解自定义文件](./understanding-the-customization-file.md)   
 [编写自己的自定义处理程序](./writing-your-own-customized-handler.md)