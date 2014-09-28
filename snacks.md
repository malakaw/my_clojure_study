#### take-nth ####
<pre><code>
user=> (take-nth 2 (range 10))
(0 2 4 6 8)
</code></pre>
就像是python的
<pre><code>
ex: >>>['a','b','c','d'][::2]
['a', 'c']
</code></pre>


## coll数据结构 ##
### map 的merge-with ★ ###<br/>
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

### clojure的set /list/map..转换成java 的set/list/map ☆</br/> ###

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

### contains?的坑<br/> ###

contains对于map /set 是我们理解的那个contains，但是vector不是的，他只是存在下标否，真蛋疼，不注意容易被莫名好久。
