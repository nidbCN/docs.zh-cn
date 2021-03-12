---
title: 如何：通过 MMC 管理单元查看证书
description: 安全 WCF 客户端或服务可以使用证书作为凭据。 了解可以使用 MMC 插件检查的证书存储类型。
ms.date: 02/25/2019
helpviewer_keywords:
- certificates [WCF], viewing with the MMC snap-in
ms.assetid: 2b8782aa-ebb4-4ee7-974b-90299e356dc5
ms.openlocfilehash: 788161db2e38dc942b0c638128f8599b0436c8fd
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604575"
---
# <a name="how-to-view-certificates-with-the-mmc-snap-in"></a><span data-ttu-id="2c4c4-104">如何：通过 MMC 管理单元查看证书</span><span class="sxs-lookup"><span data-stu-id="2c4c4-104">How to: View certificates with the MMC snap-in</span></span>

<span data-ttu-id="2c4c4-105">创建安全客户端或服务时，可以使用 [证书](working-with-certificates.md) 作为凭据。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-105">When you create a secure client or service, you can use a [certificate](working-with-certificates.md) as the credential.</span></span> <span data-ttu-id="2c4c4-106">例如，通用类型的凭据是使用方法创建的 x.509 证书 <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-106">For example, a common type of credential is the X.509 certificate, which you create with the <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A?displayProperty=nameWithType> method.</span></span>

<span data-ttu-id="2c4c4-107">有三种不同类型的证书存储，你可以通过 Microsoft 管理控制台 (MMC) 在 Windows 系统上进行检查：</span><span class="sxs-lookup"><span data-stu-id="2c4c4-107">There are three different types of certificate stores that you can examine with the Microsoft Management Console (MMC) on Windows systems:</span></span>

- <span data-ttu-id="2c4c4-108">本地计算机：存储区是设备的本地存储区，并且是设备上所有用户的全局存储区。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-108">Local computer: The store is local to the device and global to all users on the device.</span></span>

- <span data-ttu-id="2c4c4-109">当前用户：存储区是设备上当前用户帐户的本地存储。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-109">Current user: The store is local to the current user account on the device.</span></span>

- <span data-ttu-id="2c4c4-110">服务帐户：存储是设备上特定服务的本地存储。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-110">Service account: The store is local to a particular service on the device.</span></span>

## <a name="view-certificates-in-the-mmc-snap-in"></a><span data-ttu-id="2c4c4-111">在 MMC 管理单元中查看证书</span><span class="sxs-lookup"><span data-stu-id="2c4c4-111">View certificates in the MMC snap-in</span></span>

<span data-ttu-id="2c4c4-112">下面的过程演示如何检查本地设备上的存储以查找适当的证书：</span><span class="sxs-lookup"><span data-stu-id="2c4c4-112">The following procedure demonstrates how to examine the stores on your local device to find an appropriate certificate:</span></span>
  
1. <span data-ttu-id="2c4c4-113">从 "**开始**" 菜单中选择 "**运行**"，然后输入 *mmc*。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-113">Select **Run** from the **Start** menu, and then enter *mmc*.</span></span>

    <span data-ttu-id="2c4c4-114">随即出现 MMC。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-114">The MMC appears.</span></span>
  
2. <span data-ttu-id="2c4c4-115">从 " **文件** " 菜单中，选择 " **添加/删除管理单元**"。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-115">From the **File** menu, select **Add/Remove Snap In**.</span></span>

    <span data-ttu-id="2c4c4-116">此时将显示 " **添加或删除管理单元** " 窗口。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-116">The **Add or Remove Snap-ins** window appears.</span></span>
  
3. <span data-ttu-id="2c4c4-117">从 " **可用的管理单元** " 列表中，选择 " **证书**"，然后选择 " **添加**"。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-117">From the **Available snap-ins** list, choose **Certificates**, then select **Add**.</span></span>  

    ![添加证书管理单元](./media/mmc-add-certificate-snap-in.png)
  
4. <span data-ttu-id="2c4c4-119">在 " **证书管理单元** " 窗口中，选择 " **计算机帐户**"，然后选择 " **下一步**"。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-119">In the **Certificates snap-in** window, select **Computer account**, and then select **Next**.</span></span>
  
    <span data-ttu-id="2c4c4-120">还可以选择 " **我的用户帐户** " 作为特定服务的当前用户或 **服务帐户** 。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-120">Optionally, you can select **My user account** for the current user or **Service account** for a particular service.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c4c4-121">如果你不是设备的管理员，则只能为你的用户帐户管理证书。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-121">If you're not an administrator for your device, you can manage certificates only for your user account.</span></span>
  
5. <span data-ttu-id="2c4c4-122">在 " **选择计算机** " 窗口中，选择 " **本地计算机** "，然后选择 " **完成**"。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-122">In the **Select Computer** window, leave **Local computer** selected, and then select **Finish**.</span></span>  
  
