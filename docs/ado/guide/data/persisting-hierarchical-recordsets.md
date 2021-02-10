---
description: 保留分层记录集
title: 持久化分层记录集 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO], persisting
- persisting hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: 43798bb5-98a6-4ad6-9bf8-78154b3a1827
author: rothja
ms.author: jroth
ms.openlocfilehash: ddcda766d729e085ed654757d0125f18ec7a696a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037187"
---
# <a name="persisting-hierarchical-recordsets"></a>保留分层记录集
可以通过调用 [save](../../../ado/reference/ado-api/save-method.md)方法，将分层 **记录集** 以 ADTG 或 XML 格式保存到文件。 但是，以 XML 格式保存分层 **记录集** 时，有两个限制：如果分层 **记录集** 包含挂起的更新，则不能以 xml 格式保存，并且不能保存参数化的分层 **记录集**。  
  
 有关数据定形提供程序的详细信息，请参阅 [Microsoft Data 成型 service for OLE DB](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) (ADO) 和 [OLE DB 的数据定形服务概述](/previous-versions/windows/desktop/ms719615(v=vs.85))。  
  
## <a name="see-also"></a>另请参阅  
 [数据定形示例](../../../ado/guide/data/data-shaping-example.md)   
 [正式形状语法](../../../ado/guide/data/formal-shape-grammar.md)   
 [常用 Shape 命令](../../../ado/guide/data/shape-commands-in-general.md)