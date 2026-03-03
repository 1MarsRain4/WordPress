# 使用教程

具体的使用教程请参考<https://myblog.marsrains.top/?p=549>

# 内置文件说明

为了方便使用，这个文件夹里面包含的文件会分开具体说明，以免下载错误导致后面排查错误难度加大。

1.[compose.yml](#compose.yml)
2.[promethus.yml](#prometheus.yml)
3.[Caddyfile](#Caddyfile)

# 具体修改

## compose.yml

相比较wordpress那个文件夹下的源代码，添加了两个环境 **prometheus** 和 **grafana** 以及 **cadvisor**。数据卷也有修改，添加了 **prometheus** 的数据卷和 **grafana** 的数据卷

**里面`prometheus`的环境需要修改默认`账户密码`，一定记得修改！！！！！**

## prometheus.yml

这里面主要是 **prometheus** 的配置文件，可以直接下载使用

## Caddyfile

在开头添加了针对**metrics**的内容，负责起到监听的作用