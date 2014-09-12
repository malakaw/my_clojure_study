# 学习clojure笔记 #

##使用emacs开发clojure  ##


### 如何导入本地的jar文件 ###
首先你必须安装maven,然后你把你的工程（需要被导入到clojure）mvn install.最后加入此开发包到clojure的peoject.clj文件中。
<br/>**install**
<pre><code>
mvn install:install-file -Dfile=XXX.jar -DgroupId=com.x.xx  -DartifactId=xxxxxx  -Dversion=0.0.1 -Dpackaging=jar
</code></pre>
具体如何

## 遇到的错误和问题 ##
### 使用emacs ###
1) inferior-lisp-proc: No Lisp subprocess; see variable `inferior-lisp-buffer'
代码是没有问题的，那么怎么解决呢？
M-x nrepl-enable-on-existing-buffers should fix it.