6. <span data-ttu-id="2c4c4-123">在 " **添加或删除管理单元** " 窗口中，选择 **"确定"**。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-123">In the **Add or Remove Snap-in** window, select **OK**.</span></span>  
  
    ![突出显示 "确定" 按钮的 "添加或删除管理单元" 窗口](./media/mmc-certificate-snap-in-selected.png)

7. <span data-ttu-id="2c4c4-125">可选：在 " **文件** " 菜单中，选择 " **保存** " 或 " **另存为** " 以保存 MMC 控制台文件以供以后使用。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-125">Optional: From the **File** menu, select **Save** or **Save As** to save the MMC console file for later use.</span></span>  

8. <span data-ttu-id="2c4c4-126">若要在 MMC 管理单元中查看证书，请在左窗格中选择 " **控制台根** "，然后展开 " **证书 (本地计算机")**。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-126">To view your certificates in the MMC snap-in, select **Console Root** in the left pane, then expand **Certificates (Local Computer)**.</span></span>

    <span data-ttu-id="2c4c4-127">将显示每种类型的证书的目录列表。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-127">A list of directories for each type of certificate appears.</span></span> <span data-ttu-id="2c4c4-128">可以从每个证书目录查看、导出、导入和删除其证书。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-128">From each certificate directory, you can view, export, import, and delete its certificates.</span></span>

## <a name="view-certificates-with-the-certificate-manager-tool"></a><span data-ttu-id="2c4c4-129">用证书管理器工具查看证书</span><span class="sxs-lookup"><span data-stu-id="2c4c4-129">View certificates with the Certificate Manager tool</span></span>

<span data-ttu-id="2c4c4-130">你还可以使用证书管理器工具查看、导出、导入和删除证书。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-130">You can also view, export, import, and delete certificates by using the Certificate Manager tool.</span></span>

### <a name="to-view-certificates-for-the-local-device"></a><span data-ttu-id="2c4c4-131">查看本地设备的证书</span><span class="sxs-lookup"><span data-stu-id="2c4c4-131">To view certificates for the local device</span></span>

1. <span data-ttu-id="2c4c4-132">从 "**开始**" 菜单中选择 "**运行**"，然后输入 *certlm.msc*。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-132">Select **Run** from the **Start** menu, and then enter *certlm.msc*.</span></span>

    <span data-ttu-id="2c4c4-133">此时将显示本地设备的“证书管理器”工具。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-133">The Certificate Manager tool for the local device appears.</span></span>
  
2. <span data-ttu-id="2c4c4-134">若要查看证书，请在左窗格中的 " **证书-本地计算机** " 下，展开要查看的证书类型的目录。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-134">To view your certificates, under **Certificates - Local Computer** in the left pane, expand the directory for the type of certificate you want to view.</span></span>

### <a name="to-view-certificates-for-the-current-user"></a><span data-ttu-id="2c4c4-135">查看当前用户的证书</span><span class="sxs-lookup"><span data-stu-id="2c4c4-135">To view certificates for the current user</span></span>

1. <span data-ttu-id="2c4c4-136">从“开始”菜单中选择“运行”，然后输入“certmgr.msc”  。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-136">Select **Run** from the **Start** menu, and then enter *certmgr.msc*.</span></span>

    <span data-ttu-id="2c4c4-137">此时会显示当前用户的证书管理器工具。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-137">The Certificate Manager tool for the current user appears.</span></span>
  
2. <span data-ttu-id="2c4c4-138">若要查看证书，请在左窗格中的 " **证书-当前用户** " 下，展开要查看的证书类型的目录。</span><span class="sxs-lookup"><span data-stu-id="2c4c4-138">To view your certificates, under **Certificates - Current User** in the left pane, expand the directory for the type of certificate you want to view.</span></span>

## <a name="see-also"></a><span data-ttu-id="2c4c4-139">另请参阅</span><span class="sxs-lookup"><span data-stu-id="2c4c4-139">See also</span></span>

- [<span data-ttu-id="2c4c4-140">使用证书</span><span class="sxs-lookup"><span data-stu-id="2c4c4-140">Working with certificates</span></span>](working-with-certificates.md)
- [<span data-ttu-id="2c4c4-141">如何：创建在开发期间使用的临时证书</span><span class="sxs-lookup"><span data-stu-id="2c4c4-141">How to: Create temporary certificates for use during development</span></span>](how-to-create-temporary-certificates-for-use-during-development.md)
- [<span data-ttu-id="2c4c4-142">如何：检索证书的指纹</span><span class="sxs-lookup"><span data-stu-id="2c4c4-142">How to: Retrieve the thumbprint of a certificate</span></span>](how-to-retrieve-the-thumbprint-of-a-certificate.md)
