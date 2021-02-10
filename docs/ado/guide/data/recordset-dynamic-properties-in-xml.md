---
description: XML 中的记录集动态属性
title: XML 中的记录集动态属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset dynamic properties in XML [ADO]
ms.assetid: 52f8e379-812a-4db8-9210-94458926301c
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e966c3570ce8f9589ede5f60fca1fc0fa70bbac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037087"
---
# <a name="recordset-dynamic-properties-in-xml"></a>XML 中的记录集动态属性
以下记录集提供程序特定属性 (来自客户端游标引擎) 当前已保留为 XML 格式：  
  
-   更新重新同步  
  
-   唯一表  
  
-   唯一架构  
  
-   唯一目录  
  
-   重新同步命令  
  
-   IrowsetChange  
  
-   IRowsetUpdate  
  
-   CommandTimeout  
  
-   BatchSize  
  
-   UpdateCriteria  
  
-   调整名称  
  
-   AutoRecalc  
  
 这些属性保存在 schema 节中，作为要保存的记录集的元素定义的属性。 这些属性是在行集架构命名空间中定义的，并且必须具有前缀 "rs："。  
  
## <a name="see-also"></a>另请参阅  
 [以 XML 格式保留记录](../../../ado/guide/data/persisting-records-in-xml-format.md)
