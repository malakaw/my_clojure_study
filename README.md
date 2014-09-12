# 学习clojure笔记 #

##使用emacs开发clojure  ##


### 如何导入本地的jar文件 ###
首先你必须安装maven,然后你把你的工程（需要被导入到clojure）mvn install.最后加入此开发包到clojure的peoject.clj文件中。
<br/>**mvn install**
<pre><code>
mvn install:install-file -Dfile=XXX.jar -DgroupId=com.x.xx  -DartifactId=DFXXXX  -Dversion=0.0.1 -Dpackaging=jar
</code></pre>
具体细节参考maven文档<br/>

**添加入clojure工程的project.clj**
<pre><code>
 :dependencies [
			[org.clojure/clojure "1.5.1"]
			[com.x.xx/DFXXXX "0.0.1"]
		]

</code></pre>


## lein  ##
1)查看依赖库的tree
<pre><code>
lein deps :tree
</code></pre>

2)导出依赖的jar文件到lib目录
<br/>可以使用插件lein-libdir
<br/>添加下面代码到project.clj
<pre><code>
(defproject myproject "0.1.0-SNAPSHOT"
  :plugins [[lein-libdir "0.1.1"]]
  :libdir-path "lib")

</code></pre>
copy jar文件
<pre><code>
$ lein libdir
</code></pre>

## 遇到的错误和问题 ##
### 使用emacs ###
1) inferior-lisp-proc: No Lisp subprocess; see variable `inferior-lisp-buffer'
代码是没有问题的，那么怎么解决呢？
M-x nrepl-enable-on-existing-buffers should fix it.
