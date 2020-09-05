---
ms.openlocfilehash: f1129500c9b779256b2650fe6fa855152cb3ae80
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88811249"
---
### <a name="middleware-database-error-page-marked-as-obsolete"></a><span data-ttu-id="c35eb-101">中间件：数据库错误页标记为已过时</span><span class="sxs-lookup"><span data-stu-id="c35eb-101">Middleware: Database error page marked as obsolete</span></span>

<span data-ttu-id="c35eb-102">在 ASP.NET Core 5.0 中，将 [DatabaseErrorPageMiddleware](/dotnet/api/microsoft.aspnetcore.diagnostics.entityframeworkcore.databaseerrorpagemiddleware?view=aspnetcore-3.0) 及其关联的扩展方法标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="c35eb-102">The [DatabaseErrorPageMiddleware](/dotnet/api/microsoft.aspnetcore.diagnostics.entityframeworkcore.databaseerrorpagemiddleware?view=aspnetcore-3.0) and its associated extension methods were marked as obsolete in ASP.NET Core 5.0.</span></span> <span data-ttu-id="c35eb-103">将删除 ASP.NET Core 6.0 中的中间件和扩展方法。</span><span class="sxs-lookup"><span data-stu-id="c35eb-103">The middleware and extension methods will be removed in ASP.NET Core 6.0.</span></span> <span data-ttu-id="c35eb-104">该功能将改为由 `DatabaseDeveloperPageExceptionFilter` 及其扩展方法提供。</span><span class="sxs-lookup"><span data-stu-id="c35eb-104">The functionality will instead be provided by `DatabaseDeveloperPageExceptionFilter` and its extension methods.</span></span>

<span data-ttu-id="c35eb-105">有关讨论，请参阅 [dotnet/aspnetcore#24987](https://github.com/dotnet/aspnetcore/issues/24987) 上的 GitHub 问题。</span><span class="sxs-lookup"><span data-stu-id="c35eb-105">For discussion, see the GitHub issue at [dotnet/aspnetcore#24987](https://github.com/dotnet/aspnetcore/issues/24987).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="c35eb-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c35eb-106">Version introduced</span></span>

<span data-ttu-id="c35eb-107">5.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="c35eb-107">5.0 RC 1</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="c35eb-108">旧行为</span><span class="sxs-lookup"><span data-stu-id="c35eb-108">Old behavior</span></span>

<span data-ttu-id="c35eb-109">`DatabaseErrorPageMiddleware` 及其关联的扩展方法未过时。</span><span class="sxs-lookup"><span data-stu-id="c35eb-109">`DatabaseErrorPageMiddleware` and its associated extension methods weren't obsolete.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="c35eb-110">新行为</span><span class="sxs-lookup"><span data-stu-id="c35eb-110">New behavior</span></span>

<span data-ttu-id="c35eb-111">`DatabaseErrorPageMiddleware` 及其关联的扩展方法已过时。</span><span class="sxs-lookup"><span data-stu-id="c35eb-111">`DatabaseErrorPageMiddleware` and its associated extension methods are obsolete.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="c35eb-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="c35eb-112">Reason for change</span></span>

<span data-ttu-id="c35eb-113">`DatabaseErrorPageMiddleware` 已迁移到[开发人员异常页面](/aspnet/core/fundamentals/error-handling#developer-exception-page)的可扩展 API。</span><span class="sxs-lookup"><span data-stu-id="c35eb-113">`DatabaseErrorPageMiddleware` was migrated to an extensible API for the [developer exception page](/aspnet/core/fundamentals/error-handling#developer-exception-page).</span></span> <span data-ttu-id="c35eb-114">有关可扩展 API 的详细信息，请参阅 GitHub 问题 [dotnet/aspnetcore # 8536](https://github.com/dotnet/aspnetcore/issues/8536)。</span><span class="sxs-lookup"><span data-stu-id="c35eb-114">For more information on the extensible API, see GitHub issue [dotnet/aspnetcore#8536](https://github.com/dotnet/aspnetcore/issues/8536).</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="c35eb-115">建议操作</span><span class="sxs-lookup"><span data-stu-id="c35eb-115">Recommended action</span></span>

<span data-ttu-id="c35eb-116">完成以下步骤：</span><span class="sxs-lookup"><span data-stu-id="c35eb-116">Complete the following steps:</span></span>

1. <span data-ttu-id="c35eb-117">停止在项目中使用 `DatabaseErrorPageMiddleware`。</span><span class="sxs-lookup"><span data-stu-id="c35eb-117">Stop using `DatabaseErrorPageMiddleware` in your project.</span></span> <span data-ttu-id="c35eb-118">例如，从 `Startup.Configure` 中删除 `UseDatabaseErrorPage` 方法调用：</span><span class="sxs-lookup"><span data-stu-id="c35eb-118">For example, remove the `UseDatabaseErrorPage` method call from `Startup.Configure`:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDatabaseErrorPage();
        }
    }
    ```

1. <span data-ttu-id="c35eb-119">将开发人员异常页面添加到你的项目。</span><span class="sxs-lookup"><span data-stu-id="c35eb-119">Add the developer exception page to your project.</span></span> <span data-ttu-id="c35eb-120">例如，在 `Startup.Configure` 中调用 <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage%2A> 方法：</span><span class="sxs-lookup"><span data-stu-id="c35eb-120">For example, call the <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage%2A> method in `Startup.Configure`:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
    }
    ```

1. <span data-ttu-id="c35eb-121">将数据库开发人员异常页面筛选器添加到服务集合。</span><span class="sxs-lookup"><span data-stu-id="c35eb-121">Add the database developer page exception filter to the services collection.</span></span> <span data-ttu-id="c35eb-122">例如，在 `Startup.ConfigureServices` 中调用 `AddDatabaseDeveloperPageExceptionFilter` 方法：</span><span class="sxs-lookup"><span data-stu-id="c35eb-122">For example, call the `AddDatabaseDeveloperPageExceptionFilter` method in `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDatabaseDeveloperPageExceptionFilter();
    }
    ```

#### <a name="category"></a><span data-ttu-id="c35eb-123">类别</span><span class="sxs-lookup"><span data-stu-id="c35eb-123">Category</span></span>

<span data-ttu-id="c35eb-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c35eb-124">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="c35eb-125">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="c35eb-125">Affected APIs</span></span>

- [<span data-ttu-id="c35eb-126">Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage</span><span class="sxs-lookup"><span data-stu-id="c35eb-126">Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage</span></span>](/dotnet/api/microsoft.aspnetcore.builder.databaseerrorpageextensions.usedatabaseerrorpage?view=aspnetcore-3.0)
- [<span data-ttu-id="c35eb-127">Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore.DatabaseErrorPageMiddleware</span><span class="sxs-lookup"><span data-stu-id="c35eb-127">Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore.DatabaseErrorPageMiddleware</span></span>](/dotnet/api/microsoft.aspnetcore.diagnostics.entityframeworkcore.databaseerrorpagemiddleware?view=aspnetcore-3.0)

<!-- 

#### Affected APIs

- `Overload:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`
- `T:Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore.DatabaseErrorPageMiddleware`

-->