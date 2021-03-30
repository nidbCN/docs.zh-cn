---
title: 开发 ASP.NET Core MVC 应用
description: 使用 ASP.NET Core 和 Azure 构建新式 Web 应用程序 | 开发 ASP.NET Core MVC 应用
author: ardalis
ms.author: wiwagn
ms.date: 12/01/2020
no-loc:
- Blazor
- WebAssembly
ms.openlocfilehash: 494e73bd32ac6793d355b6828408b61bb09ca880
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873115"
---
# <a name="develop-aspnet-core-mvc-apps"></a><span data-ttu-id="7b9e5-103">开发 ASP.NET Core MVC 应用</span><span class="sxs-lookup"><span data-stu-id="7b9e5-103">Develop ASP.NET Core MVC apps</span></span>

> <span data-ttu-id="7b9e5-104">“第一次是否正确完成并不重要。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-104">"It's not important to get it right the first time.</span></span> <span data-ttu-id="7b9e5-105">最后一次正确完成才至关重要。”</span><span class="sxs-lookup"><span data-stu-id="7b9e5-105">It's vitally important to get it right the last time."</span></span>
> <span data-ttu-id="7b9e5-106">— Andrew Hunt 和 David Thomas</span><span class="sxs-lookup"><span data-stu-id="7b9e5-106">_- Andrew Hunt and David Thomas_</span></span>

<span data-ttu-id="7b9e5-107">ASP.NET Core 是一个跨平台的开源框架，用于构建新式云优化 Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-107">ASP.NET Core is a cross-platform, open-source framework for building modern cloud-optimized web applications.</span></span> <span data-ttu-id="7b9e5-108">ASP.NET Core 具有轻量级和模块化的特点，并且内置了对依赖关系注入的支持，因此具有更好的可测试性和可维护性。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-108">ASP.NET Core apps are lightweight and modular, with built-in support for dependency injection, enabling greater testability and maintainability.</span></span> <span data-ttu-id="7b9e5-109">而 MVC 支持构建新式 Web API 和基于视图的应用，ASP.NET Core 与之结合后将成为一个功能强大的框架，用于构建企业 Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-109">Combined with MVC, which supports building modern web APIs in addition to view-based apps, ASP.NET Core is a powerful framework with which to build enterprise web applications.</span></span>

## <a name="mvc-and-razor-pages"></a><span data-ttu-id="7b9e5-110">MVC 和 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7b9e5-110">MVC and Razor Pages</span></span>

<span data-ttu-id="7b9e5-111">ASP.NET Core MVC 提供了许多对构建基于 Web 的 API 和应用有用的功能。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-111">ASP.NET Core MVC offers many features that are useful for building web-based APIs and apps.</span></span> <span data-ttu-id="7b9e5-112">术语 MVC 代表“模型 - 视图 - 控制器”，这是一种 UI 模式，它将响应用户请求的职责分为几个部分。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-112">The term MVC stands for "Model-View-Controller", a UI pattern that breaks up the responsibilities of responding to user requests into several parts.</span></span> <span data-ttu-id="7b9e5-113">除了遵循此模式之外，还可以将 ASP.NET Core 应用中的功能实现为 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-113">In addition to following this pattern, you can also implement features in your ASP.NET Core apps as Razor Pages.</span></span> <span data-ttu-id="7b9e5-114">Razor Pages 内置于 ASP.NET Core MVC 中，并使用相同的功能进行路由、模型绑定、筛选和授权等。但是，Razor Pages 不会为控制器、模型和视图等提供单独的文件夹和文件，也不会使用基于属性的路由，而是将它们放置在一个文件夹（“/Pages”）中，根据它们在此文件夹中的相对位置进行路由，并使用处理程序而非控制器操作处理请求。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-114">Razor Pages are built into ASP.NET Core MVC, and use the same features for routing, model binding, filters, authorization, etc. However, instead of having separate folders and files for Controllers, Models, Views, etc. and using attribute-based routing, Razor Pages are placed in a single folder ("/Pages"), route based on their relative location in this folder, and handle requests with handlers instead of controller actions.</span></span> <span data-ttu-id="7b9e5-115">因此，在使用 Razor Pages 时，通常需要将所有需要的文件和类置于同一位置，而不是让其分散在整个 Web 项目中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-115">As a result, when working with Razor Pages, all of the files and classes you need are typically colocated, not spread throughout the web project.</span></span>

<span data-ttu-id="7b9e5-116">在创建新的 ASP.NET Core 应用时，应考虑好要构建的应用类型。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-116">When you create a new ASP.NET Core App, you should have a plan in mind for the kind of app you want to build.</span></span> <span data-ttu-id="7b9e5-117">在 Visual Studio 中，你可以从多个模板中进行选择。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-117">In Visual Studio, you'll choose from several templates.</span></span> <span data-ttu-id="7b9e5-118">三个最常见的项目模板是 Web API、Web 应用程序和 Web 应用程序（模型 - 视图 - 控制器）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-118">The three most common project templates are Web API, Web Application, and Web Application (Model-View-Controller).</span></span> <span data-ttu-id="7b9e5-119">虽然只能在首次创建项目时做出此决定，但此决定可以撤销。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-119">Although you can only make this decision when you first create a project, it's not an irrevocable decision.</span></span> <span data-ttu-id="7b9e5-120">Web API 项目使用标准的“模型 - 视图 - 控制器”控制器（默认情况下，它只缺少视图）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-120">The Web API project uses standard Model-View-Controller controllers – it just lacks Views by default.</span></span> <span data-ttu-id="7b9e5-121">同样，默认的 Web 应用程序模板使用 Razor Pages，因此也缺少 Views 文件夹。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-121">Likewise, the default Web Application template uses Razor Pages, and so also lacks a Views folder.</span></span> <span data-ttu-id="7b9e5-122">可以稍后向这些项目添加 Views 文件夹以支持基于视图的行为。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-122">You can add a Views folder to these projects later to support view-based behavior.</span></span> <span data-ttu-id="7b9e5-123">默认情况下，Web API 和模型 - 视图 - 控制器项目不包含 Pages 文件夹，但可以稍后添加一个以支持基于 Razor Pages 的行为。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-123">Web API and Model-View-Controller projects don't include a Pages folder by default, but you can add one later to support Razor Pages-based behavior.</span></span> <span data-ttu-id="7b9e5-124">可以将这三个模板视为支持三种不同类型的默认用户交互：数据 (Web API)、基于页面和基于视图。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-124">You can think of these three templates as supporting three different kinds of default user interaction: data (web API), page-based, and view-based.</span></span> <span data-ttu-id="7b9e5-125">但是，如果愿意，可以在单个项目中混合和匹配任何或所有这些模板。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-125">However, you can mix and match any or all of these templates within a single project if you wish.</span></span>

### <a name="why-razor-pages"></a><span data-ttu-id="7b9e5-126">为何选择 Razor Pages？</span><span class="sxs-lookup"><span data-stu-id="7b9e5-126">Why Razor Pages?</span></span>

<span data-ttu-id="7b9e5-127">Razor Pages 是 Visual Studio 中新 Web 应用程序的默认方法。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-127">Razor Pages is the default approach for new web applications in Visual Studio.</span></span> <span data-ttu-id="7b9e5-128">Razor Pages 提供了一种较为简单的方法来构建基于页面的应用程序功能，例如非 SPA 表单。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-128">Razor Pages offers a simpler way of building page-based application features, such as non-SPA forms.</span></span> <span data-ttu-id="7b9e5-129">通过使用控制器和视图，应用程序通常拥有非常大的控制器，这些控制器处理许多不同的依赖项和视图模型，并返回许多不同的视图。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-129">Using controllers and views, it was common for applications to have very large controllers that worked with many different dependencies and view models and returned many different views.</span></span> <span data-ttu-id="7b9e5-130">这大大增加了复杂性，并且经常导致控制器不能有效地遵循单一责任原则或打开/关闭原则。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-130">This resulted in more complexity and often resulted in controllers that didn't follow the Single Responsibility Principle or Open/Closed Principles effectively.</span></span> <span data-ttu-id="7b9e5-131">Razor Pages 通过使用其 Razor 标记在 Web 应用程序中封装给定逻辑“页面”的服务器端逻辑来解决此问题。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-131">Razor Pages addresses this issue by encapsulating the server-side logic for a given logical "page" in a web application with its Razor markup.</span></span> <span data-ttu-id="7b9e5-132">没有服务器端逻辑的 Razor Page 只能包含一个 Razor 文件（例如“Index.cshtml”）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-132">A Razor Page that has no server-side logic can only consist of a Razor file (for instance, "Index.cshtml").</span></span> <span data-ttu-id="7b9e5-133">但是，大多数重要的 Razor Pages 都有关联的页面模型类，按照惯例，它的名称与带有“.cs”扩展名的 Razor 文件相同（例如，“Index.cshtml.cs”）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-133">However, most non-trivial Razor Pages will have an associated page model class, which by convention is named the same as the Razor file with a ".cs" extension (for example, "Index.cshtml.cs").</span></span>

<span data-ttu-id="7b9e5-134">Razor Page 的页面模型结合了 MVC 控制器和视图模型的职责。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-134">A Razor Page's page model combines the responsibilities of an MVC controller and a viewmodel.</span></span> <span data-ttu-id="7b9e5-135">不通过控制器操作方法执行请求，而是执行“OnGet()”等页面模型处理程序，默认情况下，呈现其关联页面。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-135">Instead of handling requests with controller action methods, page model handlers like "OnGet()" are executed, rendering their associated page by default.</span></span> <span data-ttu-id="7b9e5-136">Razor Pages 简化了在 ASP.NET Core 应用中构建单个页面的过程，同时仍然提供了 ASP.NET Core MVC 的所有体系结构功能。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-136">Razor Pages simplifies the process of building individual pages in an ASP.NET Core app, while still providing all the architectural features of ASP.NET Core MVC.</span></span> <span data-ttu-id="7b9e5-137">对于基于页面的新功能，它们是很好的默认选择。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-137">They're a good default choice for new page-based functionality.</span></span>

### <a name="when-to-use-mvc"></a><span data-ttu-id="7b9e5-138">何时使用 MVC</span><span class="sxs-lookup"><span data-stu-id="7b9e5-138">When to use MVC</span></span>

<span data-ttu-id="7b9e5-139">如果正在构建 Web API，则 MVC 模式比尝试使用 Razor Pages 更有意义。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-139">If you're building web APIs, the MVC pattern makes more sense than trying to use Razor Pages.</span></span> <span data-ttu-id="7b9e5-140">如果项目将只公开 Web API 终结点，则最好从 Web API 项目模板开始。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-140">If your project will only expose web API endpoints, you should ideally start from the Web API project template.</span></span> <span data-ttu-id="7b9e5-141">否则，很容易将控制器和关联的 API 终结点添加到任何 ASP.NET Core 应用中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-141">Otherwise, it's easy to add controllers and associated API endpoints to any ASP.NET Core app.</span></span> <span data-ttu-id="7b9e5-142">如果要将现有应用程序从 ASP.NET MVC 5 或更早版本迁移到 ASP.NET Core MVC，并且需要以最少的工作量完成此操作，可使用基于视图的 MVC 方法。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-142">Use the view-based MVC approach if you're migrating an existing application from ASP.NET MVC 5 or earlier to ASP.NET Core MVC and you want to do so with the least amount of effort.</span></span> <span data-ttu-id="7b9e5-143">完成初始迁移后，可以评估针对新功能甚至批量迁移采用 Razor Pages 是否有意义。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-143">Once you've made the initial migration, you can evaluate whether it makes sense to adopt Razor Pages for new features or even as a wholesale migration.</span></span>

<span data-ttu-id="7b9e5-144">无论是选择使用 Razor Pages 还是 MVC 视图生成 Web 应用，应用都将具有类似的性能，并且都支持依赖项注入、筛选器、模型绑定、验证等。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-144">Whether you choose to build your web app using Razor Pages or MVC views, your app will have similar performance and will include support for dependency injection, filters, model binding, validation, and so on.</span></span>

## <a name="mapping-requests-to-responses"></a><span data-ttu-id="7b9e5-145">将请求映射到响应</span><span class="sxs-lookup"><span data-stu-id="7b9e5-145">Mapping requests to responses</span></span>

<span data-ttu-id="7b9e5-146">ASP.NET Core 应用的核心在于将传入请求映射到传出响应。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-146">At its heart, ASP.NET Core apps map incoming requests to outgoing responses.</span></span> <span data-ttu-id="7b9e5-147">详细说来，这种映射是通过中间件完成的，简单的 ASP.NET Core 应用和微服务可能只包含自定义中间件。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-147">At a low level, this mapping is done with middleware, and simple ASP.NET Core apps and microservices may be comprised solely of custom middleware.</span></span> <span data-ttu-id="7b9e5-148">在某种程度上，使用 ASP.NET Core MVC 可以实现更高级别的操作，需要考虑路由、控制器和操作  。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-148">When using ASP.NET Core MVC, you can work at a somewhat higher level, thinking in terms of _routes_, _controllers_, and _actions_.</span></span> <span data-ttu-id="7b9e5-149">每个传入请求都会和应用程序的路由表进行对比，如果找到匹配的路由，则调用关联的操作方法（属于控制器）来处理该请求。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-149">Each incoming request is compared with the application's routing table, and if a matching route is found, the associated action method (belonging to a controller) is called to handle the request.</span></span> <span data-ttu-id="7b9e5-150">如果未找到匹配的路由，则调用错误处理程序（此时返回 NotFound 结果）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-150">If no matching route is found, an error handler (in this case, returning a NotFound result) is called.</span></span>

<span data-ttu-id="7b9e5-151">ASP.NET Core MVC 应用可以使用传统路由或属性路由，或二者同时使用。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-151">ASP.NET Core MVC apps can use conventional routes, attribute routes, or both.</span></span> <span data-ttu-id="7b9e5-152">传统路由在代码中定义，使用类似以下示例中的语法指定路由约定：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-152">Conventional routes are defined in code, specifying routing _conventions_ using syntax like in the example below:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="7b9e5-153">此示例向路由表添加了一个名为“default”的路由。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-153">In this example, a route named "default" has been added to the routing table.</span></span> <span data-ttu-id="7b9e5-154">它定义了一个具有 `controller`、`action` 和 `id` 占位符的路由模板。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-154">It defines a route template with placeholders for `controller`, `action`, and `id`.</span></span> <span data-ttu-id="7b9e5-155">`controller` 和 `action` 占位符具有指定的默认值（分别为 `Home` 和 `Index`），`id` 占位符则是可选的（应为向其应用了“?”）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-155">The `controller` and `action` placeholders have the default specified (`Home` and `Index`, respectively), and the `id` placeholder is optional (by virtue of a "?" applied to it).</span></span> <span data-ttu-id="7b9e5-156">此处定义的约定规定，请求的第一部分应与控制器的名称对应，第二部分与操作对应，第三部分（如有）表示 ID 参数。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-156">The convention defined here states that the first part of a request should correspond to the name of the controller, the second part to the action, and then if necessary a third part will represent an ID parameter.</span></span> <span data-ttu-id="7b9e5-157">通常在同一个位置定义应用程序的传统路由，例如在 `Configure` 类的 `Startup` 方法中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-157">Conventional routes are typically defined in one place for the application, such as in the `Configure` method in the `Startup` class.</span></span>

