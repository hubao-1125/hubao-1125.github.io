# MySQL8.0修改root账户密码

首先，修改配置文件`my.cnf`，这个文件一般位置是在`/etc/`下。
``` shell
vim /etc/my.cnf
```

执行之后在文件中找到`mysqld`
``` shell
[mysqld]
```

如果没有`mysqld`，则在文件中添加`[mysqld]`，添加到`my.cnf`文件中。
然后，在`[mysqld`]下面添加以下内容
``` shell
skip-grant-tables
```

保存文件，重启MySQL。
``` shell
service mysqld restart
```

重启后，执行
``` shell
mysql -uroot -p
```

会不需要密码直接进入MySQL

``` shell
# 切记，在mysql执行任何命令都需要带;英文分号

# 选择mysql database
use mysql;

# 先把root用户的密码置空
update user set authentication_string = '' where user = 'root';

# 设置root用户新密码
alter user 'root'@'localhost' IDENTIFIED BY '新密码';

ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
# 有可能会有上面的报错信息，报错信息取决于密码难易度，如果是自己玩，想设置简单密码，往下看。

SHOW VARIABLES LIKE 'validate_password%';

# 设置校验密码政策为低，如果不是8.0一般是validate_password_policy,MySQL8.0是.而不是_了
set global validate_password.policy=LOW;
```