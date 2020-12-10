---
title: 连接字符串和配置文件
description: 了解如何将 ADO.NET 应用程序的连接字符串存储在应用程序配置文件中，作为安全和维护的最佳做法。
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 37df2641-661e-407a-a3fb-7bf9540f01e8
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: fb290f9a795c9f64bcb2ee95c66210790a80a71d
ms.sourcegitcommit: 7a3fdd3f282f634f7382790841d2c2a06c917011
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "96563113"
---
# <a name="connection-strings-and-configuration-files"></a>连接字符串和配置文件

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

在应用程序代码中嵌入连接字符串可能导致安全漏洞和维护问题。 使用 [Ildasm.exe（IL 反汇编程序）](/dotnet/framework/tools/ildasm-exe-il-disassembler)工具可以查看编译到应用程序源代码中的未加密连接字符串。 此外，如果连接字符串发生更改，则必须重新编译应用程序。 因此，我们建议您将连接字符串存储在应用程序配置文件中。

## <a name="working-with-application-configuration-files"></a>使用应用程序配置文件

应用程序配置文件包含特定应用程序特有的设置。 例如，ASP.NET 应用程序能包含一个或多个 web.config 文件，Windows 应用程序可能包含一个可选 app.config 文件。 虽然配置文件的名称和位置会因应用程序宿主的不同而有所不同，但配置文件可以有相同的元素。

### <a name="the-connectionstrings-section"></a>connectionStrings 节

连接字符串可作为键/值对存储在应用程序配置文件 configuration 元素的 connectionStrings 节中。 子元素包括 add、clear 和 remove。

下面的配置文件片段演示用于存储连接字符串的架构和语法。 name 属性是你提供用来唯一标识连接字符串的名称，以便在运行时可以检索到该字符串。 providerName 是 **Microsoft.Data.SqlClient** 与 Microsoft SqlClient Data Provider for SQL Server 的结合。

```xml  
<?xml version='1.0' encoding='utf-8'?>  
  <configuration>  
    <connectionStrings>  
      <clear />  
      <add name="Name"
       providerName="Microsoft.Data.SqlClient"
       connectionString="Valid Connection String;" />  
    </connectionStrings>  
  </configuration>  
```  

> [!NOTE]
> 您可以将连接字符串的一部分保存在配置文件中，并使用 <xref:System.Data.Common.DbConnectionStringBuilder> 类在运行时将该字符串补充完整。 如果您预先不知道连接字符串的元素，或者不希望将敏感信息保存到配置文件中，这一方法会很有用。 有关详细信息，请参阅[连接字符串生成器](connection-string-builders.md)。

### <a name="using-external-configuration-files"></a>使用外部配置文件

外部配置文件是单独的文件，此类文件包含由一部分组成的配置文件的片段。 外部配置文件由主配置文件引用。 如果在部署完应用程序后连接字符串可能会被编辑，那么将 connectionStrings 节存储在物理上独立的文件中会很有用。 例如，标准 ASP.NET 行为是在修改配置文件后重新启动应用程序域，这将导致状态信息丢失。 然而，修改外部配置文件不会导致重新启动应用程序。 外部配置文件并不局限于由 ASP.NET 使用，Windows 应用程序也可以使用。 此外，文件访问安全性和权限也可以用于限制对外部配置文件的访问。 运行时使用外部配置文件是透明的，且不需要特殊编码。

若要将连接字符串存储在外部配置文件中，请创建一个只包含 connectionStrings 节的单独的文件。 不要包含任何其他的元素、节或属性。 此示例演示外部配置文件的语法。

```xml  
<connectionStrings>  
  <add name="Name"
   providerName="Microsoft.Data.SqlClient"
   connectionString="Valid Connection String;" />  
</connectionStrings>  
```  

 在主应用程序配置文件中，可以使用 configSource 属性指定外部文件的完全限定名和位置。 此示例引用名为 `connections.config` 的外部配置文件。

```xml  
<?xml version='1.0' encoding='utf-8'?>  
<configuration>  
    <connectionStrings configSource="connections.config"/>  
</configuration>  
```  