<span data-ttu-id="7b9e5-158">属性路由直接应用到控制器和操作，而不是在全局范围内指定。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-158">Attribute routes are applied to controllers and actions directly, rather than specified globally.</span></span> <span data-ttu-id="7b9e5-159">此方法的优势在于，查看特定方法时，属性路由更容易发现，但也意味着路由信息不会保存在应用程序中的同一个地方。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-159">This approach has the advantage of making them much more discoverable when you're looking at a particular method, but does mean that routing information is not kept in one place in the application.</span></span> <span data-ttu-id="7b9e5-160">通过属性路由可以为给定操作轻松指定多个路由，并将控制器和操作之间的路由合并在一起。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-160">With attribute routes, you can easily specify multiple routes for a given action, as well as combine routes between controllers and actions.</span></span> <span data-ttu-id="7b9e5-161">例如：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-161">For example:</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")] // Combines to define the route template "Home"
    [Route("Index")] // Combines to define route template "Home/Index"
    [Route("/")] // Does not combine, defines the route template ""
    public IActionResult Index() {}
}
```

<span data-ttu-id="7b9e5-162">可以在 [HttpGet] 和类似属性上指定路由，而无需添加单独的 [Route] 属性。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-162">Routes can be specified on [HttpGet] and similar attributes, avoiding the need to add separate [Route] attributes.</span></span> <span data-ttu-id="7b9e5-163">属性路由还可以通过使用令牌来减少重复控制器或操作名称的次数，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-163">Attribute routes can also use tokens to reduce the need to repeat controller or action names, as shown below:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
    [Route("")] // Matches 'Products'
    [Route("Index")] // Matches 'Products/Index'
    public IActionResult Index() {}
}
```

<span data-ttu-id="7b9e5-164">Razor Pages 不使用属性路由。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-164">Razor Pages doesn't use attribute routing.</span></span> <span data-ttu-id="7b9e5-165">可以作为 Razor Pages 的 `@page` 指令的一部分为其指定其他路由模板信息：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-165">You can specify additional route template information for a Razor Page as part of its `@page` directive:</span></span>

```csharp
@page "{id:int}"
```

<span data-ttu-id="7b9e5-166">在前面的示例中，问题中的页面将匹配具有整数 `id` 参数的路由。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-166">In the previous example, the page in question would match a route with an integer `id` parameter.</span></span> <span data-ttu-id="7b9e5-167">例如，位于 `/Pages` 根目录中的“Products.cshtml”页面将具有以下路由：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-167">For example, the *Products.cshtml* page located in the root of `/Pages` would have this route:</span></span>

```csharp
"/Products/123"
```

<span data-ttu-id="7b9e5-168">给定请求与路由匹配之后，ASP.NET Core MVC 会对该请求执行[模型绑定](/aspnet/core/mvc/models/model-binding)和[模型验证](/aspnet/core/mvc/models/validation)，然后才调用操作方法。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-168">Once a given request has been matched to a route, but before the action method is called, ASP.NET Core MVC will perform [model binding](/aspnet/core/mvc/models/model-binding) and [model validation](/aspnet/core/mvc/models/validation) on the request.</span></span> <span data-ttu-id="7b9e5-169">模型绑定负责将传入到指定 .NET 类型的 HTTP 数据转换为要调用的操作方法的参数。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-169">Model binding is responsible for converting incoming HTTP data into the .NET types specified as parameters of the action method to be called.</span></span> <span data-ttu-id="7b9e5-170">例如，如果操作方法需要一个 `int id` 参数，模型绑定将尝试根据请求中提供的值来提供此参数。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-170">For example, if the action method expects an `int id` parameter, model binding will attempt to provide this parameter from a value provided as part of the request.</span></span> <span data-ttu-id="7b9e5-171">为此，模型绑定会查找已发布表单中的值、路由中的值和查询字符串值。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-171">To do so, model binding looks for values in a posted form, values in the route itself, and query string values.</span></span> <span data-ttu-id="7b9e5-172">假设找到了 `id` 值，该值会被转换为整数，然后传入操作方法。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-172">Assuming an `id` value is found, it will be converted to an integer before being passed into the action method.</span></span>

<span data-ttu-id="7b9e5-173">模型验证发生在绑定模型之后，调用操作方法之前。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-173">After binding the model but before calling the action method, model validation occurs.</span></span> <span data-ttu-id="7b9e5-174">模型验证对模型类型使用可选属性，且有助于确保提供的模型对象符合特定数据要求。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-174">Model validation uses optional attributes on the model type, and can help ensure that the provided model object conforms to certain data requirements.</span></span> <span data-ttu-id="7b9e5-175">可以将某些值指定为必需项，将其限制为特定长度，或将其限制在一定数值范围内，等等。如果指定了验证特性，但该模型不符合其要求，则属性 ModelState.IsValid 将为 false，并且失败的验证规则集将可被发送到发出请求的客户端。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-175">Certain values may be specified as required, or limited to a certain length or numeric range, etc. If validation attributes are specified but the model does not conform to their requirements, the property ModelState.IsValid will be false, and the set of failing validation rules will be available to send to the client making the request.</span></span>

<span data-ttu-id="7b9e5-176">使用模型验证时，执行任何状态更改命令之前，务必确保模型有效，以防无效数据损坏应用。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-176">If you're using model validation, you should be sure to always check that the model is valid before performing any state-altering commands, to ensure your app is not corrupted by invalid data.</span></span> <span data-ttu-id="7b9e5-177">使用[筛选器](/aspnet/core/mvc/controllers/filters)可避免在每个操作中都需要为此验证添加代码。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-177">You can use a [filter](/aspnet/core/mvc/controllers/filters) to avoid the need to add code for this validation in every action.</span></span> <span data-ttu-id="7b9e5-178">ASP.NET Core MVC 筛选器提供了一种拦截成组请求的方法，因此可以有针对性地应用通用策略和横切关注点。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-178">ASP.NET Core MVC filters offer a way of intercepting groups of requests, so that common policies and cross-cutting concerns can be applied on a targeted basis.</span></span> <span data-ttu-id="7b9e5-179">筛选器可应用于单个操作、整个控制器或应用程序全局。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-179">Filters can be applied to individual actions, whole controllers, or globally for an application.</span></span>

<span data-ttu-id="7b9e5-180">对于 Web API，ASP.NET Core MVC 支持 [_内容协商_](/aspnet/core/mvc/models/formatting)，允许对指定如何设置响应格式进行请求。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-180">For web APIs, ASP.NET Core MVC supports [_content negotiation_](/aspnet/core/mvc/models/formatting), allowing requests to specify how responses should be formatted.</span></span> <span data-ttu-id="7b9e5-181">根据请求中提供的标头，操作返回的数据将采用 XML、JSON 或其他支持格式作为响应的格式。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-181">Based on headers provided in the request, actions returning data will format the response in XML, JSON, or another supported format.</span></span> <span data-ttu-id="7b9e5-182">借助此功能，同一个 API 可供数据格式要求不同的多个客户端使用。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-182">This feature enables the same API to be used by multiple clients with different data format requirements.</span></span>

<span data-ttu-id="7b9e5-183">Web API 项目应考虑使用 `[ApiController]` 属性，该属性可应用于单个控制器、基本控制器类或整个程序集。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-183">Web API projects should consider using the `[ApiController]` attribute, which can be applied to individual controllers, to a base controller class, or to the entire assembly.</span></span> <span data-ttu-id="7b9e5-184">此属性添加自动模型验证检查，任何具有无效模型的操作都将返回 BadRequest 以及验证错误的详细信息。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-184">This attribute adds automatic model validation checking and any action with an invalid model will return a BadRequest with the details of the validation errors.</span></span> <span data-ttu-id="7b9e5-185">该属性还要求所有操作都具有属性路由，而不是使用传统路由，并返回更详细的 ProblemDetails 信息以响应错误。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-185">The attribute also requires all actions have an attribute route, rather than using a conventional route, and returns more detailed ProblemDetails information in response to errors.</span></span>

### <a name="keeping-controllers-under-control"></a><span data-ttu-id="7b9e5-186">使控制器处于控制之下</span><span class="sxs-lookup"><span data-stu-id="7b9e5-186">Keeping controllers under control</span></span>

<span data-ttu-id="7b9e5-187">对于基于页面的应用程序，Razor Pages 可以很好地防止控制器变得太大。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-187">For page-based applications, Razor Pages do a great job of keeping controllers from getting too large.</span></span> <span data-ttu-id="7b9e5-188">每个单独的页面都有自己的文件和类，这些文件和类专用于其处理程序。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-188">Each individual page is given its own files and classes dedicated just to its handler(s).</span></span> <span data-ttu-id="7b9e5-189">在引入 Razor Pages 之前，许多以视图为中心的应用程序可能会有负责许多不同操作和视图的大型控制器类。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-189">Prior to the introduction of Razor Pages, many view-centric applications would have large controller classes responsible for many different actions and views.</span></span> <span data-ttu-id="7b9e5-190">这些类自然会扩展并具有许多职责和依赖项，从而使其难以维护。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-190">These classes would naturally grow to have many responsibilities and dependencies, making them harder to maintain.</span></span> <span data-ttu-id="7b9e5-191">如果发现基于视图的控制器变得过大，请考虑将其重构为使用 Razor Pages，或引入转存进程等模式。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-191">If you find your view-based controllers are growing too large, consider refactoring them to use Razor Pages, or introducing a pattern like a mediator.</span></span>

