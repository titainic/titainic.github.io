---
layout: default
title: 大数据存算分离湖仓一体
nav_order: 2
has_children: true
permalink: /docs/big-data
---
{: .highlight }
本章节详细介绍使用如下技术实现大数据存算分离湖仓一体
## Spark Hive Flink Iceberg Minio 
{: .no_toc }
---

{: .highlight }
实现目标

<div class="code-example" markdown="1">
1. Hadoop基于s3a协议整合MinIO
1. Hive使用s3a协议对Hadoop整合MinIO建表读写数据
1. Hive整合Iceberg基于s3a协议对Hadoop整合MinIO建表读写数据
1. Spark 整合hive,Iceberg,Hadoop,MinIO读写数据
1. Flink 整合hive,Iceberg,Hadoop,MinIO读写数据
1. 性能测试
</div>


