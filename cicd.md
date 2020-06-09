本文进行 CI/CD 相关简单介绍。

# 什么是 CI/CD

CI/CD 其实是 Devops 的一部分。

`持续集成(CI)` 注重将各个开发者的工作集合到一个代码仓库中，通常每天会进行几次， 主要目的是尽早发现集成错误，使团队更加紧密结合，更好地协作。

`持续交付(CD)` 的目的是最小化部署或发布过程中团队固有的摩擦， 它的实现通常能够将构建部署的每个步骤自动化，以便任何时刻能够安全地完成代码发布（理想情况下）。

`持续部署` 是一种更高程度的自动化，无论何时代码有较大改动， 都会自动进行构建／部署。

目前 K8S 等容器编排系统的出现，加速了 CI/CD 及整个 Devops 生态的发展，出现了很多相关的开发工具。

![IMAGE](assets/devops))


# CI/CD 相关工具

![IMAGE](assets/cicd_tools)

上面这个图，我是从 [CNCF](https://landscape.cncf.io/) 官网拉的。

## CI 目标

CI 的目标是将集成简化成一个简单、易于重复的日常开发任务， 这样有助于降低总体的构建成本并在开发周期的早期发现缺陷。 要想有效地使用 CI 必须转变开发团队的习惯，要鼓励频繁迭代构建， 并且在发现 bug 的早期积极解决。

## CI 相关工具

主流的是 gitlab 自带的 ci-runner / Jenkins, 甚至最近流行的 [github Actions](https://github.com/features/actions)

![IMAGE](assets/ci_tools)

具体参考：
https://developer.51cto.com/art/201809/582945.htm

## CD 目标

持续交付（CD）是 CI 的扩展，其中软件交付流程进一步自动化，以便随时轻松地部署到生成环境中。 成熟的持续交付方案也展示了一个始终可部署的代码库。使用 CD 后，软件发布将成为一个没有任何紧张感的例行事件。 开发团队可以在日常开发的任何时间进行产品级的发布，而不需要详细的发布方案或者特殊的后期测试。

## CD 相关工具

CD 完成的是如何有效的进行交付到生产环境，部署到 K8S 集群中，我们这里主要介绍 GitOps 相关，其他的技术可参考 [top10 open source cicd stacks](https://medium.com/cloudscaleqa/top-10-open-source-platform-to-build-your-ci-cd-stack-74a93a043d37)

### GitOps

Gitops 是一种使用基于Git的工作流程来全面管理应用和基础设施的想法，其在最近获得了极大关注。新一代的部署工具更能说明这一点，它们将GitOps作为持续交付的主要组织原则。

#### GitOps的四条规则。

- 将所有Kubernetes资源配置存储在Git中。

- 只使用PR来修改该Git仓库上的资源。

- 一旦修改了Git，立即将更改应用到集群中，并且完全自动化。

- 如果实际状态与所需的状态有偏差，要么纠正它，要么提醒操作人员。

---

下面介绍 GitOps 中表现比较好的三款工具。

### FluxCD

![IMAGE](assets/fluxCD)

[Flux](https://github.com/fluxcd/flux) 它最初是由Weaveworks开发的，现在是在GitHub上使用Apache 2.0许可的CNCF项目。

是目前 Kubernetes 的 GitOps 方面比较简单的运维工具，适合小团队。它可以将Git仓库中的清单状态与集群中运行的内容同步。

Flux 也是运行在 k8s 中，它的主要功能是监视远程Git仓库来应用Kubernetes清单中的更改。

该工具专注于软件交付周期中的部署部分，专门针对Git仓库和容器注册表与集群中的工作负载的版本和状态同步，因此该工具易于安装和维护。

缺点：

Flux没有多租户模式。

作为一个简单的组件，我们可以在同一个集群中使用不同的Flux实例，每一个实例都可以监视不同的远程Git仓库，并在不同的目标命名空间中同步它们的变化。这种可能性可以让不同的团队拥有自己的Git仓库。

### ArgoCD

![IMAGE](assets/argoCD)

[ArgoCD](https://github.com/argoproj/argo-cd)  是用于Kubernetes的声明性GitOps持续交付（CD）工具。该功能集虽然侧重于应用程序部署的管理，但是却非常出色，涵盖了多个同步选项，用户访问控制，状态检查等。

在GitOps风格上，部署从ArgoCD跟踪的Git仓库中的应用程序清单更改开始，类似于Flux的工作方式。

- 具有多租户和多集群部署的功能。
- 可以将不同团队的多个应用程序同步到一个或多个Kubernetes集群。
- 具有漂亮的现代Web UI，用户可以在其中查看其应用程序部署的状态
- 管理员可以管理项目和用户访问权限。

ArgoCD在GitOps仓库中提供了对不同格式的最广泛支持

- Kustomize应用程序
- Helm Charts
- Ksonnet应用 (ksonnet是一个用于编写，共享和部署Kubernetes应用程序清单的框架)
- YAML/JSON清单目录，包含Jsonnet (社区的实践主要是用 jsonnet 做 Kubernetes, Prometheus, Grafana 的配置管理)
- 配置管理插件配置的任何自定义配置管理工具

### Jenkins X

[Jenkins X](https://github.com/jenkins-x) 是一个围绕GitOps构建的完整的CI/CD解决方案，其底层使用了 [Tekton](https://github.com/tektoncd/pipeline) 。
实际上，Jenkins X采取了不同的方向，与经典的Jenkins几乎没有共同点。
它抛开了Jenkins的master-worker节点架构，并已经成为Kubernetes云原生CI/CD内置引擎。

值得注意的是，除了基于GitOps的部署功能外，Jenkins X还涵盖了更广泛的开发周期，包括来自典型CI管道的构建和测试阶段，以及容器镜像的构建和存储。

Jenkins X将设置 [Skaffold](https://skaffold.dev/) 和 [Kaniko](https://cloud.google.com/blog/products/gcp/introducing-kaniko-build-container-images-in-kubernetes-and-google-container-builder-even-without-root-access) 来构建容器图像，并使用Helm图表来打包Kubernetes清单。在内部，它使用Tekton运行管道，并使用[Lighthouse](https://github.com/jenkins-x/lighthouse) 与你选择的Git提供程序集成（例如GitHub），并在PR线程中启用丰富的交互。

![IMAGE](assets/JenkinsX)

### Flux/ArgoCD/Jx 对比

![IMAGE](assets/contrast)

- Flux可作为多用户设置中的一个构件使用。

- ArgoCD可以将更新发送到Slack以及其他工具但是不能通过这些工具控制它（而且实际上也并非必要）。

- Jenkins X正在努力实现在单个实例中支持多个团队。

Flux、ArgoCD和Jenkins X这三个项目都有非常有趣和强大的功能来管理Kubernetes中的部署，但每个项目都有其优点和缺点，应该根据你的具体案例进行评估。这些工具其实并不是相互竞争，相反，它们的目的是为了实现不同的用例。

Flux是一个小型、轻量级的组件，可以实现基于GitOps的部署，可能会对你当前的设置进行互补。它只需要访问一个（也只有一个）Git仓库，并且可以在有限的RBAC权限下运行。

ArgoCD可以管理不同集群中多个应用程序的部署。它在集群内运行，但也可以管理团队和项目的访问权限和权限。它有一个流畅的Web界面来管理应用程序和项目，并提供了一套不错的功能来管理部署。

Jenkins X捆绑了一些有争议的工具来构建一个围绕GitHub仓库的开发工作流（很快就会支持其他供应商）。它运行CI管道来构建和运行应用程序的测试，在PR中提供丰富的交互，并根据Git仓库中的变化管理部署。

参考：https://blog.container-solutions.com/fluxcd-argocd-or-jenkins-x-which-is-the-right-gitops-tool-for-you