<span data-ttu-id="7b9e5-192">转存进程设计模式用于减少类之间的耦合度，同时允许它们之间进行通信。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-192">The mediator design pattern is used to reduce coupling between classes while allowing communication between them.</span></span> <span data-ttu-id="7b9e5-193">在 ASP.NET Core MVC 应用程序中，经常使用此模式将控制器分解为较小的部分，方法是使用处理程序来完成操作方法的工作。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-193">In ASP.NET Core MVC applications, this pattern is frequently employed to break up controllers into smaller pieces by using *handlers* to do the work of action methods.</span></span> <span data-ttu-id="7b9e5-194">常用的 [MediatR NuGet 包](https://www.nuget.org/packages/MediatR/)通常用于实现此目的。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-194">The popular [MediatR NuGet package](https://www.nuget.org/packages/MediatR/) is often used to accomplish this.</span></span> <span data-ttu-id="7b9e5-195">通常，控制器包括许多不同的操作方法，每种方法可能都需要某些依赖项。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-195">Typically, controllers include many different action methods, each of which may require certain dependencies.</span></span> <span data-ttu-id="7b9e5-196">必须将任何操作所需的所有依赖项集传递到控制器的构造函数中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-196">The set of all dependencies required by any action must be passed into the controller's constructor.</span></span> <span data-ttu-id="7b9e5-197">使用 MediatR 时，控制器的唯一依赖项是转存进程的实例。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-197">When using MediatR, the only dependency a controller has is on an instance of the mediator.</span></span> <span data-ttu-id="7b9e5-198">然后，每个操作都使用转存进程实例来发送消息，该消息由处理程序处理。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-198">Each action then uses the mediator instance to send a message, which is processed by a handler.</span></span> <span data-ttu-id="7b9e5-199">该处理程序特定于单个操作，因此只需要该操作所需的依赖项。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-199">The handler is specific to a single action and thus only needs the dependencies required by that action.</span></span> <span data-ttu-id="7b9e5-200">下面显示了使用 MediatR 的控制器的示例：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-200">An example of a controller using MediatR is shown here:</span></span>

```csharp
public class OrderController : Controller
{
    private readonly IMediator _mediator;

    public OrderController(IMediator mediator)
    {
        _mediator = mediator;
    }

    [HttpGet]
    public async Task<IActionResult> MyOrders()
    {
        var viewModel = await _mediator.Send(new GetMyOrders(User.Identity.Name));

        return View(viewModel);
    }

    // other actions implemented similarly
}
```

<span data-ttu-id="7b9e5-201">在 `MyOrders` 操作中，对 `Send` `GetMyOrders` 消息的调用由此类处理：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-201">In the `MyOrders` action, the call to `Send` a `GetMyOrders` message is handled by this class:</span></span>

```csharp
public class GetMyOrdersHandler : IRequestHandler<GetMyOrders, IEnumerable<OrderViewModel>>
{
    private readonly IOrderRepository _orderRepository;

    public GetMyOrdersHandler(IOrderRepository orderRepository)
    {
        _orderRepository = orderRepository;
    }

    public async Task<IEnumerable<OrderViewModel>> Handle(GetMyOrders request, CancellationToken cancellationToken)
    {
        var specification = new CustomerOrdersWithItemsSpecification(request.UserName);
        var orders = await _orderRepository.ListAsync(specification);

        return orders.Select(o => new OrderViewModel
        {
            OrderDate = o.OrderDate,
            OrderItems = o.OrderItems?.Select(oi => new OrderItemViewModel()
            {
                PictureUrl = oi.ItemOrdered.PictureUri,
                ProductId = oi.ItemOrdered.CatalogItemId,
                ProductName = oi.ItemOrdered.ProductName,
                UnitPrice = oi.UnitPrice,
                Units = oi.Units
            }).ToList(),
            OrderNumber = o.Id,
            ShippingAddress = o.ShipToAddress,
            Total = o.Total()
        });
    }
}
```

<span data-ttu-id="7b9e5-202">这种方法的最终结果是，控制器要小得多，主要侧重于路由和模型绑定，而各个处理程序则负责给定终结点所需的特定任务。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-202">The end result of this approach is for controllers to be much smaller and focused primarily on routing and model binding, while individual handlers are responsible for the specific tasks needed by a given endpoint.</span></span> <span data-ttu-id="7b9e5-203">也可以在不使用 MediatR 的情况下使用 [ApiEndpoints NuGet 包](https://www.nuget.org/packages/Ardalis.ApiEndpoints/)实现此方法，该包试图为 API 控制器带来 Razor Pages 带给基于视图的控制器的相同好处。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-203">This approach can also be achieved without MediatR by using the [ApiEndpoints NuGet package](https://www.nuget.org/packages/Ardalis.ApiEndpoints/), which attempts to bring to API controllers the same benefits Razor Pages brings to view-based controllers.</span></span>

> ### <a name="references--mapping-requests-to-responses"></a><span data-ttu-id="7b9e5-204">参考 - 将请求映射到响应</span><span class="sxs-lookup"><span data-stu-id="7b9e5-204">References – Mapping Requests to Responses</span></span>
>
> - <span data-ttu-id="7b9e5-205">**路由到控制器操作**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-205">**Routing to Controller Actions**</span></span>\
 > <https://docs.microsoft.com/aspnet/core/mvc/controllers/routing>
> - <span data-ttu-id="7b9e5-206">**模型绑定**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-206">**Model Binding**</span></span>\
 > <https://docs.microsoft.com/aspnet/core/mvc/models/model-binding>
> - <span data-ttu-id="7b9e5-207">**模型验证**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-207">**Model Validation**</span></span>\
 > <https://docs.microsoft.com/aspnet/core/mvc/models/validation>
> - <span data-ttu-id="7b9e5-208">**筛选器**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-208">**Filters**</span></span>\
 > <https://docs.microsoft.com/aspnet/core/mvc/controllers/filters>
> - <span data-ttu-id="7b9e5-209">**ApiController 属性**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-209">**ApiController Attribute**</span></span>\
 > <https://docs.microsoft.com/aspnet/core/web-api/>

## <a name="working-with-dependencies"></a><span data-ttu-id="7b9e5-210">处理依赖关系</span><span class="sxs-lookup"><span data-stu-id="7b9e5-210">Working with dependencies</span></span>

<span data-ttu-id="7b9e5-211">ASP.NET Core 内置了对[依赖关系注入](/aspnet/core/fundamentals/dependency-injection)技术的支持，并且在内部使用这一技术。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-211">ASP.NET Core has built-in support for and internally makes use of a technique known as [dependency injection](/aspnet/core/fundamentals/dependency-injection).</span></span> <span data-ttu-id="7b9e5-212">依赖关系注入技术可以在应用程序的不同部分之间实现松散耦合。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-212">Dependency injection is a technique that enables loose coupling between different parts of an application.</span></span> <span data-ttu-id="7b9e5-213">比较松散的耦合更符合需要，因为它可以更轻松地将应用程序的某些部分隔离开，便于进行测试或替换。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-213">Looser coupling is desirable because it makes it easier to isolate parts of the application, allowing for testing or replacement.</span></span> <span data-ttu-id="7b9e5-214">它还可以降低对应用程序某个部分进行更改会对应用程序中的其他位置产生意外影响的可能性。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-214">It also makes it less likely that a change in one part of the application will have an unexpected impact somewhere else in the application.</span></span> <span data-ttu-id="7b9e5-215">依赖关系注入的基础是依赖关系反转原则，并且通常是实现开放/闭合原则的关键。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-215">Dependency injection is based on the dependency inversion principle, and is often key to achieving the open/closed principle.</span></span> <span data-ttu-id="7b9e5-216">评估应用程序对其依赖关系的处理方式时，请注意 [static cling](https://deviq.com/static-cling/)（静态粘附）这一代码味，并请记住这句格言：[新增即粘附](https://ardalis.com/new-is-glue)。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-216">When evaluating how your application works with its dependencies, beware of the [static cling](https://deviq.com/static-cling/) code smell, and remember the aphorism "[new is glue](https://ardalis.com/new-is-glue)."</span></span>

<span data-ttu-id="7b9e5-217">类调用静态方法或访问静态属性时，会对基础结构造成负面影响或产生依赖关系，此时会发生静态粘附。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-217">Static cling occurs when your classes make calls to static methods, or access static properties, which have side effects or dependencies on infrastructure.</span></span> <span data-ttu-id="7b9e5-218">例如，如果一个方法调用静态方法，静态方法反过来又写入数据库，则该方法与该数据库紧密耦合。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-218">For example, if you have a method that calls a static method, which in turn writes to a database, your method is tightly coupled to the database.</span></span> <span data-ttu-id="7b9e5-219">破坏该数据库调用的任何内容都会破坏该方法。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-219">Anything that breaks that database call will break your method.</span></span> <span data-ttu-id="7b9e5-220">测试此类方法非常困难，因为此类测试要么需要使用商业模拟库来模拟静态调用，要么只能使用已有测试数据库进行测试。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-220">Testing such methods is notoriously difficult, since such tests either require commercial mocking libraries to mock the static calls, or can only be tested with a test database in place.</span></span> <span data-ttu-id="7b9e5-221">不依赖于任何基础结构的静态调用，尤其是完全无状态的调用可以进行正常调用，并且对耦合或可测试性没有任何影响（超越了将代码耦合到静态调用本身）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-221">Static calls that don't have any dependence on infrastructure, especially those calls that are completely stateless, are fine to call and have no impact on coupling or testability (beyond coupling code to the static call itself).</span></span>

<span data-ttu-id="7b9e5-222">许多开发人员知道静态粘附和全局状态的风险，但仍会通过直接实例化将其代码与具体实现紧密耦合。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-222">Many developers understand the risks of static cling and global state, but will still tightly couple their code to specific implementations through direct instantiation.</span></span> <span data-ttu-id="7b9e5-223">“新增即粘附”旨在提醒注意这种耦合，并非一律谴责使用 `new` 关键字。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-223">"New is glue" is meant to be a reminder of this coupling, and not a general condemnation of the use of the `new` keyword.</span></span> <span data-ttu-id="7b9e5-224">和静态方法调用一样，没有外部依赖关系的类型的新实例通常不会将代码紧密耦合到具体的实现，这会增加测试的难度。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-224">Just as with static method calls, new instances of types that have no external dependencies typically do not tightly couple code to implementation details or make testing more difficult.</span></span> <span data-ttu-id="7b9e5-225">但每次将类实例化时，应花一点时间来考虑在该特定位置硬编码该特定实例是否有意义，或者说如果将该实例作为依赖项进行请求会不会更好。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-225">But each time a class is instantiated, take just a brief moment to consider whether it makes sense to hard-code that specific instance in that particular location, or if it would be a better design to request that instance as a dependency.</span></span>

### <a name="declare-your-dependencies"></a><span data-ttu-id="7b9e5-226">声明依赖关系</span><span class="sxs-lookup"><span data-stu-id="7b9e5-226">Declare your dependencies</span></span>

<span data-ttu-id="7b9e5-227">ASP.NET Core 的构建原理是让方法和类声明依赖关系，并将其作为参数进行请求。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-227">ASP.NET Core is built around having methods and classes declare their dependencies, requesting them as arguments.</span></span> <span data-ttu-id="7b9e5-228">ASP.NET 应用程序通常在 Startup 类中进行设置，而该类本身就配置为在多处支持依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-228">ASP.NET applications are typically set up in a Startup class, which itself is configured to support dependency injection at several points.</span></span> <span data-ttu-id="7b9e5-229">如果 Startup 类具有构造函数，则可以通过构造函数请求依赖关系，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-229">If your Startup class has a constructor, it can request dependencies through the constructor, like so:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        var builder = new ConfigurationBuilder()
            .SetBasePath(env.ContentRootPath)
            .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
            .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true);
    }
}
```

<span data-ttu-id="7b9e5-230">Startup 类很有意思，因为它没有显式的类型要求。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-230">The Startup class is interesting in that there are no explicit type requirements for it.</span></span> <span data-ttu-id="7b9e5-231">它不继承特殊的 Startup 基类，也不实现任何特定的接口。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-231">It doesn't inherit from a special Startup base class, nor does it implement any particular interface.</span></span> <span data-ttu-id="7b9e5-232">可为其提供构造函数，也可不提供，并且可以为构造函数指定任意所需数量的参数。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-232">You can give it a constructor, or not, and you can specify as many parameters on the constructor as you want.</span></span> <span data-ttu-id="7b9e5-233">为应用程序配置的 Web 主机启动时，该主机将调用你命令其使用的 Startup 类，并将使用依赖关系注入来填充 Startup 类请求的任何依赖关系。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-233">When the web host you've configured for your application starts, it will call the Startup class you've told it to use, and will use dependency injection to populate any dependencies the Startup class requires.</span></span> <span data-ttu-id="7b9e5-234">当然，如果 ASP.NET Core 使用的服务容器中未配置请求的参数，则会引发异常，但只要是粘附到容器知晓的依赖项，则可以请求任何所需内容。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-234">Of course, if you request parameters that aren't configured in the services container used by ASP.NET Core, you'll get an exception, but as long as you stick to dependencies the container knows about, you can request anything you want.</span></span>

<span data-ttu-id="7b9e5-235">依赖关系注入从一开始创建 Startup 实例时就内置在 ASP.NET Core 应用中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-235">Dependency injection is built into your ASP.NET Core apps right from the start, when you create the Startup instance.</span></span> <span data-ttu-id="7b9e5-236">它不会为 Startup 类在此停留。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-236">It doesn't stop there for the Startup class.</span></span> <span data-ttu-id="7b9e5-237">也可以在 Configure 方法中请求依赖关系：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-237">You can also request dependencies in the Configure method:</span></span>

```csharp
public void Configure(IApplicationBuilder app,
    IHostingEnvironment env,
    ILoggerFactory loggerFactory)
{

}
```

<span data-ttu-id="7b9e5-238">ConfigureServices 方法是此行为的例外情况，它必须使用 IServiceCollection 类型的一个参数。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-238">The ConfigureServices method is the exception to this behavior; it must take just one parameter of type IServiceCollection.</span></span> <span data-ttu-id="7b9e5-239">实际上它并不需要支持依赖注入，因为一方面它负责向服务容器添加对象，另一方面它有权通过 IServiceCollection 参数访问所有当前已配置的服务。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-239">It doesn't really need to support dependency injection, since on the one hand it is responsible for adding objects to the services container, and on the other it has access to all currently configured services via the IServiceCollection parameter.</span></span> <span data-ttu-id="7b9e5-240">因此在 Startup 类的每个部分均可使用 ASP.NET Core 服务集合中定义的依赖关系，方法是以参数形式请求所需服务，或在 ConfigureServices 中使用 IServiceCollection。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-240">Thus, you can work with dependencies defined in the ASP.NET Core services collection in every part of the Startup class, either by requesting the needed service as a parameter or by working with the IServiceCollection in ConfigureServices.</span></span>

> [!NOTE]
> <span data-ttu-id="7b9e5-241">如果需要确保某些服务可供 Startup 类使用，可以使用 IWebHostBuilder 及其 ConfigureServices 方法在 CreateDefaultBuilder 调用中对其进行配置。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-241">If you need to ensure certain services are available to your Startup class, you can configure them using an IWebHostBuilder and its ConfigureServices method inside the CreateDefaultBuilder call.</span></span>

<span data-ttu-id="7b9e5-242">Startup 类是一个范例，应照此构建 ASP.NET Core 应用程序的其他部分 - 从控制器到中间件到过滤器再到自己的服务。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-242">The Startup class is a model for how you should structure other parts of your ASP.NET Core application, from Controllers to Middleware to Filters to your own Services.</span></span> <span data-ttu-id="7b9e5-243">在任何情况下都应遵守[显式依赖关系原则](https://deviq.com/explicit-dependencies-principle/)，请求依赖关系，而不要直接创建依赖关系，在整个应用程序中充分利用依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-243">In each case, you should follow the [Explicit Dependencies Principle](https://deviq.com/explicit-dependencies-principle/), requesting your dependencies rather than directly creating them, and leveraging dependency injection throughout your application.</span></span> <span data-ttu-id="7b9e5-244">注意对实现进行直接实例化的位置和方式，特别是使用基础结构或会产生负面影响的服务和对象。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-244">Be careful of where and how you directly instantiate implementations, especially services and objects that work with infrastructure or have side effects.</span></span> <span data-ttu-id="7b9e5-245">相较于对针对特定实现类型的引用进行硬编码，最好是使用在应用程序核心中定义并作为参数传递的抽象元素。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-245">Prefer working with abstractions defined in your application core and passed in as arguments to hardcoding references to specific implementation types.</span></span>

## <a name="structuring-the-application"></a><span data-ttu-id="7b9e5-246">构建应用程序</span><span class="sxs-lookup"><span data-stu-id="7b9e5-246">Structuring the application</span></span>

<span data-ttu-id="7b9e5-247">整体式应用程序通常只有一个入口点。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-247">Monolithic applications typically have a single entry point.</span></span> <span data-ttu-id="7b9e5-248">对 ASP.NET Core Web 应用程序而言，入口点是 ASP.NET Core Web 项目。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-248">In the case of an ASP.NET Core web application, the entry point will be the ASP.NET Core web project.</span></span> <span data-ttu-id="7b9e5-249">但是，这并不意味着解决方案只应包含一个项目。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-249">However, that doesn't mean the solution should consist of just a single project.</span></span> <span data-ttu-id="7b9e5-250">按照分离关注点的原则，将应用程序分解到不同层中非常有用。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-250">It's useful to break up the application into different layers in order to follow separation of concerns.</span></span> <span data-ttu-id="7b9e5-251">分解到不同层，有助于脱离文件夹的局限来分离项目，可帮助实现更好的封装。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-251">Once broken up into layers, it's helpful to go beyond folders to separate projects, which can help achieve better encapsulation.</span></span> <span data-ttu-id="7b9e5-252">通过 ASP.NET Core 应用程序实现这些目标的最佳方法是第 5 章中所述的整洁架构的变体。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-252">The best approach to achieve these goals with an ASP.NET Core application is a variation of the Clean Architecture discussed in chapter 5.</span></span> <span data-ttu-id="7b9e5-253">按照此方法，应用程序的解决方案将由分别用于 UI、基础结构和 ApplicationCore 的库构成。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-253">Following this approach, the application's solution will comprise separate libraries for the UI, Infrastructure, and ApplicationCore.</span></span>

<span data-ttu-id="7b9e5-254">除这些项目之外，还包含一个单独的测试项目（第 9 章中对测试进行介绍）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-254">In addition to these projects, separate test projects are included as well (Testing is discussed in Chapter 9).</span></span>

<span data-ttu-id="7b9e5-255">应用程序的对象模型和接口应放在 ApplicationCore 项目中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-255">The application's object model and interfaces should be placed in the ApplicationCore project.</span></span> <span data-ttu-id="7b9e5-256">此项目应具有尽可能少的依赖关系，可供解决方案中的其他项目引用。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-256">This project will have as few dependencies as possible, and the other projects in the solution will reference it.</span></span> <span data-ttu-id="7b9e5-257">需要保留的业务实体以及不直接依赖基础结构的服务在 ApplicationCore 项目中进行定义。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-257">Business entities that need to be persisted are defined in the ApplicationCore project, as are services that do not directly depend on infrastructure.</span></span>

<span data-ttu-id="7b9e5-258">实现的详细信息（例如如何执行保留或如何将通知发送给用户）保存在 Infrastructure 项目中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-258">Implementation details, such as how persistence is performed or how notifications might be sent to a user, are kept in the Infrastructure project.</span></span> <span data-ttu-id="7b9e5-259">此项目将引用特定于实现的包，例如 Entity Framework Core，但不应在此项目之外公开这些实现的详细信息。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-259">This project will reference implementation-specific packages such as Entity Framework Core, but should not expose details about these implementations outside of the project.</span></span> <span data-ttu-id="7b9e5-260">基础结构服务和存储库应实现 ApplicationCore 项目中定义的接口，其持久性实现负责检索和存储 ApplicationCore 中定义的实体。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-260">Infrastructure services and repositories should implement interfaces that are defined in the ApplicationCore project, and its persistence implementations are responsible for retrieving and storing entities defined in ApplicationCore.</span></span>

<span data-ttu-id="7b9e5-261">ASP.NET Core UI 项目负责所有 UI 级问题，但不得包含业务逻辑或基础结构详细信息。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-261">The ASP.NET Core UI project is responsible for any UI level concerns, but should not include business logic or infrastructure details.</span></span> <span data-ttu-id="7b9e5-262">实际上，最理想的情况是它甚至没有对 Infrastructure 项目的依赖关系，这样可确保不意外引入两个项目之间的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-262">In fact, ideally it shouldn't even have a dependency on the Infrastructure project, which will help ensure no dependency between the two projects is introduced accidentally.</span></span> <span data-ttu-id="7b9e5-263">第三方 DI 容器（例如 Autofac）可用于定于每个项目中模块类中的 DI 规则，从而帮助实现这一目的。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-263">This can be achieved using a third-party DI container like Autofac, which allows you to define DI rules in Module classes in each project.</span></span>

<span data-ttu-id="7b9e5-264">将应用程序与实现详细信息分离开的另一种方法是让应用程序调用微服务，微服务可能部署在各 Docker 容器中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-264">Another approach to decoupling the application from implementation details is to have the application call microservices, perhaps deployed in individual Docker containers.</span></span> <span data-ttu-id="7b9e5-265">相较于在两个项目之间利用 DI，这种方法更好地分离关注点，且解耦效果更好，但也更复杂一些。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-265">This provides even greater separation of concerns and decoupling than leveraging DI between two projects, but has additional complexity.</span></span>

### <a name="feature-organization"></a><span data-ttu-id="7b9e5-266">功能整理</span><span class="sxs-lookup"><span data-stu-id="7b9e5-266">Feature organization</span></span>

<span data-ttu-id="7b9e5-267">默认情况下，ASP.NET Core 应用程序将其文件夹结构整理为包含 Controllers 和 Views，还经常包含 ViewModels。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-267">By default, ASP.NET Core applications organize their folder structure to include Controllers and Views, and frequently ViewModels.</span></span> <span data-ttu-id="7b9e5-268">支持这些服务器端结构的客户端代码通常单独存放在 wwwroot 文件夹中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-268">Client-side code to support these server-side structures is typically stored separately in the wwwroot folder.</span></span> <span data-ttu-id="7b9e5-269">但是对于大型应用程序而言，这种整理方式可能会出现问题，因为处理任何给定功能通常会要求在这些文件夹之间跳转。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-269">However, large applications may encounter problems with this organization, since working on any given feature often requires jumping between these folders.</span></span> <span data-ttu-id="7b9e5-270">每个文件夹中的文件和子文件夹数量越多，通过解决方案资源管理器的滚动就越多，这种整理方式实现起来也就越难。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-270">This gets more and more difficult as the number of files and subfolders in each folder grows, resulting in a great deal of scrolling through Solution Explorer.</span></span> <span data-ttu-id="7b9e5-271">解决此问题的其中一种办法是按功能，而不要按文件类型来整理应用程序代码。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-271">One solution to this problem is to organize application code by _feature_ instead of by file type.</span></span> <span data-ttu-id="7b9e5-272">这种整理方式通常被称为功能文件夹或[功能切片](/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc)（另请参阅：[垂直切片](https://deviq.com/vertical-slices/)）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-272">This organizational style is typically referred to as feature folders or [feature slices](/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc) (see also: [Vertical Slices](https://deviq.com/vertical-slices/)).</span></span>

<span data-ttu-id="7b9e5-273">ASP.NET Core MVC 支持使用 Areas 实现此目的。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-273">ASP.NET Core MVC supports Areas for this purpose.</span></span> <span data-ttu-id="7b9e5-274">使用区域可以在每个 Area 文件夹中创建单独的 Controllers 和 Views 文件夹集（以及任何关联的模型）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-274">Using areas, you can create separate sets of Controllers and Views folders (as well as any associated models) in each Area folder.</span></span> <span data-ttu-id="7b9e5-275">图 7-1 显示了一个使用 Areas 的示例文件夹结构。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-275">Figure 7-1 shows an example folder structure, using Areas.</span></span>

![示例 Area 整理](./media/image7-1.png)

<span data-ttu-id="7b9e5-277">图 7-1。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-277">**Figure 7-1**.</span></span> <span data-ttu-id="7b9e5-278">示例 Area 整理</span><span class="sxs-lookup"><span data-stu-id="7b9e5-278">Sample Area Organization</span></span>

<span data-ttu-id="7b9e5-279">使用 Areas 时，必须使用属性通过控制器所属的区域名称来修饰控制器：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-279">When using Areas, you must use attributes to decorate your controllers with the name of the area to which they belong:</span></span>

```csharp
[Area("Catalog")]
public class HomeController
{}
```

<span data-ttu-id="7b9e5-280">还需要向路由添加区域支持：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-280">You also need to add area support to your routes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(name: "areaRoute", pattern: "{area:exists}/{controller=Home}/{action=Index}/{id?}");
    endpoints.MapControllerRoute(name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="7b9e5-281">除了对 Areas 的内置支持，还可以使用你自己的文件夹结构和约定来代替属性和自定义路由。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-281">In addition to the built-in support for Areas, you can also use your own folder structure, and conventions in place of attributes and custom routes.</span></span> <span data-ttu-id="7b9e5-282">这样可以让功能文件夹中不包含单独的 Views 和 Controllers 等文件夹，让层次结构更简单，并且可以更轻松地在一个地方查看每个功能的所有相关文件。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-282">This would allow you to have feature folders that didn't include separate folders for Views, Controllers, etc., keeping the hierarchy flatter and making it easier to see all related files in a single place for each feature.</span></span>

<span data-ttu-id="7b9e5-283">ASP.NET Core 使用内置的约定类型来控制其行为。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-283">ASP.NET Core uses built-in convention types to control its behavior.</span></span> <span data-ttu-id="7b9e5-284">可以修改或替换这些约定。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-284">You can modify or replace these conventions.</span></span> <span data-ttu-id="7b9e5-285">例如，可以创建一个约定，自动基于给定控制器的命名空间获取其功能名称（通常对应于控制器所在的文件夹）：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-285">For example, you can create a convention that will automatically get the feature name for a given controller based on its namespace (which typically correlates to the folder in which the controller is located):</span></span>

```csharp
public class FeatureConvention : IControllerModelConvention
{
    public void Apply(ControllerModel controller)
    {
        controller.Properties.Add("feature",
        GetFeatureName(controller.ControllerType));
    }

