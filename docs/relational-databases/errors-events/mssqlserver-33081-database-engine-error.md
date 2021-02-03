---
description: MSSQLSERVER_33081
title: MSSQLSERVER_33081 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 33081 (Database Engine error)
ms.assetid: 839705e7-fa37-4c0d-9f3f-95a9eab98bcf
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8be9f2bec59b51f75a7832768663535175946f60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208413"
---
# <a name="mssqlserver_33081"></a>MSSQLSERVER_33081
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|33081|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SEC_DLL_TRUST_VERIFICATION_FAILED|  
|消息正文|由于 Authenticode 签名或文件路径无效，未能加载加密提供程序“%.*ls”。  请检查以前的消息，了解其他失败信息。|  
  
## <a name="explanation"></a>说明  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法加载错误消息中列出的加密提供程序。 几个 Windows API 失败可能导致此错误。 环形缓冲区包含失败的 API 的名称和 API 返回的 Windows 错误代码。 若要获取更多信息，请使用以下查询来查询 sys.dm_os_ring_buffers 视图。  
  
```  
SELECT * FROM sys.dm_os_ring_buffers   
WHERE ring_buffer_type = 'RING_BUFFER_SECURITY_ERROR';  
```  
  
## <a name="user-action"></a>用户操作  
若要调查问题，请在 MSDN (https://msdn.microsoft.com/) 中搜索该 Windows 错误代码。 解决该问题，或联系 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 客户支持服务部门以获得详细信息。 如果需要与客户支持服务部门联系，请为我们的支持人员收集以下信息。  
  
-   错误日志显示无法加载加密提供程序错误。  
  
-   执行以下语句所得到的输出：  
  
    ```  
    SELECT * FROM sys.dm_os_ring_buffers   
    WHERE ring_buffer_type = 'RING_BUFFER_SECURITY_ERROR';  
    ```  
  
> [!NOTE]  
> 尝试立即收集环形缓冲区信息，因为重新启动期间该信息会丢失。  
  
