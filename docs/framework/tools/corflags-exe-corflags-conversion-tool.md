---
title: CorFlags.exe（CorFlags 转换工具）
description: 了解 CorFlags.exe（CorFlags 转换工具）。 此工具可用于配置可移植可执行映像的标头的 CorFlags 部分。
ms.date: 03/30/2017
helpviewer_keywords:
- CorFlags conversion tool
- CorFlags.exe
- portable executable files, CorFlags section
ms.assetid: ef900f8f-71ca-4dde-9b8c-95ddb0d7d89c
ms.openlocfilehash: 4481e6718372fe7b58dbc05ab7cfe35e6d3047ce
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102258047"
---
# <a name="corflagsexe-corflags-conversion-tool"></a><span data-ttu-id="49935-104">CorFlags.exe（CorFlags 转换工具）</span><span class="sxs-lookup"><span data-stu-id="49935-104">CorFlags.exe (CorFlags Conversion Tool)</span></span>

<span data-ttu-id="49935-105">利用 CorFlags 转换工具，你配置可移植可执行映像标头的 CorFlags 部分。</span><span class="sxs-lookup"><span data-stu-id="49935-105">The CorFlags Conversion tool allows you to configure the CorFlags section of the header of a portable executable image.</span></span>  
  
 <span data-ttu-id="49935-106">此工具会自动随 Visual Studio 一起安装。</span><span class="sxs-lookup"><span data-stu-id="49935-106">This tool is automatically installed with Visual Studio.</span></span> <span data-ttu-id="49935-107">若要运行该工具，请[对开发人员使用命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)。</span><span class="sxs-lookup"><span data-stu-id="49935-107">To run the tool, use a [command-line shell for developers](/visualstudio/ide/reference/command-prompt-powershell).</span></span>  
  
 <span data-ttu-id="49935-108">在命令提示符处，键入以下内容：</span><span class="sxs-lookup"><span data-stu-id="49935-108">At the command prompt, type the following:</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="49935-109">语法</span><span class="sxs-lookup"><span data-stu-id="49935-109">Syntax</span></span>  
  
```console  
CorFlags.exe assembly [options]  
```  
  
## <a name="parameters"></a><span data-ttu-id="49935-110">参数</span><span class="sxs-lookup"><span data-stu-id="49935-110">Parameters</span></span>  
  
|<span data-ttu-id="49935-111">必选参数</span><span class="sxs-lookup"><span data-stu-id="49935-111">Required parameter</span></span>|<span data-ttu-id="49935-112">描述</span><span class="sxs-lookup"><span data-stu-id="49935-112">Description</span></span>|  
|------------------------|-----------------|  
|`assembly`|<span data-ttu-id="49935-113">要为其配置 CorFlags 的程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="49935-113">The name of the assembly for which to configure the CorFlags.</span></span>|  
  
