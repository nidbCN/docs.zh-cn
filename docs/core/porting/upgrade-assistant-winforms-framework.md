---
title: 将 Windows 窗体应用升级到 .NET 5
description: 使用 .NET 升级助手将现有的 .NET Framework Windows 窗体应用升级到 .NET 5。 .NET 升级助手是一种 CLI 工具，可帮助将应用从 .NET Framework 迁移到 .NET 5。
author: ardalis
ms.date: 03/08/2021
ms.openlocfilehash: 1ba80964c52cb9b6960efdebbd8d4e8ef33d63e6
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604822"
---
# <a name="upgrade-a-windows-forms-app-to-net-5-with-the-net-upgrade-assistant"></a><span data-ttu-id="0f440-104">使用 .NET 升级助手将 Windows 窗体应用升级到 .NET 5</span><span class="sxs-lookup"><span data-stu-id="0f440-104">Upgrade a Windows Forms App to .NET 5 with the .NET Upgrade Assistant</span></span>

<span data-ttu-id="0f440-105">[.NET 升级助手](upgrade-assistant-overview.md)是一种命令行工具，可帮助将 .NET Framework Windows 窗体 (WinForms) 应用升级到 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="0f440-105">The [.NET Upgrade Assistant](upgrade-assistant-overview.md) is a command-line tool that can assist with upgrading .NET Framework Windows Forms (WinForms) apps to .NET 5.</span></span> <span data-ttu-id="0f440-106">本文提供以下内容：</span><span class="sxs-lookup"><span data-stu-id="0f440-106">This article provides:</span></span>

- <span data-ttu-id="0f440-107">演示如何针对 .NET Framework Windows 窗体应用运行该工具</span><span class="sxs-lookup"><span data-stu-id="0f440-107">A demonstration of how to run the tool against a .NET Framework Windows Forms app</span></span>
- <span data-ttu-id="0f440-108">故障排除提示</span><span class="sxs-lookup"><span data-stu-id="0f440-108">Troubleshooting tips</span></span>

## <a name="upgrade-net-framework-windows-forms-apps"></a><span data-ttu-id="0f440-109">升级 .NET Framework Windows 窗体应用</span><span class="sxs-lookup"><span data-stu-id="0f440-109">Upgrade .NET Framework Windows Forms apps</span></span>

<span data-ttu-id="0f440-110">本部分演示如何针对新创建的面向 .NET Framework 4.6.1 的 Windows 窗体应用运行 NET 升级助手。</span><span class="sxs-lookup"><span data-stu-id="0f440-110">This section demonstrates running the .NET Upgrade Assistant against a newly created Windows Forms app targeting .NET Framework 4.6.1.</span></span> <span data-ttu-id="0f440-111">若要详细了解如何安装此工具，请查看 [.NET 升级助手概述](upgrade-assistant-overview.md)。</span><span class="sxs-lookup"><span data-stu-id="0f440-111">For more information on how to install the tool, see [Overview of the .NET Upgrade Assistant](upgrade-assistant-overview.md).</span></span>

### <a name="initial-demo-setup"></a><span data-ttu-id="0f440-112">初始演示设置</span><span class="sxs-lookup"><span data-stu-id="0f440-112">Initial demo setup</span></span>

<span data-ttu-id="0f440-113">如果你要针对你自己的 .NET Framework 应用运行 .NET 升级助手，可跳过此步骤。</span><span class="sxs-lookup"><span data-stu-id="0f440-113">If you're running the .NET Upgrade Assistant against your own .NET Framework app, you can skip this step.</span></span> <span data-ttu-id="0f440-114">如果你只想试用一下来看看它的工作原理，可在此步骤中了解如何设置示例 .NET Windows 窗体项目以供使用。</span><span class="sxs-lookup"><span data-stu-id="0f440-114">If you just want to try it out to see how it works, this step shows you how to set up a sample .NET Windows Forms project to use.</span></span>

<span data-ttu-id="0f440-115">借助 Visual Studio，使用 .NET Framework 创建一个新的 Windows 窗体项目。</span><span class="sxs-lookup"><span data-stu-id="0f440-115">Using Visual Studio, create a new Windows Forms project using .NET Framework.</span></span>

