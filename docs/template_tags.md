# 14 模板标签

## 14.1 为每个页面提供目录

为每个页面的侧边栏提供目录将会非常的方便快捷.根据以前的经验我们需要做如下:

* 在`base.html`模板里我们需要添加一些代码用来展示侧边栏的目录列表.
* 在每个视图里,我们需要访问目录对象以获取所有目录,并把它返回到模板上下文字典里.

这么做好像有点繁琐.我们需要不断复制粘贴代码.而且当django-registration-redux包的视图要展示目录时会发生错误.所以我们需要一个不同的方法,用`templatetags`来请求所需的数据.

## 14.2 使用模板标签

创建`rango/templatetags`目录并且创建两个文件,一个是空文件`__init__.py`,另一个是`rango_extras.py`并添加代码如下:

```
from django import template
from rango.models import Category

register = template.Library()

@register.inclusion_tag('rango/cats.html')
def get_category_list():
    return {'cats': Category.objects.all()}
```

这里我们创建了一个`get_category_list()`方法,并且连接到了`rango/cats.html`模板.现在我们在模板目录里创建`rango/cats.html`:

```
{% if cats %}
    <ul class="nav nav-sidebar">
    {% for c in cats %}
        <li><a href="{% url 'category'  c.slug %}">{{ c.name }}</a></li>
    {% endfor %}

{% else %}
    <li> <strong >There are no category present.</strong></li>

    </ul>
{% endif %}
```

现在在你的`base.html`使用`{% load rango_extras %}`调用并用`{% get_category_list %}`使用它,例如:

```
<div class="col-sm-3 col-md-2 sidebar">

    {% block side_block %}
    {% loda rango_extras %}
    {% get_category_list %}
    {% endblock %}

</div>
```

>注意:每次修改templatetags你需要重启服务.

## 14.3 参数化模板标签

现在让我们加入自动目录高亮功能.我们需要参数化模板.所以我们需要修改`rango_extras.py`:

```
def get_category_list(cat=None):
    return {'cats': Category.objects.all(), 'act_cat': cat}
```

让我们传递我们所在的目录.我们可以修改`base.html`.

```
<div class="col-sm-3 col-md-2 sidebar">

    {% block side_block %}
    {% get_category_list category %}
    {% endblock %}

</div>
```

现在让我们更新`cats.html`模板:

```
{% for c in cats %}
    {% if c == act_cat %} <li  class="active" > {% else  %} <li>{% endif %}
            <a href="{% url 'category'  c.slug %}">{{ c.name }}</a></li>
{% endfor %}
```

这里检查是否是当前所在的目录页,如果相同就添加高亮(例如`act_cat
`)

重启Django服务并访问页面.我们可以传递`category`变量.当你浏览一个目录页面,模板会访问`catogory`变量并且为`get_category_list()`提供`cat`值.然后`cats.html`会选择哪一个目录高亮.

