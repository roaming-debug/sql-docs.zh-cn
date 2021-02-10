---
description: RDS 返回 " &quot; 未读取流" &quot; 错误
title: RDS 返回 " &quot; 未读取流" &quot; 错误 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stream not read error in RDS [ADO]
ms.assetid: cb5a68f8-dba4-41da-bafd-04efe53706b7
author: rothja
ms.author: jroth
ms.openlocfilehash: 2aba423ab0861b8e97a298b88e6016db1c759b19
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031879"
---
# <a name="rds-returns-quotstream-not-readquot-error"></a>RDS 返回 " &quot; 未读取流" &quot; 错误
"无法读取流对象，因为它是空的，或当前位置位于流的末尾。 对于非空流，请将当前位置设置为 Position 属性。 若要确定流是否为空，请检查 "大小" 属性。  
  
 如果看到此错误消息，则可能已尝试对 http 使用参数化分层查询。 RDS 不允许使用远程参数化层次结构。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="see-also"></a>另请参阅  
 [RDS 基础知识](./rds-fundamentals.md)