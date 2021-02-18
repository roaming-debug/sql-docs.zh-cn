---
title: 简单 XML 输入文件示例 (DTA)
description: 本文包含用于优化工作负荷以用于数据库引擎优化顾问的 XML 输入文件示例。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- sample applications [DTA]
ms.assetid: 5b00e4eb-1742-43ec-98d8-d84216b6b840
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 1b475ce68b38283903ecd7295a89b18fce6b9636
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354411"
---
# <a name="simple-xml-input-file-sample-dta"></a>简单 XML 输入文件示例 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

将用于优化工作负荷的这一简单 XML 输入文件示例复制并粘贴到您喜爱的 XML 编辑器或文本编辑器中。 然后将为 **Server** **Database** **Schema**、**Table**、**Workload** 和 **TuningOptions** 元素指定的值替换为你的特定优化会话的值。 有关可以与这些元素一起使用的属性和子元素的详细信息，请参阅 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)。 以下示例只使用了部分可用属性和子元素选项。  
  
## <a name="code"></a>代码  
 [!code-xml[InputFileSamples#SimpleXMLInputFile](../../tools/dta/codesnippet/xml/simple-xml-input-file-sa_1.xml)]  
  
## <a name="see-also"></a>另请参阅  
 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
