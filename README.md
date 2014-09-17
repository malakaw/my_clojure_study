# 学习clojure笔记 #

## coll数据结构 ##
1)map 的merge-with ★<br/>
这个是针对某种比较常见的需求的解决办法，就是一个map的value是set,然后需要进行“累加”set ;直接上代码吧。
<pre><code>
user=> (merge-with concat
          {"Lisp" ["Common Lisp" "Clojure"]           "ML" ["Caml" "Objective Caml"]}
          {"Lisp" ["Scheme"]           "ML" ["Standard ML"]})

 {"Lisp" ("Common Lisp" "Clojure" "Scheme"), "ML" ("Caml" "Objective Caml" "Standard ML")}


 user=> (merge-with +
                   {:a 1  :b 2}
                   {:a 9  :b 98 :c 0})

{:c 0, :a 10, :b 100}

user=> (use 'clojure.set)
user=> (merge-with union
                   {:a #{1 2 3},   :b #{4 5 6}}
                   {:a #{2 3 7 8}, :c #{1 2 3}})

{:c #{1 2 3}, :a #{1 2 3 7 8}, :b #{4 5 6}}
</code></pre>

2)clojure的set /list/map..转换成java 的set/list/map ☆</br/>
<pre><code>
[] to a java.util.ArrayList
{} to a java.util.HashMap
#{} to a java.util.HashSet
() to a java.util.LinkedList

user=> (java.util.ArrayList. [1 2 3])
#<ArrayList [1, 2, 3]>
user=> (.get (java.util.ArrayList. [1 2 3]) 0)
1

user=> (java.util.HashMap. {"a" 1 "b" 2})
#<HashMap {b=2, a=1}>
user=> (.get (java.util.HashMap. {"a" 1 "b" 2}) "a")
1

ser=> (into [] (java.util.ArrayList. [1 2 3]))
[1 2 3]
user=> (into #{} (java.util.HashSet. #{1 2 3}))
#{1 2 3}
user=> (into '() (java.util.LinkedList. '(1 2 3)))
(3 2 1)
user=> (into {} (java.util.HashMap. {:a 1 :b 2}))
{:b 2, :a 1}
</code></pre>

3) contains?的坑<br/>
contains对于map /set 是我们理解的那个contains，但是vector不是的，他只是存在下标否，真蛋疼，不注意容易被莫名好久。


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
[https://github.com/djpowell/lein-libdir](https://github.com/djpowell/lein-libdir)
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

## emacs  ##
<b>使用的是emacs live [https://github.com/overtone/emacs-live](https://github.com/overtone/emacs-live)</b><br/>
在使用的时候经常会对于一个def or defn定义后，跳转到nrepl测试，我定义了一个快捷键，很方便，二合一。
<pre><code>
(global-set-key (kbd "&lt;f6&gt;")
    (lambda ()
      (interactive)
      (nrepl-eval-last-expression nil)
      (other-window 1)))
</code></pre>

## 遇到的错误和问题 ##
### 使用emacs ###
1) inferior-lisp-proc: No Lisp subprocess; see variable `inferior-lisp-buffer'
代码是没有问题的，那么怎么解决呢？
M-x nrepl-enable-on-existing-buffers should fix it.


## 有待解决的的问题 ##
1）在emacs live中没有一个 变量在多文件中的替换，查找可以用 Helm<br/>
2) 没有像是代码行对应的todo list (类似eclipse中  tasks ),似乎helm就可以搞定。。。。。但是不是整个project 的，是所有打开的buffer中。自己写elisp??