    private string GetFeatureName(TypeInfo controllerType)
    {
        string[] tokens = controllerType.FullName.Split('.');
        if (!tokens.Any(t => t == "Features")) return "";
        string featureName = tokens
        .SkipWhile(t => !t.Equals("features",
        StringComparison.CurrentCultureIgnoreCase))
        .Skip(1)
        .Take(1)
        .FirstOrDefault();
        return featureName;
    }
}
```

<span data-ttu-id="7b9e5-286">然后在 ConfigureServices 中向应用程序添加 MVC 支持时将此约定指定为一个选项：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-286">You then specify this convention as an option when you add support for MVC to your application in ConfigureServices:</span></span>

```csharp
services.AddMvc(o => o.Conventions.Add(new FeatureConvention()));
```

<span data-ttu-id="7b9e5-287">ASP.NET Core MVC 还使用约定来确定视图的位置。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-287">ASP.NET Core MVC also uses a convention to locate views.</span></span> <span data-ttu-id="7b9e5-288">可以使用自定义约定取而代之，使视图位于功能文件夹中（使用上述 FeatureConvention 提供的功能名称）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-288">You can override it with a custom convention so that views will be located in your feature folders (using the feature name provided by the FeatureConvention, above).</span></span> <span data-ttu-id="7b9e5-289">可在 MSDN 杂志文章 [ASP.NET Core MVC 的功能切片](/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc)中详细了解此方法并下载工作示例。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-289">You can learn more about this approach and download a working sample from the MSDN Magazine article, [Feature Slices for ASP.NET Core MVC](/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc).</span></span>

### <a name="apis-and-blazor-applications"></a><span data-ttu-id="7b9e5-290">API 和 Blazor 应用程序</span><span class="sxs-lookup"><span data-stu-id="7b9e5-290">APIs and Blazor applications</span></span>

<span data-ttu-id="7b9e5-291">如果应用程序包含一组必须受到保护的 Web API，最好将这些 apis 配置为独立于 View 或 Razor Pages 应用程序的项目。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-291">If your application includes a set of web APIs, which must be secured, these apis should ideally be configured as a separate project from your View or Razor Pages application.</span></span> <span data-ttu-id="7b9e5-292">从服务器端 Web 应用程序中分离 API（尤其是公共 API）有很多好处。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-292">Separating APIs, especially public APIs, from your server-side web application has a number of benefits.</span></span> <span data-ttu-id="7b9e5-293">这些应用程序通常具有独特的部署和负载特征。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-293">These applications often will have unique deployment and load characteristics.</span></span> <span data-ttu-id="7b9e5-294">这些应用程序也很可能采用不同的安全机制，其中利用基于 Cookie 的身份验证和 API 的基于表单的标准应用程序很可能使用基于令牌的身份验证。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-294">They're also very likely to adopt different mechanisms for security, with standard form-based applications leveraging cookie-based authentication and APIs most likely using token-based authentication.</span></span>

<span data-ttu-id="7b9e5-295">此外，Blazor 应用程序（无论使用 Blazor Server 还是 Blazor WebAssembly）都应作为单独的项目进行构建。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-295">Additionally, Blazor applications, whether using Blazor Server or Blazor WebAssembly, should be built as separate projects.</span></span> <span data-ttu-id="7b9e5-296">这些应用程序具有不同的运行时特征以及安全模型。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-296">The applications have different runtime characteristics as well as security models.</span></span> <span data-ttu-id="7b9e5-297">这些应用程序可能与服务器端 Web 应用程序（或 API 项目）共享通用类型，这些类型应在通用的共享项目中进行定义。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-297">They're likely to share common types with the server-side web application (or API project), and these types should be defined in a common shared project.</span></span>

<span data-ttu-id="7b9e5-298">向 eShopOnWeb 添加 Blazor WebAssembly 管理界面需要添加几个新项目。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-298">The addition of a Blazor WebAssembly admin interface to eShopOnWeb required adding several new projects.</span></span> <span data-ttu-id="7b9e5-299">Blazor WebAssembly 项目本身，即 `BlazorAdmin`。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-299">The Blazor WebAssembly project itself, `BlazorAdmin`.</span></span> <span data-ttu-id="7b9e5-300">`PublicApi` 项目中定义了一组新的公共 API 终结点，这些终结点由 `BlazorAdmin` 使用并配置为使用基于令牌的身份验证。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-300">A new set of public API endpoints, used by `BlazorAdmin` and configured to use token-based authentication, is defined in the `PublicApi` project.</span></span> <span data-ttu-id="7b9e5-301">这两个项目使用的某些共享类型均保存在一个新的 `BlazorShared` 项目中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-301">And certain shared types used by both of these projects are kept in a new `BlazorShared` project.</span></span>

<span data-ttu-id="7b9e5-302">有人可能会问，已存在可用于共享 `PublicApi` 和 `BlazorAdmin` 所需的任何类型的通用 `ApplicationCore` 项目，为何还要添加单独的 `BlazorShared` 项目？</span><span class="sxs-lookup"><span data-stu-id="7b9e5-302">One might ask, why add a separate `BlazorShared` project when there is already a common `ApplicationCore` project that could be used to share any types required by both `PublicApi` and `BlazorAdmin`?</span></span> <span data-ttu-id="7b9e5-303">答案就是，此项目包含了应用程序的所有业务逻辑，因此比所需项目要大得多，也很有可能需要在服务器上保持安全。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-303">The answer is that this project includes all of the application's business logic and is thus much larger than necessary and also much more likely to need to be kept secure on the server.</span></span> <span data-ttu-id="7b9e5-304">请记住，`BlazorAdmin` 引用的任何库都将在用户加载 Blazor 应用程序时下载到用户的浏览器中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-304">Remember that any library referenced by `BlazorAdmin` will be downloaded to users' browsers when they load the Blazor application.</span></span>

<span data-ttu-id="7b9e5-305">Blazor WebAssembly 应用使用的 API 可能不会与 Blazor 完全共享其类型，具体取决于是否使用[前端专属的后端 (BFF) 模式](/azure/architecture/patterns/backends-for-frontends)。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-305">Depending on whether one is using the [Backends-For-Frontends (BFF) pattern](/azure/architecture/patterns/backends-for-frontends), the APIs consumed by the Blazor WebAssembly app may not share their types 100% with Blazor.</span></span> <span data-ttu-id="7b9e5-306">特别是，打算由许多不同客户端使用的公共 API 可以定义其自己的请求和结果类型，而不会在特定于客户端的共享项目中共享这些结果类型。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-306">In particular, a public API that's meant to be consumed by many different clients may define its own request and result types, rather than sharing them in a client-specific shared project.</span></span> <span data-ttu-id="7b9e5-307">在 eShopOnWeb 示例中，假设 `PublicApi` 项目实际上用于托管公共 API，因此并非所有请求和响应类型都来自 `BlazorShared` 项目。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-307">In the eShopOnWeb sample, the assumption is being made that the `PublicApi` project is, in fact, hosting a public API, so not all of its request and response types come from the `BlazorShared` project.</span></span>

### <a name="cross-cutting-concerns"></a><span data-ttu-id="7b9e5-308">横切关注点</span><span class="sxs-lookup"><span data-stu-id="7b9e5-308">Cross-cutting concerns</span></span>

<span data-ttu-id="7b9e5-309">随着应用程序的发展，将横切关注点分解出来以消除重复和保持一致性变得越来越重要。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-309">As applications grow, it becomes increasingly important to factor out cross-cutting concerns to eliminate duplication and maintain consistency.</span></span> <span data-ttu-id="7b9e5-310">ASP.NET Core 应用程序中的横切关注点非常多，例如身份验证、模型验证规则、输出缓存和错误处理等。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-310">Some examples of cross-cutting concerns in ASP.NET Core applications are authentication, model validation rules, output caching, and error handling, though there are many others.</span></span> <span data-ttu-id="7b9e5-311">ASP.NET Core MVC [过滤器](/aspnet/core/mvc/controllers/filters)允许在执行请求处理管道中的特定步骤之前或之后运行代码。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-311">ASP.NET Core MVC [filters](/aspnet/core/mvc/controllers/filters) allow you to run code before or after certain steps in the request processing pipeline.</span></span> <span data-ttu-id="7b9e5-312">例如，可以在模型绑定之前/之后、某个操作之前/之后或某个操作结果之前/之后运行过滤器。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-312">For instance, a filter can run before and after model binding, before and after an action, or before and after an action's result.</span></span> <span data-ttu-id="7b9e5-313">还可以使用授权过滤器来控制对管道其余部分的访问权限。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-313">You can also use an authorization filter to control access to the rest of the pipeline.</span></span> <span data-ttu-id="7b9e5-314">图 7-2 显示了请求执行操作是如何通过一系列过滤器（如果已配置）的。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-314">Figures 7-2 shows how request execution flows through filters, if configured.</span></span>

![请求通过授权过滤器、资源过滤器、模型绑定、操作过滤器、操作执行和操作结果转换、异常过滤器、结果过滤器和结果执行进行处理。](./media/image7-2.png)

<span data-ttu-id="7b9e5-317">**图 7-2**。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-317">**Figure 7-2**.</span></span> <span data-ttu-id="7b9e5-318">请求执行通过各过滤器和请求管道。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-318">Request execution through filters and request pipeline.</span></span>

<span data-ttu-id="7b9e5-319">筛选器通常作为属性实现，因此可应用于控制器或操作（甚至全局）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-319">Filters are usually implemented as attributes, so you can apply them to controllers or actions (or even globally).</span></span> <span data-ttu-id="7b9e5-320">以这种方式添加时，在操作级别指定的过滤器会覆盖在控制器级别指定的过滤器（会覆盖全局过滤器）或在其基础之上生成。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-320">When added in this fashion, filters specified at the action level override or build upon filters specified at the controller level, which themselves override global filters.</span></span> <span data-ttu-id="7b9e5-321">例如，\[Route\] 属性可用来生成控制器和操作之间的路由。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-321">For example, the \[Route\] attribute can be used to build up routes between controllers and actions.</span></span> <span data-ttu-id="7b9e5-322">同样，可以在控制器级别配置授权，然后被各操作覆盖，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-322">Likewise, authorization can be configured at the controller level, and then overridden by individual actions, as the following sample demonstrates:</span></span>

```csharp
[Authorize]
public class AccountController : Controller

