---
title: OLE 自动化脚本示例 | Microsoft Docs
description: 了解如何使用 Transact-SQL 语句批处理示例，该示例使用 OLE 自动化存储过程创建和使用 SQL-DMO SQLServer 对象。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
helpviewer_keywords:
- OLE Automation [SQL Server], examples
ms.assetid: e59f75a9-ed41-4f12-888e-ffc57f9b3882
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96c7d595876d823228e5397f78e275231649a2cd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473118"
---
# <a name="ole-automation-sample-script"></a>OLE 自动化脚本示例
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  本主题包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句批处理的一个示例，它使用 OLE 自动化存储过程在本地 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中创建和使用 SQL-DMO SQLServer 对象。 部分代码在 OLE 自动化系统存储过程的引用主题中用作示例。  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @Object int;  
DECLARE @HR int;  
DECLARE @Property nvarchar(255);  
DECLARE @Return nvarchar(255);  
DECLARE @Source nvarchar(255), @Desc nvarchar(255);  
  
-- Create a SQLServer object.  
SET NOCOUNT ON;  
  
-- First, create the object.  
EXEC @HR = sp_OACreate N'SQLDMO.SQLServer',  
    @Object OUT;  
IF @HR <> 0  
BEGIN  
    -- Report the error.  
    EXEC sp_OAGetErrorInfo @Object,  
        @Source OUT,  
        @Desc OUT;  
    SELECT HR = convert(varbinary(4),@HR),  
        Source=@Source,  
        Description=@Desc;  
    GOTO END_ROUTINE  
END  
ELSE  
-- A DMO.SQLServer object has been successfully created.  
BEGIN  
    -- Specify Windows Authentication for connections.  
    EXEC @HR = sp_OASetProperty @Object,  
        N'LoginSecure',  
        N'TRUE';  
    IF @HR <> 0 GOTO CLEANUP  
  
    -- Set a property.  
    EXEC @HR = sp_OASetProperty @Object,  
        N'HostName',  
        N'SampleScript';  
    IF @HR <> 0 GOTO CLEANUP  
  
    -- Get a property using an output parameter.  
    EXEC @HR = sp_OAGetProperty @Object, N'HostName', @Property OUT;  
    IF @HR <> 0   
        GOTO CLEANUP  
    ELSE  
        PRINT @Property;  
  
    -- Get a property using a result set.  
    EXEC @HR = sp_OAGetProperty @Object,  
        N'HostName';  
    IF @HR <> 0 GOTO CLEANUP  
  
    -- Get a property by calling the method.  
    EXEC @HR = sp_OAMethod @Object,  
        N'HostName',  
        @Property OUT;  
    IF @HR <> 0   
        GOTO CLEANUP  
    ELSE  
        PRINT @Property;  
  
    -- Call the connect method.  
    -- SECURITY NOTE - When possible, use Windows Authentication.  
    EXEC @HR = sp_OAMethod @Object,  
        N'Connect',  
        NULL,  
        N'localhost',  
        NULL,  
        NULL;  
    IF @HR <> 0 GOTO CLEANUP  
  
    -- Call a method that returns a value.  
    EXEC @HR = sp_OAMethod @Object,  
        N'VerifyConnection',  
        @Return OUT;  
    IF @HR <> 0  
        GOTO CLEANUP  
    ELSE  
        PRINT @Return;  
END  
  
CLEANUP:  
-- Check whether an error occurred.  
IF @HR <> 0  
BEGIN  
    -- Report the error.  
    EXEC sp_OAGetErrorInfo @Object,  
        @Source OUT,  
        @Desc OUT;  
    SELECT HR = convert(varbinary(4),@HR),  
        Source=@Source,  
        Description=@Desc;  
END  
  
-- Destroy the object.  
BEGIN  
    EXEC @HR = sp_OADestroy @Object;  
    -- Check if an error occurred.  
    IF @HR <> 0   
    BEGIN  
        -- Report the error.  
        EXEC sp_OAGetErrorInfo @Object,  
        @Source OUT,  
        @Desc OUT;  
        SELECT HR = convert(varbinary(4),@HR),  
        Source=@Source,  
        Description=@Desc;  
    END  
END  
  
END_ROUTINE:  
RETURN;  
GO  
```  
  
## <a name="related-content"></a>相关内容  
 [Transact-SQL 中的 OLE 自动化对象](../../relational-databases/stored-procedures/ole-automation-objects-in-transact-sql.md)  
  
 [sp_OACreate (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oacreate-transact-sql.md)  
  
 [sp_OAGetProperty (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oagetproperty-transact-sql.md)  
  
 [sp_OASetProperty (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oasetproperty-transact-sql.md)  
  
 [sp_OAMethod (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oamethod-transact-sql.md)  
  
 [sp_OADestroy (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oadestroy-transact-sql.md)  
  
  
