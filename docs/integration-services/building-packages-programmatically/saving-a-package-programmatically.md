---
description: 以编程方式保存包
title: 以编程方式保存包 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- programmatically saving a package
- saving a package programmatically
ms.assetid: 4204f817-d5df-475a-9338-d7f01305d566
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4174a45f7dabab28385f46f10e0490f5aa21f812
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355000"
---
# <a name="saving-a-package-programmatically"></a>以编程方式保存包

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  以编程方式生成新包或修改现有包后，通常要保存更改。  
  
 本主题中用于保存包的所有方法都需要引用 Microsoft.SqlServer.ManagedDTS 程序集。 在新项目中添加引用后，请使用 **using** 或 **Imports** 语句导入 <xref:Microsoft.SqlServer.Dts.Runtime> 命名空间。  
  
## <a name="saving-a-package-programmatically"></a>以编程方式保存包  
 若要以编程方式保存包，可调用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] <xref:Microsoft.SqlServer.Dts.Runtime.Application> 类的以下方法之一：  
  
|存储位置|调用的方法|  
|----------------------|--------------------|  
|文件|<xref:Microsoft.SqlServer.Dts.Runtime.Application.SaveToXml%2A>|  
|SSIS 包存储区|<xref:Microsoft.SqlServer.Dts.Runtime.Application.SaveToDtsServer%2A>|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|<xref:Microsoft.SqlServer.Dts.Runtime.Application.SaveToSqlServer%2A><br /><br /> 或<br /><br /> <xref:Microsoft.SqlServer.Dts.Runtime.Application.SaveToSqlServerAs%2A>|  
  
> [!IMPORTANT]  
>  <xref:Microsoft.SqlServer.Dts.Runtime.Application> 类中用于处理 SSIS 包存储区的方法只支持“.”或本地服务器的服务器名称。 不能使用“(local)”或“localhost”。  
  
## <a name="see-also"></a>另请参阅  
 [保存包](../../integration-services/save-packages.md)  
  
  
