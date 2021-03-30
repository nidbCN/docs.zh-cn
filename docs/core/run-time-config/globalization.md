---
title: 全球化配置设置
description: 了解对 .NET Core 应用的全球化方面进行配置的运行时设置。例如，如何分析日语日期。
ms.date: 05/18/2020
ms.topic: reference
ms.openlocfilehash: e8179a7e1421a3ff0ceacf07a2843c1b77af6143
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873544"
---
# <a name="run-time-configuration-options-for-globalization"></a><span data-ttu-id="6d5b6-103">用于全球化的运行时配置选项</span><span class="sxs-lookup"><span data-stu-id="6d5b6-103">Run-time configuration options for globalization</span></span>

## <a name="invariant-mode"></a><span data-ttu-id="6d5b6-104">固定模式</span><span class="sxs-lookup"><span data-stu-id="6d5b6-104">Invariant mode</span></span>

- <span data-ttu-id="6d5b6-105">确定 .NET Core 应用是否以全球化固定模式运行而无权访问特定区域性的数据和行为。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-105">Determines whether a .NET Core app runs in globalization-invariant mode without access to culture-specific data and behavior.</span></span>
- <span data-ttu-id="6d5b6-106">如果省略此设置，应用可运行并可访问区域性数据。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-106">If you omit this setting, the app runs with access to cultural data.</span></span> <span data-ttu-id="6d5b6-107">它等效于将值设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-107">This is equivalent to setting the value to `false`.</span></span>
- <span data-ttu-id="6d5b6-108">有关详细信息，请参阅 [.NET Core 全球化固定模式](https://github.com/dotnet/runtime/blob/main/docs/design/features/globalization-invariant-mode.md)。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-108">For more information, see [.NET Core globalization invariant mode](https://github.com/dotnet/runtime/blob/main/docs/design/features/globalization-invariant-mode.md).</span></span>

| | <span data-ttu-id="6d5b6-109">设置名</span><span class="sxs-lookup"><span data-stu-id="6d5b6-109">Setting name</span></span> | <span data-ttu-id="6d5b6-110">值</span><span class="sxs-lookup"><span data-stu-id="6d5b6-110">Values</span></span> |
| - | - | - |
| <span data-ttu-id="6d5b6-111">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="6d5b6-111">**runtimeconfig.json**</span></span> | `System.Globalization.Invariant` | <span data-ttu-id="6d5b6-112">`false` - 可访问区域性数据</span><span class="sxs-lookup"><span data-stu-id="6d5b6-112">`false` - access to cultural data</span></span><br/><span data-ttu-id="6d5b6-113">`true` - 以固定模式运行</span><span class="sxs-lookup"><span data-stu-id="6d5b6-113">`true` - run in invariant mode</span></span> |
| <span data-ttu-id="6d5b6-114">MSBuild 属性</span><span class="sxs-lookup"><span data-stu-id="6d5b6-114">**MSBuild property**</span></span> | `InvariantGlobalization` | <span data-ttu-id="6d5b6-115">`false` - 可访问区域性数据</span><span class="sxs-lookup"><span data-stu-id="6d5b6-115">`false` - access to cultural data</span></span><br/><span data-ttu-id="6d5b6-116">`true` - 以固定模式运行</span><span class="sxs-lookup"><span data-stu-id="6d5b6-116">`true` - run in invariant mode</span></span> |
| <span data-ttu-id="6d5b6-117">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="6d5b6-117">**Environment variable**</span></span> | `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT` | <span data-ttu-id="6d5b6-118">`0` - 可访问区域性数据</span><span class="sxs-lookup"><span data-stu-id="6d5b6-118">`0` - access to cultural data</span></span><br/><span data-ttu-id="6d5b6-119">`1` - 以固定模式运行</span><span class="sxs-lookup"><span data-stu-id="6d5b6-119">`1` - run in invariant mode</span></span> |

### <a name="examples"></a><span data-ttu-id="6d5b6-120">示例</span><span class="sxs-lookup"><span data-stu-id="6d5b6-120">Examples</span></span>

<span data-ttu-id="6d5b6-121">runtimeconfig.json 文件：</span><span class="sxs-lookup"><span data-stu-id="6d5b6-121">*runtimeconfig.json* file:</span></span>

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Globalization.Invariant": true
      }
   }
}
```

<span data-ttu-id="6d5b6-122">项目文件：</span><span class="sxs-lookup"><span data-stu-id="6d5b6-122">Project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <InvariantGlobalization>true</InvariantGlobalization>
  </PropertyGroup>

</Project>
```