## <a name="retrieving-connection-strings-at-run-time"></a>运行时检索连接字符串

.NET Framework 2.0 在 <xref:System.Configuration> 命名空间中引入了新类，以简化在运行时从配置文件检索连接字符串的操作。 您可以以编程方式按名称或提供程序名称检索连接字符串。

> [!NOTE]
> machine.config 文件还包含 connectionStrings 节，此节包含 Visual Studio 使用的连接字符串。 当按提供程序名称从 Windows 应用程序中的 app.config 文件检索连接字符串时，首先加载 machine.config 中的连接字符串，然后加载 app.config 中的项。在 connectionStrings 元素删除所有继承自内存中数据结构的引用后将立即添加 clear，以便只考虑在本地 app.config 文件中指定的连接字符串。

### <a name="working-with-the-configuration-classes"></a>使用配置类

从 .NET Framework 2.0 开始，当使用本地计算机上的配置文件时，将使用 <xref:System.Configuration.ConfigurationManager>，从而替换已不推荐使用的 <xref:System.Configuration.ConfigurationSettings>。 <xref:System.Web.Configuration.WebConfigurationManager> 与 ASP.NET 配置文件一起使用。 该管理器可以使用 Web 服务器上的配置文件，并允许以编程方式访问配置文件节（如 system.web）。

> [!NOTE]
> 在运行时访问配置文件需要对调用方授予权限，根据应用程序类型、配置文件和位置确定所需的权限。 有关详细信息，请参阅[使用配置类](/previous-versions/aspnet/ms228063(v=vs.100))和 <xref:System.Web.Configuration.WebConfigurationManager>（对于 ASP.NET 应用程序），以及 <xref:System.Configuration.ConfigurationManager>（对于 Windows 应用程序）。

您可以使用 <xref:System.Configuration.ConnectionStringSettingsCollection> 从应用程序配置文件中检索连接字符串。 它包含 <xref:System.Configuration.ConnectionStringSettings> 对象的集合，每个对象表示 connectionStrings 节中的一项。 它的属性 (Property) 映射为连接字符串属性 (Attribute)，从而允许您通过指定名称或提供程序名称来检索连接字符串。

|属性|描述|  
|--------------|-----------------|  
|<xref:System.Configuration.ConnectionStringSettings.Name%2A>|连接字符串的名称。 映射到 name 属性。|  
|<xref:System.Configuration.ConnectionStringSettings.ProviderName%2A>|完全限定提供程序名。 映射到 providerName 属性。|  
|<xref:System.Configuration.ConnectionStringSettings.ConnectionString%2A>|连接字符串。 映射到 connectionString 属性。|  

### <a name="example-listing-all-connection-strings"></a>示例：列出所有连接字符串

此示例循环访问 <xref:System.Configuration.ConnectionStringSettingsCollection>，并在控制台窗口中显示 <xref:System.Configuration.ConnectionStringSettings.Name%2A?displayProperty=nameWithType>、<xref:System.Configuration.ConnectionStringSettings.ProviderName%2A?displayProperty=nameWithType> 和 <xref:System.Configuration.ConnectionStringSettings.ConnectionString%2A?displayProperty=nameWithType> 属性。

> [!NOTE]
> System.Configuration.dll 并不包含在所有项目类型中，您可能需要对其设置引用才能使用配置类。 特定应用程序配置文件的名称和位置因应用程序类型和宿主进程的不同而有所不同。

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfig#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfig.cs#1)]

### <a name="example-retrieving-a-connection-string-by-name"></a>示例：按名称检索连接字符串

此示例演示如何通过指定连接字符串名称从配置文件中检索连接字符串。 此代码创建一个 <xref:System.Configuration.ConnectionStringSettings> 对象，以将提供的输入参数与 <xref:System.Configuration.ConfigurationManager.ConnectionStrings%2A> 名称相匹配。 如果未找到匹配名称，则该函数返回 `null`（在 Visual Basic 中返回 `Nothing`）。

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByName#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfigByName.cs#1)]

