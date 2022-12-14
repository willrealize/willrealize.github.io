# 使用文档 - 使用Denyhosts防SSH暴力破解


最近服务器一直被SSH暴力破解, 就搜索了有关服务器防暴力破解的方法, DenyHosts是一个使用Python写的程序，它会分析sshd的日志文件`/var/log/auth.log`, 当发现重复的攻击时就会把IP记录到`/etc/hosts.deny`文件中，从而屏蔽恶意攻击者的IP

<!--more-->

### 安装

这里使用的系统是Ubuntu18.04

```bash
sudo apt install denyhosts
```

DenyHosts配置文件的路径: `/etc/denyhosts.conf`

DenyHosts日志文件的路径: `/var/log/denyhosts`

DenyHosts监控的文件(ssh日志文件)的路径: `/var/log/auth.log`

### 配置

```bash
sudo vim /etc/denyhosts.conf
```

下面是各项配置的中文说明, 使用默认配置也行, 也可以根据需要进行修改

```bash
SECURE_LOG = /var/log/auth.log						# ssh日志文件, denyhosts基于此日志内容来判断
HOSTS_DENY = /etc/hosts.deny						# 控制用户登陆文件
PURGE_DENY = 4w										# 过多久后清除已经禁止的IP, 空表示永远不解禁
BLOCK_SERVICE = ALL									# 禁止的服务名, 如还要添加其他服务, 只需添加逗号跟上相应的服务即可，写个sshd也可以
DENY_THRESHOLD_INVALID = 5							# 允许无效用户失败的次数
DENY_THRESHOLD_VALID = 15							# 允许普通用户登陆失败的次数
DENY_THRESHOLD_ROOT = 5								# 允许root登陆失败的次数
DENY_THRESHOLD_RESTRICTED = 1
WORK_DIR = /var/lib/denyhosts						# denyhosts运行目录
SUSPICIOUS_LOGIN_REPORT_ALLOWED_HOSTS = YES
HOSTNAME_LOOKUP = YES								# 是否进行域名反解析
LOCK_FILE = /run/denyhosts.pid						# 程序的进程ID 
ADMIN_EMAIL = root@localhost						# 管理员邮件地址, 可设置告警邮箱
SMTP_HOST = localhost
SMTP_PORT = 25
SMTP_FROM = DenyHosts <nobody@localhost>
SMTP_SUBJECT = DenyHosts Report
AGE_RESET_VALID = 5d								# 用户的登录失败计数会在多久以后重置为0 (h: 小时, d: 天, m: 月, w: 周, y: 年) 
AGE_RESET_ROOT = 25d
AGE_RESET_RESTRICTED = 25d
AGE_RESET_INVALID = 10d
RESET_ON_SUCCESS = yes								# 如果一个ip登陆成功后, 失败的登陆计数是否重置为0
DAEMON_LOG = /var/log/denyhosts 					# denyhosts的日志文件
DAEMON_SLEEP = 60s									# 当以后台方式运行时, 每读一次日志文件的时间间隔
DAEMON_PURGE = 1h									# 当以后台方式运行时, 清除机制在 HOSTS_DENY 中终止旧条目的时间间隔, 这个会影响PURGE_DENY的间隔
```

有关指令

```bash
sudo service denyhosts start
sudo service denyhosts restart
sudo service denyhosts stop
# 与下面指令相同
sudo /etc/init.d/denyhosts start|stop|restart|force-reload|status

# 也可以使用下面指令
sudo systemctl start denyhosts
sudo systemctl restart denyhosts
sudo systemctl stop denyhosts
```

参照文档: [使用DenyHosts防SSH暴力破解](https://www.cnblogs.com/hsiangyu-meng/p/15160052.html)

