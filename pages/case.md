---
layout: default
---

# 案例分享

- 五子棋
- 节假日
- 翻译助手

---

## 案例分享 <small>[五子棋](https://github.com/frostming/frostming)</small>

基于创建 `issue` 触发 Github Actions 更新 `README.md`

<img src="/assets/images/case-1.png" class="h-80 mt-4" />

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

## 案例分享 <small>贪吃蛇</small>

## 案例分享 <small>翻译助手</small>

> 衍生出自动回复机器人？自动评审机器人？

## [Quarto](https://quarto.org/) - 闲聊机器人

## 