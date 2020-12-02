---
description: Azure Data Lake Store 源
title: Azure Data Lake Store 源 | Microsoft Docs
ms.custom: ''
ms.date: 08/16/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSSRC.F1
- sql14.dts.designer.afpadlssrc.f1
ms.assetid: f9c3311f-7316-48d6-bf10-d810e70b4304
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 38de021617ed0454048a3d8cd7f0c5732d09861f
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127329"
---
# <a name="azure-data-lake-store-source"></a>Azure Data Lake Store 源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  “Azure Data Lake Store 源”  组件允许 SSIS 包读取 Azure Data Lake Store 中的数据。 支持的文件格式：文本和 Avro。
  
 “Azure Data Lake Store 源”是[用于 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)的组件。  
  
> [!NOTE]
> 若要确保 Azure Data Lake Store 连接管理器和使用它的组件（即 Azure Data Lake Store 源和 Azure Data Lake Store 目标）可连接到服务，请确保在 [此处](https://www.microsoft.com/download/details.aspx?id=49492)下载最新版本的 Azure 功能包。 
  
## <a name="configure-the-azure-data-lake-store-source"></a>配置 Azure Data Lake Store 源
 1. 若要查看 Azure Data Lake Store 源的编辑器，请将“Azure Data Lake Store 源”  拖放到数据流设计器上，然后双击打开编辑器。  
  
2.  对于“Azure Data Lake Store 连接管理器”  字段，请指定一个现有的 Azure Data Lake Store 连接管理器，或新建一个引用 Azure Data Lake Store 服务的连接管理器。  
  
    1.  对于“文件路径”  字段，请指定 Azure Data Lake Store 中源文件的文件路径。   
  
    2.  对于“文件格式”  字段，请指定源文件的文件格式。  
  
        如果文件格式是文本，则必须指定“列分隔符字符”  值。 此外，如果文件中的第一行包含列名称，请选择“在第一个数据行中显示列名称”  。  
  
3.  指定连接信息后，切换到“列”  页，将源列映射到 SSIS 数据流的目标列。   

## <a name="text-qualifier"></a>文本限定符

Azure Data Lake Store 源不支持文本限定符。 如果必须指定文本限定符以正确处理文件，请考虑将文件下载到本地计算机并使用平面文件源处理文件。 平面文件源支持指定文本限定符。 有关详细信息，请参阅[平面文件源](flat-file-source.md)。
