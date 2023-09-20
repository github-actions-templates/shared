---
layout: default
---

# 案例分享

- 翻译助手
- 节假日
- 五子棋
- 跨仓库使用
- uptime
- 单测覆盖率

---


## 案例分享 <small>翻译助手</small>

基于 `issue` 的内容，识别中文，自动翻译成英文。


<div class="flex gap-4">

<div v-click class="overflow-auto h-90">

```yaml
name: 'issue-translator'
on:
  issue_comment:
    types: [created]
  issues:
    types: [opened]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: usthe/issues-translate-action@v2.7
        with:
          IS_MODIFY_TITLE: true
          CUSTOM_BOT_NOTE: Bot detected the issue body's language is not English, translate it automatically. 👯👭🏻🧑‍🤝‍🧑👫🧑🏿‍🤝‍🧑🏻👩🏾‍🤝‍👨🏿👬🏿
          BOT_GITHUB_TOKEN: ${{ secrets.BOT_GITHUB_TOKEN }}
```

</div>

<div v-click>

<img src="/assets/images/case-2.png" class="mt-4 w-100" />

</div>

</div>

---

## 案例分享 <small>[节假日](https://github.com/NateScarlet/holiday-cn)</small>

基于计划任务生成中国节假日信息。

<div class="flex gap-4">

<div v-click class="overflow-auto h-90">

```yaml
name: CI

on:
  push:
    branches: [ master ]
  pull_request:
  workflow_dispatch:
  schedule:
    - cron: "0 12 * * *"
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.8'
      - name: Install dependencies
        run: pip install -r dev-requirements.txt
      - name: Setup git user
        run: |
          git config user.name "GitHub Actions"
          git config user.email "actions@users.noreply.github.com"
      - name: Test
        run: coverage run -m pytest 
      - name: Lint
        run: make lint
      - name: Update(master)
        if: ${{ github.ref == 'refs/heads/master' && github.event_name != 'pull_request' }}
        env:
          GITHUB_TOKEN: ${{secrets.GITHUB_TOKEN}}
        run: coverage run -a ./scripts/update.py --release
      - name: Update
        if: ${{ !(github.ref == 'refs/heads/master' && github.event_name != 'pull_request') }}
        run: coverage run -a ./scripts/update.py 
```

</div>

<div v-click class="overflow-auto h-90">

```json
{
    "$schema": "https://raw.githubusercontent.com/NateScarlet/holiday-cn/master/schema.json",
    "$id": "https://raw.githubusercontent.com/NateScarlet/holiday-cn/master/2023.json",
    "year": 2023,
    "papers": [
        "http://www.gov.cn/zhengce/zhengceku/2022-12/08/content_5730844.htm"
    ],
    "days": [
        {
            "name": "元旦",
            "date": "2022-12-31",
            "isOffDay": true
        },
        {
            "name": "元旦",
            "date": "2023-01-01",
            "isOffDay": true
        },
        {
            "name": "元旦",
            "date": "2023-01-02",
            "isOffDay": true
        },
        {
            "name": "春节",
            "date": "2023-01-21",
            "isOffDay": true
        },
        {
            "name": "春节",
            "date": "2023-01-22",
            "isOffDay": true
        },
        {
            "name": "春节",
            "date": "2023-01-23",
            "isOffDay": true
        },
        {
            "name": "春节",
            "date": "2023-01-24",
            "isOffDay": true
        },
        {
            "name": "春节",
            "date": "2023-01-25",
            "isOffDay": true
        },
        {
            "name": "春节",
            "date": "2023-01-26",
            "isOffDay": true
        },
        {
            "name": "春节",
            "date": "2023-01-27",
            "isOffDay": true
        },
        {
            "name": "春节",
            "date": "2023-01-28",
            "isOffDay": false
        },
        {
            "name": "春节",
            "date": "2023-01-29",
            "isOffDay": false
        },
        {
            "name": "清明节",
            "date": "2023-04-05",
            "isOffDay": true
        },
        {
            "name": "劳动节",
            "date": "2023-04-23",
            "isOffDay": false
        },
        {
            "name": "劳动节",
            "date": "2023-04-29",
            "isOffDay": true
        },
        {
            "name": "劳动节",
            "date": "2023-04-30",
            "isOffDay": true
        },
        {
            "name": "劳动节",
            "date": "2023-05-01",
            "isOffDay": true
        },
        {
            "name": "劳动节",
            "date": "2023-05-02",
            "isOffDay": true
        },
        {
            "name": "劳动节",
            "date": "2023-05-03",
            "isOffDay": true
        },
        {
            "name": "劳动节",
            "date": "2023-05-06",
            "isOffDay": false
        },
        {
            "name": "端午节",
            "date": "2023-06-22",
            "isOffDay": true
        },
        {
            "name": "端午节",
            "date": "2023-06-23",
            "isOffDay": true
        },
        {
            "name": "端午节",
            "date": "2023-06-24",
            "isOffDay": true
        },
        {
            "name": "端午节",
            "date": "2023-06-25",
            "isOffDay": false
        },
        {
            "name": "中秋节、国庆节",
            "date": "2023-09-29",
            "isOffDay": true
        },
        {
            "name": "中秋节、国庆节",
            "date": "2023-09-30",
            "isOffDay": true
        },
        {
            "name": "中秋节、国庆节",
            "date": "2023-10-01",
            "isOffDay": true
        },
        {
            "name": "中秋节、国庆节",
            "date": "2023-10-02",
            "isOffDay": true
        },
        {
            "name": "中秋节、国庆节",
            "date": "2023-10-03",
            "isOffDay": true
        },
        {
            "name": "中秋节、国庆节",
            "date": "2023-10-04",
            "isOffDay": true
        },
        {
            "name": "中秋节、国庆节",
            "date": "2023-10-05",
            "isOffDay": true
        },
        {
            "name": "中秋节、国庆节",
            "date": "2023-10-06",
            "isOffDay": true
        },
        {
            "name": "中秋节、国庆节",
            "date": "2023-10-07",
            "isOffDay": false
        },
        {
            "name": "中秋节、国庆节",
            "date": "2023-10-08",
            "isOffDay": false
        }
    ]
}
```

