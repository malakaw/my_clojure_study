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
