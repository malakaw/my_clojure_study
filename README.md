# 学习clojure笔记 #

##使用emacs开发clojure  ##

## 遇到的错误和问题 ##
### 使用emacs ###
1) inferior-lisp-proc: No Lisp subprocess; see variable `inferior-lisp-buffer'
代码是没有问题的，那么怎么解决呢？
M-x nrepl-enable-on-existing-buffers should fix it.
