---
title: DTAXML 元素 (DTA)
description: 在 dta 实用工具中，DTAXML 元素包含用于描述可优化数据库引擎优化顾问生成的输入和输出的所有元素。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- DTAXML element
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 8d82b1d9198946d9ef7c14e80d443d151a7154d3
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812846"
---
# <a name="dtaxml-element-dta"></a>DTAXML 元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

数据库引擎优化顾问 XML 输入文件或输出文件的根元素 **DTAXML** 包含用于对数据库引擎优化顾问生成的优化输入和优化输出进行说明的所有元素。  
  
## <a name="syntax"></a>语法  
  
```  
  
<DTAXML   
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/dta">  
    ...code removed here...  
</DTAXML>  
```  
  
## <a name="element-attributes"></a>元素属性  
  
|Attribute|说明|  
|---------------|-----------------|  
|**xmlns: xsi**|必需。 标识 XML 架构实例命名空间。 可以使用此命名空间中的属性来引用用于验证数据库引擎优化顾问 XML 文件的架构。<br /><br /> 必需的值：[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)|  
|**xmlns**|必需。 标识数据库引擎优化顾问命名空间。<br /><br /> 如果在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中使用 XML 编辑器编辑数据库引擎优化顾问 XML 文件，则“F1 帮助”和“动态帮助”将使用此值在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中定位可能的引用主题。<br /><br /> 必需的值：<br /><br /> [Database Engine Tuning Advisor XML Schema](https://go.microsoft.com/fwlink/?LinkId=43100) （数据库引擎优化顾问 XML 架构）命名空间|  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|无。|  
|**默认值**|无。|  
|**出现次数**|每个 DTA XML 文件必须出现一次。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|无|  
|**子元素**|[DTAInput 元素 (DTA)](../../tools/dta/dtainput-element-dta.md)<br /><br /> **DTAOutput** 元素（有关信息，请参阅 [数据库引擎优化顾问 XML 架构](https://schemas.microsoft.com/sqlserver/)）|  
  
## <a name="remarks"></a>备注  
 有关 XML 命名空间的详细信息，请查看[“XML 中的命令空间”文档](/dotnet/standard/data/xml/managing-namespaces-in-an-xml-document)。 
  
## <a name="example"></a>示例  
 有关典型 **DTAXML** 元素的示例，请参阅 [XML 输入文件实例 (DTA)](../../tools/dta/xml-input-file-samples-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)   
 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)  
  
