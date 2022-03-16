目录名或文件名区分大小写

扩展名对linux操作系统没有特俗的含义

| 特殊符号 | 作 用                      |
| -------- | -------------------------- |
| ~        | 代表当前登录用户的主目录   |
| ~用户名  | 表示切换至指定用户的主目录 |
| -        | 代表上次所在目录           |
| .        | 代表当前目录               |
| ..       | 代表上级目录               |

# 文件管理

```shell
cd # 移动指定文件
# 查看文件属性
ls [
	-a 展示包含隐藏文件、..、.
	-A 展示包含隐藏文件
	-l 展示细节
]
pwd # 查看路径
# 递归创建目录
mkdir -p [
	-m 设置目录权限
]
rmdir -p # 删除多级空目录
# 移动
mv [
	-f 不询问覆盖
	-i 询问覆盖
]
cp # 复制
# 删除
rm [
	-i 询问删除
	-r 递归
	-f 忽略不存在文件
]
```

# 查看文件

```shell
cat # 从第一行开始显示内容 
tac # 从最后一行开始显示内容
more # 分页
less # 分页
	[pagedown] # 向下翻一页
	[pageup] # 向上翻一页
	/ 字符 # 向下搜索
	? 字符 # 向上搜索
	n 向下搜索
	N 向上搜索
head [-n NUMBER] # 取出文件前几行，默认10
tail [-n NUMBER] # 取出文件后几行，默认10
```

# 查找

```shell
find # 根据属性查找
which # 查看可执行文件位置，只有设置了环境变量的程序才可以用
whereis # 查找二进制文件、源文件和man手册页
```

# 归档(打包)和压缩

占用空间大小和所有文件大小之和一样

```java
tar [
    // 打包
    c 将多个文件或目录打包
    A 追加tar文件到归档文件
    // 解包
    x 对tar包解包
    t 只查看tar包中有哪些文件或目录，不对tar包解包
    C 指定解包位置
    
    v 显示过程
    f 指定包名
    z 压缩或解压缩.tar.gz
    j 压缩或解压缩.tar.bz2
] FILE_OR_DIRECTORY
```

```java
zip [
    // 压缩
	r 递归压缩目录
	m 源文件压缩后删除源文件
	q 不显示执行过程
	压缩级别 1-块，9-效果好
	u 往压缩文件里添加新文件
    // 解压
    d 解压到指定目录
    n 解压不覆盖已存在的文件
    o 解压覆盖已存在的文件
    x 不解压指定文件
    
    t 测试有无损坏
    v 显示过程
] 压缩包名 FILE_OR_DIRECTORY
    
```

# Vim

