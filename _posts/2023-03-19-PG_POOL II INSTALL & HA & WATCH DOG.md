---
title: PG_POOL II INSTALL & HA & WATCH DOG
category: test
author: "이정훈"
tags: [test, postgresql]
img : ":pg2.png"
comments_disable: true
meta_description: "PG_POOL II INSTALL & HA & WATCH DOG"
---
# CentOS 7
```
sudo yum install -y dnf
```

```
dnf install -y pgpool-II
```

```
cp /etc/pgpool-II/pgpool.conf.sample /etc/pgpool-II/pgpool.conf
cp /etc/pgpool-II/pool_hba.conf.sample  /etc/pgpool-II/pool/conf
```

```
vim /etc/pgpool-II/pgpool.conf
```

```
listen_addresses = '*'

# 1번 DB
backend_hostname0 = '호스트 1 아이피'
backend_port0 = 5432
backend_weight0 = 1
backend_data_directory0 = '/var/lib/pgsql/14/data'
backend_flag0 = 'ALLOW_TO_FAILOVER'
backend_application_name0 = 'server0'

# 1번에 연결되는 2번 DB (1번 서버 conf에 작성)
backend_hostname1 = '호스트 2 아이피'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/var/lib/pgsql/14/data'
backend_flag1 = 'ALLOW_TO_FAILOVER'
backend_application_name1 = 'db1'

# 1번에 연결되는 3번 DB (1번 서버 conf에 작성)
backend_hostname1 = '호스트 3 아이피'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/var/lib/pgsql/14/data'
backend_flag1 = 'ALLOW_TO_FAILOVER'
backend_application_name1 = 'db2'
```

```
failover set

failover_command = '/data/pgpool/script/failover.sh %d %h %p %D %m %H %M %P %N %S'
follow_primary_command = '/data/pgpool/script/follow_primary.sh %d %h %p %D %m %H %M %P %r %R'
```

```
load Balance

load_balance_mode = on
read_only_function_list = 'get_.*,select_.*,'
```

```
ONLINE RECOVERY

recovery_user = 'postgres'
recovery_password = ''
recovery_1st_stage_command = 'recovery_1st_stage'
```

```
WATCHDOG

use_watchdog = on
hostname0 = 'db1 IP'
wd_port0 = 9000
pgpool_port0 = 9999
hostname1 = 'db2 IP'
wd_port1 = 9000
pgpool_port1 = 9999
hostname2 = 'db2 IP'
wd_port2 = 9000
pgpool_port2 = 9999

delegate_ip = '27.96.135.168'
if_cmd_path = '/sbin'
if_up_cmd = '/usr/bin/sudo /sbin/ip addr add $_IP_$/24 dev eth0 label eth0:0'
if_down_cmd = '/usr/bin/sudo /sbin/ip addr del $_IP_$/24 dev eth0'
arping_path = '/usr/sbin'
arping_cmd = '/usr/bin/sudo /usr/sbin/arping -U $_IP_$ -w 1 -I eth0'

wd_lifecheck_method = 'heartbeat'
wd_interval = 10
heartbeat_hostname0 = '27.96.135.168'
heartbeat_port0 = 9694
heartbeat_device0 = ''
heartbeat_hostname1 = '118.67.134.26'
heartbeat_port1 = 9694
heartbeat_device1 = ''
heartbeat_hostname2 = '101.10.208.44'
heartbeat_port2 = 9694
heartbeat_device2 = ''
wd_heartbeat_keepalive = 2
wd_heartbeat_deadtime = 30

log_destination = 'stderr'
logging_collector = on
log_directory = '/data/pgpool/logs'
log_filename = 'pgpool-%Y-%m-%d.log'
log_truncate_on_rotation = on
log_rotation_age = 1d
log_rotation_size = 10MB
pid_file_name = '/run/pgpool-II/pgpool.pid'
logdir = '/usr/pgpool/log'
```

```
vim /etc/pgpool-II/pgpool_node_id
```

```
cp data/postgresql/script/pgpool_remote_start /var/lib/pgsql/14/data/
cp data/postgresql/script/recovery_1st_stage /var/lib/pgsql/14/data/
scp -p /etc/pgpool-II/pgpool.conf root@118.67.134.26:/etc/pgpool-II//pgpool.conf
```