|<span data-ttu-id="49935-114">选项</span><span class="sxs-lookup"><span data-stu-id="49935-114">Option</span></span>|<span data-ttu-id="49935-115">说明</span><span class="sxs-lookup"><span data-stu-id="49935-115">Description</span></span>|  
|:------------|-----------------|  
|`-32BIT[REQ]+`|<span data-ttu-id="49935-116">设置 32BITREQUIRED 标志。</span><span class="sxs-lookup"><span data-stu-id="49935-116">Sets the 32BITREQUIRED flag.</span></span>|  
|`-32BIT[REQ]-`|<span data-ttu-id="49935-117">清除 32BITREQUIRED 标志。</span><span class="sxs-lookup"><span data-stu-id="49935-117">Clears the 32BITREQUIRED flag.</span></span>|  
|`-32BITPREF+`|<span data-ttu-id="49935-118">设置 32BITPREFERRED 标志。</span><span class="sxs-lookup"><span data-stu-id="49935-118">Sets the 32BITPREFERRED flag.</span></span> <span data-ttu-id="49935-119">该应用程序甚至可以作为 32 位进程在 64 位平台上运行。</span><span class="sxs-lookup"><span data-stu-id="49935-119">The app runs as a 32-bit process even on 64-bit platforms.</span></span> <span data-ttu-id="49935-120">仅在 EXE 文件中设置此标志。</span><span class="sxs-lookup"><span data-stu-id="49935-120">Set this flag only on EXE files.</span></span> <span data-ttu-id="49935-121">如果在 DLL 上设置此标志，则 DLL 将无法在 64 位进程中加载，并引发 <xref:System.BadImageFormatException> 异常。</span><span class="sxs-lookup"><span data-stu-id="49935-121">If the flag is set on a DLL, the DLL fails to load in 64-bit processes, and a <xref:System.BadImageFormatException> exception is thrown.</span></span> <span data-ttu-id="49935-122">具有此标志的 EXE 文件可以加载到 64 位进程中。</span><span class="sxs-lookup"><span data-stu-id="49935-122">An EXE file with this flag can be loaded into a 64-bit process.</span></span><br /><br /> <span data-ttu-id="49935-123">.NET Framework 4.5 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="49935-123">New in the .NET Framework 4.5.</span></span>|  
|`-32BITPREF-`|<span data-ttu-id="49935-124">清除 32BITPREFERRED 标志。</span><span class="sxs-lookup"><span data-stu-id="49935-124">Clears the 32BITPREFERRED flag.</span></span><br /><br /> <span data-ttu-id="49935-125">.NET Framework 4.5 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="49935-125">New in the .NET Framework 4.5.</span></span>|  
|`-?`|<span data-ttu-id="49935-126">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="49935-126">Displays command syntax and options for the tool.</span></span>|  
|`-Force`|<span data-ttu-id="49935-127">强制执行更新，即使程序集具有强名称也是如此。</span><span class="sxs-lookup"><span data-stu-id="49935-127">Forces an update even if the assembly is strong-named.</span></span> <span data-ttu-id="49935-128">**重要提示：** 如果更新具有强名称的程序集，则必须在执行其代码之前再次对其签名。</span><span class="sxs-lookup"><span data-stu-id="49935-128">**Important:**  If you update a strong-named assembly, you must sign it again before executing its code.</span></span>|  
|`-help`|<span data-ttu-id="49935-129">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="49935-129">Displays command syntax and options for the tool.</span></span>|  
|`-ILONLY+`|<span data-ttu-id="49935-130">设置 ILONLY 标志。</span><span class="sxs-lookup"><span data-stu-id="49935-130">Sets the ILONLY flag.</span></span>|  
|`-ILONLY-`|<span data-ttu-id="49935-131">清除 ILONLY 标志。</span><span class="sxs-lookup"><span data-stu-id="49935-131">Clears the ILONLY flag.</span></span>|  
|`-nologo`|<span data-ttu-id="49935-132">取消显示 Microsoft 启动版权标志。</span><span class="sxs-lookup"><span data-stu-id="49935-132">Suppresses the Microsoft startup banner display.</span></span>|  
|`-RevertCLRHeader`|<span data-ttu-id="49935-133">将 CLR 标头版本还原到 2.0。</span><span class="sxs-lookup"><span data-stu-id="49935-133">Reverts the CLR header version to 2.0.</span></span>|  
|`-UpgradeCLRHeader`|<span data-ttu-id="49935-134">将 CLR 标头版本升级到 2.5。</span><span class="sxs-lookup"><span data-stu-id="49935-134">Upgrades the CLR header version to 2.5.</span></span> <span data-ttu-id="49935-135">**注意：** 程序集必须具有 2.5 版或更高版本的 CLR 标头才能在本机运行。</span><span class="sxs-lookup"><span data-stu-id="49935-135">**Note:**  Assemblies must have a CLR header version of 2.5 or greater to run natively.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="49935-136">备注</span><span class="sxs-lookup"><span data-stu-id="49935-136">Remarks</span></span>  

 <span data-ttu-id="49935-137">如果未指定任何选项，则 CorFlags 转换工具将显示指定程序集的标志。</span><span class="sxs-lookup"><span data-stu-id="49935-137">If no options are specified, the CorFlags Conversion tool displays the flags for the specified assembly.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="49935-138">请参阅</span><span class="sxs-lookup"><span data-stu-id="49935-138">See also</span></span>

- [<span data-ttu-id="49935-139">工具</span><span class="sxs-lookup"><span data-stu-id="49935-139">Tools</span></span>](index.md)
- [<span data-ttu-id="49935-140">64 位应用程序</span><span class="sxs-lookup"><span data-stu-id="49935-140">64-bit Applications</span></span>](../64-bit-apps.md)
- [<span data-ttu-id="49935-141">开发人员命令行 shell</span><span class="sxs-lookup"><span data-stu-id="49935-141">Developer command-line shells</span></span>](/visualstudio/ide/reference/command-prompt-powershell)
