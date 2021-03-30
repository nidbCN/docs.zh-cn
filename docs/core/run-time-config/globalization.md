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
# <a name="run-time-configuration-options-for-globalization"></a>用于全球化的运行时配置选项

## <a name="invariant-mode"></a>固定模式

- 确定 .NET Core 应用是否以全球化固定模式运行而无权访问特定区域性的数据和行为。
- 如果省略此设置，应用可运行并可访问区域性数据。 它等效于将值设置为 `false`。
- 有关详细信息，请参阅 [.NET Core 全球化固定模式](https://github.com/dotnet/runtime/blob/main/docs/design/features/globalization-invariant-mode.md)。

| | 设置名 | 值 |
| - | - | - |
| **runtimeconfig.json** | `System.Globalization.Invariant` | `false` - 可访问区域性数据<br/>`true` - 以固定模式运行 |
| MSBuild 属性 | `InvariantGlobalization` | `false` - 可访问区域性数据<br/>`true` - 以固定模式运行 |
| **环境变量** | `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT` | `0` - 可访问区域性数据<br/>`1` - 以固定模式运行 |

### <a name="examples"></a>示例

runtimeconfig.json 文件：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Globalization.Invariant": true
      }
   }
}
```

项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <InvariantGlobalization>true</InvariantGlobalization>
  </PropertyGroup>

</Project>
```

## <a name="era-year-ranges"></a>纪元年份范围

- 确定是否放宽对支持多个纪元的日历的范围检查，或者超出纪元日期范围的日期是否引发 <xref:System.ArgumentOutOfRangeException>。
- 如果省略此设置，则会放宽范围检查。 它等效于将值设置为 `false`。
- 有关详细信息，请参阅[日历、纪元和日期范围：放宽的范围检查](../../standard/datetime/working-with-calendars.md#calendars-eras-and-date-ranges-relaxed-range-checks)。

| | 设置名 | 值 |
| - | - | - |
| **runtimeconfig.json** | `Switch.System.Globalization.EnforceJapaneseEraYearRanges` | `false` - 放宽的范围检查<br/>`true` - 超出范围导致异常 |
| **环境变量** | 不可用 | 不可用 |

## <a name="japanese-date-parsing"></a>日语日期分析

- 确定是否成功分析包含“1”或“Gannen”作为年份的字符串，或是否仅支持“1”。
- 如果省略此设置，则成功分析包含“1”或“Gannen”作为年份的字符串。 它等效于将值设置为 `false`。
- 有关详细信息，请参阅[用多个纪元表示日历中的日期](../../standard/datetime/working-with-calendars.md#represent-dates-in-calendars-with-multiple-eras)。

| | 设置名 | 值 |
| - | - | - |
| **runtimeconfig.json** | `Switch.System.Globalization.EnforceLegacyJapaneseDateParsing` | `false` - 支持“Gannen”或“1”<br/>`true` - 仅支持“1” |
| **环境变量** | 不可用 | 不可用 |

## <a name="japanese-year-format"></a>日语年份格式

- 确定是将日本历时代的第一年的格式设置为“Gannen”还是设置为一个数字。
- 如果省略此设置，则第一年的格式设置为“Gannen”。 它等效于将值设置为 `false`。
- 有关详细信息，请参阅[用多个纪元表示日历中的日期](../../standard/datetime/working-with-calendars.md#represent-dates-in-calendars-with-multiple-eras)。

| | 设置名 | 值 |
| - | - | - |
| **runtimeconfig.json** | `Switch.System.Globalization.FormatJapaneseFirstYearAsANumber` | `false` - 将格式设置为“Gannen”<br/>`true` - 将格式设置为数字 |
| **环境变量** | 不可用 | 不可用 |

## <a name="nls"></a>NLS

- 确定 .NET 是否使用适用于 Windows 应用的 Unicode (ICU) 全球化 API 的区域语言支持 (NLS) 或国际组件。 默认情况下，.NET 5.0 和更高版本在 Windows 10 2019 年 5 月更新和更高版本上使用 ICU 全球化 API。
- 如果省略此设置，则默认情况下，.NET 使用 ICU 全球化 API。 它等效于将值设置为 `false`。
- 有关详细信息，请参阅[全球化 API 在 Windows 上使用 ICU 库](../compatibility/globalization/5.0/icu-globalization-api.md)。

| | 设置名 | 值 | 已引入 |
| - | - | - | - |
| **runtimeconfig.json** | `System.Globalization.UseNls` | `false` - 使用 ICU 全球化 API<br/>`true` - 使用 NLS 全球化 API | .NET 5.0 |
| **环境变量** | `DOTNET_SYSTEM_GLOBALIZATION_USENLS` | `false` - 使用 ICU 全球化 API<br/>`true` - 使用 NLS 全球化 API | .NET 5.0 |
