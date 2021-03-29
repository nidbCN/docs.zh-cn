---
title: SqlMetal.exe（代码生成工具）
description: 了解 SqlMetal.exe（代码生成工具）。 使用此工具为 .NET 的 LINQ to SQL 组件生成代码和映射。
ms.date: 03/30/2017
helpviewer_keywords:
- SQLMetal [LINQ to SQL]
- code generation tool
- SQLMetal.exe
- LINQ to SQL, serialization
- LINQ to SQL, DBML files
- LINQ to SQL, SQLMetal
ms.assetid: 819e5a96-7646-4fdb-b14b-fe31221b0614
ms.openlocfilehash: 57229ce767a140e1756fddb2c19fb31893a0ab90
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653616"
---
# <a name="sqlmetalexe-code-generation-tool"></a>SqlMetal.exe（代码生成工具）

SqlMetal 命令行工具可为 [!INCLUDE[vbtecdlinq](../../../includes/vbtecdlinq-md.md)] 的 .NET Framework 组件生成代码和映射。 通过应用本主题后面出现的选项，可以指示 SqlMetal 执行若干种不同的操作，其中包括：  
  
- 从数据库生成源代码和映射特性或映射文件。  
  
- 从数据库生成供自定义使用的中间数据库标记语言 (.dbml) 文件。  
  
- 从 .dbml 文件生成代码和映射特性或映射文件。  
  
