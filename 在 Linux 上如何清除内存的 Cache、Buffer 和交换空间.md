<h3>仅清除页面缓存（PageCache）</h3>

```
# sync; echo 1 > /proc/sys/vm/drop_caches       
```

<h3>清除目录项和inode</h3>

```
# sync; echo 2 > /proc/sys/vm/drop_caches       
```

<h3>清除页面缓存，目录项和inode</h3>
```
# sync; echo 3 > /proc/sys/vm/drop_caches 
```
上述命令的说明：

sync 将刷新文件系统缓冲区（buffer），命令通过“;”分隔，顺序执行，shell在执行序列中的下一个命令之前会等待命令的终止。正如内核文档中提到的，写入到drop_cache将清空缓存而不会杀死任何应用程序/服务，echo命令做写入文件的工作。

如果你必须清除磁盘高速缓存，第一个命令在企业和生产环境中是最安全，"...echo 1> ..."只会清除页面缓存。 在生产环境中不建议使用上面的第三个选项"...echo 3 > ..." ，除非你明确自己在做什么，因为它会清除缓存页，目录项和inodes。

在Linux上释放也许被内核所使用的缓冲区（Buffer）和缓存（Cache）是否是个好主意？

当你设置许多设定想要检查效果时，如果它实际上是专门针对 I/O 范围的基准测试，那么你可能需要清除缓冲区和缓存。你可以如上所示删除缓存，无需重新启动系统（即无需停机）。

Linux被设计成它在寻找磁盘之前到磁盘缓存寻找的方式。如果它发现该资源在缓存中，则该请求不会发送到磁盘。如果我们清理缓存，磁盘缓存就起不到作用了，系统会到磁盘上寻找资源。

此外，当清除缓存后它也将减慢系统运行速度，系统会将每一个被请求的资源再次加载到磁盘缓存中。

现在，我们将创建一个 shell 脚本，通过一个 cron 调度任务在每天下午2点自动清除RAM缓存。如下创建一个 shell 脚本 clearcache.sh 并在其中添加以下行：


```
#!/bin/bash
# 注意，我们这里使用了 "echo 3"，但是不推荐使用在产品环境中，应该使用 "echo 1"
echo "echo 3 > /proc/sys/vm/drop_caches"
```

给clearcache.sh文件设置执行权限
```
# chmod 755 clearcache.sh
```

现在，当你需要清除内存缓存时只需要调用脚本。

现在设置一个每天下午2点的定时任务来清除RAM缓存，打开crontab进行编辑。

```
# crontab -e
```

添加以下行，保存并退出。

```
0 3 * * * /path/to/clearcache.sh
```


<a href="https://linux.cn/article-5627-weibo.html">参考地址</a>



