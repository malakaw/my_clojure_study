不说废话，直接看代码☺;生成代码的代码，我的理解。其实这种情况在clojure下很多的。
先看个简单的，找下感觉
<pre><code>
(defmacro debug [x]
  `(+ 1 ~x (+ 9 ~x) ~'y)
  )
(let [y 9] (debug 3))
返回： 25
</code></pre>


再详细说下
<pre><code>
(defn  addpage_api   [req]
  (info req)
  (conj returnJSON {:body (format  returnCallBack (json/write-str
                                                   (if-let [
                                                            [page_id description method type is_deleted]
                                                            [
                                                             (parse-int  (get (-> req :params) "in_page_id"))
                                                             (get (-> req :params) "in_page_des")
                                                             (get (-> req :params) "in_page_method")
                                                             (parse-int  (get (-> req :params) "in_page_type"))
                                                             (parse-int  (get (-> req :params) "in_page_deleted"))
                                                             ]
                                                            ]

                                                     (vec  (dbclient/addPage page_id description method type is_deleted))
                                                     []
                                                     )
                                                   ))})
)
</code></pre>

在处理request接收参数这块可以说是在同一page模块中基本都是一样的，这么一大块东西，好多地方要调用，写个函数调用吧；可以；但是还有更好的办法，用宏。
为啥？解释下


1）写函数就需要调用操作步骤，宏的作用是在编译的时候把代码自动“嵌入”需要的地方，没有再调用函数的过程。（少一个运行时候的执行步骤？好像还是不能说服你，看第二点吧）
2）函数的话是需要传参数的，那么你要考虑返回值，然后还有传入参数的顺序和定义函数中参数的顺序一致，反正这些都是需要费心留意的；宏就没有这些问题；他就是你当前函数中的代码，甚至没有传参数这会事情，岂不是很爽。


定义宏
<pre><code>
(defmacro paramsProcess []
  `[
   ~'(parse-int  (get (-> req :params) "in_page_id"))
   ~'(get (-> req :params) "in_page_des")
   ~'(get (-> req :params) "in_page_method")
   ~'(parse-int  (get (-> req :params) "in_page_type"))
   ~'(parse-int  (get (-> req :params) "in_page_deleted"))
   ]
  )
</code></pre>

使用宏
<pre><code>
(defn  addpage_api   [req]
  (info req)
  (conj returnJSON {:body (format  returnCallBack (json/write-str
                                                   (if-let [
                                                            [page_id description method type is_deleted]
                                                            (paramsProcess)
                                                            ]
                                                     {:oper (dbclient/addPage page_id description method type is_deleted)}
                                                     {:oper "fail"}
                                                     )
                                                   ))})
)

</code></pre>
