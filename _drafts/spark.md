

## 带本地包导入
```bash
spark-submit \
--files $HIVE_CONF_DIR/hive-site.xml \
--master yarn \
--deploy-mode cluster \
--driver-cores 8 \
--driver-memory 16g \
--executor-memory 16g \
--executor-cores 4 \
--num-executors 300 \
--conf spark.speculation=true \
--conf spark.speculation.interval=300s \
--conf spark.speculation.multiplier=3 \
--conf spark.speculation.quantile=0.9 \
--conf spark.yarn.am.cores=8 \
--conf spark.yarn.am.memory=16g \
--conf spark.default.parallelism=2000 \
--conf spark.sql.shuffle.partitions=2000 \
--conf spark.yarn.appMasterEnv.yarn.nodemanager.container-executor.class=DockerLinuxContainer \
--conf spark.executorEnv.yarn.nodemanager.container-executor.class=DockerLinuxContainer \
--conf spark.yarn.appMasterEnv.yarn.nodemanager.docker-container-executor.image-name=my-docker.guofei.me:5000/wise_algorithm:latest \
--conf spark.executorEnv.yarn.nodemanager.docker-container-executor.image-name=my-docker.guofei.me:5000/wise_algorithm:latest \
--py-files dps-py.zip \
test_submit.py

# --py-files dps-py.zip 配置待import的内容
# test_submit.py 是你提交的spark脚本
```
