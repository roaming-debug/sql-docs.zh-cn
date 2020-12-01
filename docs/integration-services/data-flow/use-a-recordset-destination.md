---
description: 使用记录集目标
title: 使用记录集目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Recordset destination
ms.assetid: a7b143dc-8008-404f-83b0-b45ffbca6029
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4d823048d128d8837c80c724064ea0d8afe1a5a7
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425599"
---
# <a name="use-a-recordset-destination"></a>使用记录集目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  记录集目标不会将数据保存到外部数据源中， 而是将数据保存在內存中的一个记录集中，该记录集存储在数据类型为 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Object **的** 包变量中。 在记录集目标保存数据之后，通常使用具有 Foreach ADO 枚举器的 Foreach 循环容器来每次处理记录集的一行。 Foreach ADO 枚举器将当前行中每列的值保存到单独的包变量中。 然后，您在 Foreach 循环容器中配置的任务会从变量中读取这些值，并对它们执行某些操作。  
  
 可以在很多不同情况下使用记录集目标。 下面是一些示例：  
  
-   可以使用发送邮件任务和 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 表达式语言来为记录集中的每一行发送一封自定义电子邮件。  
  
-   可以使用在数据流任务中配置为源的脚本组件，将列值读入到数据流的相应列中。 然后，可以使用转换和目标来转换和保存该行。 在本示例中，将针对每行运行一次数据流任务。  
  
 以下各节首先介绍使用记录集目标的一般过程，然后介绍如何使用目标的特定示例。  
  
## <a name="general-steps-to-using-a-recordset-destination"></a>使用记录集目标的一般步骤  
 以下过程总结了将数据保存到记录集目标然后使用 Foreach 循环容器来处理每行所需的步骤。  
  
#### <a name="to-save-data-to-a-recordset-destination-and-process-each-row-by-using-the-foreach-loop-container"></a>将数据保存到记录集目标，然后使用 Foreach 循环容器来处理每行  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，创建或打开 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  创建一个用来包含由记录集目标保存到内存中的记录集的变量，然后将该变量的类型设置为 **Object**。  
  
3.  创建适当类型的其他变量，用来包含您要使用的记录集的每列的值。  
  
4.  添加和配置您计划在数据流中使用的数据源所需的连接管理器。  
  
5.  向包中添加数据流任务，然后在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器的“数据流”选项卡上配置源和转换，以加载并转换数据。  
  
6.  向数据流中添加记录集目标，并将其连接到转换。 对于记录集目标的 **VariableName** 属性，输入您创建的用来保存该记录集的变量的名称。  
  
7.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器的“控制流”选项卡上，添加 Foreach 循环容器，并在数据流任务之后连接此容器。 然后，打开 **“Foreach 循环编辑器”** ，使用以下设置来配置容器：  
  
    1.  在 **“集合”** 页上，选择 Foreach ADO 枚举器。 然后，对于 **“ADO 对象源变量”**，选择包含该记录集的变量。  
  
    2.  在“变量映射”页上，将你要使用的每列的基于零的索引映射到适当的变量。  
  
         在循环的每次迭代中，枚举器使用当前行的列值来填充这些变量。  
  
8.  在 Foreach 循环容器中，添加并配置任务以通过从变量读取值来每次处理记录集的一行。  
  
## <a name="example-of-using-the-recordset-destination"></a>使用记录集目标的示例  
 在以下示例中，数据流任务将有关 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 雇员的信息从 Sales.SalesPerson 表加载到记录集目标中。 然后，Foreach 循环容器每次读取一行数据，并调用发送邮件任务。 发送邮件任务使用表达式将有关奖金额的自定义电子邮件发送给每位销售人员。  
  
#### <a name="to-create-the-project-and-configure-the-variables"></a>创建项目和配置变量  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，创建新的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在 **SSIS** 菜单上，选择 **“变量”**。  
  
3.  在 **“变量”** 窗口中，创建用来保存记录集和当前行的列值的变量：  
  
    1.  创建名为 **BonusRecordset** 的变量，然后将其类型设置为 **Object**。  
  
         **BonusRecordset** 变量保存记录集。  
  
    2.  创建名为 **EmailAddress** 的变量，然后将其类型设置为 **String**。  
  
         **EmailAddress** 变量保存销售人员的电子邮件地址。  
  
    3.  创建名为 **FirstName** 的变量，然后将其类型设置为 **String**。  
  
         **FirstName** 变量保存销售人员的名字。  
  
    4.  创建名为 **Bonus** 的变量，然后将其类型设置为 **Double**。  
  
         **Bonus** 变量保存销售人员的奖金额。  
  
#### <a name="to-configure-the-connection-managers"></a>配置连接管理器  
  
