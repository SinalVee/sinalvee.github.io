title: mongodb备份及恢复
date: 2015-11-11 14:39:30
updated: 2015-11-11 14:39:30
categories:
  - Database
  - MongoDB
tags:
  - mongodb
---

## 备份
`mongodump --host 127.0.0.1 --port 27017 -d {database} --out {out-path}`

## 恢复
`mongorestore -d {database} --drop {src-path}`
