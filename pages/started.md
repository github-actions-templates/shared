---
layout: default
---

# 上手指南

- 创建工作流(`workflows`)
- `workflows` 文件的结构和语法
- 触发器和事件
- 任务和步骤
- 使用环境变量和密钥

---

## 上手指南 <small>创建 workflows</small>

<v-click>

- 方式一：使用官方模板（适合新手）

</v-click>

<div class="flex gap-4">
<div>

<img v-click src="/assets/images/started-create-workflow.png" class="w-80 mt-2"/>

<img v-click src="/assets/images/started-choose-template.png" class="w-80 mt-2"/>

</div>

<div>
<img v-click src="/assets/images/started-workflow-blank.png" class="h-98 mt-2 fr"/>
</div>

</div>

---
hideInToc: true
---

- 方式二：手动创建（适合有经验的用户）

> 文件路径：`.github/workflows/[xxx].yml`，支持配置多个 `workflows` 文件。

<div class="overflow-auto h-110 code-wrap">

```yaml
name: CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Run a one-line script
        run: echo Hello, world!

      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.
```

</div>

---

## 上手指南 <small>workflows 文件的结构和语法</small>

<div class="overflow-auto h-100 code-wrap">

```yaml
# 工作流的名称
name: CI

# 触发器
on:
  # 当往 main 分支推送代码，或发起合并请求时触发
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

# 任务列表（此处只列了一个）
jobs:
  # 任务名称，比如此处为 build 任务；多个任务默认是并行，可通过配置建立依赖关系
  build:
    # 任务运行的服务器环境
    runs-on: ubuntu-latest
    
    # 任务运行的步骤，此处是串行
    steps:
      # 拉取代码，此处使用的时第三方（虽然是官方组织）的 actions 组件
      - uses: actions/checkout@v3

      # 执行一个单行命令（其中 name 为该命令的名称）
      - name: Run a one-line script
        run: echo Hello, world!

      # 执行多行命令，用 | 隔开
      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.
```

</div>

---

<img src="/assets/images/started-1.png" class="h-100" />

---

<img src="/assets/images/started-2.png" class="h-100" />

<p v-click class="text-1">

