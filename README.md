# hadoop-word-count

拉取Github上的仓库地址

```bash
git clone https://github.com/my-big-data-study/hadoop-word-count.git
```

进入项目根目录，拉取镜像容器，并且启动伪分布的 Hadoop 镜像，会自动挂载 Data 目录到容器内

```bash
docker-compose up -d
```

打包WordCount的程序代码

```bash
./gradlew clean jar
```

进入Hadoop容器内部，并在HDFS上新建wordcount目录，并把输入目录上传到HDFS

```bash
# 进入容器内部
docker exec -it --user hadoop `docker ps -l -q` bash

# 在HDFS上新建 wordcount 目录
hadoop fs -mkdir /wordcount

# 上传 input 目录到 hdfs 上
hadoop fs -put /data/input /wordcount
```

提交MapReduce任务

```bash
hadoop jar /data/hadoop-word-count-1.0.jar WordCountV1 /wordcount/input /wordcount/output
```

查看单词统计的结果

```bash
hadoop fs -cat /wordcount/output/part-r-00000
```

## reference

1. [MapReduce WordCountV1 官网教程](https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html#Example:_WordCount_v1.0)
2. [Docker 镜像仓库](https://hub.docker.com/r/5200710/hadoop)