:::image type="content" source="media/upgrade-assistant-winforms-framework/vs-new-project-winforms.png" alt-text="在 Visual Studio 中新建 Windows 窗体项目":::

<span data-ttu-id="0f440-117">将项目命名为 WinformsTest。</span><span class="sxs-lookup"><span data-stu-id="0f440-117">Name the project **WinformsTest**.</span></span> <span data-ttu-id="0f440-118">将项目配置为使用 .NET Framework 4.6.1。</span><span class="sxs-lookup"><span data-stu-id="0f440-118">Configure the project to use **.NET Framework 4.6.1**.</span></span>

:::image type="content" source="media/upgrade-assistant-winforms-framework/vs-configure-project-winforms.png" alt-text="在 Visual Studio 中配置 Windows 窗体项目":::

<span data-ttu-id="0f440-120">查看所创建的项目及其文件，尤其是它的项目文件。</span><span class="sxs-lookup"><span data-stu-id="0f440-120">Review the created project and its files, especially its project file(s).</span></span>

### <a name="run-upgrade-assistant"></a><span data-ttu-id="0f440-121">运行升级助手</span><span class="sxs-lookup"><span data-stu-id="0f440-121">Run upgrade-assistant</span></span>

<span data-ttu-id="0f440-122">打开终端，导航到目标项目或解决方案所在的文件夹。</span><span class="sxs-lookup"><span data-stu-id="0f440-122">Open a terminal and navigate to the folder where the target project or solution is located.</span></span> <span data-ttu-id="0f440-123">运行 `upgrade-assistant` 命令，传入你要针对的项目的名称（可从任意位置运行该命令，只要项目文件的路径有效就行）。</span><span class="sxs-lookup"><span data-stu-id="0f440-123">Run the `upgrade-assistant` command, passing in the name of the project you're targeting (you can run the command from anywhere, as long as the path to the project file is valid).</span></span>

```console
upgrade-assistant .\WinformsTest.csproj
```

<span data-ttu-id="0f440-124">该工具将运行并显示它将执行的步骤列表。</span><span class="sxs-lookup"><span data-stu-id="0f440-124">The tool runs and shows you a list of the steps it will do.</span></span>

:::image type="content" source="media/upgrade-assistant-winforms-framework/step1.png" alt-text=".NET 升级助手初始屏幕":::

<span data-ttu-id="0f440-126">完成每个步骤后，该工具都会提供一组命令，用户可应用这些命令，也可跳过下一步骤、查看更多详细信息、配置日志记录或退出该过程。</span><span class="sxs-lookup"><span data-stu-id="0f440-126">As each step is completed, the tool provides a set of commands allowing the user to apply or skip the next step, see more details, configure logging, or exit the process.</span></span> <span data-ttu-id="0f440-127">如果该工具检测到某个步骤将不执行任何操作，它将自动跳过该步骤，转到下一步骤，直到到达有要执行的操作的步骤为止。</span><span class="sxs-lookup"><span data-stu-id="0f440-127">If the tool detects that a step will perform no actions, it will automatically skip that step and continue to the next step until it reaches one that will have actions to perform.</span></span> <span data-ttu-id="0f440-128">如果未进行其他任何选择，那么按 Enter 将执行下一步。</span><span class="sxs-lookup"><span data-stu-id="0f440-128">Pressing enter will perform the next step if no other selection is made.</span></span>

<span data-ttu-id="0f440-129">在此示例中，每次都会选择“应用”步骤。</span><span class="sxs-lookup"><span data-stu-id="0f440-129">In this example, the apply step is chosen each time.</span></span> <span data-ttu-id="0f440-130">第一步是备份项目。</span><span class="sxs-lookup"><span data-stu-id="0f440-130">The first step is to back up the project.</span></span>

:::image type="content" source="media/upgrade-assistant-winforms-framework/backup-project.png" alt-text=".NET 升级助手备份项目":::

