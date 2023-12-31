---
layout: default
---

# 应用场景

- 自动化构建和测试
- 自动化部署
- 定时任务和计划作业
- 集成第三方服务

---

## 应用场景 <small>自动化构建和测试</small>

<div class="flex gap-4">

<div class="h-100 overflow-auto">

<div v-click class="h-50 overflow-auto">

```go
// package
package example

func Example() string {
	return "Hello, world!"
}

// package test
package example

import "testing"

func TestExample(t *testing.T) {
	if Example() != "Hello, world!" {
		t.Error("Expected 'Hello, world!'")
	}
}
```

</div>

<div v-click class="h-50 overflow-auto mt-2">

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
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Install Go
        uses: actions/setup-go@v3
      - run: go version

      - name: Run tests
        run: go test ./... -v -covermode=atomic -race -coverprofile=coverage.txt

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
```

</div>

</div>

<div v-click class="h-100 overflow-auto mt-1">

<img src="/assets/images/scene-1.png" width="530" />

</div>

</div>

<div v-click class="absolute top-5 right-3">

[案例地址](https://github.com/github-actions-templates/example/blob/main/.github/workflows/go-test.yml)

</div>

---

## 应用场景 <small>自动化部署</small>

<div class="flex gap-4">

<div v-click  class="overflow-auto h-100">

```yaml
name: Github Pages

on:
  push:
    branches:
      - main

jobs:
  pages:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v3

      - uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: 'npm'

      - uses: actions/cache@v3
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.OS }}-npm-cache

      - name: Install Dependencies
        run: npm install

      - name: Build
        run: npm run build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```
</div>


<div v-click class="h-100 overflow-auto mt-1">

<img src="/assets/images/scene-2.png" width="530" />

</div>

</div>

<div v-click class="absolute top-5 right-3">

[案例地址](https://github.com/github-actions-templates/hexo)

</div>

---

[效果地址](https://github-actions-hexo-template.flc.io/)

![](/assets/images/scene-3.png)

---

## 应用场景 <small>定时任务和计划作业</small>

<div class="flex gap-4">

<div>

<div v-click class="overflow-auto h-75">

```yaml
name: Cron
on:
  schedule:
    - cron: "0 0 * * *"
    - cron: "0 12 * * *"
    - cron: "*/5 * * * *"

env:
  GOPROXY: "https://proxy.golang.org"

jobs:
  test:
    name: "go test"
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Install Go
        uses: actions/setup-go@v3
      - run: go version

      - name: Run tests
        run: cd go && go test ./... -v -covermode=atomic -race -coverprofile=coverage.txt

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
```
</div>

<v-click>

**说明：**

</v-click>

<v-clicks>

- 时区相关：基于 UTC 时区
- 最短支持：每 5 分钟一次

</v-clicks>

</div>


<div class="h-100 overflow-auto mt-1">

<img v-click src="/assets/images/scene-4.png" width="530" />

<v-click>

> 实际操作下来，并不一定非常准时。（可能是 Queue 机制）

</v-click>

</div>

</div>

<div v-click class="absolute top-5 right-3">

[案例地址](https://github.com/github-actions-templates/example/blob/main/.github/workflows/cron.yml)

</div>

---

## 应用场景 <small>集成第三方服务</small>


<div class="flex gap-4">

<div class="overflow-auto h-100">

```yaml {all|17|20|27|all}
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
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Install Go
        uses: actions/setup-go@v3
      - run: go version

      - name: Run tests
        run: go test ./... -v -covermode=atomic -race -coverprofile=coverage.txt

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
```
</div>


<div class="h-100 overflow-auto mt-1">

<div v-click class="h-70 overflow-auto">

<img src="/assets/images/scene-5.png" width="530" />

</div>

<div v-click class="mt-2">

**Github Actions 第三方服务：**

- Marketplace: https://github.com/marketplace?type=actions

</div>


</div>

</div>