### <a name="example-retrieving-a-connection-string-by-provider-name"></a>示例：按提供程序名称检索连接字符串

此示例演示如何通过指定 Microsoft.Data.SqlClient 格式的提供程序名称来检索连接字符串。 此代码循环访问 <xref:System.Configuration.ConnectionStringSettingsCollection>，并返回找到的第一个 <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A> 的连接字符串。 如果未找到提供程序名称，则该函数返回 `null`（在 Visual Basic 中返回 `Nothing`）。

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfigByProvider.cs#1)]

## <a name="encrypting-configuration-file-sections-using-protected-configuration"></a>使用受保护配置加密配置文件节

ASP.NET 2.0 引入了一个称为“受保护配置”的新功能，可以通过此功能来加密配置文件中的敏感信息。 虽然受保护配置主要是为 ASP.NET 应用程序设计的，但它也可以用于加密 Windows 应用程序中的配置文件节。 有关受保护配置功能的详细说明，请参阅[使用受保护的配置加密配置信息](/previous-versions/aspnet/53tyfkaw(v=vs.100))。

下面的配置文件片段演示加密后的 connectionStrings 节。 configProtectionProvider 可指定用于加密和解密连接字符串的受保护配置提供程序。 EncryptedData 节包含密文。

```xml  
<connectionStrings configProtectionProvider="DataProtectionConfigurationProvider">  
  <EncryptedData>  
    <CipherData>  
      <CipherValue>AQAAANCMnd8BFdERjHoAwE/Cl+sBAAAAH2... </CipherValue>  
    </CipherData>  
  </EncryptedData>  
</connectionStrings>  
```  

在运行时检索加密连接字符串时，.NET Framework 会使用指定的提供程序来解密 CipherValue 并将其提供给应用程序。 你无需额外编写任何代码来管理解密过程。

### <a name="protected-configuration-providers"></a>受保护配置提供程序

受保护配置提供程序是在本地计算机的 machine.config 文件的 configProtectedData 节中注册的（如下面的片段所示），此片段演示 .NET Framework 附带的两个受保护配置提供程序。 此处显示的值已被截断，以便于阅读。

```xml  
<configProtectedData defaultProvider="RsaProtectedConfigurationProvider">  
  <providers>  
    <add name="RsaProtectedConfigurationProvider"
      type="System.Configuration.RsaProtectedConfigurationProvider" />  
    <add name="DataProtectionConfigurationProvider"
      type="System.Configuration.DpapiProtectedConfigurationProvider" />  
  </providers>  
</configProtectedData>  
```  

可以配置其他受保护配置提供程序，方法是将它们添加到 machine.config 文件中。 还可以通过从 <xref:System.Configuration.ProtectedConfigurationProvider> 抽象基类继承来创建自己的受保护配置提供程序。 下表描述了 .NET Framework 附带的两个配置文件。

|提供程序|描述|  
|--------------|-----------------|  
|<xref:System.Configuration.RsaProtectedConfigurationProvider>|使用 RSA 加密算法来加密和解密数据。 RSA 算法既可用于公钥加密，也可用于数字签名。 它还称为“公共密钥”或非对称加密，因为它使用两个不同的密钥。 可以使用 [ASP.NET IIS 注册工具 (Aspnet_regiis.exe)](/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90)) 来加密 Web.config 文件中的节和管理加密密钥。 ASP.NET 在处理配置文件时解密该文件。 ASP.NET 应用程序的标识必须对用于加密和解密各加密节的加密密钥具有读取权限。|  
|<xref:System.Configuration.DpapiProtectedConfigurationProvider>|使用 Windows Data Protection API (DPAPI) 来加密配置节。 它使用 Windows 内置加密服务，并可为计算机特定或用户帐户特定保护进行配置。 对于同一服务器上需要共享信息的多个应用程序来说，计算机特定保护非常有用。 用户帐户特定保护可与以特定用户标识运行的服务（如共享宿主环境）一起使用。 每个应用程序都在单独的标识下运行，这会限制对文件和数据库等资源的访问。|

