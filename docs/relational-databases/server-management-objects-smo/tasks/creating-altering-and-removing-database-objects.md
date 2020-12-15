---
description: 创建、更改和删除数据库对象
title: 使用数据库对象
ms.custom: seo-dt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- database objects [SMO]
- objects [SMO]
ms.assetid: 702fd63d-8734-4a02-872e-aecfb037c787
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f2ae7dbbfc762ed342ddc085afcb799f9b292966
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468498"
---
# <a name="creating-altering-and-removing-database-objects"></a>创建、更改和删除数据库对象
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  创建 SMO 对象的各个阶段如下：  
  
1.  创建对象的实例。  
  
2.  设置对象属性。  
  
3.  创建子对象的实例。  
  
4.  设置子对象属性。  
  
5.  创建对象。  

 当在 SMO 应用程序中创建 SMO 对象的实例时，它们在的实例上不存在， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 直到发出 **Create** 方法。 但是，不需要为每个单独的对象发出 **Create** 方法。 如果对象具有一组子对象，则只需父对象即可运行 **Create** 方法。 例如，表定义要求它至少包含一个列才能存在。 并且，列不能独立于表存在。 表与其列之间存在相互依赖的关系。  
  
 使用 <xref:Microsoft.SqlServer.Management.Dmf.Policy.Alter%2A> 方法可以更改对象。 对对象的若干更改（例如，向对象的集合之一添加子对象或更改属性值）将组成一批共同运行。 **Alter** 方法减少了网络流量，并提高了整体性能。  
  
 **Drop** 语句用于删除最初创建该对象所需的对象及其所有相互依赖子对象。  
  
## <a name="see-also"></a>另请参阅  
 [SMO 对象模型](../../../relational-databases/server-management-objects-smo/smo-object-model.md)  
  
  