{
    [AllowAnonymous] // overrides the Authorize attribute
    public async Task<IActionResult> Login() {}
    public async Task<IActionResult> ForgotPassword() {}
}
```

<span data-ttu-id="7b9e5-323">第一个方法 Login 使用 AllowAnonymous 筛选器（属性）来覆盖在控制器级别设置的 Authorize 筛选器。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-323">The first method, Login, uses the AllowAnonymous filter (attribute) to override the Authorize filter set at the controller level.</span></span> <span data-ttu-id="7b9e5-324">ForgotPassword 操作（以及类中没有 AllowAnonymous 属性的任何其他操作）需要经过身份验证的请求。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-324">The ForgotPassword action (and any other action in the class that doesn't have an AllowAnonymous attribute) will require an authenticated request.</span></span>

<span data-ttu-id="7b9e5-325">筛选器可作为 API 的常见错误处理策略，用于消除重复。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-325">Filters can be used to eliminate duplication in the form of common error handling policies for APIs.</span></span> <span data-ttu-id="7b9e5-326">例如，如果请求引用的关键字不存在，典型的 API 策略会返回 NotFound 响应，如果模型验证失败，则返回 BadRequest 响应。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-326">For example, a typical API policy is to return a NotFound response to requests referencing keys that do not exist, and a BadRequest response if model validation fails.</span></span> <span data-ttu-id="7b9e5-327">下面的示例通过操作演示了这两种策略：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-327">The following example demonstrates these two policies in action:</span></span>

```csharp
[HttpPut("{id}")]
public async Task<IActionResult> Put(int id, [FromBody]Author author)
{
    if ((await _authorRepository.ListAsync()).All(a => a.Id != id))
    {
        return NotFound(id);
    }
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }
    author.Id = id;
    await _authorRepository.UpdateAsync(author);
    return Ok();
}
```

<span data-ttu-id="7b9e5-328">切勿让操作方法因类似于此的条件代码变得杂乱。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-328">Don't allow your action methods to become cluttered with conditional code like this.</span></span> <span data-ttu-id="7b9e5-329">应将策略放在可按需应用的过滤器中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-329">Instead, pull the policies into filters that can be applied on an as-needed basis.</span></span> <span data-ttu-id="7b9e5-330">此示例中，可使用以下属性替换（每当向 API 发送命令就会触发的）模型验证检查：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-330">In this example, the model validation check, which should occur anytime a command is sent to the API, can be replaced by the following attribute:</span></span>

```csharp
public class ValidateModelAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        if (!context.ModelState.IsValid)
        {
            context.Result = new BadRequestObjectResult(context.ModelState);
        }
    }
}
```

<span data-ttu-id="7b9e5-331">可以通过包含 [Ardalis.ValidateModel](https://www.nuget.org/packages/Ardalis.ValidateModel) 包将 `ValidateModelAttribute` 作为 NuGet 依赖项添加到项目中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-331">You can add the `ValidateModelAttribute` to your project as a NuGet dependency by including the [Ardalis.ValidateModel](https://www.nuget.org/packages/Ardalis.ValidateModel) package.</span></span> <span data-ttu-id="7b9e5-332">对于 API，可以使用 `ApiController` 属性强制执行此行为，而无需单独的 `ValidateModel` 筛选器。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-332">For APIs, you can use the `ApiController` attribute to enforce this behavior without the need for a separate `ValidateModel` filter.</span></span>

<span data-ttu-id="7b9e5-333">同样，可以使用过滤器来检查某条记录是否存在，并在执行操作前返回 404，而无需在操作中执行这些检查。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-333">Likewise, a filter can be used to check if a record exists and return a 404 before the action is executed, eliminating the need to perform these checks in the action.</span></span> <span data-ttu-id="7b9e5-334">在将常见约定提取出来、整理解决方案、将基础结构代码和业务逻辑与 UI 分离开后，MVC 操作方法会变得极其精简：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-334">Once you've pulled out common conventions and organized your solution to separate infrastructure code and business logic from your UI, your MVC action methods should be extremely thin:</span></span>

```csharp
[HttpPut("{id}")]
[ValidateAuthorExists]
public async Task<IActionResult> Put(int id, [FromBody]Author author)
{
    await _authorRepository.UpdateAsync(author);
    return Ok();
}
```

<span data-ttu-id="7b9e5-335">你可阅读 MSDN 杂志文章[真实的 ASP.NET Core MVC 过滤器](/archive/msdn-magazine/2016/august/asp-net-core-real-world-asp-net-core-mvc-filters)，了解有关实现过滤器的详细信息并下载工作示例。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-335">You can read more about implementing filters and download a working sample from the MSDN Magazine article, [Real-World ASP.NET Core MVC Filters](/archive/msdn-magazine/2016/august/asp-net-core-real-world-asp-net-core-mvc-filters).</span></span>

> ### <a name="references--structuring-applications"></a><span data-ttu-id="7b9e5-336">参考 - 构建应用程序</span><span class="sxs-lookup"><span data-stu-id="7b9e5-336">References – Structuring applications</span></span>
>
> - <span data-ttu-id="7b9e5-337">**Areas**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-337">**Areas**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/mvc/controllers/areas>
> - <span data-ttu-id="7b9e5-338">**MSDN 杂志 - ASP.NET Core MVC 的功能切片**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-338">**MSDN Magazine – Feature Slices for ASP.NET Core MVC**</span></span>\
>   <https://docs.microsoft.com/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc>
> - <span data-ttu-id="7b9e5-339">**筛选器**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-339">**Filters**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/mvc/controllers/filters>
> - <span data-ttu-id="7b9e5-340">**MSDN 杂志 - 真实的 ASP.NET Core MVC 筛选器**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-340">**MSDN Magazine – Real World ASP.NET Core MVC Filters**</span></span>\
>   <https://docs.microsoft.com/archive/msdn-magazine/2016/august/asp-net-core-real-world-asp-net-core-mvc-filters>

## <a name="security"></a><span data-ttu-id="7b9e5-341">安全性</span><span class="sxs-lookup"><span data-stu-id="7b9e5-341">Security</span></span>

<span data-ttu-id="7b9e5-342">保护 Web 应用程序是一个非常大的主题，涉及到许多问题。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-342">Securing web applications is a large topic, with many considerations.</span></span> <span data-ttu-id="7b9e5-343">安全性涉及的最基本问题是，确保你知道是谁发出的给定请求，然后确保该请求只对它应访问的资源具有访问权限。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-343">At its most basic level, security involves ensuring you know who a given request is coming from, and then ensuring that the request only has access to resources it should.</span></span> <span data-ttu-id="7b9e5-344">身份验证是将请求提供的凭据与受信任数据存储中的凭据进行对比的过程，目的在于确定该请求是否应被视为来源于已知实体。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-344">Authentication is the process of comparing credentials provided with a request to those in a trusted data store, to see if the request should be treated as coming from a known entity.</span></span> <span data-ttu-id="7b9e5-345">授权是根据用户标识限制对某些资源的访问权限的过程。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-345">Authorization is the process of restricting access to certain resources based on user identity.</span></span> <span data-ttu-id="7b9e5-346">第三个安全问题是保护请求免遭第三方窃听，对于此问题至少应[确保 SSL 由你的应用程序使用](/aspnet/core/security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-346">A third security concern is protecting requests from eavesdropping by third parties, for which you should at least [ensure that SSL is used by your application](/aspnet/core/security/enforcing-ssl).</span></span>

### <a name="identity"></a><span data-ttu-id="7b9e5-347">标识</span><span class="sxs-lookup"><span data-stu-id="7b9e5-347">Identity</span></span>

<span data-ttu-id="7b9e5-348">ASP.NET Core 标识是一个成员身份系统，可用于支持应用程序的登录功能。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-348">ASP.NET Core Identity is a membership system you can use to support login functionality for your application.</span></span> <span data-ttu-id="7b9e5-349">它支持本地用户帐户，也支持来自 Microsoft Account、Twitter、Facebook 和 Google 等提供者的外部登录。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-349">It has support for local user accounts as well as external login provider support from providers like Microsoft Account, Twitter, Facebook, Google, and more.</span></span> <span data-ttu-id="7b9e5-350">除 ASP.NET Core 标识之外，应用程序还可以使用 Windows 身份验证或 [Identity Server](https://github.com/IdentityServer/IdentityServer4) 等第三方标识提供者。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-350">In addition to ASP.NET Core Identity, your application can use windows authentication, or a third-party identity provider like [Identity Server](https://github.com/IdentityServer/IdentityServer4).</span></span>

<span data-ttu-id="7b9e5-351">如果选择了“个人用户帐户”选项，ASP.NET Core 标识将包含在新的项目模板中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-351">ASP.NET Core Identity is included in new project templates if the Individual User Accounts option is selected.</span></span> <span data-ttu-id="7b9e5-352">此模板包括对注册、登录名、外部登录名、忘记的密码和其他功能的支持。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-352">This template includes support for registration, login, external logins, forgotten passwords, and additional functionality.</span></span>

![选择“个人用户帐户”以预配标识](./media/image7-3.png)

<span data-ttu-id="7b9e5-354">**图 7-3**。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-354">**Figure 7-3**.</span></span> <span data-ttu-id="7b9e5-355">选择“个人用户帐户”以预配标识。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-355">Select Individual User Accounts to have Identity preconfigured.</span></span>

<span data-ttu-id="7b9e5-356">在 ConfigureServices 和 Configure 中，标识支持都在 Startup 中配置：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-356">Identity support is configured in Startup, both in ConfigureServices and Configure:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();
    services.AddMvc();
}

public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();
    app.UseIdentity();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllerRoute(name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="7b9e5-357">在 Configure 方法中，UseIdentity 应出现在 UseMvc 之前，这一点很重要。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-357">It's important that UseIdentity appear before UseMvc in the Configure method.</span></span> <span data-ttu-id="7b9e5-358">在 ConfigureServices 中配置标识时，请注意对 AddDefaultTokenProviders 的调用。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-358">When configuring Identity in ConfigureServices, you'll notice a call to AddDefaultTokenProviders.</span></span> <span data-ttu-id="7b9e5-359">这与用于保护 Web 通信的令牌无关，它引用的是创建提示的提供者，该提示会通过短信或电子邮件发送给用户让其确认身份。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-359">This has nothing to do with tokens that may be used to secure web communications, but instead refers to providers that create prompts that can be sent to users via SMS or email in order for them to confirm their identity.</span></span>

<span data-ttu-id="7b9e5-360">可从官方 ASP.NET Core 文档详细了解有关[配置双重身份验证](/aspnet/core/security/authentication/2fa)和[启用外部登录提供者](/aspnet/core/security/authentication/social/)的信息。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-360">You can learn more about [configuring two-factor authentication](/aspnet/core/security/authentication/2fa) and [enabling external login providers](/aspnet/core/security/authentication/social/) from the official ASP.NET Core docs.</span></span>

### <a name="authentication"></a><span data-ttu-id="7b9e5-361">身份验证</span><span class="sxs-lookup"><span data-stu-id="7b9e5-361">Authentication</span></span>

<span data-ttu-id="7b9e5-362">身份验证是确定谁在访问系统的过程。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-362">Authentication is the process of determining who is accessing the system.</span></span> <span data-ttu-id="7b9e5-363">如果使用的是上一部分中介绍的 ASP.NET Core 标识和配置方法，将在应用程序中自动配置一些身份验证默认值。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-363">If you're using ASP.NET Core Identity and the configuration methods shown in the previous section, it will automatically configure some authentication defaults in the application.</span></span> <span data-ttu-id="7b9e5-364">不过，你也可以手动配置这些默认值，或者替代由 AddIdentity 设置的默认值。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-364">However, you can also configure these defaults manually, or override the ones set by AddIdentity.</span></span> <span data-ttu-id="7b9e5-365">如果使用的是标识，它会将基于 Cookie 的身份验证配置为默认的方案。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-365">If you're using Identity, it configures cookie-based authentication as the default *scheme*.</span></span>

<span data-ttu-id="7b9e5-366">在基于 Web 的身份验证中，通常可以在身份验证系统客户端的过程中最多执行五个操作。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-366">In web-based authentication, there are typically up to five actions that may be performed in the course of authenticating a client of a system.</span></span> <span data-ttu-id="7b9e5-367">它们是：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-367">These are:</span></span>

- <span data-ttu-id="7b9e5-368">身份验证。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-368">Authenticate.</span></span> <span data-ttu-id="7b9e5-369">使用客户端提供的信息创建标识，供其在应用程序中使用。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-369">Use the information provided by the client to create an identity for them to use within the application.</span></span>
- <span data-ttu-id="7b9e5-370">质询。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-370">Challenge.</span></span> <span data-ttu-id="7b9e5-371">此操作用于要求客户端标识它们自己。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-371">This action is used to require the client to identify themselves.</span></span>
- <span data-ttu-id="7b9e5-372">禁止。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-372">Forbid.</span></span> <span data-ttu-id="7b9e5-373">告知客户端禁止执行操作。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-373">Inform the client they are forbidden from performing an action.</span></span>
- <span data-ttu-id="7b9e5-374">登录。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-374">Sign-in.</span></span> <span data-ttu-id="7b9e5-375">通过某种方式留在现有的客户端中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-375">Persist the existing client in some way.</span></span>
- <span data-ttu-id="7b9e5-376">注销。从暂留中删除客户端。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-376">Sign-out. Remove the client from persistence.</span></span>

<span data-ttu-id="7b9e5-377">可以通过多种常用方法在 Web 应用程序中执行身份验证。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-377">There are a number of common techniques for performing authentication in web applications.</span></span> <span data-ttu-id="7b9e5-378">这些方法称为方案。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-378">These are referred to as schemes.</span></span> <span data-ttu-id="7b9e5-379">给定的方案将为上述部分选项或所有选项定义操作。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-379">A given scheme will define actions for some or all of the above options.</span></span> <span data-ttu-id="7b9e5-380">一些方案仅支持部分操作，可能需要单独的方案才能执行其不支持的操作。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-380">Some schemes only support a subset of actions, and may require a separate scheme to perform those it does not support.</span></span> <span data-ttu-id="7b9e5-381">例如，OpenId-Connect (OIDC) 方案不支持登录或注销，但通常配置为使用 Cookie 身份验证来实现暂留。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-381">For example, the OpenId-Connect (OIDC) scheme doesn't support Sign-in or Sign-out, but is commonly configured to use Cookie authentication for this persistence.</span></span>

<span data-ttu-id="7b9e5-382">在 ASP.NET Core 应用程序中，可以为上述每个操作配置 `DefaultAuthenticateScheme` 以及可选的特定方案。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-382">In your ASP.NET Core application, you can configure a `DefaultAuthenticateScheme` as well as optional specific schemes for each of the actions described above.</span></span> <span data-ttu-id="7b9e5-383">例如 `DefaultChallengeScheme` 和 `DefaultForbidScheme` 等。调用 [`AddIdentity<TUser,TRole>`](https://github.com/dotnet/aspnetcore/blob/release/3.1/src/Identity/Core/src/IdentityServiceCollectionExtensions.cs#L38-L102) 可以配置应用程序的许多方面，并添加许多必需的服务。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-383">For example, `DefaultChallengeScheme`, `DefaultForbidScheme`, etc. Calling [`AddIdentity<TUser,TRole>`](https://github.com/dotnet/aspnetcore/blob/release/3.1/src/Identity/Core/src/IdentityServiceCollectionExtensions.cs#L38-L102) configures a number of aspects of the application and adds many required services.</span></span> <span data-ttu-id="7b9e5-384">其中还包括用于配置身份验证方案的此调用：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-384">It also includes this call to configure the authentication scheme:</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = IdentityConstants.ApplicationScheme;
    options.DefaultChallengeScheme = IdentityConstants.ApplicationScheme;
    options.DefaultSignInScheme = IdentityConstants.ExternalScheme;
});
```

