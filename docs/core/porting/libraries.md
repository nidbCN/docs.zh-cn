---
title: 将库移植到 .NET
description: 了解如何将 .NET Framework 中的库项目移植到 .NET。
author: cartermp
ms.date: 03/04/2021
ms.openlocfilehash: 5fe7b57e583f74c12649746cd9968fde03b94435
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604939"
---
# <a name="port-net-framework-libraries-to-net"></a>将 .NET Framework 库移植到 .NET 中

了解如何将 .NET Framework 库代码移植到 .NET，它在其中跨平台运行并扩展使用它的应用的范围。

## <a name="prerequisites"></a>先决条件

本文假定你：

- 正在使用 Visual Studio 2019 或更高版本？
  - .NET 5 需要 Visual Studio 2019 (v16.9) 或更高版本。
  - .NET Core 3.1 需要 Visual Studio 2019 (v16.4) 或更高版本。
- 了解[推荐的移植过程](index.md)。
- 已解决与[第三方依赖项](third-party-deps.md)有关的任何问题。

熟悉以下文章的内容：

- [.NET Standard。](../../standard/net-standard.md)\
本文介绍了适用于所有 .NET 实现代码的 .NET API 正式规范。

- [使用 .NET CLI 开发库。](../tutorials/libraries.md)\
本文介绍如何使用 .NET CLI 编写库。

- [.NET 项目 SDK。](../project-sdk/overview.md)\
本文介绍 SDK 样式的项目文件格式。

- [分析依赖项以将代码从 .NET Framework 移植到 .NET 中。](third-party-deps.md)\
本文介绍了第三方依赖项的可移植性及 NuGet 包依赖项无法在 .NET 上运行时要执行的操作。

## <a name="retarget-to-net-framework-472"></a>重定向到 .NET Framework 4.7.2

如果代码不面向 .NET Framework 4.7.2，建议重定向到 .NET Framework 4.7.2。 在 .NET Standard 不支持现有 API 情况下，这可确保最新备用 API 的可用性。

对于每个想要移植的项目，请在 Visual Studio 中执行以下操作：

01. 右键单击该项目，然后选择“属性”。
01. 在“目标框架”下拉列表中，选择“.NET Framework 4.7.2”。
01. 重新编译该项目。

因为项目现在面向 .NET Framework 4.7.2，因此可使用该版本的 .NET Framework 作为移植代码的基准。

## <a name="determine-portability"></a>确定可移植性

下一步是运行 API 可移植性分析器 (ApiPort) 生成可供分析的可移植性报表。

确保了解 [API 可移植性分析器 (ApiPort)](../../standard/analyzers/portability-analyzer.md) 及如何生成用于面向 .NET 的可移植性报表。 执行此操作的方式可能取决于需求和个人偏好。 以下部分详细介绍了几种不同的方法。 用户可能会发现自己根据生成代码的方式混合使用了这些方法中的步骤。

### <a name="deal-primarily-with-the-compiler"></a>主要处理编译器

此方法可能较适合小项目或不会用很多 .NET Framework API 的项目。 此方法很简单：

01. 可选择在项目上运行 ApiPort。 若运行 ApiPort，则从报告获取有关需要解决的问题的信息。
01. 将所有代码复制到新的 .NET 项目。
01. 查看可移植性报表（如果已生成）时，解决编译器错误，直至项目完全得到编译。

尽管它是非结构化的，但这种以代码为中心的方法通常可以快速解决问题。 只包含数据模型的项目可能是此方法的理想选择。

### <a name="stay-on-the-net-framework-until-portability-issues-are-resolved"></a>可移植性问题得到解决前停留在 .NET Framework 上

如果更希望拥有在整个过程期间编译的代码，此方法可能是最佳选择。 该方法如下所示：

01. 在项目上运行 ApiPort。
01. 通过使用可移植的不同 API 解决问题。
01. 记录阻止你使用直接替代方案的所有区域。
01. 对所有要移植的项目重复前面的步骤，直到确信每个项目都做好被复制到新的 .NET 项目中的准备。
01. 将代码复制到新的 .NET 项目中。
01. 解决所有已记录的不存在直接替代方案的问题。

这种谨慎的方法比单纯解决编译器错误更有条理，但相对而言，它仍以代码为中心，且优点是始终拥有编译的代码。 解决不能通过只使用另一个 API 解决的某些问题的方法大不相同。 你可能会发现对于某些项目，需要制定更全面的计划，这将在下一种方法中介绍。

