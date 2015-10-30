
理解querySet

### 触发数据库

queryset 是懒加载的，多个query方法链接并不会导致多次数据库查询，他们会被智能的合并成一句，并在需要的时候触发

一般只有发生这些动作才会触发数据库行为

+ 迭代： for e in Entry.objects.all(): .....
+ 切片：一般切片不求值，但带有步长的切片求值：result[0:10:2]
+ 序列化
+ 缓存
+ repr(queryset)
+ len(queryset) 对queryset求长度最好用 filter 标签 count()
+ list(queryset)
+ bool 求值： if Entry.objects.filter(headline="Test")

总之，一般来讲当值用到的时候，queryset就回去数据库求值

### queryset 结果缓存

这里的缓存指的是django模型是不是会再次出发sql

重复的语句知行多次，会重复出发sql执行


> print([e.headline for e in Entry.objects.all()])

> print([e.pub_date for e in Entry.objects.all()])


这里执行了两遍


> queryset = Entry.objects.all() #将语句赋值给一个变量

> print([p.headline for p in queryset]) #只在这里触发一次sql求值.

> print([p.pub_date for p in queryset]) #这里直接用如上结果


但有两种例外，就是切片后的语句，角标或者范围


> queryset = Entry.objects.all()

> print queryset[5] #执行sql

> print queryset[0:5] #执行sql


