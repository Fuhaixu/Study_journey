## linux常用指令

#### 1、查看当前路径

pwd

#### 2、切换到身份

su <身份>

例如: su root

#### 3、文件分组

>chown -R <群组> <文件>

[-cfhvR]参数列表：

- -c : 显示更改的部分的信息
- -f : 忽略错误信息
- -h :修复符号链接
- -v : 显示详细的处理信息
- -R : 处理指定目录以及其子目录下的所有文件

#### 4、文件授予权限

> chown [-cfhvR] [--help] [--version]   user[:group] file...

将目前upload目录下的所有文件与子目录的拥有者皆设为overdue

chown  -R overdue   upload

#### 5、查看当前路径下的文件

ls   查看路径下所有文件

ll    查看路径下文件详细信息