---
description: 自定义文件 Connect 部分
title: 自定义文件连接部分 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connect section in RDS [ADO]
- customization file in RDS [ADO]
ms.assetid: d50eb3cc-a822-486f-b80b-65bb50547ecd
author: rothja
ms.author: jroth
ms.openlocfilehash: 0b3508479d6d52799f286421c1bb2c67683088f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032009"
---
# <a name="customization-file-connect-section"></a>自定义文件 Connect 部分
处理程序的默认行为是拒绝所有连接。 " **连接** " 部分指定该行为的例外情况。 例如，如果所有 **连接** 部分都不存在或为空，则默认情况下无法建立连接。  
  
 " **连接** " 部分可以包含：  
  
-   指定在此连接上允许的默认读写操作的默认访问项。 如果部分中没有默认访问项，则将忽略该部分。  
  
-   替换客户端连接字符串的新连接字符串。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
 默认访问项的格式为：  
  
```console
  
Access=  
accessRight  
  
```  
  
 替换连接字符串条目的格式如下：  
  
```console
  
Connect=  
connectionString  
  
```  
  
## <a name="remarks"></a>备注  
  
|部分|说明|  
|----------|-----------------|  
|**“连接”**|指示这是一个连接字符串项的文字字符串。|  
|**_connectionString_**|替换整个客户端连接字符串的字符串。|  
|**访问**|指示这是一个访问项的文字字符串。|  
|**_accessRight_**|以下访问权限之一：<br /><br /> -   **NoAccess** -用户无法访问数据源。<br />-   **ReadOnly** -用户可以读取数据源。<br />-   **ReadWrite** -用户可以读取或写入数据源。|  
  
 如果要允许 (有效的连接，请) 禁用默认处理程序行为，将 "**连接默认值**" 部分中的 "访问" 项设置为 `Access=ReadWrite` ，并删除或注释掉任何其他 **连接**_标识符_ 部分。  
  
## <a name="see-also"></a>另请参阅  
 [自定义文件日志部分](./customization-file-logs-section.md)   
 [自定义文件 SQL 部分](./customization-file-sql-section.md)   
 [自定义文件 UserList 部分](./customization-file-userlist-section.md)   
 [自定义 DataFactory](./datafactory-customization.md)   
 [必需的客户端设置](./required-client-settings.md)   
 [了解自定义文件](./understanding-the-customization-file.md)   
 [编写自己的自定义处理程序](./writing-your-own-customized-handler.md)