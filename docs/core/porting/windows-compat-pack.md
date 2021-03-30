---
title: 使用 Windows 兼容性包移植代码
description: 了解 Windows 兼容性包，以及如何使用它将现有 .NET Framework 代码移植到 .NET 5 和 .NET Core 3.1。
author: terrajobst
ms.date: 03/04/2021
ms.openlocfilehash: 655657e38f564d84ea3e56b5374debc04b405eeb
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873635"
---
# <a name="use-the-windows-compatibility-pack-to-port-code-to-net-5"></a><span data-ttu-id="62608-103">使用 Windows 兼容性包将代码移植到 .NET 5+</span><span class="sxs-lookup"><span data-stu-id="62608-103">Use the Windows Compatibility Pack to port code to .NET 5+</span></span>

<span data-ttu-id="62608-104">将现有代码从 .NET Framework 移植到 .NET 时发现的一些最常见问题是对只有在 .NET Framework 中才能找到的 API 和技术的依赖。</span><span class="sxs-lookup"><span data-stu-id="62608-104">Some of the most common issues found when porting existing code from .NET Framework to .NET are dependencies on APIs and technologies that are only found in .NET Framework.</span></span> <span data-ttu-id="62608-105">Windows 兼容性包提供了许多这样的技术，因此可以更轻松地生成 .NET 应用程序和 .NET Standard 库。</span><span class="sxs-lookup"><span data-stu-id="62608-105">The *Windows Compatibility Pack* provides many of these technologies, so it's much easier to build .NET applications and .NET Standard libraries.</span></span>

