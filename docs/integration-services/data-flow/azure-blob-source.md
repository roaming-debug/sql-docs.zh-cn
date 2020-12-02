---
description: Azure blob 源
title: Azure blob 源 | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobsrc.f1
- sql14.dts.designer.afpblobsrc.f1
ms.assetid: 80645c5c-88c8-4fb0-8607-de1bb7bffcbb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ce39fed32923ae46bd499c32d5b58660db5dcd8b
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127358"
---
# <a name="azure-blob-source"></a>Azure blob 源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  借助“Azure blob 源”  组件，SSIS 包可以读取 Azure blob 中的数据。 支持的文件格式：CSV 和 AVRO。
  
  若要查看 Azure blob 源的编辑器，请将“Azure blob 源”拖放到数据流设计器上，然后双击它打开编辑器。  
  
 “Azure blob 源”是[适用于 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)的组件。  
  
1.  对于“Azure 存储空间连接管理器”  字段，请指定一个现有的 Azure 存储空间连接管理器，或新建一个引用 Azure 存储空间帐户的连接管理器。  
  
2.  对于“blob 容器名称”  字段，请指定包含源文件的 blob 容器的名称。  
  
3.  对于“blob 名称”  字段，请指定 blob 的路径。  
  
4.  对于“Blob 文件格式”字段，请选择要使用的 blob 格式，“Text”或“Avro”。  
  
5.  如果文件格式是 Text，则必须指定“列分隔符字符”值。 （不支持多字符分隔符。）

    此外，如果文件中的第一行包含列名称，请选择“在第一个数据行中显示列名称”  。

6.  如果为压缩文件，请选择“解压缩文件”。

7.  如果为压缩文件，请选择“压缩类型”：“GZIP”、“DEFLATE”或者“BZIP2”。 请注意，不支持 Zip 格式。
  
8.  指定连接信息后，切换到“列”页，将源列映射到 SSIS 数据流的目标列。  
  
  
