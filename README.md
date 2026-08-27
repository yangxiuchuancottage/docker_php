php运行环境
====================

~~~shell
docker compose up -d
~~~

swoole_start.ini
~~~
#进程名
[program:swoole_start]
# 进程名
;process_name=%(program_name)s_%(process_num)02d
# 启动用户
user=www-data
#执行脚本目录
directory=/var/www/html/
#启动命令
command=php think swoole start
#守护进程启动时是否同时启动
autorestart=true
#启动多少秒后状态判定
startsecs=3
#启动失败尝试次数
startretries=3
#日志输出
stdout_logfile=/var/log/php/supervisor_swoole_start.out.log
stderr_logfile=/var/log/php/supervisor_swoole_start.err.log
#日志文件大小
stdout_logfile_maxbytes=2MB
stderr_logfile_maxbytes=2MB
# 进程优先级值越小优先级越大,取值范围:999-1
priority=999
# 同时启动多少个进程
numprocs=1
~~~