---
layout: default
---

# 最佳实践

- 使用徽章来标记 CI 状态
- 使用缓存提高性能
- 错误处理和调试
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

---

## 最佳实践 <small>错误处理和调试</small>

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

<img v-click src="assets/images/practice-3.png" class="h-90"/>

<v-click>

**说明：**


</v-click>


<v-clicks>

- 容器化服务仅限于在 `ubuntu` 系统下运行
- [案例地址](https://github.com/github-actions-templates/example/blob/main/.github/workflows/redis.yml)

</v-clicks>

---

## 最佳实践 <small>版本控制和代码审查</small>

---

## 最佳实践 <small>效率和资源管理</small>

---

## 最佳实践 <small>社区资源和扩展</small>
