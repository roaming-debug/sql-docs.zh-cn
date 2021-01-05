---
title: ADO.NET 连接管理器 | Microsoft Docs
description: ADO.NET 连接管理器使包能够使用 .NET 访问接口访问数据源。
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.adonetconnection.f1
helpviewer_keywords:
- connection managers [Integration Services], ADO.NET
- ADO.NET connection manager [Integration Services]
- connections [Integration Services], ADO.NET
ms.assetid: fc5daa2f-0159-4bda-9402-c87f1035a96f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c2efe0145643f3f9e969c298e76115967477019
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637446"
---
# <a name="adonet-connection-manager"></a>ADO.NET 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器使包能够使用 .NET 访问接口访问数据源。 通常，可以使用此连接管理器来访问数据源（例如 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）。 还可以访问在用 C# 等语言以托管代码编写的自定义任务中通过 OLE DB 和 XML 公开的数据源。  
  
将 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器添加到包时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 会创建在运行时作为 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接进行解析的连接管理器。 它设置连接管理器的属性，并将连接管理器添加到包上的“Connections”集合  。  
  
该连接管理器的 `ConnectionManagerType` 属性设置为 `ADO.NET`。 `ConnectionManagerType` 的值受到限定，以包含连接管理器所使用的 .NET 访问接口的名称。  
  
## <a name="adonet-connection-manager-troubleshooting"></a>ADO.NET 连接管理器故障排除  
可以记录 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器对外部数据访问接口所做的调用。 然后可以对 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器与外部数据源的连接进行故障排除。 若要记录 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器对外部数据访问接口所做的调用，请在包级别启用包日志记录并选择“诊断”  事件。 有关详细信息，请参阅 [包执行的疑难解答工具](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)。  
  