<span data-ttu-id="0f440-132">该工具会提示输入自定义路径进行备份或使用默认路径，后者会将项目备份放在具有 `.backup` 扩展名的同一文件夹中。</span><span class="sxs-lookup"><span data-stu-id="0f440-132">The tool prompts for a custom path for the backup, or to use the default, which will place the project backup in the same folder with a `.backup` extension.</span></span> <span data-ttu-id="0f440-133">此工具接下来做的是将项目文件转换为 SDK 样式。</span><span class="sxs-lookup"><span data-stu-id="0f440-133">The next step the tool does is to convert the project file to SDK style.</span></span>

:::image type="content" source="media/upgrade-assistant-winforms-framework/convert-project.png" alt-text=".NET 升级助手将项目转换为 SDK 样式":::

<span data-ttu-id="0f440-135">更新项目格式后，下一步是更新项目的 TFM。</span><span class="sxs-lookup"><span data-stu-id="0f440-135">Once the project format has been updated, the next step is to update the TFM of the project.</span></span>

:::image type="content" source="media/upgrade-assistant-winforms-framework/update-tfm.png" alt-text=".NET 升级助手更新 TFM":::

<span data-ttu-id="0f440-137">接下来，该工具会更新项目的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="0f440-137">Next, the tool updates the project's NuGet packages.</span></span>

:::image type="content" source="media/upgrade-assistant-winforms-framework/update-nuget-packages.png" alt-text=".NET 升级助手更新 NuGet 包":::

<span data-ttu-id="0f440-139">更新包后，接下来是添加模板文件（如果有）。</span><span class="sxs-lookup"><span data-stu-id="0f440-139">Once the packages are updated, the next step is to add template files, if any.</span></span> <span data-ttu-id="0f440-140">在本例中，没有需要添加的模板文件。</span><span class="sxs-lookup"><span data-stu-id="0f440-140">In this case, there are no template files that need to be added.</span></span> <span data-ttu-id="0f440-141">该步骤将继续，迁移应用配置文件并更新 C# 源来应用修补程序，如下所示。</span><span class="sxs-lookup"><span data-stu-id="0f440-141">This step continues and migrates app config files and updates C# source to apply fixes, as shown below.</span></span> <span data-ttu-id="0f440-142">此项目无需任何配置文件或源代码更改，因此会自动继续这些步骤。</span><span class="sxs-lookup"><span data-stu-id="0f440-142">This project didn't need any config file or source code changes, so these steps proceeded automatically.</span></span>

:::image type="content" source="media/upgrade-assistant-winforms-framework/add-template-files.png" alt-text=".NET 升级助手添加模板文件和迁移配置":::

<span data-ttu-id="0f440-144">这是最后一个项目，因此下一步是“移动到新的项目”，它提示完成迁移整个解决方案的过程。</span><span class="sxs-lookup"><span data-stu-id="0f440-144">Since this is the last project, the next step, "Move to next project", prompts to complete the process of migrating the entire solution.</span></span>

:::image type="content" source="media/upgrade-assistant-winforms-framework/complete-solution.png" alt-text=".NET 升级助手完成解决方案":::

<span data-ttu-id="0f440-146">完成此过程后，已迁移的 Windows 窗体项目将如下所示：</span><span class="sxs-lookup"><span data-stu-id="0f440-146">Once this process has completed, the migrated Windows Forms project will look something like this:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net5.0-windows</TargetFramework>
    <OutputType>WinExe</OutputType>
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    <UseWindowsForms>true</UseWindowsForms>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.CSharp" Version="4.7.0" />
    <PackageReference Include="Microsoft.DotNet.UpgradeAssistant.Extensions.Default.Analyzers" Version="0.2.211730">
      <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Microsoft.Windows.Compatibility" Version="5.0.2" />
  </ItemGroup>
