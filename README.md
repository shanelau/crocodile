![](https://img.shields.io/badge/language-golang-orange.svg)
# Crocodile 分布式任务调度系统


## Master
- 用户管理
  用户增删改查 
  用户分为普通用户和管理员 普通用户只可以对自已创建的任务进行修改 管理员具有全部权限
- 任务管理 mysql
  任务: 增加，删除，修改，查询，强杀，停止调度，立即触发，执行状态
  任务存放在mysql 每次将任务轮询一边 取出最近需要运行的任务，并更新所有的任务下一次运行的时间
  任务到期后，会发布一个执行任务的消息，订阅者会执行这个任务
- 执行器 
  执行器包含着主机 任务实际会在这些主机中运行
- 任务日志收集
  任务执行完成后会调用日志收集的模块进行日志的入库

 
## Worker
- 任务执行模块 
- 订阅任务 接收到执行任务的消息后，查询任务的执行所属的执行器是否为自已注册的(任务的执行器为test1,test2自已注册的执行器名称为test1，则会去抢锁尝试执行)
- 完成后会向日志接收的模块发布消息

- crocodile
  主目录
  - common
    一些公共的包
  - service
    服务
    - actuator
      执行器管理
    - executor
      执行任务的服务
    - taskjob
      任务管理的服务
    - log
      日志管理的服务
    - user
      用户管理的服务
  - web
    web接口
    - job
    任务的接口平台
    - actutor
    执行器接口
    - user
    用户管理接口
    
    
## 运行

先将conf文件下`config.yaml`复制到每个模块下面

然后运行