<span data-ttu-id="7b9e5-385">这些方案默认使用 Cookie 实现暂留以及重定向到登录页面以进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-385">These schemes use cookies for persistence and redirection to login pages for authentication by default.</span></span> <span data-ttu-id="7b9e5-386">这些方案适用于通过 Web 浏览器与用户交互的 Web 应用程序，但不建议用于 API。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-386">These schemes are appropriate for web applications that interact with users via web browsers, but not recommended for APIs.</span></span> <span data-ttu-id="7b9e5-387">相反，API 通常将使用其他身份验证形式，例如 JWT 持有者令牌。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-387">Instead, APIs will typically use another form of authentication, such as JWT bearer tokens.</span></span>

<span data-ttu-id="7b9e5-388">Web API 基于代码使用，例如 .NET 应用程序中的 `HttpClient` 和其他框架中的等效类型。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-388">Web APIs are consumed by code, such as `HttpClient` in .NET applications and equivalent types in other frameworks.</span></span> <span data-ttu-id="7b9e5-389">这些客户端期望从 API 调用或指示发生某种问题（如果有）的状态代码中获得可用响应。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-389">These clients expect a usable response from an API call, or a status code indicating what, if any, problem has occurred.</span></span> <span data-ttu-id="7b9e5-390">这些客户端不会通过浏览器进行交互，也不会呈现 API 可能返回的任何 HTML 或与其进行交互。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-390">These clients are not interacting through a browser and do not render or interact with any HTML that an API might return.</span></span> <span data-ttu-id="7b9e5-391">因此，如果 API 终结点未通过身份验证，则不适合将其客户端重定向到登录页面。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-391">Thus, it is not appropriate for API endpoints to redirect their clients to login pages if they are not authenticated.</span></span> <span data-ttu-id="7b9e5-392">另一种方案更为合适。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-392">Another scheme is more appropriate.</span></span>

<span data-ttu-id="7b9e5-393">若要为 API 配置身份验证，可以按如下所示进行设置（用于 eShopOnWeb 引用应用程序中的 `PublicApi` 项目）：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-393">To configure authentication for APIs, you might set up authentication like the following, used by the `PublicApi` project in the eShopOnWeb reference application:</span></span>

```csharp
services.AddAuthentication(config =>
{
    config.DefaultScheme = JwtBearerDefaults.AuthenticationScheme;
})
    .AddJwtBearer(config =>
    {
        config.RequireHttpsMetadata = false;
        config.SaveToken = true;
        config.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(key),
            ValidateIssuer = false,
            ValidateAudience = false
        };
    });
```

<span data-ttu-id="7b9e5-394">虽然可以在一个项目中配置多个不同的身份验证方案，但配置一个默认方案要简单得多。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-394">While it is possible to configure multiple different authentication schemes within a single project, it is much simpler to configure a single default scheme.</span></span> <span data-ttu-id="7b9e5-395">出于此原因，eShopOnWeb 引用应用程序将其 API 划分到自己的项目 `PublicApi` 中，与包含应用程序视图和 Razor Pages 的主 `Web` 项目分开。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-395">For this reason, among others, the eShopOnWeb reference application separates its APIs into their own project, `PublicApi`, separate from the main `Web` project that includes the application's views and Razor Pages.</span></span>

#### <a name="authentication-in-blazor-apps"></a><span data-ttu-id="7b9e5-396">Blazor 应用中的身份验证</span><span class="sxs-lookup"><span data-stu-id="7b9e5-396">Authentication in Blazor apps</span></span>

<span data-ttu-id="7b9e5-397">Blazor Server 应用程序可以利用与任何其他 ASP.NET Core 应用程序相同的身份验证功能。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-397">Blazor Server applications can leverage the same authentication features as any other ASP.NET Core application.</span></span> <span data-ttu-id="7b9e5-398">不过，Blazor WebAssembly 应用程序无法使用内置的标识和身份验证提供程序，因为它们在浏览器中运行。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-398">Blazor WebAssembly applications cannot use the built-in Identity and Authentication providers, however, since they run in the browser.</span></span> <span data-ttu-id="7b9e5-399">Blazor WebAssembly 应用程序可以在本地存储用户身份验证状态，并且可以访问声明，以确定用户应执行的操作。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-399">Blazor WebAssembly applications can store user authentication status locally and can access claims to determine what actions users should be able to perform.</span></span> <span data-ttu-id="7b9e5-400">但是，不管在 Blazor WebAssembly 应用内部实现了什么逻辑，都应在服务器上执行所有身份验证和授权检查，因为用户可以轻松地绕过该应用并直接与 API 进行交互。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-400">However, all authentication and authorization checks should be performed on the server regardless of any logic implemented inside the Blazor WebAssembly app, since users can easily bypass the app and interact with the APIs directly.</span></span>

> ### <a name="references--authentication"></a><span data-ttu-id="7b9e5-401">引用 - 身份验证</span><span class="sxs-lookup"><span data-stu-id="7b9e5-401">References – Authentication</span></span>
>
> - <span data-ttu-id="7b9e5-402">**身份验证操作和默认值**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-402">**Authentication Actions and Defaults**</span></span>\
>   <https://stackoverflow.com/a/52493428>
> - <span data-ttu-id="7b9e5-403">**SPA 的身份验证和授权**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-403">**Authentication and Authorization for SPAs**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/security/authentication/identity-api-authorization>
> - <span data-ttu-id="7b9e5-404">**ASP.NET Core Blazor 身份验证和授权**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-404">**ASP.NET Core Blazor Authentication and Authorization**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/blazor/security/>
> - <span data-ttu-id="7b9e5-405">**安全性：ASP.NET Web Forms 和 Blazor** \ 中的身份验证和授权</span><span class="sxs-lookup"><span data-stu-id="7b9e5-405">**Security: Authentication and Authorization in ASP.NET Web Forms and Blazor**\</span></span>
>   <https://docs.microsoft.com/dotnet/architecture/blazor-for-web-forms-developers/security-authentication-authorization>

### <a name="authorization"></a><span data-ttu-id="7b9e5-406">授权</span><span class="sxs-lookup"><span data-stu-id="7b9e5-406">Authorization</span></span>

