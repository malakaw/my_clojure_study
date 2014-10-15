# 学习clojure笔记 #


[使用小列子，snacks](https://github.com/malakaw/my_clojure_study/blob/master/snacks.md)

## 宏 ##
不说废话，直接看代码☺
<pre><code>
(defmacro debug [x]
  `(+ 1 ~x (+ 9 ~x) ~'y)
  )
(let [y 9] (debug 3))
返回： 25
</code></pre>

## clojure的 quote  `' 符号 ##
自己的理解，通俗点，容易记住
clojure 有很多的符号，让像我这种java程序员很莫名，首先还是说下quote,这分两种
一种是  ' ，另外一个是 ｀ ，看上去差不多，其实功能也差不多。

1） ' <br/>
这个的功能就是，任何出现此符号的地方，'后的刮号内的东西就是理解为原封不动传给jvm. 当作整个是脚本。
<br/>
2)  `<br/>
这个其实也和'差不多，也是把出现此符号后面 刮号内的东西 原封不动传给jvm，但是，是的肯定是有但是的（你预料到了 ☺）, 这个 原封不动的东西里面我们还是可以再做点小动作 ，当我们对于 原封不动的脚本内 需要运算一些东西，那么就加上 ～<br/>
<pre><code>
user=> `(+ 1 (* 2 3))
(clojure.core/+ 1 (clojure.core/* 2 3))
user=> `(+ 1 ~(* 2 3))
(clojure.core/+ 1 6)
user=> (let [x 2] `(1 ~x 3))
(1 2 3)
</code></pre>



那我们java 程序员很纳闷了，clojure  干嘛那么费劲搞这么一出呢，多麻烦啊。其实clojure有鞋情况不得不使用他们，我们知道“( ”后的第一个元素一般是函数或是宏来计算的，但是如果我只是表述一个 list ,something like (1 2 3 4),那么整数1肯定是不能被当作函数或是宏的，这个时候就需要 ' 或是`了。看个列子
<pre><code>
user=> (cons 1 (1 2))
ClassCastException java.lang.Long cannot be cast to clojure.lang.IFn  user/eval677 (NO_SOURCE_FILE:1)
user=> (cons 1 '(1 2 3))
(1 1 2 3)
user=> (cons 1 `(1 2 3))
(1 1 2 3)
</code></pre>

所以说类似这种情况下我们需要 ` '.
<br/>
<b>补充说明下～@ ，这个～@的意思是先运算， 然后运算出来的结果拼装到前面的 list中，理解这个，你可以想像下concat;</b>


## vector  ##
可以理解为栈<br/>
<b> 优点</b><br/>
在集合的最右边删除或是添加操作速度很快<br/>
使用下标位置访问数据或是修改数据速度快<br/>
反向遍历<br/>
<br/>
<b> 缺点</b><br/>

备注：建议使用 conj (类似push) /pop /peek, 这几个最高效



### 如何导入本地jar文件 ###
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

2)列出你依赖的包的最新版本<br/>
有时候你需要这种功能的，使用lein-ancient<br/>

3)代码质量控制工具 <br/>
有时候我们会写一些不合理的代码，或许你没留意到，或许你一直认为那是很“合理”的写法；但是有这么一个工具告诉你应该这么写代码，不是人，是工具（他不会鄙视你），你可以悄悄的接受他的建议优化代码。 他们是： kibit / Eastwood
<br/>

4)导出依赖的jar文件到lib目录
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

<br/>
<b>编辑clojure/lisp语言的一些使用快捷键</b>
<table>
<tr>
<td>C-k</td><td>剪切当前刮号内的东西(先把光标定位的刮号开始位置)</td>
</tr>
<tr>
<td>C-k-y</td><td>复制当前刮号内的东西</td>
</tr>

<tr>
<td>M-k</td><td>他的作用是删除光标所在位置到parent刮号开始位置（有点饶口，自己理解)</td>
</tr>
<tr>
<td>C-M-n</td><td>光标标注当前form(可以理解为刮号),再按，会再标注外层的form,一直到最外层</td>
</tr>

<tr>
<td>C-M-p</td><td>这个和C-M-n相反 ，一直到最里层</td>
</tr>
<tr>
<td>C-M-f</td><td>标注出前面的兄弟form</td>
</tr>
<tr>
<td>C-M-b</td><td>标注出后面的兄弟form</td>
</tr>
<tr>
<td>C-M-d</td><td>标注到下一个form的（</td>
</tr>
<tr>
<td>C-M-a</td><td>标注到最外层form</td>
</tr>

</table>

## 遇到的错误和问题 ##
### 使用emacs ###
1) inferior-lisp-proc: No Lisp subprocess; see variable `inferior-lisp-buffer'
代码是没有问题的，那么怎么解决呢？
M-x nrepl-enable-on-existing-buffers should fix it.


## 有待解决的的问题 ##
1）在emacs live中没有一个 变量在多文件中的替换，查找可以用 Helm<br/>
2) 没有像是代码行对应的todo list (类似eclipse中  tasks ),似乎helm就可以搞定。。。。。但是不是整个project 的，是所有打开的buffer中。自己写elisp??
