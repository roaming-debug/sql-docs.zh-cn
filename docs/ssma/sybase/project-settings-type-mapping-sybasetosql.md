---
description: 项目设置（类型映射）(SybaseToSQL)
title: 项目设置 (类型映射)  (SybaseToSQL) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2698fb3a-f9e6-4e04-94e0-dad289d7ed0a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9e9086feb8adcb64ba1a24191e91028fc870d488
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069900"
---
# <a name="project-settings-type-mapping-sybasetosql"></a>项目设置（类型映射）(SybaseToSQL)
" **项目设置** " 对话框的 "类型映射" 页包含用于自定义 SSMA 将 Sybase 自适应服务器企业 (ASE) 数据类型转换为数据类型的方式的设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
"类型映射" 页在 " **项目设置** " 和 " **默认项目设置** " 对话框中可用。  
  
-   若要指定所有未来 SSMA 项目的类型映射设置，请在 " **工具** " 菜单上，选择 " **默认项目设置**"，从 " **迁移目标版本** " 下拉菜单中选择需要查看或更改其设置的迁移项目类型，然后选择左窗格底部的 " **类型映射** "。  
  
-   若要指定当前项目的设置，请在 " **工具** " 菜单上选择 " **项目设置**"，然后在左窗格底部选择 " **类型映射** "。  
  
## <a name="options"></a>选项  
**源类型**  
映射的 ASE 数据类型。  
  
**目标类型**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]指定 ASE 数据类型的目标数据类型。  
  
请参阅以下部分中的表，了解用于 Sybase 类型映射的默认 SSMA。  
  
**添加**  
单击此可将数据类型添加到 "映射" 列表中。  
  
**编辑**  
单击此项可在 "映射" 列表中编辑所选的数据类型。  
  
**删除**  
单击此选项可从 "映射" 列表中删除所选的数据类型映射。  
  
重置为默认值  
单击可将类型映射列表重置为 SSMA 默认值。  
  
## <a name="default-type-mapping"></a>默认类型映射  
下表包含 ASE 和数据类型之间的默认类型映射 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
|ASE 数据类型|SQL Server 数据类型|  
|-----------------|------------------------|  
|**bigint**|**bigint**|  
|**binary**|**binary**|  
|**binary [ \* .。8000]**|**binary [ \* ]**|  
|**binary [8001 \* ]**|**varbinary(max)**|  
|**bit**|**bit**|  
|**char**|**char**|  
|**char varying**|**varchar**|  
|**char 改变 [ \* .。8000]**|**varchar [ \* ]**|  
|**char 改变 [8001 \* ]**|**varchar(max)**|  
|**char [ \* .。8000]**|**char [ \* ]**|  
|**char [8001 \* ]**|**varchar(max)**|  
|**character**|**char**|  
|**字符变化**|**varchar**|  
|**字符变化 [ \* 。8000]**|**varchar [ \* ]**|  
|**字符变化 [8001 \* ]**|**varchar(max)**|  
|**字符 [ \* .。8000]**|**char [ \* ]**|  
|**字符 [8001 \* ]**|**varchar(max)**|  
|**date**|**date**|  
|**datetime**|**datetime2 [3]**|  
|**十进制**|**decimal**|  
|**dec [ \* ... \* ]**|**decimal [ \* ]**|  
|**dec [ \* ... \* ][\*..\*]**|**decimal [ \* ] [ \* ]**|  
|**decimal**|**decimal**|  
|**decimal [ \* ... \* ]**|**decimal [ \* ]**|  
|**decimal [ \* ... \* ][\*..\*]**|**decimal [ \* ] [ \* ]**|  
|**双精度**|**float [53]**|  
|**float**|**float [53]**|  
|**float [ \* .。15**|**float [24]**|  
|**float [16 ... \* ]**|**float [53]**|  
|**图像**|**图像**|  
|**int**|**int**|  
|**integer**|**int**|  
|**longsysname**|**nvarchar [255]**|  
|**money**|**money**|  
|**国家字符**|**nchar**|  
|**国家字符 [ \* .。4000]**|**nchar [ \* ]**|  
|**国家字符不同**|**nvarchar**|  
|**国家字符变化 [ \* 。4000]**|**nvarchar [ \* ]**|  
|**国家字符变化 [4001 ... \* ]**|**nvarchar(max)**|  
|**国家字符 [4001 ... \* ]**|**nvarchar(max)**|  
|**国家字符**|**nchar**|  
|**国家字符 [ \* .。4000]**|**nchar [ \* ]**|  
|**国家字符 [4001 ... \* ]**|**nvarchar(max)**|  
|**国家字符可变**|**nvarchar**|  
|**国家字符变化 [ \* 。4000]**|**nvarchar [ \* ]**|  
|**国家字符可变 [4001 ... \* ]**|**nvarchar(max)**|  
|**民族 varchar**|**nvarchar**|  
|**国家 varchar [ \* .。4000]**|**nvarchar [ \* ]**|  
|**国家 varchar [4001 ... \* ]**|**nvarchar(max)**|  
|**nchar**|**nchar**|  
|**nchar 不同**|**nvarchar**|  
|**nchar 不同 [ \* 。4000]**|**nvarchar [ \* ]**|  
|**nchar 不同 [4001 ... \* ]**|**nvarchar(max)**|  
|**nchar [ \* .。。4000]**|**nchar [ \* ]**|  
|**nchar [4001 ... \* ]**|**nvarchar(max)**|  
|**numeric**|**numeric**|  
|**数值 [ \* ... \* ]**|**数值 [ \* ]**|  
|**数值 [ \* ... \* ][\*..\*]**|**numeric [ \* ] [ \* ]**|  
|**nvarchar**|**nvarchar**|  
|**nvarchar [ \* .。4000]**|**nvarchar [ \* ]**|  
|**nvarchar [4001 ... \* ]**|**nvarchar(max)**|  
|**real**|**float [24]**|  
|**smalldatetime**|**smalldatetime**|  
|**smallint**|**smallint**|  
|**smallmoney**|**smallmoney**|  
|**sysname**|**nvarchar [128]**|  
|**sysname [ \* ... \* ]**|**nvarchar [255]**|  
|**text**|**text**|  
|**time**|**时间 [3]**|  
|**timestamp**|**rowversion**|  
|**tinyint**|**tinyint**|  
|**unichar**|**nchar**|  
|**unichar**|**nvarchar**|  
|**unichar \*4000]**|**nvarchar [ \* ]**|  
|**unichar 改变 [4001 ... \* ]**|**nvarchar(max)**|  
|**unichar [ \* .。4000]**|**nchar [ \* ]**|  
|**unichar [4001 ... \* ]**|**nvarchar(max)**|  
|**unitext**|**nvarchar(max)**|  
|**univarchar**|**nvarchar**|  
|**univarchar [ \* .。4000]**|**nvarchar [ \* ]**|  
|**univarchar [4001 ... \* ]**|**nvarchar(max)**|  
|**无符号 bigint**|**数值 [20] [0]**|  
|**unsigned int**|**bigint**|  
|**无符号 smallint**|**int**|  
|**无符号 tinyint**|**tinyint**|  
|**varbinary**|**varbinary**|  
|**varbinary [ \* .。8000]**|**varbinary [ \* ]**|  
|**varbinary [8001 \* ]**|**varbinary(max)**|  
|**varchar**|**varchar**|  
|**varchar [ \* .。。8000]**|**varchar [ \* ]**|  
|**varchar [8001 \* ]**|**varchar(max)**|  
  