这两种提供程序都可以对数据进行强加密。 但是，如果计划在多台服务器（如网络场）上使用相同的加密配置文件，则只有通过 <xref:System.Configuration.RsaProtectedConfigurationProvider> 才能导出用于加密数据的加密密钥，并将其导入其他服务器。 有关详细信息，请参阅[导入和导出受保护配置的 RSA 密钥容器](/previous-versions/aspnet/yxw286t2(v=vs.100))。

### <a name="using-the-configuration-classes"></a>使用配置类

<xref:System.Configuration> 命名空间提供以编程方式使用配置设置的类。 <xref:System.Configuration.ConfigurationManager> 类可提供对计算机、应用程序和用户配置文件的访问。 如果要创建 ASP.NET 应用程序，可以使用 <xref:System.Web.Configuration.WebConfigurationManager> 类，它可提供相同的功能，同时还允许访问 ASP.NET 应用程序特有的设置（如 \<system.web> 中的设置）。

> [!NOTE]
> <xref:System.Security.Cryptography> 命名空间包含提供用于加密和解密数据的其他选项的类。 如果需要采用在使用受保护配置时不可用的加密服务，请使用这些类。 一些类是非托管 Microsoft CryptoAPI 的包装类，而其他类则是纯托管实现。 有关更多信息，请参阅[加密服务](/previous-versions/visualstudio/visual-studio-2008/93bskf9z(v=vs.90))。

### <a name="appconfig-example"></a>App.config 示例

此示例演示如何对 Windows 应用程序的 app.config 文件中的 connectionStrings 节的加密进行切换。 在此示例中，该过程将应用程序的名称（例如“MyApplication.exe”）作为一个参数。 然后，将对 app.config 文件进行加密，并采用名称“MyApplication.exe.config”将其复制到包含可执行文件的文件夹中。

> [!NOTE]
> 只能在加密连接字符串的计算机上对其进行解密。

代码使用 <xref:System.Configuration.ConfigurationManager.OpenExeConfiguration%2A> 方法打开 app.config 文件以进行编辑，<xref:System.Configuration.ConfigurationManager.GetSection%2A> 方法返回 connectionStrings 节。 然后，代码检查 <xref:System.Configuration.SectionInformation.IsProtected%2A> 属性，调用 <xref:System.Configuration.SectionInformation.ProtectSection%2A> 以加密未加密的节。 调用 <xref:System.Configuration.SectionInformation.UnprotectSection%2A> 方法以加密该节。 <xref:System.Configuration.Configuration.Save%2A> 方法完成该操作并保存所做更改。

> [!NOTE]
> 必须在要运行代码的项目中设置对 `System.Configuration.dll` 的引用。

[!code-csharp[DataWorks ConnectionStrings.Encrypt#1](~/../SqlClient/doc/samples/ConnectionStrings_Encrypt.cs#1)]

### <a name="webconfig-example"></a>Web.config 示例

此示例使用 <xref:System.Web.Configuration.WebConfigurationManager.OpenWebConfiguration%2A> 的 `WebConfigurationManager` 方法。 注意，在这种情况下，可以通过使用波形符来提供 Web.config 文件的相对路径。 代码需要对 `System.Web.Configuration` 类的引用。  
  
[!code-csharp[DataWorks ConnectionStringsWeb.Encrypt#1](~/../SqlClient/doc/samples/ConnectionStringsWeb_Encrypt.cs#1)]

有关保护 ASP.NET 应用程序的详细信息，请参阅[保护 ASP.NET 网站](/previous-versions/aspnet/91f66yxt(v=vs.100))。

## <a name="see-also"></a>另请参阅

- [连接字符串生成器](connection-string-builders.md)
- [保护连接信息](protecting-connection-information.md)
- [使用配置类](/previous-versions/visualstudio/visual-studio-2008/ms228063(v=vs.90))
- [配置应用程序](/dotnet/framework/configure-apps/index)
- [ASP.NET 网站管理](/previous-versions/aspnet/6hy1xzbw(v=vs.100))
