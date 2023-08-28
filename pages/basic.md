---
layout: default
---

# Github Actions 基础概念

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

## 操作

---

## 变量




**参考资料**

- https://docs.github.com/zh/actions/learn-github-actions/variables

---

## 分支

## 123