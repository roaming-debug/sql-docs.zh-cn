---
description: 在数据流中使用数据类型
title: 在数据流中使用数据类型 | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom data flow components [Integration Services], mapping data types
- data flow components [Integration Services], mapping data types
- data types [Integration Services], converting
ms.assetid: 941260d0-4ec3-4bf0-ab48-2b26733e6b24
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a7dc7cac9f158034ca293e133b096ea4c32876ca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354356"
---
# <a name="working-with-data-types-in-the-data-flow"></a>在数据流中使用数据类型

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  在 Integration Services 中开发自定义数据流组件时，将数据复制到数据流缓冲区和从数据流缓冲区复制数据以及转换值的过程中，需要经常使用数据类型。 本主题中的信息有助于您选择正确的 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型，以及在使用这些数据类型时选择正确的方法。  
  
## <a name="inserting-data-into-the-data-flow"></a>将数据插入数据流  
 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> 类提供一系列将数据复制到缓冲区的 Set 方法，以及对应的一系列从缓冲区列检索数据的 Get 方法。 下表显示了与每种 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型相对应的方法。  
  
### <a name="set-methods-to-use-with-data-types"></a>用于各数据类型的 Set 方法  
 下表在第一列中列出数据类型，然后列出对应的 Set 和 Get 方法。  
  
|数据类型|Set 方法|Get 方法|  
|---------------|----------------|----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_BOOL>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetBoolean%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetBoolean%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_BYTES>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetBytes%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetBytes%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_CY>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDecimal%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetDecimal%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DATE>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDateTime%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetDateTime%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBDATE>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDate%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetDate%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIME>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetTime%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetTime%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIME2>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetTime%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetTime%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDateTime%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetDateTime%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP2>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDateTime%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetDateTime%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMPOFFSET>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDateTimeOffset%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetDateTimeOffset%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DECIMAL>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDecimal%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetDecimal%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_FILETIME>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDateTime%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetDateTime%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_GUID>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetGuid%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetGuid%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I1>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetSByte%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetSByte%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I2>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetInt16%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetInt16%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I4>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetInt32%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetInt32%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I8>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetInt64%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetInt64%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_IMAGE>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddBlobData%2A> 或 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddBlobData%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetBlobData%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_NTEXT>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddBlobData%2A> 或 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddBlobData%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetBlobData%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_NULL>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetNull%2A>|没有适用于此数据类型的 Get 方法。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_NUMERIC>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDecimal%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetDecimal%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_R4>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetSingle%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetSingle%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_R8>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDouble%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetDouble%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_STR>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetString%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetString%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_TEXT>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddBlobData%2A> 或 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddBlobData%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetBlobData%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI1>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetByte%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetByte%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI2>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetUInt16%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetUInt16%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI4>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetUInt32%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetUInt32%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI8>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetUInt64%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetUInt64%2A>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_WSTR>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetString%2A>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.GetString%2A>|  
  
### <a name="data-types-to-use-with-the-set-methods"></a>用于各 Set 方法的数据类型  
  
|Set 方法|数据类型|  
|----------------|---------------|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddBlobData%2A> 或 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddBlobData%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_IMAGE>、<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_NTEXT> 或 <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_TEXT>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetBoolean%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_BOOL>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetByte%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI1>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetBytes%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_BYTES>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDate%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBDATE>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDateTime%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DATE>、<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP>、<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP2> 或 <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_FILETIME>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDateTimeOffset%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMPOFFSET>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDecimal%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_CY>、<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DECIMAL> 或 <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_NUMERIC>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetDouble%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_R8>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetGuid%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_GUID>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetInt16%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I2>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetInt32%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I4>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetInt64%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I8>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetNull%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_NULL>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetSByte%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I1>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetSingle%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_R4>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetString%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_STR> 或 <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_WSTR>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetTime%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIME> 或 <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIME2>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetUInt16%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI2>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetUInt32%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI4>|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetUInt64%2A>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI8>|  
  
## <a name="mapping-data-types-in-the-data-flow"></a>在数据流中映射数据类型  
 将数据从源经过转换移动到目标时，数据流组件有时必须在 <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType> 枚举中定义的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 类型和在 System 命名空间中定义的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 托管数据类型之间转换数据类型。 此外，组件有时必须先将一种 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型转换为另一种数据类型，然后才能将该类型转换为托管类型。  
  
