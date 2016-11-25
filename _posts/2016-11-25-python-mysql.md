---
layout: post
title: Python 3.5连接mysql
excerpt: ""
categories: [python]
comments: true
---

## pymysql模块安装

python3.5可以用命令pip install pymysql安装模块

## 例子

*	引用模块

{% highlight python %}
import pymysql;
{% endhighlight %}

*	连接数据库

{% highlight python %}
conn = pymysql.connect(
    host = 'localhost',
    port = 3306,
    user = 'root',
    passwd = '********',
    db = 'test',
    charset = 'utf8');
{% endhighlight %}

*	获取cursor

{% highlight python %}
cursor = conn.cursor();
{% endhighlight %}

*	创建、插入表

{% highlight python %}
cursor.execute("create table SolarSystem(name char(20) not null, diameter int(10))");

nr_rows = cursor.execute("insert into SolarSystem (name,diameter) values('Earth',12756),('Mars',6794)");
print ("Add %d records" % nr_rows)
# 提交，不然无法保存新建或者修改的数据
conn.commit()
{% endhighlight %}

*	查询表

{% highlight python %}
# 返回行数
nr_rows = cursor.execute("select name from SolarSystem");
print ("%d selected" % nr_rows)
# 获取第一行数据
row = cursor.fetchone()
print (row)

nr_rows = cursor.execute("select name from SolarSystem");
# 获取所有行数据
rows = cursor.fetchall()
print (rows)
for row in rows:
    print (row)

nr_rows = cursor.execute("select name,diameter from SolarSystem");
# 获取指定行数据
rows = cursor.fetchmany(nr_rows);
print(rows)
for row in rows:
    print (row)
{% endhighlight %}

*	关闭句柄 

{% highlight python %}   
cursor.close()
conn.close()
{% endhighlight %}