<span data-ttu-id="7b9e5-407">最简单的授权方式包括限制匿名用户的访问权限。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-407">The simplest form of authorization involves restricting access to anonymous users.</span></span> <span data-ttu-id="7b9e5-408">只需对某些控制器和操作应用 \[Authorize\] 属性即可实现此功能。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-408">This functionality can be achieved by applying the \[Authorize\] attribute to certain controllers or actions.</span></span> <span data-ttu-id="7b9e5-409">如果正在使用角色，可进一步扩展该属性，用于限制属于特定角色的用户的访问权限，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-409">If roles are being used, the attribute can be further extended to restrict access to users who belong to certain roles, as shown:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{

}
```

<span data-ttu-id="7b9e5-410">在此示例中，属于 `HRManager` 或 `Finance` 角色（或同时属于这两个角色）的用户将有权访问 SalaryController。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-410">In this case, users belonging to either the `HRManager` or `Finance` roles (or both) would have access to the SalaryController.</span></span> <span data-ttu-id="7b9e5-411">如需要求用户属于多个角色，而不是属于多个角色中的某一个角色，可多次应用该属性，每次指定一个所需角色。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-411">To require that a user belong to multiple roles (not just one of several), you can apply the attribute multiple times, specifying a required role each time.</span></span>

<span data-ttu-id="7b9e5-412">在许多不同的控制器和操作中以字符串形式指定特定角色集可能会导致不必要的重复。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-412">Specifying certain sets of roles as strings in many different controllers and actions can lead to undesirable repetition.</span></span> <span data-ttu-id="7b9e5-413">至少要为这些字符串文本定义常量，并在需要指定字符串的任何位置使用常量。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-413">At a minimum, define constants for these string literals and use the constants anywhere you need to specify the string.</span></span> <span data-ttu-id="7b9e5-414">也可配置封装授权规则的授权策略，然后在应用 \[Authorize\] 属性时指定该策略，而不是各个角色：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-414">You can also configure authorization policies, which encapsulate authorization rules, and then specify the policy instead of individual roles when applying the \[Authorize\] attribute:</span></span>

```csharp
[Authorize(Policy = "CanViewPrivateReport")]
public IActionResult ExecutiveSalaryReport()
{
    return View();
}
```

<span data-ttu-id="7b9e5-415">以这种方式使用策略可将受限制的操作与适用于该操作的特定角色或规则区分开。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-415">Using policies in this way, you can separate the kinds of actions being restricted from the specific roles or rules that apply to it.</span></span> <span data-ttu-id="7b9e5-416">之后创建需访问特定资源的新角色时，只需更新策略即可，而无需更新每个 \[Authorize\] 属性上的每个角色列表。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-416">Later, if you create a new role that needs to have access to certain resources, you can just update a policy, rather than updating every list of roles on every \[Authorize\] attribute.</span></span>

#### <a name="claims"></a><span data-ttu-id="7b9e5-417">声明</span><span class="sxs-lookup"><span data-stu-id="7b9e5-417">Claims</span></span>

<span data-ttu-id="7b9e5-418">声明是名称值对，代表已通过身份验证的用户的属性。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-418">Claims are name value pairs that represent properties of an authenticated user.</span></span> <span data-ttu-id="7b9e5-419">例如，可以将用户的员工编号存储为声明。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-419">For example, you might store users' employee number as a claim.</span></span> <span data-ttu-id="7b9e5-420">该声明随后可用作授权策略的一部分。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-420">Claims can then be used as part of authorization policies.</span></span> <span data-ttu-id="7b9e5-421">可以创建一个名为“EmployeeOnly”的策略，该策略要求存在名为“EmployeeNumber”的声明，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-421">You could create a policy called "EmployeeOnly" that requires the existence of a claim called "EmployeeNumber", as shown in this example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

<span data-ttu-id="7b9e5-422">此策略随后可与 \[Authorize\] 属性一起用于保护任何控制器和/或操作，如上所述。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-422">This policy could then be used with the \[Authorize\] attribute to protect any controller and/or action, as described above.</span></span>

#### <a name="securing-web-apis"></a><span data-ttu-id="7b9e5-423">保护 Web API</span><span class="sxs-lookup"><span data-stu-id="7b9e5-423">Securing web APIs</span></span>

<span data-ttu-id="7b9e5-424">大多数 Web API 应实现基于令牌的身份验证系统。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-424">Most web APIs should implement a token-based authentication system.</span></span> <span data-ttu-id="7b9e5-425">令牌身份验证无状态，且可缩放。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-425">Token authentication is stateless and designed to be scalable.</span></span> <span data-ttu-id="7b9e5-426">在基于令牌的身份验证系统中，客户端必须首先使用身份验证提供程序进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-426">In a token-based authentication system, the client must first authenticate with the authentication provider.</span></span> <span data-ttu-id="7b9e5-427">如果成功，则向该客户端颁发一个令牌，该令牌即是字符经过加密的有意义的字符串。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-427">If successful, the client is issued a token, which is simply a cryptographically meaningful string of characters.</span></span> <span data-ttu-id="7b9e5-428">最常见的令牌格式是 JSON Web 令牌（或称 JWT，通常发音为“jot”）。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-428">The most common format for tokens is JSON Web Token, or JWT (often pronounced "jot").</span></span> <span data-ttu-id="7b9e5-429">然后当客户端需要向 API 发出请求时，会将此令牌添加为请求的标题。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-429">When the client then needs to issue a request to an API, it adds this token as a header on the request.</span></span> <span data-ttu-id="7b9e5-430">继而，服务器会在完成请求之前验证请求标头中的令牌。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-430">The server then validates the token found in the request header before completing the request.</span></span> <span data-ttu-id="7b9e5-431">图 7-4 展示了此过程。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-431">Figure 7-4 demonstrates this process.</span></span>

![TokenAuth](./media/image7-4.png)

<span data-ttu-id="7b9e5-433">**图 7-4.**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-433">**Figure 7-4.**</span></span> <span data-ttu-id="7b9e5-434">Web API 基于令牌的身份验证。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-434">Token-based authentication for Web APIs.</span></span>

<span data-ttu-id="7b9e5-435">可以创建自己的身份验证服务，与 Azure AD 和 OAuth 集成，或使用开源工具（如 [IdentityServer](https://github.com/IdentityServer)）实现服务。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-435">You can create your own authentication service, integrate with Azure AD and OAuth, or implement a service using an open-source tool like [IdentityServer](https://github.com/IdentityServer).</span></span>

<span data-ttu-id="7b9e5-436">JWT 令牌可以嵌入有关用户的声明，该声明可在客户端或服务器上读取。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-436">JWT tokens can embed claims about the user, which can be read on the client or server.</span></span> <span data-ttu-id="7b9e5-437">可以使用 [jwt.io](https://jwt.io/) 等工具来查看 JWT 令牌的内容。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-437">You can use a tool like [jwt.io](https://jwt.io/) to view the contents of a JWT token.</span></span> <span data-ttu-id="7b9e5-438">请勿将敏感数据（例如密码或密钥）存储在 JTW 令牌中，因为它们的内容易于读取。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-438">Do not store sensitive data like passwords or keys in JTW tokens, since their contents are easily read.</span></span>

<span data-ttu-id="7b9e5-439">在 SPA 或 Blazor WebAssembly 应用程序中使用 JWT 令牌时，必须将令牌存储在客户端上的某个位置，然后将其添加到每个 API 调用。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-439">When using JWT tokens with SPA or Blazor WebAssembly applications, you must store the token somewhere on the client and then add it to every API call.</span></span> <span data-ttu-id="7b9e5-440">此活动通常以标头的形式完成，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-440">This activity is typically done as a header, as the following code demonstrates:</span></span>

```csharp
// AuthService.cs in BlazorAdmin project of eShopOnWeb
private async Task SetAuthorizationHeader()
{
    var token = await GetToken();
    _httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
}
```

<span data-ttu-id="7b9e5-441">调用上述方法后，使用 `_httpClient` 发出的请求将在请求的标头中嵌入令牌，使服务器端 API 可以对请求进行身份验证和授权。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-441">After calling the above method, requests made with the `_httpClient` will have the token embedded in the request's headers, allowing the server-side API to authenticate and authorize the request.</span></span>

#### <a name="custom-security"></a><span data-ttu-id="7b9e5-442">自定义安全</span><span class="sxs-lookup"><span data-stu-id="7b9e5-442">Custom Security</span></span>

<span data-ttu-id="7b9e5-443">要特别注意加密、用户成员身份或令牌生成系统的“自己回滚”实现。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-443">Be especially careful about "rolling your own" implementation of cryptography, user membership, or token generation system.</span></span> <span data-ttu-id="7b9e5-444">有许多商业和开源替代方案可供使用，几乎可以肯定，它们比自定义实现更具安全性。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-444">There are many commercial and open-source alternatives available, which will almost certainly have better security than a custom implementation.</span></span>

> ### <a name="references--security"></a><span data-ttu-id="7b9e5-445">参考 - 安全</span><span class="sxs-lookup"><span data-stu-id="7b9e5-445">References – Security</span></span>
>
> - <span data-ttu-id="7b9e5-446">**安全性文档概述**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-446">**Security Docs Overview**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/security/>
> - <span data-ttu-id="7b9e5-447">**在 ASP.NET Core 应用中强制执行 SSL**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-447">**Enforcing SSL in an ASP.NET Core App**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/security/enforcing-ssl>
> - <span data-ttu-id="7b9e5-448">**标识简介**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-448">**Introduction to Identity**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/security/authentication/identity>
> - <span data-ttu-id="7b9e5-449">**授权简介**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-449">**Introduction to Authorization**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/security/authorization/introduction>
> - <span data-ttu-id="7b9e5-450">**Azure 应用服务中 API 应用的身份验证和授权**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-450">**Authentication and Authorization for API Apps in Azure App Service**</span></span>\
>   <https://docs.microsoft.com/azure/app-service-api/app-service-api-authentication>
> - <span data-ttu-id="7b9e5-451">**标识服务器**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-451">**Identity Server**</span></span>\
>   <https://github.com/IdentityServer>

## <a name="client-communication"></a><span data-ttu-id="7b9e5-452">客户端通信</span><span class="sxs-lookup"><span data-stu-id="7b9e5-452">Client communication</span></span>

<span data-ttu-id="7b9e5-453">除了通过 Web API 提供页面和响应数据请求之外，ASP.NET Core 应用还能与已连接的客户端直接通信。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-453">In addition to serving pages and responding to requests for data via web APIs, ASP.NET Core apps can communicate directly with connected clients.</span></span> <span data-ttu-id="7b9e5-454">这种出站通信可以使用多种传输技术，其中最常见的是 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-454">This outbound communication can use a variety of transport technologies, the most common being WebSockets.</span></span> <span data-ttu-id="7b9e5-455">ASP.NET Core SignalR 是一个库，它简化了向应用程序添加某种实时服务器到客户端的通信功能的过程。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-455">ASP.NET Core SignalR is a library that makes it simple to add real-time server-to-client communication functionality to your applications.</span></span> <span data-ttu-id="7b9e5-456">SignalR 支持多种传输技术，包括 WebSocket，并从开发人员处抽象出许多实现细节。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-456">SignalR supports a variety of transport technologies, including WebSockets, and abstracts away many of the implementation details from the developer.</span></span>

<span data-ttu-id="7b9e5-457">无论是直接使用 WebSocket 还是使用其他技术，实时客户端通信在许多应用程序方案中都很有用。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-457">Real-time client communication, whether using WebSockets directly or other techniques, are useful in a variety of application scenarios.</span></span> <span data-ttu-id="7b9e5-458">一些示例包括：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-458">Some examples include:</span></span>

- <span data-ttu-id="7b9e5-459">实时聊天室应用程序</span><span class="sxs-lookup"><span data-stu-id="7b9e5-459">Live chat room applications</span></span>

- <span data-ttu-id="7b9e5-460">监视应用程序</span><span class="sxs-lookup"><span data-stu-id="7b9e5-460">Monitoring applications</span></span>

- <span data-ttu-id="7b9e5-461">作业进度更新</span><span class="sxs-lookup"><span data-stu-id="7b9e5-461">Job progress updates</span></span>

- <span data-ttu-id="7b9e5-462">通知</span><span class="sxs-lookup"><span data-stu-id="7b9e5-462">Notifications</span></span>

- <span data-ttu-id="7b9e5-463">交互式窗体应用程序</span><span class="sxs-lookup"><span data-stu-id="7b9e5-463">Interactive forms applications</span></span>

<span data-ttu-id="7b9e5-464">在应用程序中构建客户端通信时，通常有两个组件：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-464">When building client communication into your applications, there are typically two components:</span></span>

- <span data-ttu-id="7b9e5-465">服务器端连接管理器（SignalR Hub、WebSocketManager WebSocketHandler）</span><span class="sxs-lookup"><span data-stu-id="7b9e5-465">Server-side connection manager (SignalR Hub, WebSocketManager WebSocketHandler)</span></span>

- <span data-ttu-id="7b9e5-466">客户端库</span><span class="sxs-lookup"><span data-stu-id="7b9e5-466">Client-side library</span></span>

<span data-ttu-id="7b9e5-467">客户端不限于浏览器 - 移动应用、控制台应用和其他本地应用也可以使用 SignalR/WebSocket 进行通信。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-467">Clients aren't limited to browsers – mobile apps, console apps, and other native apps can also communicate using SignalR/WebSockets.</span></span> <span data-ttu-id="7b9e5-468">下面的简单程序是 WebSocketManager 示例应用程序的一部分，它向控制台回显发送给聊天应用程序的所有内容：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-468">The following simple program echoes all content sent to a chat application to the console, as part of a WebSocketManager sample application:</span></span>

```csharp
public class Program
{
    private static Connection _connection;
    public static void Main(string[] args)
    {
        StartConnectionAsync();
        _connection.On("receiveMessage", (arguments) =>
        {
            Console.WriteLine($"{arguments[0]} said: {arguments[1]}");
        });
        Console.ReadLine();
        StopConnectionAsync();
    }

    public static async Task StartConnectionAsync()
    {
        _connection = new Connection();
        await _connection.StartConnectionAsync("ws://localhost:65110/chat");
    }

