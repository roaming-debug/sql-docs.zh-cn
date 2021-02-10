---
description: 数据提供程序
title: 数据访问接口 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data providers [ADO]
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 877b9f25-60c4-4ab6-8052-2c28a3849e89
author: rothja
ms.author: jroth
ms.openlocfilehash: 67abe2bd8d2af4e43b1f6d14aed6588b9ddd7a2e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033298"
---
# <a name="data-providers"></a>数据提供程序
数据访问接口表示不同的数据源，例如 SQL 数据库、索引顺序文件、电子表格、文档存储和邮件文件。 提供程序使用称为行集的通用抽象来统一公开数据。  
  
 ADO 的功能强大且灵活，因为它可以连接到多个不同的数据访问接口中的任何一种，并且仍公开相同的编程模型，而与任何给定提供程序的特定功能无关。 但是，由于每个数据提供程序都是唯一的，因此，应用程序与 ADO 交互的方式将因数据访问接口而异。  
  
 例如，用于访问 Microsoft SQL Server 数据库的 SQL Server 的 OLE DB 提供程序的功能和功能与用于访问 Web 服务器上的文件存储的 Microsoft OLE DB 提供程序的功能和用于 Internet 发布的提供程序的功能大不相同。