此工具会自动随 Visual Studio 一起安装。 默认情况下，此文件位于 `drive`:\Program Files\Microsoft SDKs\Windows\v`n.nn`\bin 下。 如果没有安装 Visual Studio，也可以通过下载 [Windows SDK](https://go.microsoft.com/fwlink/?LinkId=142225)获取 SQLMetal 文件。  
  
> [!NOTE]
> 使用 Visual Studio 的开发人员还可以使用对象关系设计器生成实体类。 对于大型数据库，这种命令行方法具有很好的扩展性。 由于 SqlMetal 是一个命令行工具，因此可以在生成过程中使用它。  
  
若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。 在命令提示符处，输入下列命令：

```console  
sqlmetal [options] [<input file>]  
```  
  
## <a name="options"></a>选项  

 要查看最新的选项列表，请在安装位置的命令提示符处键入 `sqlmetal /?` 。  
  
 **连接选项**  
  
|选项|说明|  
|------------|-----------------|  
|**/server:** *\<name>*|指定数据库服务器名称。|  
|**/database:** *\<name>*|指定服务器上的数据库目录。|  
|**/user:** *\<name>*|指定登录用户 ID。默认值：使用 Windows 身份验证。|  
|**/password:** *\<password>*|指定登录密码。 默认值：使用 Windows 身份验证。|  
|**/conn:** *\<connection string>*|指定数据库连接字符串。 不能与 **/server**、 **/database**、 **/user** 或 **/password** 选项一起使用。<br /><br /> 不要在连接字符串中包含文件名， 而应将文件名作为输入文件添加到命令行中。 例如，下一行命令将“c:\northwnd.mdf”指定为输入文件： **sqlmetal /code:"c:\northwind.cs" /language:csharp "c:\northwnd.mdf"** 。|  
|**/timeout:** *\<seconds>*|指定 SqlMetal 访问数据库时的超时值。 默认值：0（即没有时间限制）。|  
  
 **提取选项**  
  
|选项|说明|  
|------------|-----------------|  
|**/views**|提取数据库视图。|  
|**/functions**|提取数据库函数。|  
|**/sprocs**|提取存储过程。|  
  
 **输出选项**  
  
|选项|说明|  
|------------|-----------------|  
|/dbml [:file]|采用 .dbml 格式发送输出。 不能与 **/map** 选项一起使用。|  
|/code [:file]|以源代码形式发送输出。 不能与 **/dbml** 选项一起使用。|  
|/map [:file]|生成 XML 映射文件而不是特性。 不能与 **/dbml** 选项一起使用。|  
  
 **杂项**  
  
|选项|说明|  
|------------|-----------------|  
|**/language:** *\<language>*|指定源代码语言。<br /><br /> 有效的 *\<language>* ：vb、csharp。<br /><br /> 默认值：从代码文件的扩展名派生。|  
|**/namespace:** *\<name>*|为生成的代码指定命名空间。 默认值：无命名空间。|  
|**/context:** *\<type>*|指定数据上下文类的名称。 默认值：从数据库名称派生。|  
|**/entitybase:** *\<type>*|指定生成的代码中的实体类的基类。 默认值：实体没有基类。|  
|**/pluralize**|自动为类和成员名称应用复数或单数形式。<br /><br /> 此选项仅在美国可用。英文版。|  
|**/serialization:** *\<option>*|生成可序列化类。<br /><br /> 有效的 *\<option>* ：无，单向。 默认值：无。<br /><br /> 有关详细信息，请参阅[序列化](../data/adonet/sql/linq/serialization.md)。|  
  
 **输入文件**  
  
|选项|说明|  
|------------|-----------------|  
|**\<input file>**|指定 SQL Server Express .mdf 文件、SQL Server Compact 3.5 .sdf 文件或 .dbml intermediate 文件。|  
  
## <a name="remarks"></a>备注  

 SqlMetal 功能实际涉及两个步骤：  
  
- 将数据库的元数据提取到一个 .dbml 文件中。  
  
- 生成一个代码输出文件。  
  
     通过使用适当的命令行选项，可以生成 Visual Basic 或 C# 源代码，也可以生成 XML 映射文件。  
  
 若要从 .mdf 文件中提取元数据，必须在所有其他选项后指定 .mdf 文件的名称。  
  
 如果没有指定 **/server** ，则假定为 **localhost/sqlexpress** 。  
  
 如果下述一个或多个条件为 true，则 Microsoft SQL Server 2005 会引发异常：  
  
- SqlMetal 尝试提取进行自我调用的存储过程。  
  
- 存储过程、函数或视图的嵌套级别超过 32。  
  
     SqlMetal 将捕获此异常并将其报告为警告。  
  
 若要指定一个输入文件名，请将该名称作为输入文件添加到命令行。 不支持在连接字符串中包含文件名（使用 **/conn** 选项）。  
  
## <a name="examples"></a>示例  

 生成一个包含提取的 SQL 元数据的 .dbml 文件：  
  
 **sqlmetal /server:myserver /database:northwind /dbml:mymeta.dbml**  
  
 使用 SQL Server Express 生成一个包含从 .mdf 文件中提取的 SQL 元数据的 .dbml 文件：  
  
 **sqlmetal /dbml:mymeta.dbml mydbfile.mdf**  
  
 生成一个包含从 SQL Server Express 中提取的 SQL 元数据的 .dbml 文件：  
  
 **sqlmetal /server:.\sqlexpress /dbml:mymeta.dbml /database:northwind**  
  
 从 .dbml 元数据文件生成源代码：  
  
 **sqlmetal /namespace:nwind /code:nwind.cs /language:csharp mymetal.dbml**  
  
 直接从 SQL 元数据生成源代码：  
  
 **sqlmetal /server:myserver /database:northwind /namespace:nwind /code:nwind.cs /language:csharp**  
  
> [!NOTE]
> 如果对 Northwind 示例数据库应用 **/pluralize** 选项，请注意以下行为。 如果 SqlMetal 为表创建了行类型的名称，表名将采用单数形式。 如果它为表创建了 <xref:System.Data.Linq.DataContext> 属性，则表名将采用复数形式。 巧合的是，Northwind 示例数据库中的表名已采用复数形式。 因此，你将看不到这种效果。 尽管数据库表普遍命名为单数形式，但在 .NET 中，将集合命名为复数形式也是一种常见的做法。  
  
## <a name="see-also"></a>请参阅

- [如何：在 Visual Basic 或 C# 中生成对象模型](../data/adonet/sql/linq/how-to-generate-the-object-model-in-visual-basic-or-csharp.md)
- [LINQ to SQL 中的代码生成](../data/adonet/sql/linq/code-generation-in-linq-to-sql.md)
- [外部映射](../data/adonet/sql/linq/external-mapping.md)
