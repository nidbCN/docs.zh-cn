---
title: 构建基于微服务的容器化应用程序
description: 构建基于微服务的容器化应用程序并不简单，不应掉以轻心。 了解本章的核心概念。
ms.date: 01/13/2021
ms.openlocfilehash: b31c560fefa0928cefef2fdb92d6cfdbac084c57
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873141"
---
# <a name="architecting-container-and-microservice-based-applications"></a>构建基于微服务的容器化应用程序

微服务提供很多优点，但也会引起新的巨大挑战。创建基于微服务的应用程序时，微服务体系结构模式是基础支柱。

本指南前面部分介绍了有关容器和 Docker 的基本概念。 要开始使用容器，至少要了解这些基本信息。 虽然容器是微服务运行的基础，并且非常适合用于微服务，但它们并非是微服务体系结构所必需的。 此体系结构部分中的许多体系结构概念都适用于没有容器的情况。 然而，由于已介绍容器的重要性，本指南将重点介绍两者交叉部分。

企业应用程序可能会很复杂，它们通常由多个服务组成，而不是基于单个服务的应用程序。 对于这些情况，需要了解其他体系结构方法，如微服务和某些域驱动设计 (DDD) 模式以及容器业务流程的概念。 请注意，本章节不仅介绍容器上的微服务，还介绍任何容器化应用程序。

## <a name="container-design-principles"></a>容器设计原则

在容器模型中，容器映像实例表示单个进程。 将容器映像定义为进程边界，可以创建可用于对进程进行缩放或批处理的基元。

设计容器映像时，可在 Dockerfile 中看到[入口点](https://docs.docker.com/engine/reference/builder/#entrypoint)定义。 此定义定义了一个进程，其生命周期控制容器的生命周期。 该进程完成，则容器的生命周期结束。 容器可以表示 Web 服务器等长时间运行的进程，但也可表示批处理作业等生存期较短的进程，这些进程以前可能已实现为 Azure [WebJobs](https://github.com/Azure/azure-webjobs-sdk/wiki)。

如果进程失败，则容器结束，Orchestrator 接管。 如果 Orchestrator 已配置为使五个实例保持运行，而其中一个实例失败，则 Orchestrator 会创建另一个容器实例，来替换失败的进程。 在批处理作业中，使用参数启动该进程。 进程完成，则工作完成。 本指南接下来将深入介绍业务流程协调程序。

某些情况下，可能需要在单个容器中运行多个进程。 对于这种情况，因为每个容器只有一个入口点，所以在可根据需要启动任意数目的程序的容器中运行脚本。 例如，可以使用 [Supervisor](http://supervisord.org/) 或类似的工具处理在单个容器内启动多个进程的情况。 尽管可以找到用于在每个容器中承载多个进程的体系结构，但这种方法并不常见。

>[!div class="step-by-step"]
>[上一页](../net-core-net-framework-containers/official-net-docker-images.md)
>[下一页](containerize-monolithic-applications.md)
