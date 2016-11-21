
<h3>首先是设置全局环境变量，对所有用户都会生效</h3>

etc/profile: 此文件为系统的每个用户设置环境信息。当用户登录时，该文件被执行一次，并从 /etc/profile.d 目录的配置文件中搜集shell 的设置。一般用于设置所有用户使用的全局变量。
/etc/bashrc: 当 bash shell 被打开时，该文件被读取。也就是说，每次新打开一个终端 shell，该文件就会被读取。
接着是与上述两个文件对应，但只对单个用户生效：

~/.bash_profile 或 ~/.profile: 只对单个用户生效，当用户登录时该文件仅执行一次。用户可使用该文件添加自己使用的 shell 变量信息。另外在不同的LINUX操作系统下，这个文件可能是不同的，可能是 ~/.bash_profile， ~/.bash_login 或 ~/.profile 其中的一种或几种，如果存在几种的话，那么执行的顺序便是：~/.bash_profile、 ~/.bash_login、 ~/.profile。比如 Ubuntu 系统一般是 ~/.profile 文件。
~/.bashrc: 只对单个用户生效，当登录以及每次打开新的 shell 时，该文件被读取。
此外，修改 /etc/environment 这个文件也能实现环境变量的设置。/etc/environment 设置的也是全局变量，从文件本身的作用上来说， /etc/environment 设置的是整个系统的环境，而/etc/profile是设置所有用户的环境。有几点需注意：

系统先读取 etc/profile 再读取 /etc/environment（还是反过来？）
/etc/environment 中不能包含命令，即直接通过 VAR="..." 的方式设置，不使用 export 。
使用 source /etc/environment 可以使变量设置在当前窗口立即生效，需注销/重启之后，才能对每个新终端窗口都生效。
