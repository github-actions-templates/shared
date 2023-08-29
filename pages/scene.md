---
layout: default
---

# 应用场景

- 自动化构建和测试
- 自动化部署
- 发布软件包和文档
- 定时任务和计划作业
- 集成第三方服务

---

## 应用场景 <small>自动化构建和测试</small>

222

---

## 应用场景 <small>自动化部署</small>

> 以为 Hexo 举例

<div grid="~ cols-2 gap-4">

<div class="overflow-auto h-100">

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

<div>

- `on` - 触发条件: `push` 事件，且分支为 `main`
- `jobs` - 作业
- `${{ secrets.GITHUB_TOKEN }}` - [自动令牌认证](https://docs.github.com/en/actions/security-guides/automatic-token-authentication), GitHub 会自动创建一个唯一的GITHUB_TOKEN密钥以在您的工作流程中使用。需要配合 `permissions` 一起使用。

</div>

</div>

---

## 应用场景 <small>发布软件包和文档</small>

---

## 应用场景 <small>定时任务和计划作业</small>

---

## 应用场景 <small>集成第三方服务</small>