## <a name="era-year-ranges"></a><span data-ttu-id="6d5b6-123">纪元年份范围</span><span class="sxs-lookup"><span data-stu-id="6d5b6-123">Era year ranges</span></span>

- <span data-ttu-id="6d5b6-124">确定是否放宽对支持多个纪元的日历的范围检查，或者超出纪元日期范围的日期是否引发 <xref:System.ArgumentOutOfRangeException>。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-124">Determines whether range checks for calendars that support multiple eras are relaxed or whether dates that overflow an era's date range throw an <xref:System.ArgumentOutOfRangeException>.</span></span>
- <span data-ttu-id="6d5b6-125">如果省略此设置，则会放宽范围检查。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-125">If you omit this setting, range checks are relaxed.</span></span> <span data-ttu-id="6d5b6-126">它等效于将值设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-126">This is equivalent to setting the value to `false`.</span></span>
- <span data-ttu-id="6d5b6-127">有关详细信息，请参阅[日历、纪元和日期范围：放宽的范围检查](../../standard/datetime/working-with-calendars.md#calendars-eras-and-date-ranges-relaxed-range-checks)。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-127">For more information, see [Calendars, eras, and date ranges: Relaxed range checks](../../standard/datetime/working-with-calendars.md#calendars-eras-and-date-ranges-relaxed-range-checks).</span></span>

| | <span data-ttu-id="6d5b6-128">设置名</span><span class="sxs-lookup"><span data-stu-id="6d5b6-128">Setting name</span></span> | <span data-ttu-id="6d5b6-129">值</span><span class="sxs-lookup"><span data-stu-id="6d5b6-129">Values</span></span> |
| - | - | - |
| <span data-ttu-id="6d5b6-130">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="6d5b6-130">**runtimeconfig.json**</span></span> | `Switch.System.Globalization.EnforceJapaneseEraYearRanges` | <span data-ttu-id="6d5b6-131">`false` - 放宽的范围检查</span><span class="sxs-lookup"><span data-stu-id="6d5b6-131">`false` - relaxed range checks</span></span><br/><span data-ttu-id="6d5b6-132">`true` - 超出范围导致异常</span><span class="sxs-lookup"><span data-stu-id="6d5b6-132">`true` - overflows cause an exception</span></span> |
| <span data-ttu-id="6d5b6-133">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="6d5b6-133">**Environment variable**</span></span> | <span data-ttu-id="6d5b6-134">不可用</span><span class="sxs-lookup"><span data-stu-id="6d5b6-134">N/A</span></span> | <span data-ttu-id="6d5b6-135">不可用</span><span class="sxs-lookup"><span data-stu-id="6d5b6-135">N/A</span></span> |

## <a name="japanese-date-parsing"></a><span data-ttu-id="6d5b6-136">日语日期分析</span><span class="sxs-lookup"><span data-stu-id="6d5b6-136">Japanese date parsing</span></span>

- <span data-ttu-id="6d5b6-137">确定是否成功分析包含“1”或“Gannen”作为年份的字符串，或是否仅支持“1”。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-137">Determines whether a string that contains either "1" or "Gannen" as the year parses successfully or whether only "1" is supported.</span></span>
- <span data-ttu-id="6d5b6-138">如果省略此设置，则成功分析包含“1”或“Gannen”作为年份的字符串。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-138">If you omit this setting, strings that contain either "1" or "Gannen" as the year parse successfully.</span></span> <span data-ttu-id="6d5b6-139">它等效于将值设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-139">This is equivalent to setting the value to `false`.</span></span>
- <span data-ttu-id="6d5b6-140">有关详细信息，请参阅[用多个纪元表示日历中的日期](../../standard/datetime/working-with-calendars.md#represent-dates-in-calendars-with-multiple-eras)。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-140">For more information, see [Represent dates in calendars with multiple eras](../../standard/datetime/working-with-calendars.md#represent-dates-in-calendars-with-multiple-eras).</span></span>

| | <span data-ttu-id="6d5b6-141">设置名</span><span class="sxs-lookup"><span data-stu-id="6d5b6-141">Setting name</span></span> | <span data-ttu-id="6d5b6-142">值</span><span class="sxs-lookup"><span data-stu-id="6d5b6-142">Values</span></span> |
| - | - | - |
| <span data-ttu-id="6d5b6-143">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="6d5b6-143">**runtimeconfig.json**</span></span> | `Switch.System.Globalization.EnforceLegacyJapaneseDateParsing` | <span data-ttu-id="6d5b6-144">`false` - 支持“Gannen”或“1”</span><span class="sxs-lookup"><span data-stu-id="6d5b6-144">`false` - "Gannen" or "1" is supported</span></span><br/><span data-ttu-id="6d5b6-145">`true` - 仅支持“1”</span><span class="sxs-lookup"><span data-stu-id="6d5b6-145">`true` - only "1" is supported</span></span> |
| <span data-ttu-id="6d5b6-146">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="6d5b6-146">**Environment variable**</span></span> | <span data-ttu-id="6d5b6-147">不可用</span><span class="sxs-lookup"><span data-stu-id="6d5b6-147">N/A</span></span> | <span data-ttu-id="6d5b6-148">不可用</span><span class="sxs-lookup"><span data-stu-id="6d5b6-148">N/A</span></span> |

## <a name="japanese-year-format"></a><span data-ttu-id="6d5b6-149">日语年份格式</span><span class="sxs-lookup"><span data-stu-id="6d5b6-149">Japanese year format</span></span>

- <span data-ttu-id="6d5b6-150">确定是将日本历时代的第一年的格式设置为“Gannen”还是设置为一个数字。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-150">Determines whether the first year of a Japanese calendar era is formatted as "Gannen" or as a number.</span></span>
- <span data-ttu-id="6d5b6-151">如果省略此设置，则第一年的格式设置为“Gannen”。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-151">If you omit this setting, the first year is formatted as "Gannen".</span></span> <span data-ttu-id="6d5b6-152">它等效于将值设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-152">This is equivalent to setting the value to `false`.</span></span>
- <span data-ttu-id="6d5b6-153">有关详细信息，请参阅[用多个纪元表示日历中的日期](../../standard/datetime/working-with-calendars.md#represent-dates-in-calendars-with-multiple-eras)。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-153">For more information, see [Represent dates in calendars with multiple eras](../../standard/datetime/working-with-calendars.md#represent-dates-in-calendars-with-multiple-eras).</span></span>

| | <span data-ttu-id="6d5b6-154">设置名</span><span class="sxs-lookup"><span data-stu-id="6d5b6-154">Setting name</span></span> | <span data-ttu-id="6d5b6-155">值</span><span class="sxs-lookup"><span data-stu-id="6d5b6-155">Values</span></span> |
| - | - | - |
| <span data-ttu-id="6d5b6-156">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="6d5b6-156">**runtimeconfig.json**</span></span> | `Switch.System.Globalization.FormatJapaneseFirstYearAsANumber` | <span data-ttu-id="6d5b6-157">`false` - 将格式设置为“Gannen”</span><span class="sxs-lookup"><span data-stu-id="6d5b6-157">`false` - format as "Gannen"</span></span><br/><span data-ttu-id="6d5b6-158">`true` - 将格式设置为数字</span><span class="sxs-lookup"><span data-stu-id="6d5b6-158">`true` - format as number</span></span> |
| <span data-ttu-id="6d5b6-159">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="6d5b6-159">**Environment variable**</span></span> | <span data-ttu-id="6d5b6-160">不可用</span><span class="sxs-lookup"><span data-stu-id="6d5b6-160">N/A</span></span> | <span data-ttu-id="6d5b6-161">不可用</span><span class="sxs-lookup"><span data-stu-id="6d5b6-161">N/A</span></span> |

## <a name="nls"></a><span data-ttu-id="6d5b6-162">NLS</span><span class="sxs-lookup"><span data-stu-id="6d5b6-162">NLS</span></span>

- <span data-ttu-id="6d5b6-163">确定 .NET 是否使用适用于 Windows 应用的 Unicode (ICU) 全球化 API 的区域语言支持 (NLS) 或国际组件。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-163">Determines whether .NET uses National Language Support (NLS) or International Components for Unicode (ICU) globalization APIs for Windows apps.</span></span> <span data-ttu-id="6d5b6-164">默认情况下，.NET 5.0 和更高版本在 Windows 10 2019 年 5 月更新和更高版本上使用 ICU 全球化 API。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-164">.NET 5.0 and later versions use ICU globalization APIs by default on Windows 10 May 2019 Update and later versions.</span></span>
- <span data-ttu-id="6d5b6-165">如果省略此设置，则默认情况下，.NET 使用 ICU 全球化 API。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-165">If you omit this setting, .NET uses ICU globalization APIs by default.</span></span> <span data-ttu-id="6d5b6-166">它等效于将值设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-166">This is equivalent to setting the value to `false`.</span></span>
- <span data-ttu-id="6d5b6-167">有关详细信息，请参阅[全球化 API 在 Windows 上使用 ICU 库](../compatibility/globalization/5.0/icu-globalization-api.md)。</span><span class="sxs-lookup"><span data-stu-id="6d5b6-167">For more information, see [Globalization APIs use ICU libraries on Windows](../compatibility/globalization/5.0/icu-globalization-api.md).</span></span>

| | <span data-ttu-id="6d5b6-168">设置名</span><span class="sxs-lookup"><span data-stu-id="6d5b6-168">Setting name</span></span> | <span data-ttu-id="6d5b6-169">值</span><span class="sxs-lookup"><span data-stu-id="6d5b6-169">Values</span></span> | <span data-ttu-id="6d5b6-170">已引入</span><span class="sxs-lookup"><span data-stu-id="6d5b6-170">Introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="6d5b6-171">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="6d5b6-171">**runtimeconfig.json**</span></span> | `System.Globalization.UseNls` | <span data-ttu-id="6d5b6-172">`false` - 使用 ICU 全球化 API</span><span class="sxs-lookup"><span data-stu-id="6d5b6-172">`false` - Use ICU globalization APIs</span></span><br/><span data-ttu-id="6d5b6-173">`true` - 使用 NLS 全球化 API</span><span class="sxs-lookup"><span data-stu-id="6d5b6-173">`true` - Use NLS globalization APIs</span></span> | <span data-ttu-id="6d5b6-174">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="6d5b6-174">.NET 5.0</span></span> |
| <span data-ttu-id="6d5b6-175">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="6d5b6-175">**Environment variable**</span></span> | `DOTNET_SYSTEM_GLOBALIZATION_USENLS` | <span data-ttu-id="6d5b6-176">`false` - 使用 ICU 全球化 API</span><span class="sxs-lookup"><span data-stu-id="6d5b6-176">`false` - Use ICU globalization APIs</span></span><br/><span data-ttu-id="6d5b6-177">`true` - 使用 NLS 全球化 API</span><span class="sxs-lookup"><span data-stu-id="6d5b6-177">`true` - Use NLS globalization APIs</span></span> | <span data-ttu-id="6d5b6-178">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="6d5b6-178">.NET 5.0</span></span> |
