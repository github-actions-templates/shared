---
layout: default
---

# 基本介绍

- Github Actions 是什么？
- Features
- Pricing
- 基础概念

---

## 基本介绍 <small>Github Actions 是什么？</small>

<v-click>

先看个东西：

</v-click>

<v-click>

![](/assets/images/basic-1.png)

> 来源：https://github.com/features/actions

</v-click>

---

<div class="flex">

<div class="mr-15 pt-4 opacity-50"><img width="650" src="/assets/images/actions-icon-actions.svg" /></div>

<div class="mr-18">

<h4 v-click class="flex">
Automate your workflow from idea to production
</h4>

<p v-click class="text-gray-400">
从想法到生产，自动化您的工作流程
</p>

<p v-click class="text-gray-200">
GitHub Actions makes it easy to automate all your software workflows, now with world-class CI/CD. Build, test, and deploy your code right from GitHub. Make code reviews, branch management, and issue triaging work the way you want.
</p>

<p v-click class="text-gray-400">
GitHub Actions 现在可以使用世界一流的 CI/CD 轻松实现所有软件工作流程的自动化。直接从 GitHub 构建、测试和部署您的代码。按照您想要的方式进行代码审查、分支管理和问题分类。
</p>

<div class="mt-2 text-gray-200">

<v-clicks>

- 首个公测版，发布于 [2018 年 10 月](https://github.blog/changelog/2018-10-16-github-actions-limited-beta/)
- 在此之前 Github 的 CI/CD 以第三方服务 [Travis CI](https://www.travis-ci.com/) 居多。

</v-clicks>

</div>

</div>
</div>

---

## 基本介绍 <small>Features</small>

<v-clicks>

- 🖥 **操作系统** - 适用于 Linux、macOS、Windows、ARM 和容器；直接在虚拟机或容器内运行。
- 📎 **矩阵构建** - 通过矩阵工作流程可以同时跨多个操作系统和运行时版本进行测试，从而节省时间。
- 🧑‍💻 **任何语言** - 支持 Node.js、Python、Java、Ruby、PHP、Go、Rust、.NET 等。
- 🗒 **实时日志** - 查看您的工作流程实时运行的颜色和表情符号。
- 🔑 **秘钥存储** - 通过将包含 Git 流程的工作流程文件编码到您的存储库中，自动执行您的软件开发实践。
- ⛽️ **多容器测试** - compose 只需将一些内容添加到工作流程文件中即可在工作流程中测试您的 Web 服务及其数据库。
- 🎊 **社区支持的工作流程** - GitHub Actions 连接您的所有工具，以自动化开发工作流程的每一步。

</v-clicks>

<p v-click class="text-gray-400 text-1">
详看：<a href="https://github.com/features/actions" target="_blank">官方介绍</a>
</p>

---
layout: default
hideInToc: true
level: 2
---

## 基本介绍 <small>Pricing</small>

<v-clicks>

- **免费版** - 2000 分钟/月
- **Pro** - 3000 分钟/月（其中 Pro 为 4 美元/月）
- **Team** - 3000 分钟/月（其中 Team 为 4 美元/用户/月）
- **Enterprise** - 50000 分钟/月（其中 Enterprise 为 21 美元/用户/月）

</v-clicks>

<h4 v-click class="mt-6">对于 <span class="bg-clip-text text-transparent bg-gradient-to-r from-green-400 to-blue-500">开源</span> 项目，<span class="bg-clip-text text-transparent bg-gradient-to-r from-green-400 to-blue-500">无限制</span>。</h4>

<p v-click>
开源就可以薅羊毛……
</p>

<div v-click class="text-gray-400 footnotes">

<hr class="footnotes-sep" />

详见：[个人计划](https://github.com/settings/billing/plans) | [组织计划](https://github.com/organizations/addcnos/billing/plans)

</div>

---

## 基本介绍 <small>基础概念</small>

<div grid="~ cols-2 gap-4">

<div>

<v-click>

<img src="/assets/images/actions-components.png" class="h-30" />

</v-click>

<div v-click class="pt-3">

```yaml
on: push
jobs:
  test:
    strategy:
      matrix:
        platform: [ubuntu-latest, macos-latest, windows-latest]
    runs-on: ${{ matrix.platform }}
    steps:
    - uses: actions/checkout@v3
    - uses: actions/setup-node@v3
      with:
        node-version: 16
    - run: npm run test
```

</div>

</div>

<div v-click class="pt-0">

|                 |                                                                                                          |
|:----------------|:---------------------------------------------------------------------------------------------------------|
| 工作流 `workflows` | 自动化流程，可以有多个，存放在 `.github/workflows` 目录中。                                                                 |
| 事件 `event`      | 比如推送代码，创建 PR 等；[支持的事件](https://docs.github.com/zh/actions/using-workflows/events-that-trigger-workflows) |
| 作业 `jobs`       | 在同一运行器上执行的一组步骤。顺序执行，相互依赖。                                                                                |
| 操作 `actions`    | 作业中的一组任务，可以是自定义的，也可以是开源市场提供的。                                                                            |
| 运行程序 `runners`  | 运行工作流的服务器；支持 Ubuntu Linux、Microsoft Windows 和 macOS。                                                     |

</div>

</div>

---

## 基本介绍 <small>简单演示</small>

<div class="w-178 m-auto">

<video controls class="w-178 h-100">
  <source src="/assets/video/basic-video.mp4" type="video/mp4">
</video>

</div>