---
description: 连接到 Microsoft Azure 存储
title: 连接到 Microsoft Azure 存储
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.windowsazurestorage.connect.f1
- SQL13.SWB.WINDOWSAZURESTORAGE.CONNECT.F1
ms.assetid: ''
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/12/2017
ms.openlocfilehash: 073a2a9597fa05d36ee3fe4724dffd7b38284493
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351666"
---
# <a name="connect-to-microsoft-azure-storage"></a>连接到 Microsoft Azure 存储

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
使用“Azure 存储连接”对话框指定存储帐户并验证与 Azure 的连接  。  
  
## <a name="options"></a>选项  
指定以下有关 Azure 帐户的信息，然后选择“下一步”继续。  
  
1.  **存储帐户** - 指定存储帐户名称。

   >[!NOTE]
   > 只能连接到[常规用途存储帐户](/azure/storage/common/storage-introduction#azure-storage-services)。 连接到其他类型的存储帐户可能会导致如下错误：
   >
   >  其中一个 HTTP 标头的值格式不正确。 (Microsoft.SqlServer.StorageClient)。
   >
   >  远程服务器返回错误：(400)错误的请求。 （系统）

2.  **帐户密钥** - 为指定的存储帐户指定帐户密钥。  
  
3.  **使用安全终结点 (HTTPS)** - 此选项利用网络 Web 服务器的加密通信和安全标识。  
  
4.  **保存帐户密钥** - 此选项可将密码保存在加密文件中。  