> [!NOTE]  
>  默认情况下安装在 C:\Program Files\Microsoft SQL Server\130\DTS\MappingFiles 中的 XML 格式的映射文件与本主题中讨论的数据类型映射无关。 这些文件将数据类型从一个数据库版本或系统映射到另一个数据库版本或系统（例如，从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 映射到 Oracle），并且只由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 导入和导出向导使用。 有关这些映射文件的详细信息，请参阅 [SQL Server 导入和导出向导](~/integration-services/import-export-data/welcome-to-sql-server-import-and-export-wizard.md)。  
  
### <a name="mapping-between-integration-services-and-managed-data-types"></a>Integration Services 与托管数据类型之间的映射  
 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.DataRecordTypeToBufferType%2A> 方法将 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型映射到托管数据类型。  
  
> [!CAUTION]  
>  开发人员在使用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> 类的这些方法时应小心，并且可能需要编写更适合自己自定义组件的独特需求的数据类型映射方法代码。 现有方法不考虑数值精度或小数位数，或其他与数据类型本身紧密相关的属性。 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 的未来版本中，可能会修改或删除这些方法，或修改这些方法执行的映射。  
  
 下表列出了 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.DataRecordTypeToBufferType%2A> 方法如何将各 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型映射到托管数据类型。  
  
|Integration Services 数据类型|映射到此托管数据类型|  
|------------------------------------|------------------------------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_WSTR>|System.String|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_BYTES>|System.Byte 的数组|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP>|System.DateTime|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP2>|System.DateTime|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMPOFFSET>|System.DateTimeOffset|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBDATE>|System.DateTime|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIME>|System.TimeSpan|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIME2>|System.TimeSpan|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DATE>|System.DateTime|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_FILETIME>|System.DateTime|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_NUMERIC>|System.Decimal|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_GUID>|System.Guid|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I1>|System.SByte|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I2>|System.Int16|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I4>|System.Int32|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I8>|System.Int64|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_BOOL>|System.Boolean|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_R4>|System.Single|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_R8>|System.Double|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI1>|System.Byte|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI2>|System.UInt16|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI4>|System.UInt32|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_UI8>|System.UInt64|  
  
### <a name="mapping-integration-services-data-types-to-fit-managed-data-types"></a>映射 Integration Services 数据类型以适合托管数据类型  
 有时数据流组件还必须先将一种 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型转换为另一种数据类型，然后才能将该类型转换为托管类型。 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A> 方法类将 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型映射到其他 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型，然后使用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A> 方法将该数据类型映射到托管数据类型。  
  
> [!CAUTION]  
>  开发人员在使用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> 类的这些方法时应小心，并且可能需要编写更适合自己自定义组件的独特需求的数据类型映射方法代码。 现有方法不考虑数值精度或小数位数，或其他与数据类型本身紧密相关的属性。 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 的未来版本中，可能会修改或删除这些方法，或修改这些方法执行的映射。  
  
 下表列出了 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A> 方法如何将 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型映射到其他 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型。  
  
|原始 Integration Services 数据类型|映射到此 Integration Services 数据类型|  
|---------------------------------------------|-------------------------------------------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DECIMAL>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_NUMERIC>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_CY>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_NUMERIC>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DATE>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBDATE>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_FILETIME>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP2>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIMESTAMP>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIME>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_DBTIME2>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_BOOL>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_I4>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_TEXT>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_WSTR>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_NTEXT>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_WSTR>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_STR>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_WSTR>|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_IMAGE>|<xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DataType.DT_BYTES>|  
  
> [!NOTE]  
>  <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A> 方法不返回 DT_DBTIMESTAMPOFFSET 数据类型的值，并发生 <xref:Microsoft.SqlServer.Dts.Pipeline.UnsupportedBufferDataTypeException>。 必须将 DT_DBTIMESTAMPOFFSET 数据类型转换为可映射到托管数据类型的一种 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 日期/时间数据类型。 有关可映射到托管数据类型的 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 日期/时间数据类型的列表，请参阅前述“Integration Services 与托管数据类型之间的映射”中的表。 有关转换数据类型的信息，请参阅 [Integration Services 数据类型](../../../integration-services/data-flow/integration-services-data-types.md)。  
  
## <a name="see-also"></a>另请参阅  
 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A>   
 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.DataRecordTypeToBufferType%2A>   
 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A>   
 [Integration Services 数据类型](../../../integration-services/data-flow/integration-services-data-types.md)  
  
  