</div>

</div>

---

## 案例分享 <small>[五子棋](https://github.com/frostming/frostming)</small>

基于创建 `issue` 触发 Github Actions 更新 `README.md`

<div class="flex gap-4">

<div v-click class="overflow-auto h-90">

```yaml
name: "Gomoku"

on:
  issues:
    types: [opened]

jobs:
  move:
    runs-on: ubuntu-latest
    if: startsWith(github.event.issue.title, 'gomoku|')
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Setup Python
        uses: actions/setup-python@v2
        with:
          python-version: 3.8
      - name: Cache PIP
        uses: actions/cache@v2
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-py38

      - name: Install Dependencies
        run: |
          pip install -r requirements.txt
      - name: Play
        env:
          REPO: ${{ github.repository }}
          ISSUE_TITLE: ${{ github.event.issue.title }}
          PLAYER: ${{ github.event.issue.user.login }}
          ISSUE_NUMBER: ${{ github.event.issue.number }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          python -m chess.runner

      - name: Push changes
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          branch: ${{ github.ref }}
```

</div>

<div v-click>

<img src="/assets/images/case-1.png" class="h-80 mt-4" />

</div>

</div>


---

## 案例分享 <small>跨仓库使用</small>

`Laravel` 的 `workflows`，统一托管到 [.github](https://github.com/laravel/.github) 下维护。


<div class="flex gap-4">

<div v-click>

<img src="/assets/images/case-3.png" class="h-80 mt-1" />

</div>

<div v-click class="overflow-auto h-90">

[`laravel/framework`](https://github.com/laravel/framework/blob/10.x/.github/workflows/pull-requests.yml)


```yaml
name: pull requests

on:
  pull_request_target:
    types: [opened]

permissions:
  pull-requests: write

jobs:
  uneditable:
    uses: laravel/.github/.github/workflows/pull-requests.yml@main
```

</div>

</div>

---

## 案例分享 <small>[uptime](https://github.com/upptime/upptime)</small>

定时请求网站状态，并可视化相关数据。[案例地址](https://status.flc.io/)、[案例仓库地址](https://github.com/flc1125/status.flc.io)

<iframe v-click src="https://status.flc.io" class="w-210 h-95"></iframe>

---

## 案例分享 <small>[单测覆盖率](https://about.codecov.io/)</small>

推送代码后，触发单测执行，并上报至第三方平台，便于分析查看。[案例地址](https://github.com/go-packagist/go-kratos-components)

<div v-click class="flex gap-1">

<span class="opacity-20">[![Go Version](https://badgen.net/github/release/go-packagist/go-kratos-components/stable)](https://github.com/go-packagist/go-kratos-components/releases)</span>

<span class="opacity-20">[![GoDoc](https://pkg.go.dev/badge/github.com/go-packagist/go-kratos-components)](https://pkg.go.dev/github.com/go-packagist/go-kratos-components)</span>

[![codecov](https://codecov.io/gh/go-packagist/go-kratos-components/branch/master/graph/badge.svg?token=5TWGQ9DIRU)](https://codecov.io/gh/go-packagist/go-kratos-components)

<span class="opacity-20">[![Go Report Card](https://goreportcard.com/badge/github.com/go-packagist/go-kratos-components)](https://goreportcard.com/report/github.com/go-packagist/go-kratos-components)</span>

<span>[![tests](https://github.com/go-packagist/go-kratos-components/actions/workflows/go.yml/badge.svg)](https://github.com/go-packagist/go-kratos-components/actions/workflows/go.yml)</span>

<span class="opacity-20">[![MIT license](https://img.shields.io/badge/license-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)</span>

</div>

<div class="flex gap-4">

<div v-click class="overflow-auto h-80">

```yaml
name: Go Test
on:
  push:
    branches: [ master, feature/* ]
  pull_request:
    branches: [ master ]
env:
  GOPROXY: "https://proxy.golang.org"

jobs:
  test:
    name: "go test"
    strategy:
      matrix:
        go-version: [ 1.18.x, 1.19.x, 1.20.x, 1.21.x ]
        platform: [ ubuntu-latest ]
    runs-on: ${{ matrix.platform }}

    services:
      redis:
        image: redis
        options: >-
          --health-cmd "redis-cli ping" --health-interval 10s --health-timeout 5s --health-retries 5
        ports:
          - 6379:6379

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Install Go
        uses: actions/setup-go@v3
        with:
          go-version: ${{ matrix.go-version }}
          cache: true
          cache-dependency-path: ./go.sum
      - run: go version

      - name: Run tests
        run: go test ./... -v -covermode=atomic -race -coverprofile=coverage.txt

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
```

</div>

<div v-click>

<img src="/assets/images/case-6.png" class="h-80 mt-1" />

</div>

</div>

---
layout: center
class: text-center
hideToc: false
---

<div>

<img src="/assets/images/case-7.png" class="h-120 mt-1" />

</div>

---
layout: center
class: text-center
hideToc: false
---

更多案例……