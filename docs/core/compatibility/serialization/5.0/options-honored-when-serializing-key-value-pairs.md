---
title: 中断性变更：将 PropertyNamingPolicy、PropertyNameCaseInsensitive 和 Encoder 选项用于键值对
description: 了解 .NET 5 中的中断性变更：在序列化和反序列化键值对实例的 Key 和 Value 属性名称时，将使用 PropertyNamingPolicy、PropertyNameCaseInsensitive 和 Encoder 选项。
ms.date: 10/18/2020
ms.openlocfilehash: fe6298a677488574fdd7bdc7e887ed3b244ba8d6
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256317"
---
# <a name="propertynamingpolicy-propertynamecaseinsensitive-and-encoder-options-are-honored-when-serializing-and-deserializing-key-value-pairs"></a><span data-ttu-id="cd8f3-103">对键值对进行序列化和反序列化时，使用“PropertyNamingPolicy”、“PropertyNameCaseInsensitive”和“Encoder”选项</span><span class="sxs-lookup"><span data-stu-id="cd8f3-103">PropertyNamingPolicy, PropertyNameCaseInsensitive, and Encoder options are honored when serializing and deserializing key-value pairs</span></span>

<span data-ttu-id="cd8f3-104">现在，在序列化 <xref:System.Collections.Generic.KeyValuePair%602> 实例的 <xref:System.Collections.Generic.KeyValuePair%602.Key> 和 <xref:System.Collections.Generic.KeyValuePair%602.Value> 属性名称时，<xref:System.Text.Json.JsonSerializer> 将使用 <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> 和 <xref:System.Text.Json.JsonSerializerOptions.Encoder> 选项。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-104"><xref:System.Text.Json.JsonSerializer> now honors the <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> and <xref:System.Text.Json.JsonSerializerOptions.Encoder> options when serializing the <xref:System.Collections.Generic.KeyValuePair%602.Key> and <xref:System.Collections.Generic.KeyValuePair%602.Value> property names of a <xref:System.Collections.Generic.KeyValuePair%602> instance.</span></span> <span data-ttu-id="cd8f3-105">此外，在反序列化 <xref:System.Collections.Generic.KeyValuePair%602> 实例时，<xref:System.Text.Json.JsonSerializer> 将使用 <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> 和 <xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive> 选项。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-105">Additionally, <xref:System.Text.Json.JsonSerializer> honors the <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> and <xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive> options when deserializing <xref:System.Collections.Generic.KeyValuePair%602> instances.</span></span>

## <a name="change-description"></a><span data-ttu-id="cd8f3-106">更改描述</span><span class="sxs-lookup"><span data-stu-id="cd8f3-106">Change description</span></span>

### <a name="serialization"></a><span data-ttu-id="cd8f3-107">序列化</span><span class="sxs-lookup"><span data-stu-id="cd8f3-107">Serialization</span></span>

