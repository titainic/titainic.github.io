---
layout: default
title: Big Data
nav_order: 2
has_children: true
permalink: /docs/big-data
---

# Spark Hive Flink Iceberg Minio 
# 实现大数据存算分离湖仓一体
{: .no_toc }
---

## hive建立的表,spark和flink都能基于iceberg和minio进行正常读写
---


<div class="code-example" markdown="1">
1. hadoop先整合Minio 
1. hive使用s3a协议，能在hadoop整合minio基础之上进行读写操作 
1. hive整合iceberg使用s3a协议，能在hadoop整合minio基础之上进行读写操作 
1. spark读写第三步hive已经建好的表进行读写操作 
1. flink读写第三步hive已经建好的表进行读写操作 
</div>
