---
description: 服务提供商和组件
title: 服务提供商和组件 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
- service providers [ADO]
ms.assetid: 1fd7a374-587b-4ca9-9204-3a4019b67a71
author: rothja
ms.author: jroth
ms.openlocfilehash: 11e105becc0d62e315b4d44de9d26c093bb27326
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037017"
---
# <a name="service-providers-and-components"></a>服务提供商和组件
服务提供程序是通过实现不受数据存储本机支持的扩展接口来扩展数据提供程序功能的组件。  
  
 通用数据访问提供了一个 *组件体系结构，该体系结构* 允许单个专用组件在不太适用的存储区之上实现一组不连续的数据库功能或 "服务"。 因此，服务组件可提供任何应用程序在访问任何数据存储时可以使用的公共实现，而不是强制每个数据存储提供自己的扩展功能实现或强制一般应用程序在内部实现数据库功能。 数据存储本身实现了某些功能，而某些功能是通过一般组件实现的，对应用程序而言是透明的。  
  
 例如，游标引擎（如 [OLE DB 的游标服务](/previous-versions/windows/desktop/ms714397(v=vs.85))）是一个服务组件，它可以使用顺序的只进数据存储中的数据来生成可滚动的数据。 ADO 通常使用的其他服务提供程序包括 [Microsoft OLE DB 永久性提供程序 (ADO 服务提供程序)](../../../ado/guide/appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md) (用于将数据保存到 **文件) 、** OLE DB ([ado 服务提供商)  () ado 服务提供商](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)OLE DB ()  (ado 服务提供商) [ado 服务提供](../../../ado/guide/appendixes/microsoft-ole-db-remoting-provider-ado-service-provider.md)商用于在远程计算机上调用数据提供程序。  
  
 有关服务和数据提供程序的详细信息，请参阅 [附录 A： providers](../../../ado/guide/appendixes/appendix-a-providers.md)。