[!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器在读取数据时，某些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日期数据类型的数据生成下表中显示的结果。  
  
|SQL Server 数据类型|结果|  
|--------------------------|------------|  
|**time**、 **datetimeoffset**|除非包使用参数化 SQL 命令，否则，包将失败。 若要使用参数化 SQL 命令，请在包中使用执行 SQL 任务。 有关详细信息，请参阅 [执行 SQL 任务](../../integration-services/control-flow/execute-sql-task.md) 和 [执行 SQL 任务中的参数和返回代码](../control-flow/execute-sql-task.md)。|  
|**datetime2**|[!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器截断毫秒值。|  
  
> [!NOTE]  
>  有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型和它们如何映射到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据类型的详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md) 和 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
## <a name="adonet-connection-manager-configuration"></a>ADO.NET 连接管理器配置  
  
可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
-   提供配置为满足选定 .NET 访问接口的要求的特定连接字符串。  
  
-   包括要连接到的数据源的名称（取决于访问接口）。  
  
-   为选定的访问接口提供相应的安全凭据。  
  
-   指示是否在运行时保留从连接管理器中创建的连接。  
  
[!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器的许多配置选项取决于该连接管理器所使用的 .NET 访问接口。  
  
有关可以在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请参阅[配置 ADO.NET 连接管理器]()。  
  
 有关以编程方式配置连接管理器的信息，请参阅 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 和 [以编程方式添加连接](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)项目。  
  
### <a name="configure-adonet-connection-manager"></a>配置 ADO.NET 连接管理器
可以使用“配置 ADO.NET 连接管理器”  对话框，添加与可使用 .NET Framework 数据访问接口访问的数据源之间的连接。 此类访问接口的示例有 SqlClient 访问接口。 连接管理器可使用现有连接，或者您也可以创建一个新的连接。  
  
 若要了解有关 ADO.NET 连接管理器的详细信息，请参阅 [ADO.NET Connection Manager](../../integration-services/connection-manager/ado-net-connection-manager.md)。  
  
#### <a name="options"></a>选项  
**数据连接**  
从列表中选择一个现有的 ADO.NET 数据连接。  
  
**数据连接属性**  
查看所选 ADO.NET 数据连接的属性和值。  
  
**新建**  
通过使用“连接管理器”对话框创建 ADO.NET 数据连接。   
  
**删除**  
选择一个连接，然后通过选择“删除”  来删除该连接。  
  
#### <a name="managed-identities-for-azure-resources-authentication"></a>Azure 资源身份验证的托管标识
在 [Azure 数据工厂中的 Azure-SSIS 集成运行时](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime)上运行 SSIS 包时，可以使用与数据工厂关联的[托管标识](/azure/data-factory/connector-azure-sql-database#managed-identity)进行 Azure SQL 数据库或 Azure SQL 托管实例身份验证。 指定的工厂可以使用此标识访问数据库数据或从/向数据库复制数据。

> [!NOTE]
>  使用 Azure Active Directory (Azure AD) 身份验证（包括托管标识身份验证）连接到 Azure SQL 数据库或 Azure SQL 托管实例时，可能遇到与包执行失败或意外行为变更有关的问题。 有关详细信息，请参阅 [Azure AD 功能和限制](/azure/sql-database/sql-database-aad-authentication#azure-ad-features-and-limitations)。

要对 Azure SQL 数据库使用托管身份验证，请按照以下步骤配置数据库：

1. 在 Azure 门户上为 Azure SQL Server [预配 Azure Active Directory 管理员](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)（如果尚未执行该操作）。 Azure AD 管理员可以是 Azure AD 用户或 Azure AD 组。 如果为具有托管标识的组授予管理员角色，请跳过步骤 2 和 3。 管理员将拥有对数据库的完全访问权限。

1. 为数据工厂托管标识[创建包含的数据库用户](/azure/sql-database/sql-database-aad-authentication-configure#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)。 使用 SSMS 等工具连接到要从/向其复制数据的数据库，其 Azure Azure 标识至少具有 ALTER ANY USER 权限。 运行以下 T-SQL： 
    
    ```sql
    CREATE USER [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. 授予数据工厂托管标识所需的权限，就像通常为 SQL 用户和其他用户所做的那样。 有关相应角色，请参阅[数据库级别角色](../../relational-databases/security/authentication-access/database-level-roles.md)。 运行以下代码。 有关更多选项，请参阅[本文档](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)。

    ```sql
    EXEC sp_addrolemember [role name], [your data factory name];
    ```

要对 Azure SQL 托管实例使用托管身份验证，请按照以下步骤配置数据库：
    
1. 在 Azure 门户上为托管实例[预配 Azure Active Directory 管理员](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-managed-instance)（如果尚未执行该操作）。 Azure AD 管理员可以是 Azure AD 用户或 Azure AD 组。 如果为具有托管标识的组授予管理员角色，请跳过步骤 2-4。 管理员将拥有对数据库的完全访问权限。

1. 为数据工厂托管标识[创建登录名](../../t-sql/statements/create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)。 在 SQL Server Management Studio (SSMS) 中，使用 SQL Server 帐户 sysadmin  连接到托管实例。 在 master  数据库中，运行以下 T-SQL：

    ```sql
    CREATE LOGIN [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. 为数据工厂托管标识[创建包含的数据库用户](/azure/sql-database/sql-database-aad-authentication-configure#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)。 连接到要从/向其复制数据的数据库，运行以下 T-SQL： 
  
    ```sql
    CREATE USER [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. 授予数据工厂托管标识所需的权限，就像通常为 SQL 用户和其他用户所做的那样。 运行以下代码。 有关更多选项，请参阅[本文档](../../t-sql/statements/alter-role-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)。

    ```sql
    ALTER ROLE [role name e.g., db_owner] ADD MEMBER [your data factory name];
    ```

最后，为 ADO.NET 连接管理器配置托管标识身份验证。 完成此操作的方法有两种：
    
- **在设计时进行配置。** 在 SSIS 设计器中，右键单击 ADO.NET 连接管理器，然后选择“属性”  。 将属性 `ConnectUsingManagedIdentity` 更新为 `True`。
    > [!NOTE]
    >  目前，当你在 SSIS 设计器或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server 中运行 SSIS 包时，连接管理器属性 `ConnectUsingManagedIdentity` 不生效（表明托管标识身份验证不起作用）。
    
- **在运行时进行配置。** 通过 [SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md) 或 [Azure 数据工厂执行 SSIS 包活动](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)运行包时，找到 ADO.NET 连接管理器。 将其属性 `ConnectUsingManagedIdentity` 更新为 `True`。
    > [!NOTE]
    >  在 Azure-SSIS 集成运行时中，当使用托管标识身份验证来建立数据库连接时，ADO.NET 连接管理器上预配的所有其他身份验证方法（例如，集成身份验证和密码）将被重写。

> [!NOTE]
>  要在现有包上配置托管标识身份验证，首选方法是至少使用[最新 SSIS 设计器](../../ssdt/download-sql-server-data-tools-ssdt.md)重新生成一次 SSIS 项目。 将该 SSIS 项目重新部署到 Azure SSIS 集成运行时，这样新的连接管理器属性 `ConnectUsingManagedIdentity` 才会自动添加到 SSIS 项目中的所有 ADO.NET 连接管理器。 另一种方法是，直接在运行时结合使用属性重写和属性路径 **\Package.Connections[{连接管理器名称}].Properties[ConnectUsingManagedIdentity]** 。

## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
