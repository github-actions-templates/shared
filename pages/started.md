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
    
    # 任务运行的步骤
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

---

## 上手指南 <small>任务和步骤</small>

---

## 上手指南 <small>使用环境变量和密钥</small>