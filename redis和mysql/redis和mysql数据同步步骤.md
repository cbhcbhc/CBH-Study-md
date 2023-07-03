https://blog.csdn.net/qq_40464599/article/details/119718639

使用redis做缓存，以普通[web项目](https://so.csdn.net/so/search?q=web项目&spm=1001.2101.3001.7020)来举例。我们一般将用户访问频繁，且修改频度低的数据放在缓存中，以提高响应速度。在前端发来访问请求时，我们一般进行以下逻辑操作：

1.查询操作：

前端发来请求时，先进行缓存的查询，如果缓存存在要查询的数据，则返回。否则去数据库中查询，并添加到缓存中，再返回数据，这样在下次查询时，便可直接从缓存中取。

![img](redis%E5%92%8Cmysql%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%E6%AD%A5%E9%AA%A4.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FfY2FyYW1lbA==,size_16,color_FFFFFF,t_70.png)

==读操作的时候用上面这个没问题==

==写操作时要注意延迟双删==：https://blog.csdn.net/UCLoveLikeTheWind/article/details/125213485

![双删](redis%E5%92%8Cmysql%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%E6%AD%A5%E9%AA%A4.assets/3f635c330d514aa2bb0858a64c8ad75c.png)



2.添加操作：

添加操作我们直接添加到数据库即可，也可以在添加到缓存的同时添加到数据库。但在数据量较大时，推荐的做法是先将数据添加到缓存，在另一个线程中将数据同步到数据库。

![img](redis%E5%92%8Cmysql%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%E6%AD%A5%E9%AA%A4.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FfY2FyYW1lbA==,size_16,color_FFFFFF,t_70-16883538526343.png)





3.修改操作：

修改操作先修改数据库，再将缓存的数据删除即可，不要直接更新缓存，效率太低。

![img](redis%E5%92%8Cmysql%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%E6%AD%A5%E9%AA%A4.assets/20200305104153517.png)