</Project>
```

<span data-ttu-id="0f440-147">请注意，.NET 升级助手还会向项目添加分析器，它可帮助继续执行升级过程。</span><span class="sxs-lookup"><span data-stu-id="0f440-147">Notice that the .NET Upgrade Assistant also adds analyzers to the project that assist with continuing the upgrade process.</span></span>

## <a name="troubleshooting-tips"></a><span data-ttu-id="0f440-148">故障排除提示</span><span class="sxs-lookup"><span data-stu-id="0f440-148">Troubleshooting tips</span></span>

<span data-ttu-id="0f440-149">使用 .NET 升级助手时，可能会出现一些已知问题。</span><span class="sxs-lookup"><span data-stu-id="0f440-149">There are several known problems that can occur when using the .NET Upgrade Assistant.</span></span> <span data-ttu-id="0f440-150">某些情况下，.NET 升级助手在内部使用的 [try-convert 工具](https://github.com/dotnet/try-convert)会出现问题。</span><span class="sxs-lookup"><span data-stu-id="0f440-150">In some cases, these are problems with the [try-convert tool](https://github.com/dotnet/try-convert) that the .NET Upgrade Assistant uses internally.</span></span> <span data-ttu-id="0f440-151">此工具会频繁更新来处理更多场景，因此请务必使用最新版本。</span><span class="sxs-lookup"><span data-stu-id="0f440-151">This tool is being frequently updated to address more scenarios, so make sure you're using a recent version.</span></span>

- <span data-ttu-id="0f440-152">必须安装 try-convert 并将其更新到不低于 0.7.212201 版本。</span><span class="sxs-lookup"><span data-stu-id="0f440-152">The **try-convert** tool must be installed and updated to at least version _0.7.212201_.</span></span>
- <span data-ttu-id="0f440-153">更低版本的 try-convert 工具不支持自定义目标或属性文件。</span><span class="sxs-lookup"><span data-stu-id="0f440-153">Earlier versions of the **try-convert** tool didn't support custom target or props files.</span></span> <span data-ttu-id="0f440-154">如果无法升级到最新版，则可能需要手动处理这些问题。</span><span class="sxs-lookup"><span data-stu-id="0f440-154">If you can't upgrade to the latest version, you may need to manually address these issues.</span></span> <span data-ttu-id="0f440-155">如果目标项目文件包含对自定义目标或属性文件的引用，则在对其运行 .NET 升级助手之前，可能需要从文件中手动删除这些引用。</span><span class="sxs-lookup"><span data-stu-id="0f440-155">If the target project file includes references to custom targets or props files, these references may need to be manually deleted from the file before the .NET Upgrade Assistant is run against it.</span></span>

<span data-ttu-id="0f440-156">有关其他故障排除提示和已知问题，可查看[此工具的 GitHub 存储库](https://github.com/dotnet/upgrade-assistant#troubleshooting-common-issues)。</span><span class="sxs-lookup"><span data-stu-id="0f440-156">[The tool's GitHub repository](https://github.com/dotnet/upgrade-assistant#troubleshooting-common-issues) has additional troubleshooting tips and known issues.</span></span>

## <a name="see-also"></a><span data-ttu-id="0f440-157">另请参阅</span><span class="sxs-lookup"><span data-stu-id="0f440-157">See also</span></span>

- [<span data-ttu-id="0f440-158">使用 .NET 升级助手将 WPF 应用升级到 .NET 5</span><span class="sxs-lookup"><span data-stu-id="0f440-158">Upgrade a WPF App to .NET 5 with the .NET Upgrade Assistant</span></span>](upgrade-assistant-wpf-framework.md)
- [<span data-ttu-id="0f440-159">使用 .NET 升级助手将 ASP.NET MVC 应用升级到 .NET 5</span><span class="sxs-lookup"><span data-stu-id="0f440-159">Upgrade an ASP.NET MVC App to .NET 5 with the .NET Upgrade Assistant</span></span>](upgrade-assistant-aspnetmvc.md)
- [<span data-ttu-id="0f440-160">.NET 升级助手概述</span><span class="sxs-lookup"><span data-stu-id="0f440-160">Overview of the .NET Upgrade Assistant</span></span>](upgrade-assistant-overview.md)
- [<span data-ttu-id="0f440-161">.NET 升级助手 GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="0f440-161">.NET Upgrade Assistant GitHub Repository</span></span>](https://github.com/dotnet/upgrade-assistant)
