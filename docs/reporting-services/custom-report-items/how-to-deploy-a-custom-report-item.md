---
title: 如何：部署自定义报表项 | Microsoft Docs
description: 了解部署自定义报表项。 你将修改报表服务器配置文件，并将组件程序集复制到相应的文件夹。
ms.date: 03/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items, deploying
ms.assetid: 80e97b0d-e355-4240-aebd-08cbc84089ed
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 240be4e7d05e4daac27e15bf5f8a99ec6bc27563
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061902"
---
# <a name="how-to-deploy-a-custom-report-item"></a>如何：部署自定义报表项
  若要在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中部署自定义报表项，必须修改报表服务器配置文件，并将设计时和运行时组件程序集复制到报表设计器和报表服务器的相应应用程序文件夹中。  
  
### <a name="to-deploy-a-custom-report-item"></a>部署自定义报表项  
  
1.  编辑 Rsreportdesigner.config 文件，以配置供在相应设计器中使用的自定义报表项运行时组件和设计时组件。 注意：ReportItemName 条目必须匹配 CustomReportItemDesigner 类中所用的 CustomReportItemAttribute 属性    。 例如：  
  
    ```  
    <ReportItems>  
       <ReportItem Name="Polygons" Type="PolygonsCRI.PolygonsCRI,PolygonsCRI"/>  
    </ReportItems>  
    <ReportItemDesigner>  
       <ReportItem Name="Polygons" Type="PolygonsCRI.PolygonsDesigner, PolygonsDesigner" />  
    </ReportItemDesigner>  
    <ReportItemConverter>  
       <Converter Source="Chart" Target="Polygons" Type="PolygonsCRI.PolygonsConverter, PolygonsDesigner" />  
    </ReportItemConverter>  
    ```  
  
2.  编辑 Rsreportserver.config 文件以注册自定义报表项运行时组件。 例如：  
  
    ```  
    <ReportItems>  
       <ReportItem Name="Polygons" Type="PolygonsCRI.PolygonsCRI,PolygonsCRI"/>  
    </ReportItems>  
    ```  
  
3.  编辑 Rsssrvpolicy.config 文件以添加可向相应自定义报表项授予合适权限的 CodeGroup  。 例如：  
  
    ```  
    <CodeGroup   
       class="UnionCodeGroup"   
       version="1"   
       PermissionSetName="FullTrust"  
       Description="This code group grants MyCustomReportItem.dll FullTrust permission. ">  
       <IMembershipCondition   
          class="UrlMembershipCondition"  
          version="1"  
       Url="C:\Program Files\Microsoft SQL Server\ MSRS10_50.SQLSERVER\Reporting Services\ReportServer\bin\MyCustomReportItem.dll" />  
    </CodeGroup>  
    ```  
  
4.  将相应自定义报表项运行时组件 DLL 复制到 %ProgramFiles%\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies 和 \Program Files\Microsoft SQL Server\MSRS10_50.SQLSERVER\Reporting Services\ReportServer\bin 目录下。  
  
5.  将相应自定义报表项设计时组件 DLL 复制到 %ProgramFiles%\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies 目录下。  
  
## <a name="see-also"></a>另请参阅  
 [Reporting Services 配置文件](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [自定义报表项类库](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)  
  
  
