---
title: 查找报表定义架构版本 | Microsoft Docs
description: 了解如何标识报表定义文件的报表定义语言 (RDL) 架构版本。
ms.date: 06/06/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reports
ms.topic: conceptual
helpviewer_keywords:
- XML schemas [Reporting Services]
- Report Definition Language, XML schema
- schemas [Reporting Services]
ms.assetid: 67954419-1b61-4481-a3b9-23b4ba7a5624
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f3e656d797f876fdd40ab1904f153a332673f504
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069452"
---
# <a name="find-the-report-definition-schema-version-ssrs"></a>查找报表定义架构版本 (SSRS)

报表定义文件为用于验证 rdl 文件的报表定义架构的版本指定 RDL 命名空间。 在报表创作环境（例如 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 中的报表设计器、Visual Studio 或报表生成器）中打开 .rdl 文件时。 如果为先前的命名空间创建了报表，则会自动创建一个备份文件，并将报表升级到当前的命名空间。 如果保存升级后的报告定义，则同时还会保存转换后的 .rdl 文件。 这是升级报表定义的唯一方法。 报表定义本身在报表服务器上不升级。 已编译的报表在报表服务器上升级。 有关更多信息，请参见 [Upgrade Reports](../../reporting-services/install-windows/upgrade-reports.md)。  
  
## <a name="how-to-identify-the-rdl-schema-version-of-a-report"></a>如何：确定报表的 RDL 架构版本  
  
1. 在可查看 XML 的应用程序（如记事本或 XML Notepad）中打开报表 .rdl 文件。  
  
     XML REPORT 元素指定架构命名空间。 例如，下面的 Report 元素指定报表设计器的命名空间以及报表定义的命名空间。  
  
    ``` XML 
    <Report xmlns:rd="http://schemas.microsoft.com/SQLServer/reporting/reportdesigner" xmlns="http://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition" xmlns:df="http://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition/defaultfontfamily" MustUnderstand="df">  
    ```  
  
     最新的报表定义命名空间是 2016。 但是，最新发布的报表定义命名空间是 2010，由以下 URL 指定：`https://schemas.microsoft.com/sqlserver/reporting/2010/01/reportdefinition`。
  
### <a name="how-to-identify-the-rdl-schema-version-of-report-designer"></a>如何：确定报表设计器的 RDL 架构版本  
  
1.  打开一个新项目。 您选择的项目版本决定 RDL 架构的版本。 在 SQL Server 中，支持多个架构版本。 有关详细信息，请参阅 [SQL Server Data Tools 中的部署和版本支持](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)。  
  
2.  在 **“项目”** 菜单上，单击 **“添加新项”** 。 **“添加新项”** 对话框将会打开。  
  
3.  在 **“模板”** 窗格中，单击 **“报表”** 。  
  
4.  在 **“名称”** 中，键入报表名称，或接受默认名称。  
  
5.  单击“添加”  。 报表设计器将在“设计”视图中打开一个新的空白报表。  
  
6.  在 **“视图”** 菜单上，单击 **“代码”** 。 该报告定义将显示为一个 XML 文件。  
  
    XML REPORT 元素指定架构命名空间。 例如，下面的 Report 元素指定报表设计器的命名空间以及报表定义的命名空间。  
  
    ``` XML 
    <Report xmlns:rd="http://schemas.microsoft.com/SQLServer/reporting/reportdesigner" xmlns="http://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition" xmlns:df="http://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition/defaultfontfamily" MustUnderstand="df">  
    ```  
  
     以下 URL 指定了报表定义命名空间： `https://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition`  
  
### <a name="how-to-identify-the-rdl-schema-version-on-the-report-server"></a>如何：确定报表服务器上的 RDL 架构版本  
  
-   在 Web 门户中，键入报表服务器的 URL。 例如，以下 URL 指定一个本地计算机上的报表服务器：  
  
     `https://localhost/reportserver/reportdefinition.xsd`  
  
     将在浏览器中打开 .xsd 文件。  
  
     XML schema 元素指定架构命名空间。 例如，下面的架构元素指定三个命名空间：由 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]内部使用的 targetNamespace 引用、架构自身 (xsd) 的 xsd 引用以及报表定义引用。  “年份”  表示报表使用的架构的年份。 例如，2010 或 2016。
  
    ``` XML  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition" targetNamespace="http://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition" elementFormDefault="qualified">  
    ```  
  
     以下 URL 指定了报表定义命名空间： `https://schemas.microsoft.com/sqlserver/reporting/*year*/01/reportdefinition`  

## <a name="next-steps"></a>后续步骤
[升级报表](../../reporting-services/install-windows/upgrade-reports.md)   
[报表定义语言](../../reporting-services/reports/report-definition-language-ssrs.md)   

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
