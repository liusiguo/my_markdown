### 常用命令记录

---

#### 一、常用字符串处理，文件处理命令

##### mount

-  mount -t nfs  IP:/tftpboot  /tftpboot

---

##### echo

- 常用参数

  ~~~shell
  1. -n ：不换行输出
  2. -e ：输出转义字符串
  ~~~

- 向文件写内容(追加)

  ~~~shell
  echo 'xxxx'  >>  filename
  ~~~

- 向文件写内容(覆盖)

  ~~~shell
  echo 'xxx' >> filename
  ~~~

---

##### cat

---

##### awk

---

##### sed

- 执行多个命令

  ~~~shell
  sed -e 's/系统/00/g' -e '2d' test.txt
  ~~~

- 查看某个时间段日志

  ~~~shell
  # 取消默认控制台输出，与p可打印指定内容
  sed -n '/2019-10-24 22:16:21/,/2019-10-24 22:16:59/p'  filename.log
  ~~~

- 替换文件中的字符串

  ~~~shell
  # s表示替换，g表示一行中的多个
  sed -i 's/被替换内容/替换内容/g'  filename.xx
  ~~~

  ~~~shell
  # 将文本中的/替换为空
  sed -i 's/\///g' filename.xx
  ~~~

---

##### grep

---

#### 二、网络相关

---

##### netstat

~~~markdown
centos安装 netstat
yum install -y net-tooks
~~~

- 查看所有被占用端口

  ~~~shell
  netstat -tulnp
  ~~~

- 杀死某个端口占用的进程

  ~~~shell
  kill -s 9 端口号
  ~~~

- 强制杀死某个端口

  ~~~shell
  sudo fuser -k -n tcp 2049
  ~~~

  