![img](http://c.biancheng.net/uploads/allimg/181008/2-1Q00Q41T01J.jpg)

# 三剑客

#### grep(擅长查找)

```shell
grep [
	-n 显示行号
	-c 统计匹配行数
	-v 显示不被匹配到的行
	-i 忽略大小写
	-w 匹配整个单词
	-o 显示匹配到的字符串（适用正则）
	-e PATTERN 逻辑或
	-f FILE 根据文件处理
] PATTERN FILENAME
```

#### sed(擅长取行替换)

```shell
sed [
	-n 不自动打印
	-i 直接编辑内容
	-f FILE 从FILE读入脚本
	-e SCRIPT 添加SCRIPT到脚本中
] SCRIPT FILENAME
```

**脚本命令s(替换)**

```
s/PATTERN/REPLACEMENT/FLAGS
```

| FLAGS标记 | 功能             |
| --------- | ---------------- |
| 数字      | 第几次替换       |
| g         | 全文匹配         |
| p         | 打印替换后的行   |
| w FILE    | 替换内容写到FILE |
| &         | 正则表达式匹配   |

```shell
-cat data.txt
line 1
line 2
-sed 's/line/LINE/g' data.txt
LINE 1
LINE 2
```

**脚本命令d(删除)**

```java
删除行
-sed '1d' data.txt
line 2
```

**脚本命令a和i(插入)**

```java
插入到第二行
-sed '2i\this is new line' data.txt
line 1
this is new line
line 2
插入到第二行后
-sed '2a\this is new line' data.txt
line 1
line 2
this is new line
```

**脚本命令c(整行替换)**

```java
整行替换
-sed '1c\this is new line' data.txt
this is new line
line 2
```

**脚本命令y(字符替换)**

```java
单个字符对应替换
-sed 'y/12/34/' data.txt
line 3
line 4
```

**脚本命令w(导出)**

```java
-set '1w data1.txt' data.txt
line 1
-cat data1.txt
line 1
```

**脚本命令r(文件内容插入)**

```java
-cat data1.txt
line 3
插入到指定位置后
-set '1r data1.txt' data.txt
line 1
line 3
line 2
```

#### awk(擅长取列)

awk默认空格或制表符作为分隔符，\$0代表整行，\$1代表第1个数据字段，\$2代表第2个数据字段

```java
awk [
	-F SEPARATOR 以SEPARATOR作为分隔符
    -f FILE 从FILE读入脚本
    -v VAR=VAL 执行前设置变量VAR，并设置初始值VAL
] '匹配规则{执行命令}' FILENAME
```

```java
-cat data.txt
line 1
line 2
-awk '/1/{print $0}' data.txt
line 1
```

**begin**

在读数据前执行脚本

```java
-awk 'begin{print "beginning..."} {print $0}' data.txt
starting...
line 1
line 2
```

**end**

```java
-awk '{print $0} end{print "ending..."}' data.txt
line 1
line 2
ending...
```

**内置变量**

| 变量名   | 属性                         |
| -------- | ---------------------------- |
| $0       | 整行                         |
| \$1,\$2  | 分隔后第几列                 |
| FS       | 分隔符                       |
| NF       | 多少列                       |
| NR       | 行号                         |
| RS       | 输入记录分隔符               |
| OFS      | 输出区域分隔符               |
| FNR      | 读入记录号，每个文件重新计算 |
| FILENAME | 正在处理的文件名             |

# WC命令

```java
wc [
	-l 统计行数，默认
	-w 统计字数，默认
    -c 统计字节数，默认
] FILE
输出结果：行数 字数 字节数 文件名
```

# 权限

**修改文件所属组**

```java
chgrp[
	-R 连同子目录中的文件一同修改
] GROUP FILE_OR_DIRECTORY
```

**修改文件所有者**

```java
chown [
	-R 连同子目录中的文件一同修改
] OWNER FILE_OR_DIRECTORY
```

文件拥有三种权限4-r、2-w、1-x；所有者权限：所属组权限：其他人权限

**修改文件权限**

```java
chmod [
	-R 连同子目录中的文件一同修改
] PERMISSION FILE
    
chmod 777 data.txt
chmod u+r,g-w,o=rwx data.txt
```

**修改文件创建时默认权限**

文件(目录)的初始权限 = 文件(目录)的最大权限 - umask权限

umask默认022

文件最大默认权限是666，所以创建文件获得权限-rw-r--r--

目录最大默认权限是777，所以创建目录获得权限drwxr-xr-x

```java
修改掩码
umask 764
umask u=rwx, g=rw, o=r 
```

# Kill

```shell
kill [
	-1(hup) 重新加载进程
	-9(kill) 杀死一个进程
	-15(term) 正常停止一个进程
]
```

# PS

```shell
ps [
	a 显示所有线程，除会话引线
	u 显示进程归属用户及内存使用情况
	x 显示没有控制终端的进程
	-l 详细信息
	-e 显示所有进程
	-o 自定义输出格式 [
		pid 进程id,
		ppid 父进程id,
		%mem 进程使用RAM比例，
		%cpu 进程占用CPU比例，
		cmd 进程名字，
		comm 命令名字
	]
	--sort=-%cpu 基于cpu使用率排序
]
```

查看CPU占用高的进程

`ps -eo pid,ppid,%mem,%cpu,comm --sort=-%cpu | head -n 1

# top

提供了linux系统运行中的进程的动态实时视图，按CPU占用进行排序

```shell
top [
	-p 仅查看指定ID进程
	-b 使用批处理模式输出
]
```

