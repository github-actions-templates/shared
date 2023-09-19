---
layout: default
---

# 最佳实践

- 使用徽章来标记 CI 状态
- 使用缓存提高性能
- GITHUB_TOKEN
- 容器化服务
- 版本控制和代码审查
- 效率和资源管理

---

## 最佳实践 <small>使用徽章来标记 CI 状态</small>

<div grid="~ cols-2 gap-4">

<v-clicks>

![](/assets/images/practice-1.png)

![](/assets/images/practice-2.png)

</v-clicks>

</div>

<div class="code-wrap">

<v-clicks>

```markdown
[![Lint](https://github.com/addcnos/youdu/actions/workflows/lint.yml/badge.svg)](https://github.com/addcnos/youdu/actions/workflows/lint.yml)
```

[![Lint](https://github.com/addcnos/youdu/actions/workflows/lint.yml/badge.svg)](https://github.com/addcnos/youdu/actions/workflows/lint.yml)

</v-clicks>

</div>

---

## 最佳实践 <small>使用缓存提高性能</small>

**基础版本**

<div grid="~ cols-2 gap-4">

<div v-click class="overflow-auto h-90">

```yaml
name: Cache
on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Mkdir
        run: mkdir -p ~/test

      - uses: actions/cache@v3 # 缓存服务
        id: cache-test # 缓存 ID
        with:
          path: ~/test/ # 需要缓存的路径
          key: ${{ runner.os }}-cache-test # 缓存唯一标识
          restore-keys: |
            ${{ runner.os }}-cache- # 备选标识（多个换行）

      - run: sleep 10 && echo "Hello world" > ~/test/1.log
        if: steps.cache-test.outputs.cache-hit != 'true' # 若缓存没命中

      - run: sleep 10 && echo "Hello world" > ~/test/2.log
        if: steps.cache-test.outputs.cache-hit != 'true'

      - run: cat ~/test/*.log
```

</div>

<div v-click>

[案例地址](https://github.com/github-actions-templates/example/blob/main/.github/workflows/cache.yml)

</div>

</div>

---

<div grid="~ cols-2 gap-2">

<div v-click>

**首次（未命中缓存）**

<img src="/assets/images/practice-4.png" class="h-70"/>

</div>

<div v-click>

**第二次（命中缓存）**

<img src="/assets/images/practice-5.png" class="h-70"/>

</div>


</div>

<div v-click class="mt-4">

<img src="/assets/images/practice-6.png" class="w-200"/>

</div>

---

**一些第三方服务直接支持的缓存参考**

| 包管理器                | 	用于缓存的 setup-* 操作                                        |
|---------------------|----------------------------------------------------------|
| npm、Yarn、pnpm       | 	[setup-node](https://github.com/actions/setup-node)     |
| pip、pipenv、Poetry   | 	[setup-python](https://github.com/actions/setup-python) |
| Go `go.sum`         | 	[setup-go](https://github.com/actions/setup-go)         |
| PHP `composer.lock` | 	[setup-php](https://github.com/shivammathur/setup-php)  |
| ……                  | 	                                                        |

<div v-click class="mt-4">

> 一般情况下，我们会结合上下文等实现缓存 key 的命名。如：  
> `${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/package-lock.json') }}`

</div>

---

**一个简单的 🌰**

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: actions/setup-go@v4
    with:
      go-version: '1.21'
      check-latest: true
      cache-dependency-path: |
             subdir/go.sum
             tools/go.sum
    # cache-dependency-path: "**/*.sum"

  - run: go run hello.go
```

---

如果你想手动清理缓存：

<img src="/assets/images/practice-7.png" class="w-200"/>

<v-click>

**其他小知识：**

- 存储库中的多个工作流运行可以**共享缓存**。 可以从**同一存储库和分支**的另一个工作流运行访问和还原为工作流运行中的分支创建的缓存。

</v-click>

---

## 最佳实践 <small>`GITHUB_TOKEN`</small>

**💡 自动令牌身份验证**

<v-click>

在每个工作流作业开始时，GitHub 会自动创建唯一的 `GITHUB_TOKEN` 机密以在工作流中使用。 可以使用 `GITHUB_TOKEN` 在工作流作业中进行身份验证。

</v-click>

<v-click>

<hr class="opacity-10 mt-1 mb-1" />

**那可以做什么？**

</v-click>

<v-clicks>

- Github Cli 命令
- 调用 Github REST API
- 仓库写入，生成 `gh-page`
- ……
- 代码自动审查（发起 PR）
- 接入 AI 呢？
- ……

</v-clicks>

---

#### 一个🌰

<div class="flex gap-4">

<div v-click class="overflow-auto h-100 w-100">

```yaml
name: issue welcome
on:
  issues:
    types:
      - opened

jobs:
  comment:
    runs-on: ubuntu-latest
    permissions: # 设置权限
      issues: write # 写入权限
    steps:
      - run: gh issue comment $ISSUE --body "Thank you for opening this issue!"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          ISSUE: ${{ github.event.issue.html_url }}
```
</div>

<div>

<img v-click src="/assets/images/practice-8.png" class="w-100"/>

<div v-click>

[案例地址](https://github.com/github-actions-templates/example/blob/main/.github/workflows/issue-welcome.yml)

</div>

</div>

</div>


---

## 最佳实践 <small>容器化服务</small>


<div class="flex gap-4">

<div v-click class="overflow-auto h-100 w-100">

```yaml
name: Redis
on: push

jobs:
  runner-job:
    runs-on: ubuntu-latest

    services:
      redis:
        image: redis # Docker Hub 镜像名称
        ports:
          - 6379:6379

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Install Go
        uses: actions/setup-go@v3
      - run: go version

      - name: Run main.go
        run: cd redis && go run main.go
```
</div>

<div v-click class="overflow-auto h-100 w-100">

```go
package main

import (
	"context"
	"time"

	redis "github.com/redis/go-redis/v9"
)

var ctx = context.Background()

func main() {
	rdb := redis.NewClient(&redis.Options{
		Addr: "localhost:6379",
	})

	if err := rdb.Set(ctx, "test", time.Now().String(), time.Second*10).Err(); err != nil {
		panic(err)
	}

	if result := rdb.Get(ctx, "test"); result.Err() != nil {
		panic(result.Err())
	} else {
		println(result.Val())
	}
}
```

</div>

</div>

---

<img v-click src="/assets/images/practice-3.png" class="h-90"/>

<v-click>

**说明：**


</v-click>


<v-clicks>

- 容器化服务仅限于在 `ubuntu` 系统下运行
- [案例地址](https://github.com/github-actions-templates/example/blob/main/.github/workflows/redis.yml)

</v-clicks>