# PagingDjango
Django快速分页

> ![分页](http://upload-images.jianshu.io/upload_images/3203841-5fd261312d36cfc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 在web开发中,对大量的商品进行分页显示,是常见的需求,django对分页直接提供了现成的函数,让我们的开发更为快速便捷...


> ![动图_Django快速分页](http://upload-images.jianshu.io/upload_images/3203841-2dbeb83e30083249.gif?imageMogr2/auto-orient/strip)





# 在后端(视图函数中)

```python
from django.shortcuts import render
from .models import ShowMyComputer
# 引入方法
from django.core.paginator import Paginator
# Create your views here.

def show(request, page_id):

    # 获取需要分页的对象集合
    all_goods = ShowMyComputer.objects.all()

    # 创建分页对象
    paginator = Paginator(all_goods, 3)

    # 根据当前页码,确定返回的数据
    current_page = paginator.page(page_id)

    # 保证前端取到的"页数"为整型
    page_id = int(page_id)


    return render(request, 'computer/list.html', locals())

```
#在前端(html模板中)
```html
<body>
    {# 展示当前页面的数据 #}
    {% for goods in current_page %}
    <div class="my_goods">

        <div class="goods_image">       
            ![图片占位](/static/{{ goods.goods_image }})
        </div>
        
        <br>
        
        <div class="goods_name">{{ goods.goods_name }}</div>

    </div>

    {% endfor %}


    <div class="page_num">

    {# 判断'上一页'是否存在,如果存在则保留`上一页`标签 ,反之则不显示`上一页`标签 #}
    {% if current_page.has_previous %}

        <a href="{% url 'computer:show' current_page.previous_page_number %}">上一页</a>

    {% endif %}


    {# 确定分页数量 #}

    {% for index in paginator.page_range %}

        {# 如果页码与当前页面相符,则添加红色背景 #}
    {% if page_id == index %}
        <a href= "{% url 'computer:show' index %}" style="background-color: red" >{{ index }}</a>
        {# 如果页面与当前页面不符,则正常显示 #}
    {% else %}
        <a href="{% url 'computer:show' index %}" >{{ index }}</a>
    {% endif %}

    {% endfor %}

    {# 判断'下一页'是否存在,如果存在则保留`下一页`标签 ,反之则不显示`下一页`标签 #}
    {% if current_page.has_next%}

        <a href="{% url 'computer:show' current_page.next_page_number %}">下一页</a>

    {% endif %}


    </div>

</body>
```

>文章涉及到的资源我会通过百度网盘分享，为便于管理,资源整合到一张独立的帖子，链接如下:
http://www.jianshu.com/p/4f28e1ae08b1