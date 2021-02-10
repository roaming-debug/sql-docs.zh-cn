---
description: XML 安全注意事项
title: XML 安全注意事项 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML security in ADO
ms.assetid: fadbd38e-6e7b-4b81-96ea-85169c664374
author: rothja
ms.author: jroth
ms.openlocfilehash: 82557d241ff804ec2da6f97a1dc96073da2a2b7d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036707"
---
# <a name="xml-security-considerations"></a>XML 安全注意事项
Recordset 对象上 ADO Save 和 Open 方法不被视为在 Internet Explorer 中运行的安全操作。 因此，如果在浏览器承载的应用程序或控件中运行的脚本代码中使用这些方法，浏览器的安全配置将对其行为产生影响。  
  
 Internet Explorer 5 默认情况下，在 Internet 区域中为此类操作提供安全限制。 在该配置下，记录集无法对客户端上的本地文件系统进行任何访问，也无法访问从中下载该页的服务器的域之外的任何数据源。 具体而言，在浏览器宿主内运行时，仅当记录集位于下载该页的同一服务器上时，才能将该记录集保存回文件中。 同样，仅当记录集位于从中下载页面的同一服务器上时，才能使用该文件从文件中加载该记录集。  
  
## <a name="see-also"></a>另请参阅  
 [以 XML 格式保留记录](./persisting-records-in-xml-format.md)