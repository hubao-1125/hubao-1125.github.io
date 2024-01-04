# MySQL创建用户、授权

## 一、创建用户

```
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```
> username:你需要创建的用户名
> host:指定创建的用户可以在哪里访问，如果写了`localhost`，那么该用户只能在当前安装`MySQL`的服务器上访问，如果写`192.168.0.1`，那么，该用户可以在`192.168.0.1`上发起对MySQL的访问。写`%`表示该用户可以在任意主机上对`MySQL`发起访问。
> password:该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器

## 二、用户授权

```
GRANT privileges ON databasename.tablename TO 'username'@'host'
```
> privileges:给定用户访问权限，ALL、SELECT、INSERT、UPDATE，太多了，暂时写这些，够用。
> databasename:数据库名
> tablename:表名
> username:用户名
> host:同上面`一、创建用户`的`host`
> ps:`privileges`如果想给多权限，比如只给插入和更新，可以这么写
```
GRANT INSERT,UPDATE ON test01.* TO 'test01rw'@'%'
```
> 另外，如果`privileges`给的不是`ALL`，则`databasename`后面的`tablename`必须写具体表名，也可以`.*`。

> 额，发现个问题。如果授权的时候，你是这么写的
```
GRANT INSERT,UPDATE ON test01 TO 'test01rw'@'%'
```
很神奇的事情发生了，你在连接`MySQL`的客户端会看不到`test01`这个`database`，但是，如果你这么写
```
GRANT INSERT,UPDATE ON test01.* TO 'test01rw'@'%'
```
就可以看到。