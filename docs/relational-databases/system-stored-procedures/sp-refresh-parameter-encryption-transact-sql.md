---
description: 'sp_refresh_parameter_encryption (Transact-sql) '
title: sp_refresh_parameter_encryption (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_refresh_parameter_encryption
- sp_refresh_parameter_encryption_TSQL
- sys.sp_refresh_parameter_encryption
- sys.sp_refresh_parameter_encryption_TSQL
helpviewer_keywords:
- sp_refresh_parameter_encryption
- Always Encrypted, sp_refresh_parameter_encryption
ms.assetid: 00b44baf-fcf0-4095-aabe-49fa87e77316
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2e55b0a69fde6e80c6c7124b9fe800c2f6818109
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97410564"
---
# <a name="sp_refresh_parameter_encryption-transact-sql"></a>sp_refresh_parameter_encryption (Transact-sql) 

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

为当前数据库中指定的非绑定到架构的存储过程、用户定义函数、视图、DML 触发器、数据库级 DDL 触发器或服务器级 DDL 触发器的参数更新 Always Encrypted 元数据。 

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sys.sp_refresh_parameter_encryption [ @name = ] 'module_name' 
    [ , [ @namespace = ] '<class>' ]
[ ; ]

<class> ::=
{ DATABASE_DDL_TRIGGER | SERVER_DDL_TRIGGER }
```

## <a name="arguments"></a>自变量

`[ @name = ] 'module_name'` 存储过程、用户定义函数、视图、DML 触发器、数据库级 DDL 触发器或服务器级 DDL 触发器的名称。 *module_name* 不能是公共语言运行时 (CLR) 存储过程或 clr 函数。 *module_name* 不能是绑定到架构的。 *module_name* 为 `nvarchar` ，无默认值。 *module_name* 可以是由多个部分组成的标识符，但只能引用当前数据库中的对象。

`[ @namespace = ] ' < class > '` 指定模块的类。 如果 *module_name* 是 DDL 触发器， `<class>` 则是必需的。 `<class>` 为 `nvarchar(20)`。 有效的输入为 `DATABASE_DDL_TRIGGER` 和 `SERVER_DDL_TRIGGER` 。    

## <a name="return-code-values"></a>返回代码值  

0（成功）或非零数字（失败）


## <a name="remarks"></a>备注

如果以下情况，模块的参数的加密元数据可能会过时：   
* 模块引用的表中列的加密属性已更新。 例如，已删除某列，并且添加了一个具有相同名称但具有不同加密类型、加密密钥或加密算法的新列。  
* 模块引用了具有过时参数加密元数据的另一个模块。  

当修改表的加密属性时， `sp_refresh_parameter_encryption` 应直接或间接引用该表。 可以按任意顺序在这些模块上调用此存储过程，而无需用户先刷新内部模块，然后才能移动到其调用方。

`sp_refresh_parameter_encryption` 不会影响与对象关联的任何权限、扩展属性或 `SET` 选项。 

若要刷新服务器级 DDL 触发器，可以在任何数据库的上下文中执行此存储过程。

> [!NOTE]
>  当你运行时，将删除与对象关联的所有签名 `sp_refresh_parameter_encryption` 。

## <a name="permissions"></a>权限

需要对 `ALTER` 模块的权限，以及 `REFERENCES` 对象引用的任何 CLR 用户定义类型和 XML 架构集合的权限。   

当指定的模块是数据库级 DDL 触发器时，需要 `ALTER ANY DATABASE DDL TRIGGER` 在当前数据库中具有权限。    

当指定的模块是服务器级 DDL 触发器时，需要 `CONTROL SERVER` 权限。

对于用子句定义的模块 `EXECUTE AS` ， `IMPERSONATE` 需要对指定主体拥有权限。 通常，刷新对象时不会更改其 `EXECUTE AS` 主体，除非该模块是使用定义的， `EXECUTE AS USER` 并且该主体的用户名现在解析为其他用户，而不是创建模块时的用户。
 
## <a name="examples"></a>示例

下面的示例创建一个表和一个过程，该过程引用该表，配置 Always Encrypted，然后演示更改表并运行该 `sp_refresh_parameter_encryption` 过程。  

首先创建一个引用该表的初始表和一个存储过程。
```sql
CREATE TABLE [Patients]([PatientID] [int] IDENTITY(1,1) NOT NULL,
    [SSN] [char](11), 
    [FirstName] [nvarchar](50) NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [MiddleName] [nvarchar](50) NULL,
    [StreetAddress] [nvarchar](50) NOT NULL,
    [City] [nvarchar](50) NOT NULL,
    [ZipCode] [char](5) NOT NULL,
    [State] [char](2) NOT NULL,
    [BirthDate] [date] NOT NULL,
 CONSTRAINT [PK_Patients] PRIMARY KEY CLUSTERED 
(
    [PatientID] ASC
) WITH 
    (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, 
     IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, 
     ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY];
GO

CREATE PROCEDURE [find_patient] @SSN [char](11)
AS
BEGIN
    SELECT * FROM [Patients] WHERE SSN=@SSN
END;
GO
```

然后，设置 Always Encrypted 密钥。
```sql
CREATE COLUMN MASTER KEY [CMK1]
WITH
(
       KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',
    KEY_PATH = N'CurrentUser/my/A66BB0F6DD70BDFF02B62D0F87E340288E6F9305'
);
GO

CREATE COLUMN ENCRYPTION KEY [CEK1]
WITH VALUES
(
       COLUMN_MASTER_KEY = [CMK1],
    ALGORITHM = 'RSA_OAEP',
    ENCRYPTED_VALUE = 
       0x016E000001630075007200720065006E00740075007300650072002F006D0079002F006100360036006200620030006600360064006400370030006200640066006600300032006200360032006400300066003800370065003300340030003200380038006500360066003900330030003500CA0D0CEC74ECADD1804CF99137B4BD06BBAB15D7EA74E0C249A779C7768A5B659E0125D24FF827F5EA8CA517A8E197ECA1353BA814C2B0B2E6C8AB36E3AE6A1E972D69C3C573A963ADAB6686CF5D24F95FE43140C4F9AF48FBA7DF2D053F3B4A1F5693A1F905440F8015BDB43AF8A04BE4E045B89876A0097E5FBC4E6A3B9C3C0D278C540E46C53938B8C957B689C4DC095821C465C73117CBA95B758232F9E5B2FCC7950B8CA00AFE374DE42847E3FBC2FDD277035A2DEF529F4B735C20D980073B4965B4542A34723276A1646998FC6E1C40A3FDB6ABCA98EE2B447F114D2AC7FF8C7D51657550EC5C2BABFFE8429B851272086DCED94332CF18FA854C1D545A28B1EF4BE64F8E035175C1650F6FC5C4702ACF99850A4542B3747EAEC0CC726E091B36CE24392D801ECAA684DE344FECE05812D12CD72254A014D42D0EABDA41C89FC4F545E88B4B8781E5FAF40D7199D4842D2BFE904D209728ED4F527CBC169E2904F6E711FF81A8F4C25382A2E778DD2A58552ED031AFFDA9D9D891D98AD82155F93C58202FC24A77F415D4F8EF22419D62E188AC609330CCBD97CEE1AEF8A18B01958833604707FDF03B2B386487CC679D7E352D0B69F9FB002E51BCD814D077E82A09C14E9892C1F8E0C559CFD5FA841CEF647DAB03C8191DC46B772E94D579D8C80FE93C3827C9F0AE04D5325BC73111E07EEEDBE67F1E2A73580085
);
GO
```


最后，我们将 SSN 列替换为已加密的列，然后运行该 `sp_refresh_parameter_encryption` 过程来更新 Always Encrypted 组件。
```sql
ALTER TABLE [Patients] DROP COLUMN [SSN];
GO

ALTER TABLE [Patients] 
    ADD [SSN] [char](11) COLLATE Latin1_General_BIN2 
    ENCRYPTED WITH 
        (COLUMN_ENCRYPTION_KEY = [CEK1], 
        ENCRYPTION_TYPE = Deterministic, 
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') 
    NOT NULL;
GO

EXEC sp_refresh_parameter_encryption [find_patient];
GO
```

## <a name="see-also"></a>另请参阅 

[Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
[始终加密向导](../../relational-databases/security/encryption/always-encrypted-wizard.md)   