    public static async Task StopConnectionAsync()
    {
        await _connection.StopConnectionAsync();
    }
}
```

<span data-ttu-id="7b9e5-469">请思考应用程序可通过哪些方式与客户端应用程序直接通信，以及实时通信是否会改善应用的用户体验。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-469">Consider ways in which your applications communicate directly with client applications, and consider whether real-time communication would improve your app's user experience.</span></span>

> ### <a name="references--client-communication"></a><span data-ttu-id="7b9e5-470">参考 - 客户端通信</span><span class="sxs-lookup"><span data-stu-id="7b9e5-470">References – Client Communication</span></span>
>
> - <span data-ttu-id="7b9e5-471">**ASP.NET Core SignalR**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-471">**ASP.NET Core SignalR**</span></span>\
>   <https://github.com/dotnet/aspnetcore/tree/main/src/SignalR>
> - <span data-ttu-id="7b9e5-472">**WebSocket 管理器**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-472">**WebSocket Manager**</span></span>\
>   <https://github.com/radu-matei/websocket-manager>

## <a name="domain-driven-design--should-you-apply-it"></a><span data-ttu-id="7b9e5-473">领域驱动设计 - 是否该使用？</span><span class="sxs-lookup"><span data-stu-id="7b9e5-473">Domain-driven design – Should you apply it?</span></span>

<span data-ttu-id="7b9e5-474">领域驱动设计 (DDD) 是一种敏捷方法，用于构建强调业务领域的软件。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-474">Domain-Driven Design (DDD) is an agile approach to building software that emphasizes focusing on the _business domain_.</span></span> <span data-ttu-id="7b9e5-475">它非常注重与业务领域专家的沟通和互动，这些专家可以让开发人员了解真实世界中的系统是如何工作的。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-475">It places a heavy emphasis on communication and interaction with business domain expert(s) who can relate to the developers how the real-world system works.</span></span> <span data-ttu-id="7b9e5-476">例如，如果你在构建处理股票交易的系统，那么领域专家可能是一位经验丰富的股票经纪人。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-476">For example, if you're building a system that handles stock trades, your domain expert might be an experienced stock broker.</span></span> <span data-ttu-id="7b9e5-477">DDD 旨在解决大型、复杂的业务问题，通常不适合小型简单的应用程序，因为在理解领域和为领域建模上所花费的投入是不必要的。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-477">DDD is designed to address large, complex business problems, and is often not appropriate for smaller, simpler applications, as the investment in understanding and modeling the domain is not worth it.</span></span>

<span data-ttu-id="7b9e5-478">采用 DDD 方法构建软件时，团队（包括非技术型利益干系人和参与者）应为问题空间开发一种通用语言。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-478">When building software following a DDD approach, your team (including non-technical stakeholders and contributors) should develop a _ubiquitous language_ for the problem space.</span></span> <span data-ttu-id="7b9e5-479">即，要进行建模的实际概念、软件同义词以及可能存在以维持该概念的任何结构（例如数据库表）应使用相同的术语。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-479">That is, the same terminology should be used for the real-world concept being modeled, the software equivalent, and any structures that might exist to persist the concept (for example, database tables).</span></span> <span data-ttu-id="7b9e5-480">因此，通用语言中所述的概念应该形成域模型的基础。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-480">Thus, the concepts described in the ubiquitous language should form the basis for your _domain model_.</span></span>

<span data-ttu-id="7b9e5-481">组成域模型的对象彼此交互，以表现系统行为。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-481">Your domain model comprises objects that interact with one another to represent the behavior of the system.</span></span> <span data-ttu-id="7b9e5-482">这些对象可分为以下几类：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-482">These objects may fall into the following categories:</span></span>

- <span data-ttu-id="7b9e5-483">[实体](https://deviq.com/entity/)，表示具有一系列标识的对象。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-483">[Entities](https://deviq.com/entity/), which represent objects with a thread of identity.</span></span> <span data-ttu-id="7b9e5-484">实体通常使用键永久存储，并且之后可使用该键进行检索。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-484">Entities are typically stored in persistence with a key by which they can later be retrieved.</span></span>

- <span data-ttu-id="7b9e5-485">[聚合](https://deviq.com/aggregate-pattern/)，表示应作为单元保留的一组对象。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-485">[Aggregates](https://deviq.com/aggregate-pattern/), which represent groups of objects that should be persisted as a unit.</span></span>

- <span data-ttu-id="7b9e5-486">[值对象](https://deviq.com/value-object/)，表示可以根据其属性值的总和进行比较的概念。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-486">[Value objects](https://deviq.com/value-object/), which represent concepts that can be compared on the basis of the sum of their property values.</span></span> <span data-ttu-id="7b9e5-487">例如，包含开始日期和结束日期的 DateRange。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-487">For example, DateRange consisting of a start and end date.</span></span>

- <span data-ttu-id="7b9e5-488">[领域事件](https://martinfowler.com/eaaDev/DomainEvent.html)，表示系统中发生的与系统其他部分相关的事件。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-488">[Domain events](https://martinfowler.com/eaaDev/DomainEvent.html), which represent things happening within the system that are of interest to other parts of the system.</span></span>

<span data-ttu-id="7b9e5-489">DDD 领域模型应在模型中包含复杂行为。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-489">A DDD domain model should encapsulate complex behavior within the model.</span></span> <span data-ttu-id="7b9e5-490">尤其是实体，它不应该仅仅是属性的集合。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-490">Entities, in particular, should not merely be collections of properties.</span></span> <span data-ttu-id="7b9e5-491">域模型缺少行为，并且仅表示系统状态时，就是所谓的[贫乏性模型](https://deviq.com/anemic-model/)，DDD 中应避免此类模型。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-491">When the domain model lacks behavior and merely represents the state of the system, it is said to be an [anemic model](https://deviq.com/anemic-model/), which is undesirable in DDD.</span></span>

<span data-ttu-id="7b9e5-492">除这些模型类型之外，DDD 通常还采用多种模式：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-492">In addition to these model types, DDD typically employs a variety of patterns:</span></span>

- <span data-ttu-id="7b9e5-493">[存储库](https://deviq.com/repository-pattern/)，用于提取持久保留详细信息。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-493">[Repository](https://deviq.com/repository-pattern/), for abstracting persistence details.</span></span>

- <span data-ttu-id="7b9e5-494">[工厂](https://en.wikipedia.org/wiki/Factory_method_pattern)，用于封装复杂对象创建。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-494">[Factory](https://en.wikipedia.org/wiki/Factory_method_pattern), for encapsulating complex object creation.</span></span>

- <span data-ttu-id="7b9e5-495">[服务](http://gorodinski.com/blog/2012/04/14/services-in-domain-driven-design-ddd/)，用于封装复杂行为和/或基础结构实现细节。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-495">[Services](http://gorodinski.com/blog/2012/04/14/services-in-domain-driven-design-ddd/), for encapsulating complex behavior and/or infrastructure implementation details.</span></span>

- <span data-ttu-id="7b9e5-496">[命令模式](https://en.wikipedia.org/wiki/Command_pattern)，用于分离发出的命令并执行命令本身。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-496">[Command](https://en.wikipedia.org/wiki/Command_pattern), for decoupling issuing commands and executing the command itself.</span></span>

- <span data-ttu-id="7b9e5-497">[规约模式](https://deviq.com/specification-pattern/)，用于封装查询细节。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-497">[Specification](https://deviq.com/specification-pattern/), for encapsulating query details.</span></span>

<span data-ttu-id="7b9e5-498">DDD 还建议使用之前介绍过的整洁架构，以实现松散耦合、封装和使用单元测试即可轻松验证的代码。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-498">DDD also recommends the use of the Clean Architecture discussed previously, allowing for loose coupling, encapsulation, and code that can easily be verified using unit tests.</span></span>

### <a name="when-should-you-apply-ddd"></a><span data-ttu-id="7b9e5-499">该何时使用 DDD</span><span class="sxs-lookup"><span data-stu-id="7b9e5-499">When should you apply DDD</span></span>

<span data-ttu-id="7b9e5-500">DDD 非常适合业务（不仅仅是技术）非常复杂的大型应用程序。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-500">DDD is well suited to large applications with significant business (not just technical) complexity.</span></span> <span data-ttu-id="7b9e5-501">这种应用程序需要借助领域专家的知识。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-501">The application should require the knowledge of domain experts.</span></span> <span data-ttu-id="7b9e5-502">领域模型本身应包含有某种意义的行为，应体现出业务规则和交互，而不仅仅是存储和检索数据存储中各种记录的当前状态。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-502">There should be significant behavior in the domain model itself, representing business rules and interactions beyond simply storing and retrieving the current state of various records from data stores.</span></span>

### <a name="when-shouldnt-you-apply-ddd"></a><span data-ttu-id="7b9e5-503">何时不该使用 DDD</span><span class="sxs-lookup"><span data-stu-id="7b9e5-503">When shouldn't you apply DDD</span></span>

<span data-ttu-id="7b9e5-504">DDD 需要在建模、体系结构和通信方面进行投资，这对于较小型的应用程序或本质只是 CRUD（创建/读取/更新/删除）的应用程序来说可能并不值得。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-504">DDD involves investments in modeling, architecture, and communication that may not be warranted for smaller applications or applications that are essentially just CRUD (create/read/update/delete).</span></span> <span data-ttu-id="7b9e5-505">如果选择采用 DDD 处理应用程序，但发现域中有一个没有任何行为的贫乏性模型，则可能需要重新考虑处理方法。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-505">If you choose to approach your application following DDD, but find that your domain has an anemic model with no behavior, you may need to rethink your approach.</span></span> <span data-ttu-id="7b9e5-506">可能是该应用程序不需要 DDD，也可能是你需要别人帮助你重构应用程序，将业务逻辑封装在域模型中，而不是数据库或用户界面中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-506">Either your application may not need DDD, or you may need assistance refactoring your application to encapsulate business logic in the domain model, rather than in your database or user interface.</span></span>

<span data-ttu-id="7b9e5-507">可以使用混合方法，只对应用程序中的事务性区域或比较复杂的区域使用 DDD，而不对应用程序中比较简单的 CRUD 或只读部分使用 DDD。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-507">A hybrid approach would be to only use DDD for the transactional or more complex areas of the application, but not for simpler CRUD or read-only portions of the application.</span></span> <span data-ttu-id="7b9e5-508">例如，如果是为显示报表或将仪表板数据可视化而查询数据，则无需具有聚合约束。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-508">For instance, you don't need the constraints of an Aggregate if you're querying data to display a report or to visualize data for a dashboard.</span></span> <span data-ttu-id="7b9e5-509">使用单独的、更简单的读取模型处理这类要求是完全可以接受的。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-509">It's perfectly acceptable to have a separate, simpler read model for such requirements.</span></span>

> ### <a name="references--domain-driven-design"></a><span data-ttu-id="7b9e5-510">参考 - 域驱动设计</span><span class="sxs-lookup"><span data-stu-id="7b9e5-510">References – Domain-Driven Design</span></span>
>
> - <span data-ttu-id="7b9e5-511">**简明 DDD（StackOverflow 答案）** </span><span class="sxs-lookup"><span data-stu-id="7b9e5-511">**DDD in Plain English (StackOverflow Answer)**</span></span>\
>   <https://stackoverflow.com/questions/1222392/can-someone-explain-domain-driven-design-ddd-in-plain-english-please/1222488#1222488>

## <a name="deployment"></a><span data-ttu-id="7b9e5-512">部署</span><span class="sxs-lookup"><span data-stu-id="7b9e5-512">Deployment</span></span>

<span data-ttu-id="7b9e5-513">无论在哪里托管 ASP.NET Core 应用，部署过程都包含以下几个步骤。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-513">There are a few steps involved in the process of deploying your ASP.NET Core application, regardless of where it will be hosted.</span></span> <span data-ttu-id="7b9e5-514">第一步，发布应用程序，这可以使用 `dotnet publish` CLI 命令来完成。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-514">The first step is to publish the application, which can be done using the `dotnet publish` CLI command.</span></span> <span data-ttu-id="7b9e5-515">此步骤将编译应用程序，并将运行应用程序所需的所有文件放到指定的文件夹中。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-515">This step will compile the application and place all of the files needed to run the application into a designated folder.</span></span> <span data-ttu-id="7b9e5-516">从 Visual Studio 部署时，系统将自动执行此步骤。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-516">When you deploy from Visual Studio, this step is performed for you automatically.</span></span> <span data-ttu-id="7b9e5-517">发布文件夹中包含应用程序的 .exe 和 .dll 文件及其依赖项。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-517">The publish folder contains .exe and .dll files for the application and its dependencies.</span></span> <span data-ttu-id="7b9e5-518">自包含应用程序中还包含一个 .NET 运行时版本。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-518">A self-contained application will also include a version of the .NET runtime.</span></span> <span data-ttu-id="7b9e5-519">ASP.NET Core 应用程序还将包含配置文件、静态客户端资产和 MVC 视图。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-519">ASP.NET Core applications will also include configuration files, static client assets, and MVC views.</span></span>

<span data-ttu-id="7b9e5-520">ASP.NET Core 应用程序是控制台应用程序，服务器启动时必须启动，应用程序（或服务器）崩溃时必须重新启动。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-520">ASP.NET Core applications are console applications that must be started when the server boots and restarted if the application (or server) crashes.</span></span> <span data-ttu-id="7b9e5-521">可以使用流程管理器自动执行此过程。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-521">A process manager can be used to automate this process.</span></span> <span data-ttu-id="7b9e5-522">适用于 ASP.NET Core 的最常见的进程管理器是 Linux 上的 Nginx 和 Apache，以及 Windows 上的 IIS 或 Windows Service。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-522">The most common process managers for ASP.NET Core are Nginx and Apache on Linux and IIS or Windows Service on Windows.</span></span>

<span data-ttu-id="7b9e5-523">除流程管理器之外，ASP.NET Core 应用程序可以使用反向代理服务器。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-523">In addition to a process manager, ASP.NET Core applications may use a reverse proxy server.</span></span> <span data-ttu-id="7b9e5-524">反向代理服务器接收到来自 Internet 的 HTTP 请求，并在进行一些初步处理后将这些请求转发到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-524">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="7b9e5-525">反向代理服务器为应用程序提供了一层安全性。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-525">Reverse proxy servers provide a layer of security for the application.</span></span> <span data-ttu-id="7b9e5-526">Kestrel 也不支持在同一端口上承载多个应用程序，因此不能将其用于主机头之类的技术以实现在同一端口和 IP 地址上承载多个应用程序。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-526">Kestrel also doesn't support hosting multiple applications on the same port, so techniques like host headers cannot be used with it to enable hosting multiple applications on the same port and IP address.</span></span>

![Kestrel 到 Internet](./media/image7-5.png)

<span data-ttu-id="7b9e5-528">**图 7-5**。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-528">**Figure 7-5**.</span></span> <span data-ttu-id="7b9e5-529">反向代理服务器背后托管在 Kestrel 中的 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7b9e5-529">ASP.NET hosted in Kestrel behind a reverse proxy server</span></span>

<span data-ttu-id="7b9e5-530">反向代理发挥作用的其他情况包括使用 SSL/HTTPS 保护多个应用程序。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-530">Another scenario in which a reverse proxy can be helpful is to secure multiple applications using SSL/HTTPS.</span></span> <span data-ttu-id="7b9e5-531">在这种情况下，只需要为反向代理服务器配置 SSL。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-531">In this case, only the reverse proxy would need to have SSL configured.</span></span> <span data-ttu-id="7b9e5-532">反向代理服务器和 Kestrel 之间的通信可以通过 HTTP 进行，如图 7-6 所示。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-532">Communication between the reverse proxy server and Kestrel could take place over HTTP, as shown in Figure 7-6.</span></span>

![HTTPS 保护的反向代理服务器后托管的 ASP.NET](./media/image7-6.png)

<span data-ttu-id="7b9e5-534">**图7-6**。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-534">**Figure 7-6**.</span></span> <span data-ttu-id="7b9e5-535">HTTPS 保护的反向代理服务器后托管的 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7b9e5-535">ASP.NET hosted behind an HTTPS-secured reverse proxy server</span></span>

<span data-ttu-id="7b9e5-536">可以将 ASP.NET Core 应用程序托管在 Docker 容器中，然后该应用程序即可本地托管或部署到 Azure 进行基于云的托管，这种方法正日益普及。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-536">An increasingly popular approach is to host your ASP.NET Core application in a Docker container, which then can be hosted locally or deployed to Azure for cloud-based hosting.</span></span> <span data-ttu-id="7b9e5-537">Docker 容器可以包含应用程序代码（在 Kestrel 上运行），并部署在反向代理服务器后，如上所述。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-537">The Docker container could contain your application code, running on Kestrel, and would be deployed behind a reverse proxy server, as shown above.</span></span>

<span data-ttu-id="7b9e5-538">如果在 Azure 上托管应用程序，则可以使用 Microsoft Azure 应用程序网关作为专用虚拟设备来提供多项服务。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-538">If you're hosting your application on Azure, you can use Microsoft Azure Application Gateway as a dedicated virtual appliance to provide several services.</span></span> <span data-ttu-id="7b9e5-539">除了充当单个应用程序的反向代理之外，应用程序网关还可以提供以下功能：</span><span class="sxs-lookup"><span data-stu-id="7b9e5-539">In addition to acting as a reverse proxy for individual applications, Application Gateway can also offer the following features:</span></span>

- <span data-ttu-id="7b9e5-540">HTTP 负载均衡</span><span class="sxs-lookup"><span data-stu-id="7b9e5-540">HTTP load balancing</span></span>

- <span data-ttu-id="7b9e5-541">SSL 卸载（仅到 Internet 的 SSL）</span><span class="sxs-lookup"><span data-stu-id="7b9e5-541">SSL offload (SSL only to Internet)</span></span>

- <span data-ttu-id="7b9e5-542">端到端 SSL</span><span class="sxs-lookup"><span data-stu-id="7b9e5-542">End to End SSL</span></span>

- <span data-ttu-id="7b9e5-543">多站点路由（在单个应用程序网关上整合最多 20 个站点）</span><span class="sxs-lookup"><span data-stu-id="7b9e5-543">Multi-site routing (consolidate up to 20 sites on a single Application Gateway)</span></span>

- <span data-ttu-id="7b9e5-544">Web 应用程序防火墙</span><span class="sxs-lookup"><span data-stu-id="7b9e5-544">Web application firewall</span></span>

- <span data-ttu-id="7b9e5-545">Websocket 支持</span><span class="sxs-lookup"><span data-stu-id="7b9e5-545">Websocket support</span></span>

- <span data-ttu-id="7b9e5-546">高级诊断</span><span class="sxs-lookup"><span data-stu-id="7b9e5-546">Advanced diagnostics</span></span>

<span data-ttu-id="7b9e5-547">请在[第 10 章](development-process-for-azure.md)中了解有关 Azure 部署选项的详细信息。</span><span class="sxs-lookup"><span data-stu-id="7b9e5-547">_Learn more about Azure deployment options in [Chapter 10](development-process-for-azure.md)._</span></span>

> ### <a name="references--deployment"></a><span data-ttu-id="7b9e5-548">参考 - 部署</span><span class="sxs-lookup"><span data-stu-id="7b9e5-548">References – Deployment</span></span>
>
> - <span data-ttu-id="7b9e5-549">**托管和部署概述**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-549">**Hosting and Deployment Overview**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/publishing/>
> - <span data-ttu-id="7b9e5-550">**何时结合使用 Kestrel 和反向代理**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-550">**When to use Kestrel with a reverse proxy**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy>
> - <span data-ttu-id="7b9e5-551">**在 Docker 中托管 ASP.NET Core 应用**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-551">**Host ASP.NET Core apps in Docker**</span></span>\
>   <https://docs.microsoft.com/aspnet/core/publishing/docker>
> - <span data-ttu-id="7b9e5-552">**Azure 应用程序网关简介**</span><span class="sxs-lookup"><span data-stu-id="7b9e5-552">**Introducing Azure Application Gateway**</span></span>\
>   <https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction>

>[!div class="step-by-step"]
><span data-ttu-id="7b9e5-553">[上一页](common-client-side-web-technologies.md)
>[下一页](work-with-data-in-asp-net-core-apps.md)</span><span class="sxs-lookup"><span data-stu-id="7b9e5-553">[Previous](common-client-side-web-technologies.md)
[Next](work-with-data-in-asp-net-core-apps.md)</span></span>
