---
title: 中断性变更：反序列化 char 需要单字符的字符串
description: 了解 .NET 5 中的中断性变更：反序列化为 char 目标时，System.Text.Json 在 JSON 中需要单字符字符串。
ms.date: 12/15/2020
ms.openlocfilehash: e901f8ee7e7521af948a3bcde5cf969640436f7f
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256330"
---
# <a name="systemtextjson-requires-single-char-string-to-deserialize-a-char"></a><span data-ttu-id="f966c-103">System.Text.Json 需要使用单字符字符串才能反序列化 char</span><span class="sxs-lookup"><span data-stu-id="f966c-103">System.Text.Json requires single-char string to deserialize a char</span></span>

<span data-ttu-id="f966c-104">若要成功使用 <xref:System.Text.Json> 反序列化 <xref:System.Char>，JSON 字符串必须包含单字符。</span><span class="sxs-lookup"><span data-stu-id="f966c-104">To successfully deserialize a <xref:System.Char> using <xref:System.Text.Json>, the JSON string must contain a single character.</span></span>

## <a name="change-description"></a><span data-ttu-id="f966c-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="f966c-105">Change description</span></span>

<span data-ttu-id="f966c-106">在 .NET 早期版本中，JSON 中的多 `char` 字符串会被成功地反序列化为 `char` 属性或字段。</span><span class="sxs-lookup"><span data-stu-id="f966c-106">In previous .NET versions, a multi-`char` string in the JSON is successfully deserialized to a `char` property or field.</span></span> <span data-ttu-id="f966c-107">只使用字符串的第一个 `char`，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="f966c-107">Only the first `char` of the string is used, as in the following example:</span></span>

```csharp
// .NET Core 3.0 and 3.1: Returns the first char 'a'.
// .NET 5 and later: Throws JsonException because payload has more than one char.
char deserializedChar = JsonSerializer.Deserialize<char>("\"abc\"");
```

<span data-ttu-id="f966c-108">在 .NET 5 及更高版本中，当反序列化目标为 `char` 时，除单 `char` 字符串以外的任何内容都会导致引发 <xref:System.Text.Json.JsonException>。</span><span class="sxs-lookup"><span data-stu-id="f966c-108">In .NET 5 and later, anything other than a single-`char` string causes a <xref:System.Text.Json.JsonException> to be thrown when the deserialization target is a `char`.</span></span> <span data-ttu-id="f966c-109">以下示例字符串在所有 .NET 版本中都已成功反序列化：</span><span class="sxs-lookup"><span data-stu-id="f966c-109">The following example string is successfully deserialized in all .NET versions:</span></span>

```csharp
// Correct usage.
char deserializedChar = JsonSerializer.Deserialize<char>("\"a\"");
```

## <a name="version-introduced"></a><span data-ttu-id="f966c-110">引入的版本</span><span class="sxs-lookup"><span data-stu-id="f966c-110">Version introduced</span></span>

<span data-ttu-id="f966c-111">5.0</span><span class="sxs-lookup"><span data-stu-id="f966c-111">5.0</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="f966c-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="f966c-112">Reason for change</span></span>

<span data-ttu-id="f966c-113">仅在提供的有效负载对目标类型有效时，反序列化分析才会成功。</span><span class="sxs-lookup"><span data-stu-id="f966c-113">Parsing for deserialization should only succeed when the provided payload is valid for the target type.</span></span> <span data-ttu-id="f966c-114">对于 `char` 类型，唯一有效的有效负载是单 `char` 字符串。</span><span class="sxs-lookup"><span data-stu-id="f966c-114">For a `char` type, the only valid payload is a single-`char` string.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="f966c-115">建议的操作</span><span class="sxs-lookup"><span data-stu-id="f966c-115">Recommended action</span></span>

<span data-ttu-id="f966c-116">将 JSON 反序列化为 `char` 目标时，请确保字符串包含单 `char`。</span><span class="sxs-lookup"><span data-stu-id="f966c-116">When you deserialize JSON into a `char` target, make sure the string consists of a single `char`.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="f966c-117">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="f966c-117">Affected APIs</span></span>

- <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=fullName>

<!--

### Affected APIs

- `Overload:System.Text.Json.JsonSerializer.Deserialize`

### Category

Serialization

-->
