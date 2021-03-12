---
title: 编译器错误和警告
description: F# 编译器将发出的错误和警告的说明和解决方案
ms.date: 12/21/2019
ms.openlocfilehash: 9769ddee989f0774cfae8842e60dbd3fd2065f9c
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103190172"
---
# <a name="f-compiler-messages"></a><span data-ttu-id="359bf-103">F# 编译器消息</span><span class="sxs-lookup"><span data-stu-id="359bf-103">F# compiler messages</span></span>

<span data-ttu-id="359bf-104">此部分详细介绍 F# 编译器将针对某些构造发出的编译器错误和警告。</span><span class="sxs-lookup"><span data-stu-id="359bf-104">This section details compiler errors and warnings that the F# compiler will emit for certain constructs.</span></span> <span data-ttu-id="359bf-105">可以通过以下方式更改默认错误集：</span><span class="sxs-lookup"><span data-stu-id="359bf-105">The default sets of errors can be changed by:</span></span>

- <span data-ttu-id="359bf-106">使用 `-warnaserror+` 编译器选项，将特定警告视为错误，</span><span class="sxs-lookup"><span data-stu-id="359bf-106">Treating specific warnings as if they were errors by using the `-warnaserror+` compiler option,</span></span>

- <span data-ttu-id="359bf-107">使用 `-nowarn` 编译器选项，忽略特定警告</span><span class="sxs-lookup"><span data-stu-id="359bf-107">Ignoring specific warnings by using the `-nowarn` compiler option</span></span>

<span data-ttu-id="359bf-108">如果此部分中尚未记录特定警告或错误：</span><span class="sxs-lookup"><span data-stu-id="359bf-108">If a particular warning or error is not yet recorded in this section:</span></span>

- <span data-ttu-id="359bf-109">请转到此页末尾，然后发送包含错误号或文本的反馈，或者</span><span class="sxs-lookup"><span data-stu-id="359bf-109">Go to the end of this page and send feedback that includes the number or text of the error, or</span></span>
- <span data-ttu-id="359bf-110">按照 [create-new-fsharp-compiler-message.fsx](https://github.com/dotnet/docs/blob/main/docs/fsharp/language-reference/compiler-messages/util/create-new-fsharp-compiler-message.fsx) 中的说明进行操作，并打开此存储库的拉取请求，自行添加。</span><span class="sxs-lookup"><span data-stu-id="359bf-110">Add it yourself by following the instructions in [create-new-fsharp-compiler-message.fsx](https://github.com/dotnet/docs/blob/main/docs/fsharp/language-reference/compiler-messages/util/create-new-fsharp-compiler-message.fsx) and opening a pull request for this repository.</span></span>

## <a name="see-also"></a><span data-ttu-id="359bf-111">另请参阅</span><span class="sxs-lookup"><span data-stu-id="359bf-111">See also</span></span>

- [<span data-ttu-id="359bf-112">F# 编译器选项</span><span class="sxs-lookup"><span data-stu-id="359bf-112">F# Compiler Options</span></span>](../compiler-options.md)