示例地址：[传送门](https://github.com/github-actions-templates/blank/actions/runs/6019109272/job/16328450482)

</p>

---

## 上手指南 <small>触发器和事件</small>

<div class="flex gap-4">

<div>

<v-click>

#### 工作流程触发器是导致工作流程运行的事件。 这些事件可以是：

</v-click>

<v-clicks>

* 工作流存储库中发生的事件，如：推送代码、创建 ISSUE、发起 PR
* 在 GitHub 之外发生并在 GitHub 上触发 `repository_dispatch` 事件的事件，如：通过 Webhook 发送自定义时间
* 预定时间，如：每天 4 点全量单测
* 手动，如：点击按钮

</v-clicks>

<v-click>

> 例如：当你推送代码到某个分支时，可以触发事件，以运行相关任务。

</v-click>

</div>

<div v-click>

```yaml
on:
  issues:
    types:
      - opened

jobs:
  label_issue:
    runs-on: ubuntu-latest
    steps:
      - env:
          GITHUB_TOKEN: ${{ secrets.MY_TOKEN }}
          ISSUE_URL: ${{ github.event.issue.html_url }}
        run: |
          gh issue edit $ISSUE_URL --add-label "triage"
```

> 配合官方的 `gh` 命令工具效果更佳。

</div>

</div>

--- 

#### 常用工作流事件

- 推送代码，创建合并等

```yaml {all|5|7|9-10|14|all}
on:
  push:
    branches:
      - 'main'
      - 'releases/**' # 指定分支
    tags:
      - 'v1.*' # 指定 Tag
    paths:
      - '**.js' # 指定文件
      - '!**-aplha.js' # 排除文件
  pull_request:
    branches:
      - 'main'
      - '!releases/**-alpha' # 忽略分支
```

---

#### 常用工作流事件

<div v-click>

- 定时事件

```yaml
on:
  schedule:
    - cron: '30 5 * * 1,3'
    - cron: '30 5 * * 2,4'
```

</div>

<div v-click>

Cron 语法：

```bash
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of the month (1 - 31)
│ │ │ ┌───────────── month (1 - 12 or JAN-DEC)
│ │ │ │ ┌───────────── day of the week (0 - 6 or SUN-SAT)
│ │ │ │ │
│ │ │ │ │
│ │ │ │ │
* * * * *
```

</div>

--- 

#### 工作流事件

- 更多事件：https://docs.github.com/zh/actions/using-workflows/events-that-trigger-workflows
- 基础概念：https://docs.github.com/zh/actions/using-workflows/triggering-a-workflow
- 手动运行工作流程：https://docs.github.com/zh/actions/using-workflows/manually-running-a-workflow
- 工作流语法：https://docs.github.com/zh/actions/using-workflows/workflow-syntax-for-github-actions

---

## 上手指南 <small>任务和步骤</small>

- 工作流运行由一个或多个 `jobs` 组成，默认情况下 **并行** 运行。
- 可使用 `jobs.<job_id>.needs` 关键字定义对其他作业的依赖关系，来实现顺序运行。
- 每个作业在 `runs-on` 指定的运行器环境中运行。

---

#### 一个 🌰

<div class="flex gap-4">

<div v-click class="h-100 overflow-auto">

```yaml
name: Test Jobs

on:
  workflow_dispatch:
    inputs:
      test-jobs:
        description: '测试 Jobs'


jobs:
  jobs-1:
    runs-on: ubuntu-latest
    steps:
      - name: Run a one-line script
        run: echo Hello, world! This is jobs-1

  jobs-2:
    runs-on: ubuntu-latest
    steps:
      - name: Run a one-line script
        run: echo Hello, world! This is jobs-2

  jobs-3:
    runs-on: ubuntu-latest
    needs:
      - jobs-1
    steps:
      - name: Run a one-line script
        run: echo Hello, world! This is jobs-3, but after jobs-1
```

</div>


<div v-click class="h-100 overflow-auto mt-1">

<img src="assets/images/started-3.png" width="530" />

</div>

</div>


<div v-click class="absolute top--1 right-3">

[案例地址](https://github.com/github-actions-templates/example/actions/workflows/jobs.yml)

</div>

---

## 上手指南 <small>使用环境变量和密钥</small>


<div class="flex gap-4">

<div v-click class="h-100 overflow-auto">

```yaml
name: Test Env

on:
  workflow_dispatch:
    inputs:
      test-jobs:
        description: '测试 Env'

env:
  ENVIRONMENT: prod

jobs:
  dev:
    runs-on: ubuntu-latest
    env:
      ENVIRONMENT: dev
    steps:
      - run: echo The env is ${{ env.ENVIRONMENT }}

  prod:
    runs-on: ubuntu-latest
    steps:
      - run: echo The env is ${{ env.ENVIRONMENT }}
```

</div>


<div v-click class="h-100 overflow-auto mt-1">

<img src="assets/images/started-4.png" width="530" />

</div>

</div>


<div v-click class="absolute top--1 right-3">

[案例地址](https://github.com/github-actions-templates/example/actions/workflows/env.yml)

</div>

---

#### 补充

<v-clicks>

- 环境变量你可以理解为执行了：`export ENVIRONMENT=prod`
- 支持编程语言中的获取，如：Go 语言的 `os.LookupEnv`

</v-clicks>

<div class="flex gap-4 mt-3">

<div v-click>

```go
package main

import "os"

func main() {
	if env, ok := os.LookupEnv("o"); !ok {
		panic("ENVIRONMENT is not set")
	} else {
		println(env)
	}
}
```

</div>

<div v-click><img src="assets/images/started-6.png" width="530" /></div>

</div>

---

#### 如果我要输入我的银行卡密码怎么办？

<v-clicks>

- 路径：`Settings -> Security -> Actions secrets and variables -> Actions -> New repository secret`

</v-clicks>

<div class="flex gap-4 mt-3">

<img v-click src="assets/images/started-7.png" class="h-90" />

<img v-click src="assets/images/started-8.png" class="h-90" />

</div>

---


<div class="flex gap-4 h-80">

<div v-click class="h-100 overflow-auto">

```yaml
name: Test Env

on:
  workflow_dispatch:
    inputs:
      test-jobs:
        description: '测试 Env'

env:
  PWD: ${{ secrets.BANK_PASSWORD }}

jobs:
  pwd:
    runs-on: ubuntu-latest
    steps:
      - run: echo The bank password is ${{ env.PWD }}
```

</div>


<div v-click class="h-100 overflow-auto mt-1">

<img src="assets/images/started-9.png" class="h-80" />

</div>

</div>

<v-click>

**适用场景：**

</v-click>


<v-clicks>

- 单元测试：API 秘钥、地址等
- 第三方服务：单测覆盖率（如：`codecov`）
- 敏感信息：密码等
- ……

</v-clicks>