1.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器的“连接管理器”区域中，添加并配置连接到 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 示例数据库的新 OLE DB 连接管理器。  
  
     数据流任务中的 OLE DB 源将使用此连接管理器来检索数据。  
  
2.  在“连接管理器”区域中，添加并配置连接到可用 SMTP 服务器的新 SMTP 连接管理器。  
  
     Foreach 循环容器中的发送邮件任务将使用此连接管理器来发送电子邮件。  
  
#### <a name="to-configure-the-data-flow-and-the-recordset-destination"></a>配置数据流和记录集目标  
  
1.  在 **设计器的** “控制流” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 选项卡上，将数据流任务添加到设计图面中。  
  
2.  在 **“数据流”** tab, add an OLE DB source to the “数据流” task, and then open the **“OLE DB 源编辑器”**。  
  
3.  在编辑器的 **“连接管理器”** 页上，使用以下设置来配置源：  
  
    1.  对于 **“OLE DB 连接管理器”**，选择以前创建的 OLE DB 连接管理器。  
  
    2.  对于 **“数据访问模式”**，选择 **“SQL 命令”**。  
  
    3.  对于 **“SQL 命令文本”**，输入以下查询：  
  
        ```sql 
        SELECT     Person.Contact.EmailAddress, Person.Contact.FirstName, CONVERT(float, Sales.SalesPerson.Bonus) AS Bonus  
        FROM         Sales.SalesPerson INNER JOIN  
                              Person.Contact ON Sales.SalesPerson.SalesPersonID = Person.Contact.ContactID  
        ```  
  
        > [!NOTE]  
        >  必须先将 Bonus 列中 **currency** 值转换为 **float** 类型，然后才可以将该值加载到类型为 **Double** 的包变量中。  
  
4.  在 **“数据流”** 选项卡上，添加记录集目标，并在 OLE DB 源之后连接该目标。  
  
5.  打开 **“记录集目标编辑器”**，并使用以下设置来配置目标：  
  
    1.  在“组件属性”  选项卡上，为 **VariableName** 属性选择 **User::BonusRecordset**。  
  
    2.  在 **“输入列”** 选项卡上，选择所有三个可用列。  
  
#### <a name="to-configure-the-foreach-loop-container-and-run-the-package"></a>配置 Foreach 循环容器并运行包  
  
1.  在 **设计器的** “控制流” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 选项卡上，添加 Foreach 循环容器，并在数据流任务之后连接该容器。  
  
2.  打开 **“Foreach 循环编辑器”**，并使用以下设置来配置容器：  
  
    1.  在“集合”  页上，为“枚举器” 选择“Foreach ADO 枚举器” ，并为“ADO 对象源变量” 选择 **User::BonusRecordset**。  
  
    2.  在“变量映射”  页中，将 **User::EmailAddress** 映射到索引 0，将 **User::FirstName** 映射到索引 1，将 **User::Bonus** 映射到索引 2。  
  
3.  在 **“控制流”** 选项卡的 Foreach 循环容器中，添加发送邮件任务。  
  
4.  打开 **“发送邮件任务编辑器”**，然后在 **“邮件”** 页上使用以下设置来配置任务：  
  
    1.  对于 **SmtpConnection**，选择以前配置的 SMTP 连接管理器。  
  
    2.  对于“发件人”，输入适当的电子邮件地址。  
  
         如果使用自己的电子邮件地址，则可以确认包成功运行。 对于由发送邮件任务发送到 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]中的虚构销售人员的邮件，您将收到关于无法投递的回执。  
  
    3.  对于“收件人”，输入默认电子邮件地址。  
  
         此值不会使用，但在运行时将替换为每位销售人员的电子邮件地址。  
  
    4.  对于 **“主题”**，输入“Your annual bonus”。  
  
    5.  对于 **MessageSourceType**，选择 **“直接输入”**。  
  
5.  在“发送邮件任务编辑器”的“表达式”页上，单击省略号按钮 (**...**)，以打开“属性表达式编辑器”。  
  
6.  在 **“属性表达式编辑器”** 中，输入以下信息：  
  
    1.  对于 **ToLine**，添加以下表达式：  
  
        ```  
        @[User::EmailAddress]  
        ```  
  
    2.  对于 **MessageSource** 属性，添加以下表达式：  
  
        ```  
        "Dear " +  @[User::FirstName] + ": The amount of your bonus for this year is $" +  (DT_WSTR, 12) @[User::Bonus] + ". Thank you!"  
        ```  
  
7.  运行包。  
  
     如果已经指定有效的 SMTP 服务器并提供了自己的电子邮件地址，则对于由发送邮件任务发送到 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]中的虚构销售人员的邮件，您将收到关于无法投递的回执。  
  
  
