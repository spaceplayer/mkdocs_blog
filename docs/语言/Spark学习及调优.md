# Spark基础

job(action切分) --> stage(shuffle切分) --> task(并行执行)

application --> executer(container) -->  task



spark中rdd由多个partition组成，任务运行作用于partition。spark有两种类型的task：

1. ShuffleMapTask,  负责rdd之间的transform，map输出也就是shuffle write
2. ResultTask, job最后阶段运行的任务，也就是action（上面代码中foreach就是一个action，一个action会触发生成一个job并提交）操作触发生成的task，用来收集job运行的结果并返回结果到driver端。



# Spark调优

# Spark调试

# Spark监控

### Tracking UI

### Application log

### 日志

yarn logs -applicationId xxx

hadoop fs -ls /var/log/hadoop-yarn/apps/peipeidu/logs/



我们这个开发机不是spark的server机吧



