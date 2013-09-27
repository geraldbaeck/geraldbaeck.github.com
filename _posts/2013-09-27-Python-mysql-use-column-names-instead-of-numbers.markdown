---
layout: post
title:  "Python use MySQL column names instead of numbers"
date:   2013-09-27
categories: python
---
{% include JB/setup %}

Instead of:
{% highlight python %}
cursor = conn.cursor()
cursor.execute("SELECT name, value FROM table")
result_set = cursor.fetchall()
for row in result_set:
    print "%s, %s" % (row[0], row[1])
{% endhighlight %}

Just do this:
{% highlight python %}
cursor = conn.cursor(MySQLdb.cursors.DictCursor)
cursor.execute("SELECT name, value FROM table")
result_set = cursor.fetchall()
for row in result_set:
    print "%s, %s" % (row["name"], row["value"])
{% endhighlight %}

Nice to know and saved 3 lines of code!-)