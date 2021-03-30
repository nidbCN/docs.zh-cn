---
title: 编译配置设置
description: 了解用于为 .NET Core 应用配置 JIT 编译器工作原理的运行时设置。
ms.date: 11/27/2019
ms.topic: reference
ms.openlocfilehash: 1badb063ea6fd7504636d431fbdc7927129239d2
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873583"
---
# <a name="run-time-configuration-options-for-compilation"></a>用于编译的运行时配置选项

## <a name="tiered-compilation"></a>分层编译

- 配置实时 (JIT) 编译器是否使用[分层编译](../whats-new/dotnet-core-3-0.md#tiered-compilation)。 分层编译将方法转换到两个层级：
  - 第一层可以更快速地生成代码（[快速 JIT](#quick-jit)）或加载预编译的代码 ([ReadyToRun](#readytorun))。
  - 第二层在后台生成优化的代码（“优化 JIT”）。
- 在 NET Core 3.0 及更高版本中，默认情况下已启用分层编译。
- 在 NET Core 2.1 和 2.2 中，默认情况下已禁用分层编译。
- 有关详细信息，请参阅[分层编译指南](https://github.com/dotnet/runtime/blob/main/docs/design/features/tiered-compilation.md)。

| | 设置名 | 值 |
| - | - | - |
| **runtimeconfig.json** | `System.Runtime.TieredCompilation` | `true` - 启用<br/>`false` - 禁用 |
| MSBuild 属性 | `TieredCompilation` | `true` - 启用<br/>`false` - 禁用 |
| **环境变量** | `COMPlus_TieredCompilation` | `1` - 启用<br/>`0` - 禁用 |

### <a name="examples"></a>示例

runtimeconfig.json 文件：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Runtime.TieredCompilation": false
      }
   }
}
```

项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TieredCompilation>false</TieredCompilation>
  </PropertyGroup>

</Project>
```

## <a name="quick-jit"></a>快速 JIT

- 配置 JIT 编译器是否使用快速 JIT。 对于不包含循环且不可使用预编译代码的方法，快速 JIT 可以更快完成编译，但不会进行优化。
- 启用快速 JIT 会缩短启动时间，但可能会生成性能下降的代码。 例如，代码可能会使用更多堆栈空间、分配更多内存并以更慢的速度运行。
- 如果禁用了快速 JIT 但启用了[分层编译](#tiered-compilation)，则只有预编译的代码参与分层编译。 如果未使用 [ReadyToRun](#readytorun) 预编译方法，则 JIT 行为与禁用[分层编译](#tiered-compilation)时相同。
- 在 NET Core 3.0 及更高版本中，默认启用快速 JIT。
- 在 NET Core 2.1 和 2.2 中，默认禁用快速 JIT。

| | 设置名 | 值 |
| - | - | - |
| **runtimeconfig.json** | `System.Runtime.TieredCompilation.QuickJit` | `true` - 启用<br/>`false` - 禁用 |
| MSBuild 属性 | `TieredCompilationQuickJit` | `true` - 启用<br/>`false` - 禁用 |
| **环境变量** | `COMPlus_TC_QuickJit` | `1` - 启用<br/>`0` - 禁用 |

### <a name="examples"></a>示例

runtimeconfig.json 文件：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Runtime.TieredCompilation.QuickJit": false
      }
   }
}
```

项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TieredCompilationQuickJit>false</TieredCompilationQuickJit>
  </PropertyGroup>

</Project>
```

## <a name="quick-jit-for-loops"></a>适用于循环的快速 JIT

- 配置 JIT 编译器是否对包含循环的方法使用快速 JIT。
- 启用适用于循环的快速 JIT 可以提高启动性能。 不过，在优化程度较低的代码中，长时间运行的循环可能会停滞较长时间。
- 如果禁用[快速 JIT](#quick-jit)，则此设置不起作用。
- 如果省略此设置，则不会对包含循环的方法使用“快速 JIT”。 它等效于将值设置为 `false`。

| | 设置名 | 值 |
| - | - | - |
| **runtimeconfig.json** | `System.Runtime.TieredCompilation.QuickJitForLoops` | `false` - 禁用<br/>`true` - 启用 |
| MSBuild 属性 | `TieredCompilationQuickJitForLoops` | `false` - 禁用<br/>`true` - 启用 |
| **环境变量** | `COMPlus_TC_QuickJitForLoops` | `0` - 禁用<br/>`1` - 启用 |

### <a name="examples"></a>示例

runtimeconfig.json 文件：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Runtime.TieredCompilation.QuickJitForLoops": false
      }
   }
}
```

项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TieredCompilationQuickJitForLoops>true</TieredCompilationQuickJitForLoops>
  </PropertyGroup>

</Project>
```

## <a name="readytorun"></a>ReadyToRun

- 配置 .NET Core 运行时是否要为具有可用 ReadyToRun 数据的映像使用预编译代码。 如果禁用此选项，会强制运行时对框架代码进行 JIT 编译。
- 有关详细信息，请参阅[准备好运行](../deploying/ready-to-run.md)。
- 如果省略此设置，则 .NET 将使用 ReadyToRun 数据（如果可用）。 它等效于将值设置为 `1`。

| | 设置名 | 值 |
| - | - | - |
| **环境变量** | `COMPlus_ReadyToRun` | `1` - 启用<br/>`0` - 禁用 |
