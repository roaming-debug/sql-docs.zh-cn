---
description: 了解为何验证用户输入对于保护应用程序免受 SQL 注入攻击至关重要。
title: 验证用户输入
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 8aa867b0-e6f0-49eb-93d3-817ae2ed8f77
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 00aa7d14354ca29859806611550540674a798e48
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673929"
---
# <a name="validating-user-input"></a>验证用户输入

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

当您构造访问数据的应用程序时，应假定所有用户输入在获得验证之前都是恶意的。 如果未能实现此目的，则可能会使您的应用程序易于受到攻击。 可能发生的一种类型的攻击称作 SQL 注入，其中恶意代码被添加到字符串中，然后这些字符串被传递到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中以进行分析和执行。 为了避免这种类型的攻击，应尽可能使用带参数的存储过程，并始终验证用户输入。

在客户端代码中验证用户输入是非常重要的，这样，您就无需浪费时间往返服务器。 在服务器上验证存储过程的参数以捕获无效的和跳过客户端验证的输入，这同样非常重要。

有关 SQL 注入和如何避免此攻击的详细信息，请参阅 [SQL 注入](../../relational-databases/security/sql-injection.md)。 有关验证存储过程参数的详细信息，请参阅[存储过程](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)以及相关文章。

## <a name="see-also"></a>另请参阅

[保护 JDBC 驱动程序应用程序](securing-jdbc-driver-applications.md)