### <a name="develop-a-comprehensive-plan-of-attack"></a>制定全面的攻击计划

此方法可能最适合大型或更复杂的项目，在这种情况下，为支持 .NET，可能必需重构代码或将某些代码区域完全重写。 该方法如下所示：

01. 在项目上运行 ApiPort。
01. 了解每个非可移植类型使用的位置以及位置对整体可移植性的影响。

    - 了解这些类型的特性。 它们是否数量少，但使用频繁？ 它们是否数量大，但使用不频繁？ 它们是串联使用，还是在整个代码中传播？
    - 是否可以轻松隔离不可移植的代码，以便可以更有效地处理它？
    - 是否需要重构代码？
    - 对于这些不可移植的类型，是否存在可完成相同任务的备用 API？ 例如，如果使用的是 <xref:System.Net.WebClient> 类，则改用 <xref:System.Net.Http.HttpClient> 类。
    - 是否存在其他可用于完成任务的可移植 API，即使它不是直接替代 API？ 例如，如果使用 <xref:System.Xml.Schema.XmlSchema> 来分析 XML，但是无需 XML 架构发现，则可使用 <xref:System.Xml.Linq> API 并自行实现分析，而不是依赖于某个 API。

01. 如果具有难以移植的程序集，是否值得将其暂时留在 .NET Framework 上？ 以下是一些需要考虑的事项：

    - 库中可能具有某些与 .NET 不兼容的功能，因为它过于依赖 .NET Framework 或 Windows 特定的功能。 是否值得暂时搁置该功能，并直到资源可用于移植这些功能，才发布具有较少功能的库的临时 .NET 版本？
    - 重构是否有用？

01. 编写自己对不可用 .NET Framework API 的实现是否合理？

    可以考虑复制、修改，并使用 [.NET Framework 参考源](https://github.com/Microsoft/referencesource)中的代码。 参考源代码已在 [MIT 许可证](https://github.com/Microsoft/referencesource/blob/master/LICENSE.txt)下获得许可，因此可以自由选择将此源作为自己代码的基础。 只需确保在代码中正确设置 Microsoft。

01. 根据不同项目的需要，重复此过程。

分析阶段可能需要一些时间，具体取决于代码库的大小。 在此阶段花费时间（尤其是在具有复杂的代码库时），全面了解所需的更改范围并制定计划，最终通常可节省时间。

计划可能包括对代码库做重大更改，同时仍面向 .NET Framework 4.7.2。 这是前一种方法更有条理的版本。 着手执行计划的方式具体取决于代码库。

### <a name="mixed-approach"></a>混合方法

在每个项目的基础上，可能会将上述方法进行混合。 执行哪些操作对你和代码库最有意义。

## <a name="port-your-tests"></a>移植测试

要确保移植代码后一切正常的最佳方式是在将代码移植到 .NET 时进行测试。 为此，需要使用将针对 .NET 生成和运行测试的测试框架。 当前，有三个选择：

- [xUnit](https://xunit.net/)
  - [入门](https://xunit.net/docs/getting-started/netcore/cmdline)
  - [将 MSTest 项目转换为 xUnit 的工具](https://github.com/dotnet/codeformatter/tree/master/src/XUnitConverter)
- [NUnit](https://nunit.org/)
  - [入门](https://github.com/nunit/docs/wiki/Installation)
  - [关于从 MSTest 迁移到 NUnit 的博客文章](https://www.florian-rappl.de/News/Page/275/convert-mstest-to-nunit)
- [MSTest](/visualstudio/test/unit-test-basics)

## <a name="recommended-approach"></a>推荐的方法

从根本上讲，移植工作在很大程度上取决于生成 .NET Framework 代码的方式。 移植代码的一个好方法是从库的基项开始，这是代码的基础组件。 这可能是数据模型或某些其他内容直接或间接使用的基本类和方法。

01. 移植测试项目，该项目测试当前正在移植的库层。
01. 将库中的基项复制到新的 .NET 项目中，然后选择想要支持的 .NET Standard 版本。
01. 进行任何所需的更改，使代码进行编译。 大部分内容可能会要求将 NuGet 包依赖项添加到 csproj 文件。
01. 运行测试并进行任何所需调整。
01. 选择下一层代码进行移植，并重复前面的步骤。

如果从库的基项开始并从基项向外移动并根据需要测试每一层，移植将是一个系统化的过程，在这种情况下，问题可以一次隔离到一层代码中。

## <a name="next-steps"></a>后续步骤

>[!div class="nextstepaction"]
>[整理 .NET 的项目](project-structure.md)
