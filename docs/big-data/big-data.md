---
layout: default
title: Big Data
nav_order: 2
has_children: true
permalink: /docs/big-data
---

# Spark Hive Flink Iceberg Minio 实现大数据存算分离湖仓一体

## 第一步：hadoop先整合Minio
## 第二步：hive使用s3a协议，_能在hadoop整合minio基础之上进行读写操作
## 第三步：hive整合iceberg使用s3a协议，能在hadoop整合minio基础之上进行读写操作
## 第四步：spark读写第三步hive已经建好的表进行读写操作
## 第五步：flink读写第三步hive已经建好的表进行读写操作