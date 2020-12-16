---
description: MSSQL_ENG020572
title: MSSQL_ENG020572 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG020572 error
ms.assetid: 636566db-ffcf-4109-8c11-15b8c7cb9cd9
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: bcddbd0498a6753b9d6e3e4f8279125229f62dc3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473378"
---
# <a name="mssql_eng020572"></a>MSSQL_ENG020572
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|20572|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|在验证失败之后，订阅服务器“%s”对发布“%s”中项目“%s”的订阅已被重新初始化。|  
  
## <a name="explanation"></a>说明  
 根据发布服务器上的数据对订阅服务器上的数据进行验证，数据不匹配；因此验证失败。 在指定应该执行的验证时，选择了如果验证失败则应重新初始化订阅的选项。 重新初始化订阅包括在订阅服务器上应用新的快照。 有关验证的详细信息，请参阅 [Validate Replicated Data](../../relational-databases/replication/validate-data-at-the-subscriber.md)。  
  
## <a name="user-action"></a>用户操作  
 在订阅服务器上应用了新的快照后，发布服务器和订阅服务器上的数据将会匹配。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