<span data-ttu-id="cd8f3-108">在 .NET Core 3.x 版本和 [System.Text.Json NuGet 包](https://www.nuget.org/packages/System.Text.Json)的 4.6.0 - 4.7.2 版本中，<xref:System.Collections.Generic.KeyValuePair%602> 实例的属性始终准确序列化为“Key”和“Value”，而不考虑任何 <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy?displayProperty=nameWithType> 和 <xref:System.Text.Json.JsonSerializerOptions.Encoder?displayProperty=nameWithType> 选项。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-108">In .NET Core 3.x versions and in the 4.6.0-4.7.2 versions of the [System.Text.Json NuGet package](https://www.nuget.org/packages/System.Text.Json), the properties of <xref:System.Collections.Generic.KeyValuePair%602> instances are always serialized as "Key" and "Value" exactly, regardless of any <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy?displayProperty=nameWithType> and <xref:System.Text.Json.JsonSerializerOptions.Encoder?displayProperty=nameWithType> options.</span></span> <span data-ttu-id="cd8f3-109">下面的代码示例演示在序列化后 <xref:System.Collections.Generic.KeyValuePair%602.Key> 和 <xref:System.Collections.Generic.KeyValuePair%602.Value> 属性如何不采用 camel 大小写，即使指定的属性命名策略要求这样做也是如此。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-109">The following code example shows how the <xref:System.Collections.Generic.KeyValuePair%602.Key> and <xref:System.Collections.Generic.KeyValuePair%602.Value> properties are *not* camel-cased after serialization, even though the specified property-naming policy dictates so.</span></span>

```csharp
var options = new JsonSerializerOptions { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };
KeyValuePair<int, int> kvp = KeyValuePair.Create(1, 1);
Console.WriteLine(JsonSerializer.Serialize(kvp, options));
// Expected: {"key":1,"value":1}
// Actual: {"Key":1,"Value":1}
```

<span data-ttu-id="cd8f3-110">从 .NET 5 开始，序列化 <xref:System.Collections.Generic.KeyValuePair%602> 实例时，将使用 <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> 和 <xref:System.Text.Json.JsonSerializerOptions.Encoder> 选项。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-110">Starting in .NET 5, the <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> and <xref:System.Text.Json.JsonSerializerOptions.Encoder> options are honored when serializing <xref:System.Collections.Generic.KeyValuePair%602> instances.</span></span> <span data-ttu-id="cd8f3-111">下面的代码示例演示在序列化后 <xref:System.Collections.Generic.KeyValuePair%602.Key> 和 <xref:System.Collections.Generic.KeyValuePair%602.Value> 属性如何按照指定的属性命名策略采用 camel 大小写。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-111">The following code example shows how the <xref:System.Collections.Generic.KeyValuePair%602.Key> and <xref:System.Collections.Generic.KeyValuePair%602.Value> properties are camel-cased after serialization, in accordance with the specified property-naming policy.</span></span>

```csharp
var options = new JsonSerializerOptions { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };
KeyValuePair<int, int> kvp = KeyValuePair.Create(1, 1);
Console.WriteLine(JsonSerializer.Serialize(kvp, options));
// {"key":1,"value":1}
```

### <a name="deserialization"></a><span data-ttu-id="cd8f3-112">反序列化</span><span class="sxs-lookup"><span data-stu-id="cd8f3-112">Deserialization</span></span>

<span data-ttu-id="cd8f3-113">在 .NET Core 3.x 版本和 [System.Text.Json NuGet 包](https://www.nuget.org/packages/System.Text.Json)的 4.7.x 版本中，如果 JSON 属性名称不精确地表示为 `Key` 和 `Value`（例如，它们不以大写字母开头），则会引发 <xref:System.Text.Json.JsonException>。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-113">In .NET Core 3.x versions and in the 4.7.x versions of the [System.Text.Json NuGet package](https://www.nuget.org/packages/System.Text.Json), a <xref:System.Text.Json.JsonException> is thrown when the JSON property names are not precisely `Key` and `Value`, for example, if they don't start with an uppercase letter.</span></span> <span data-ttu-id="cd8f3-114">即使指定的属性命名策略明确允许该操作，也会引发该异常。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-114">The exception is thrown even if a specified property-naming policy expressly permits it.</span></span>

```csharp
var options = new JsonSerializerOptions { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };
string json = @"{""key"":1,""value"":1}";
// Throws JsonException.
JsonSerializer.Deserialize<KeyValuePair<int, int>>(json, options);
```

<span data-ttu-id="cd8f3-115">从 .NET 5 开始，使用 <xref:System.Text.Json.JsonSerializer> 进行反序列化时，将使用 <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> 和 <xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive> 选项。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-115">Starting in .NET 5, the <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> and <xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive> options are honored when deserializing using <xref:System.Text.Json.JsonSerializer>.</span></span> <span data-ttu-id="cd8f3-116">例如，下面的代码片段演示了小写的 <xref:System.Collections.Generic.KeyValuePair%602.Key> 和 <xref:System.Collections.Generic.KeyValuePair%602.Value> 属性名称的成功反序列化，因为指定的属性命名策略允许这样做。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-116">For example, the following code snippet shows successful deserialization of lowercased <xref:System.Collections.Generic.KeyValuePair%602.Key> and <xref:System.Collections.Generic.KeyValuePair%602.Value> property names because the specified property-naming policy permits it.</span></span>

```csharp
var options = new JsonSerializerOptions { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };
string json = @"{""key"":1,""value"":1}";

KeyValuePair<int, int> kvp = JsonSerializer.Deserialize<KeyValuePair<int, int>>(json);
Console.WriteLine(kvp.Key); // 1
Console.WriteLine(kvp.Value); // 1
```

<span data-ttu-id="cd8f3-117">为了适应在以前版本中序列化的有效负载，“Key”和“Value”采用特殊大小写，以在反序列化时匹配。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-117">To accommodate payloads that were serialized with previous versions, "Key" and "Value" are special-cased to match when deserializing.</span></span> <span data-ttu-id="cd8f3-118">即使 <xref:System.Collections.Generic.KeyValuePair%602.Key> 和 <xref:System.Collections.Generic.KeyValuePair%602.Value> 属性名称未根据以下代码示例中的 <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> 选项采用 camel 大小写，它们也会成功进行反序列化。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-118">Even though the <xref:System.Collections.Generic.KeyValuePair%602.Key> and <xref:System.Collections.Generic.KeyValuePair%602.Value> property names aren't camel-cased according to the in <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> option in the following code example, they deserialize successfully.</span></span>

```csharp
var options = new JsonSerializerOptions { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };
string json = @"{""Key"":1,""Value"":1}";

KeyValuePair<int, int> kvp = JsonSerializer.Deserialize<KeyValuePair<int, int>>(json);
Console.WriteLine(kvp.Key); // 1
Console.WriteLine(kvp.Value); // 1
```

## <a name="version-introduced"></a><span data-ttu-id="cd8f3-119">引入的版本</span><span class="sxs-lookup"><span data-stu-id="cd8f3-119">Version introduced</span></span>

<span data-ttu-id="cd8f3-120">5.0</span><span class="sxs-lookup"><span data-stu-id="cd8f3-120">5.0</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="cd8f3-121">更改原因</span><span class="sxs-lookup"><span data-stu-id="cd8f3-121">Reason for change</span></span>

<span data-ttu-id="cd8f3-122">大量客户反馈表明应使用 <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy>。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-122">Substantial customer feedback indicated that the <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy> should be honored.</span></span> <span data-ttu-id="cd8f3-123">出于完整性考虑，还使用了 <xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive> 和 <xref:System.Text.Json.JsonSerializerOptions.Encoder> 选项，使 <xref:System.Collections.Generic.KeyValuePair%602> 实例与任何其他普通旧 CLR 对象 (POCO) 得到相同的处理。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-123">For completeness, the <xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive> and <xref:System.Text.Json.JsonSerializerOptions.Encoder> options are also honored, so that <xref:System.Collections.Generic.KeyValuePair%602> instances are treated the same as any other plain old CLR object (POCO).</span></span>

## <a name="recommended-action"></a><span data-ttu-id="cd8f3-124">建议的操作</span><span class="sxs-lookup"><span data-stu-id="cd8f3-124">Recommended action</span></span>

<span data-ttu-id="cd8f3-125">如果此更改对你造成干扰，则可以使用[自定义转换器](../../../../standard/serialization/system-text-json-converters-how-to.md)来实现所需语义。</span><span class="sxs-lookup"><span data-stu-id="cd8f3-125">If this change is disruptive to you, you can use a [custom converter](../../../../standard/serialization/system-text-json-converters-how-to.md) that implements the desired semantics.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="cd8f3-126">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="cd8f3-126">Affected APIs</span></span>

- <xref:System.Text.Json.JsonSerializer.Serialize%2A?displayProperty=fullName>
- <xref:System.Text.Json.JsonSerializer.SerializeToUtf8Bytes%2A?displayProperty=fullName>
- <xref:System.Text.Json.JsonSerializer.SerializeAsync%2A?displayProperty=fullName>
- <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=fullName>
- <xref:System.Text.Json.JsonSerializer.DeserializeAsync%2A?displayProperty=fullName>

<!--

### Affected APIs

- `Overload:System.Text.Json.JsonSerializer.Serialize`
- `Overload:System.Text.Json.JsonSerializer.SerializeAsync`
- `Overload:System.Text.Json.JsonSerializer.SerializeToUtf8Bytes`
- `Overload:System.Text.Json.JsonSerializer.Deserialize`
- `Overload:System.Text.Json.JsonSerializer.DeserializeAsync`

### Category

Serialization

-->
