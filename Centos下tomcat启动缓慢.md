
解决方法如下：

找到 $JAVA_HOME/jre/lib/security/java.security 这个文件，找到里面的

securerandom.source=file:/dev/random

或者

securerandom.source=file:/dev/urandom

修改为

securerandom.source=file:/dev/./urandom
