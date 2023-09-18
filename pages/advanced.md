---
layout: default
---

# 高级特性

- 矩阵任务
- 依赖和条件

---

## 高级特性 <small>矩阵任务</small>


<div class="flex gap-4">

<div>

<v-click>

**Q：** 测试用例希望在多个环境，多个不同版本的语言环境下运行？

</v-click>

<div v-click class="overflow-auto h-85">

```yaml
name: Go Test
on:
  push:
    branches: [ main ]
    paths:
      - "go/**"
env:
  GOPROXY: "https://proxy.golang.org"

jobs:
  test:
    name: "go test"
    strategy:
      matrix:
        go-version: [ 1.16.x, 1.17.x, 1.18.x, 1.19.x, 1.20.x, 1.21.x ] # Go 版本
        platform: [ ubuntu-latest, macos-latest, windows-latest ] # 运行环境
    runs-on: ${{ matrix.platform }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Install Go
        uses: actions/setup-go@v3
        with:
          go-version: ${{ matrix.go-version }} # 获取矩阵变量

      - name: Run tests
        run: cd go && go test ./... -v -covermode=atomic -race -coverprofile=coverage.txt

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
```
</div>


</div>

<div>

<div v-click class="h-75 overflow-auto mt-1">

<img v-click src="/assets/images/advanced-1.png" class="w-100" />

</div>


<v-click>

**说明：**

</v-click>

<v-clicks>

- 任务运行：并行运行
- [案例地址](https://github.com/github-actions-templates/example/blob/main/.github/workflows/go-matrix.yml)

</v-clicks>

</div>

</div>

---

## 高级特性 <small>依赖和条件</small>

<div class="flex gap-4">

<div>

<v-click>

#### 依赖

</v-click>

<div v-click class="h-90 overflow-auto">

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
    needs: # 依赖设定
      - jobs-1
    steps:
      - name: Run a one-line script
        run: echo Hello, world! This is jobs-3, but after jobs-1
```

</div>

</div>

<div>

<v-click>

#### 条件

</v-click>

<div v-click class="h-90 overflow-auto">

```yaml
name: Test IF

on:
  workflow_dispatch:
    inputs:
      test-jobs:
        description: '测试 IF'

env:
  ENVIRONMENT: prod

jobs:
  dev:
    runs-on: ubuntu-latest
    steps:
      - run: echo The dev env is ${{ env.ENVIRONMENT }}
        if: env.ENVIRONMENT == 'dev'

      - run: echo The prod env is ${{ env.ENVIRONMENT }}
        if: env.ENVIRONMENT == 'prod'
```

</div>

</div>

</div>

---

<div>

<img src="/assets/images/advanced-2.png" class="w-100" />

</div>

<v-click>

**说明：**

</v-click>

<v-clicks>

- 表达式：https://docs.github.com/zh/actions/learn-github-actions/expressions
- 您需要使用特定语法指示 GitHub 对表达式求值，而不是将其视为字符串。 `${{ <expression> }}`  
  在 `if` 条件下使用表达式时，可以省略 `${{ }}` 表达式语法，因为 GitHub Actions 会自动将 `if` 条件作为表达式求值。 使用 `${{ }}` 表达式语法将内容转换为字符串，并且字符串是真值。 例如，**`if: true && ${{ false }}` 的计算结果为 `true`**。

</v-clicks>
