# 说明

* sentinel 集成influxdb、grafana
* 参考地址：https://zhuanlan.zhihu.com/p/88176525

## 步骤
 
1. 安装 influxdb  
    > sudo apt install influxdb
    
    > sudo apt install influxdb-client
1. 连接 influxdb  
    * influx
1. 创建用户  
    * CREATE USER admin with PASSWORD 'admin' WITH ALL PRIVILEGES
1. 退出  
    * exit
1. 用户密码登录  
    * influx -username admin -password admin
1. 创建数据库  
    * CREATE DATABASE sentinel_log
1. 启动sentinel
    * java -jar -Dspring.influx.url=http://localhost:8086 -Dspring.influx.user=admin -Dspring.influx.password=admin -Dspring.influx.database=sentinel_log sentinel-dashboard.jar

1. 配置grafana

    1. 数据源
        * ![](img/shujuyuan.png)
    2. dashboard
        * ![](img/dashboard.png)
        
    3. sql
        
        ```sql
        select QPS  from (SELECT mean("successQps") AS "QPS" FROM "autogen"."sentinelInfo" WHERE ("app" = 'cloud-live-gateway') AND $timeFilter GROUP BY time($__interval), "resource" fill(none)) where QPS > 2 group by "resource" 
        
        select avgRt from (SELECT mean("avgRt") AS "avgRt" FROM "autogen"."sentinelInfo" WHERE ("app" = 'cloud-live-service') AND $timeFilter GROUP BY time($__interval), "resource" fill(none)) where avgRt > 1 group by "resource" 
        ``` 

 