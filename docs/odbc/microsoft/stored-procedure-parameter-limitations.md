---
description: 存储过程参数限制
title: 存储过程参数限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- stored procedures [ODBC], ODBC driver for Oracle
- ODBC driver for Oracle [ODBC], stored procedures
ms.assetid: 8b804bcf-4cce-4e6f-aa45-00bab9ef9921
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c92ff03bf5cf8899706966169c9b3c770f64f57c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188953"
---
# <a name="stored-procedure-parameter-limitations"></a>存储过程参数限制
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 请改用 Oracle 提供的 ODBC 驱动程序。  
  
 运行使用10个或更多个输出参数的 Oracle 存储过程时，存储过程调用将失败，从而导致访问冲突或 ActiveX 数据对象 (ADO) 错误。 当使用适用于 Oracle 的 Microsoft ODBC 驱动程序和 Oracle 客户端软件的版本8.0.4.0.0 和8.0.4.0.4 时，就会发生这种情况。  
  
 若要解决此问题，必须将 Oracle 客户端软件升级到版本8.0.4.2.0 或更高版本。 有关 [修补程序](../../odbc/microsoft/oracle-software-patches.md)的详细信息，请与 Oracle 公司联系。  
  
> [!NOTE]  
>  Oracle 客户端软件版本8.0.3.0.0 的早期版本不会出现此问题。
