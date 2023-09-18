---
layout: default
---

# 高级特性

- 矩阵任务
- 依赖和条件
- 存储和共享数据
- 自定义动作
- 安全性和凭证管理

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

---

## 高级特性 <small>存储和共享数据</small>

---

## 高级特性 <small>自定义动作</small>

---

## 高级特性 <small>安全性和凭证管理</small>