<span data-ttu-id="62608-106">兼容包是 [.NET Standard 2.0 的逻辑扩展](../whats-new/dotnet-core-2-0.md#api-changes-and-library-support)，它大幅扩展了 API 集。</span><span class="sxs-lookup"><span data-stu-id="62608-106">The compatibility pack is a logical [extension of .NET Standard 2.0](../whats-new/dotnet-core-2-0.md#api-changes-and-library-support) that significantly increases the API set.</span></span> <span data-ttu-id="62608-107">现有代码几乎不修改即可编译。</span><span class="sxs-lookup"><span data-stu-id="62608-107">Existing code compiles with almost no modifications.</span></span> <span data-ttu-id="62608-108">为了信守 .NET Standard 的承诺（“所有 .NET 实现都提供的一组 API”），.NET Standard 不包括无法跨所有平台工作的技术，如注册表、Windows Management Instrumentation (WMI) 或反射发出 API。</span><span class="sxs-lookup"><span data-stu-id="62608-108">To keep its promise of "the set of APIs that all .NET implementations provide", .NET Standard doesn't include technologies that can't work across all platforms, such as registry, Windows Management Instrumentation (WMI), or reflection emit APIs.</span></span> <span data-ttu-id="62608-109">Windows 兼容性包位于 .NET Standard 顶部，提供对这些仅限 Windows 的技术的访问权限。</span><span class="sxs-lookup"><span data-stu-id="62608-109">The Windows Compatibility Pack sits on top of .NET Standard and provides access to these Windows-only technologies.</span></span> <span data-ttu-id="62608-110">对于那些想要迁移到 .NET 但至少第一步仍计划停留在 Windows 上的客户，这尤其有用。</span><span class="sxs-lookup"><span data-stu-id="62608-110">It's especially useful for customers that want to move to .NET but plan to stay on Windows, at least as a first step.</span></span> <span data-ttu-id="62608-111">在这种情况下，可以使用仅限 Windows 的技术消除迁移障碍。</span><span class="sxs-lookup"><span data-stu-id="62608-111">In that scenario, you can use Windows-only technologies removes the migration hurdle.</span></span>

## <a name="package-contents"></a><span data-ttu-id="62608-112">包的内容</span><span class="sxs-lookup"><span data-stu-id="62608-112">Package contents</span></span>

<span data-ttu-id="62608-113">Windows 兼容性包通过 [Microsoft.Windows.Compatibility NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)提供，并可从面向 .NET 或 .NET Standard 的项目引用。</span><span class="sxs-lookup"><span data-stu-id="62608-113">The Windows Compatibility Pack is provided via the [Microsoft.Windows.Compatibility NuGet package](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) and can be referenced from projects that target .NET or .NET Standard.</span></span>

<span data-ttu-id="62608-114">它提供了约 20,000 个 API，包括仅限 Windows 的 API 以及以下技术领域中的跨平台 API：</span><span class="sxs-lookup"><span data-stu-id="62608-114">It provides about 20,000 APIs, including Windows-only and cross-platform APIs from the following technology areas:</span></span>

- <span data-ttu-id="62608-115">代码页</span><span class="sxs-lookup"><span data-stu-id="62608-115">Code Pages</span></span>
- <span data-ttu-id="62608-116">CodeDom</span><span class="sxs-lookup"><span data-stu-id="62608-116">CodeDom</span></span>
- <span data-ttu-id="62608-117">配置</span><span class="sxs-lookup"><span data-stu-id="62608-117">Configuration</span></span>
- <span data-ttu-id="62608-118">目录服务</span><span class="sxs-lookup"><span data-stu-id="62608-118">Directory Services</span></span>
- <span data-ttu-id="62608-119">绘图</span><span class="sxs-lookup"><span data-stu-id="62608-119">Drawing</span></span>
- <span data-ttu-id="62608-120">ODBC</span><span class="sxs-lookup"><span data-stu-id="62608-120">ODBC</span></span>
- <span data-ttu-id="62608-121">权限</span><span class="sxs-lookup"><span data-stu-id="62608-121">Permissions</span></span>
- <span data-ttu-id="62608-122">端口</span><span class="sxs-lookup"><span data-stu-id="62608-122">Ports</span></span>
- <span data-ttu-id="62608-123">Windows 访问控制列表 (ACL)</span><span class="sxs-lookup"><span data-stu-id="62608-123">Windows Access Control Lists (ACL)</span></span>
- <span data-ttu-id="62608-124">Windows Communication Foundation (WCF)</span><span class="sxs-lookup"><span data-stu-id="62608-124">Windows Communication Foundation (WCF)</span></span>
- <span data-ttu-id="62608-125">Windows 加密</span><span class="sxs-lookup"><span data-stu-id="62608-125">Windows Cryptography</span></span>
- <span data-ttu-id="62608-126">Windows 事件日志</span><span class="sxs-lookup"><span data-stu-id="62608-126">Windows EventLog</span></span>
- <span data-ttu-id="62608-127">Windows 管理规范 (WMI)</span><span class="sxs-lookup"><span data-stu-id="62608-127">Windows Management Instrumentation (WMI)</span></span>
- <span data-ttu-id="62608-128">Windows 性能计数器</span><span class="sxs-lookup"><span data-stu-id="62608-128">Windows Performance Counters</span></span>
- <span data-ttu-id="62608-129">Windows 注册表</span><span class="sxs-lookup"><span data-stu-id="62608-129">Windows Registry</span></span>
- <span data-ttu-id="62608-130">Windows 运行时缓存</span><span class="sxs-lookup"><span data-stu-id="62608-130">Windows Runtime Caching</span></span>
- <span data-ttu-id="62608-131">Windows 服务</span><span class="sxs-lookup"><span data-stu-id="62608-131">Windows Services</span></span>

<span data-ttu-id="62608-132">有关详细信息，请参阅[兼容包规范](https://github.com/dotnet/designs/blob/main/accepted/2018/compat-pack/compat-pack.md)。</span><span class="sxs-lookup"><span data-stu-id="62608-132">For more information, see the [specification of the compatibility pack](https://github.com/dotnet/designs/blob/main/accepted/2018/compat-pack/compat-pack.md).</span></span>

## <a name="get-started"></a><span data-ttu-id="62608-133">入门</span><span class="sxs-lookup"><span data-stu-id="62608-133">Get started</span></span>

1. <span data-ttu-id="62608-134">移植之前，请确保查看[移植过程](index.md)。</span><span class="sxs-lookup"><span data-stu-id="62608-134">Before porting, make sure to take a look at the [Porting process](index.md).</span></span>

2. <span data-ttu-id="62608-135">将现有代码移植到 .NET 或 .NET Standard 时，请安装 [Microsoft.Windows.Compatibility NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)。</span><span class="sxs-lookup"><span data-stu-id="62608-135">When porting existing code to .NET or .NET Standard, install the [Microsoft.Windows.Compatibility NuGet package](https://www.nuget.org/packages/Microsoft.Windows.Compatibility).</span></span>

   <span data-ttu-id="62608-136">如果要停留在 Windows 上，则已准备完毕。</span><span class="sxs-lookup"><span data-stu-id="62608-136">If you want to stay on Windows, you're all set.</span></span>

3. <span data-ttu-id="62608-137">如果要在 Linux 或 macOS 上运行 .NET 应用程序或 .NET Standard 库，请使用 [API 分析器](../../standard/analyzers/api-analyzer.md)查找不会跨平台工作的 API 的使用情况。</span><span class="sxs-lookup"><span data-stu-id="62608-137">If you want to run the .NET application or .NET Standard library on Linux or macOS, use the [API Analyzer](../../standard/analyzers/api-analyzer.md) to find usage of APIs that won't work cross-platform.</span></span>

4. <span data-ttu-id="62608-138">删除这些 API 的使用情况、将其替换为跨平台替代项，或使用平台检查对其实施保护，例如：</span><span class="sxs-lookup"><span data-stu-id="62608-138">Either remove the usages of those APIs, replace them with cross-platform alternatives, or guard them using a platform check, like:</span></span>

    ```csharp
    private static string GetLoggingPath()
    {
        // Verify the code is running on Windows.
        if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
        {
            using (var key = Registry.CurrentUser.OpenSubKey(@"Software\Fabrikam\AssetManagement"))
            {
                if (key?.GetValue("LoggingDirectoryPath") is string configuredPath)
                    return configuredPath;
            }
        }

        // This is either not running on Windows or no logging path was configured,
        // so just use the path for non-roaming user-specific data files.
        var appDataPath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
        return Path.Combine(appDataPath, "Fabrikam", "AssetManagement", "Logging");
    }
    ```

<span data-ttu-id="62608-139">有关演示，请查看 [Windows 兼容性包的第 9 频道视频](https://channel9.msdn.com/Events/Connect/2017/T123)。</span><span class="sxs-lookup"><span data-stu-id="62608-139">For a demo, check out the [Channel 9 video of the Windows Compatibility Pack](https://channel9.msdn.com/Events/Connect/2017/T123).</span></span>

## <a name="see-also"></a><span data-ttu-id="62608-140">另请参阅</span><span class="sxs-lookup"><span data-stu-id="62608-140">See also</span></span>

- [<span data-ttu-id="62608-141">有关从 .NET Framework 移植到 .NET 的概述</span><span class="sxs-lookup"><span data-stu-id="62608-141">Overview of porting from .NET Framework to .NET</span></span>](index.md)
- [<span data-ttu-id="62608-142">ASP.NET 到 ASP.NET Core 迁移</span><span class="sxs-lookup"><span data-stu-id="62608-142">ASP.NET to ASP.NET Core migration</span></span>](/aspnet/core/migration/proper-to-2x)
- [<span data-ttu-id="62608-143">将 .NET Framework WPF 应用迁移到 .NET</span><span class="sxs-lookup"><span data-stu-id="62608-143">Migrate .NET Framework WPF apps to .NET</span></span>](/dotnet/desktop/wpf/migration/convert-project-from-net-framework?view=netdesktop-5.0&preserve-view=true)
- [<span data-ttu-id="62608-144">将 .NET Framework Windows 窗体应用迁移到 .NET</span><span class="sxs-lookup"><span data-stu-id="62608-144">Migrate .NET Framework Windows Forms apps to .NET</span></span>](/dotnet/desktop/winforms/migration/?view=netdesktop-5.0&preserve-view=true)
- [<span data-ttu-id="62608-145">将 .NET Framework 库移植到 .NET 中</span><span class="sxs-lookup"><span data-stu-id="62608-145">Port .NET Framework libraries to .NET</span></span>](libraries.md)
