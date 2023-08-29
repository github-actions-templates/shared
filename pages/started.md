---
layout: default
---

# 上手指南

- 创建 `workflows`
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

- 方式二：手动创建（适合有经验的用户）

> 文件路径：`.github/workflows/[xxx].yml`，支持配置多个 `workflows` 文件。

<div class="overflow-auto h-110 code-wrap">

```yaml
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the "main" branch
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v3

      # Runs a single command using the runners shell
      - name: Run a one-line script
        run: echo Hello, world!

      # Runs a set of commands using the runners shell
      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.
```

</div>

---

## 上手指南 <small>workflows 文件的结构和语法</small>

---

## 上手指南 <small>触发器和事件</small>

---

## 上手指南 <small>任务和步骤</small>

---

## 上手指南 <small>使用环境变量和密钥</small>