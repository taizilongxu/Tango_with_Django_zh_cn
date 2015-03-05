# 5 模板和静态媒体

在本章里你将会学到模板引擎的知识同时也会学习怎么在你的web页面添加静态媒体.

## 5.1 使用模板

为了创建一个Django网页,我们需要把许多东西都集中到一起.现在我们有两个视图以及一些连接它们的映射.下面我们将学习如何把模板加入到里面.

设计优秀的网站会重用他们的布局结构.你是否看到过同样header和footer的页面,这些重复布局会提高网站的组织性同时会给人连续感.Django提供了与应用逻辑上可分的模板来为开发者更容易的实现这样的设计目标.在本章,你将会创建一个模板来创建一个HTML页面.这个模板将会传递给Django的视图.在第7章,我们会用模板做更复杂的事情.

### 5.1.1 设置模板目录

为了建立和启动模板,你需要设置一个储存模板的目录.

在你的Django项目目录里(例如`<workspace>/tango_with_django_project/)`),创建一个新的目录叫做`templates`.在这个目录里创建另一个`rango`目录.所以我们将在`<workspace>/tango_with_django_project/templates/rango/`目录里存放关于`rango`应用的模板.

为了告诉Django我们的模板在哪里,我们需要修改项目的`settings.py`文件.找到`TEMPLATE_DIRS`并把创建的`templates`目录路径加入到里面.

```
TEMPLATE_DIRS = ['<workspace>/tango_with_django_project/']
```

注意你需要提供`templates`目录的绝对路径.如果你是一个团队的一员或者工作在不同的电脑上,这在将来可能出现问题.你可以有不同的用户名这意味着你`<workspace>`目录有不同的路径.如果是硬编码路径的话上面的路径在不同电脑上是不同的.当然,你需要为不同的设置增加不同目录,难道没有什么好的方法了吗?

>警告:硬编码路径绝逼带你通往地狱.硬编码在软件工程类是绝对抵制的一种模式,它会让你的项目不利于移植.

### 5.1.2 动态路径

解决硬编码路径问题的方法是利用Python内建函数来动态的创建`teemplates`目录的路径.通过这个方法你不必考虑你的Django项目位置就能获取它的绝对路径.这将利于你的项目代码的移植.

在Djang1.7的`settings.py`文件里包含了一个变量`BASE_DIR`.这个变量会保存`settings.py`文件的路径.这里面用了一个特殊的`___file__`属性,它能获取模块的绝对路径.然后通过调用`os.path.dirname()`来提供绝对路径的目录.再次调用`os.path.dirname()`我们回得到上层的目录,所以`BASE_DIR`内容是` <workspace>/tango_with_django_project/`.如果你还是很好奇的话你可以输入下面的命令.

```
print __file__
print os.path.dirname(__file__)
print os.path.dirname(os.path.dirname(__file__))
```

现在让我们使用它看一看.在`setting.py`中创建`TEMPLATE_PATH`变量,用它来储存`templates`目录.这里我们使用`os.path.join()`函数.

```
TEMPLATE_PATH = os.path.join(BASE_DIR, 'templates')
```

我们用`os.path.join()`函数来连接`BASW_DIR`变量和`templates`,它将返回`<workspace>/tango_with_django_project/templates/`.我们可以为`TEMPLATE_DIRS`添加`TEMPLATE_PATH`,就像下面一样.

```
TEMPLATE_DIRS = [
    # Put strings here, like "/home/html/django_templates" or "C:/www/django/templates".
    # Always use forward slashes, even on Windows.
    # Don't forget to use absolute paths, not relative paths.
    TEMPLATE_PATH,
]
```

我们可以吧`TEMPLATE_PATH`放在`settings.py`模块的开头以方便我们对它的修改.这就是为什么我们创建变量来存储模板路径.

>警告:当连接系统路径的时候用`os.path.join()`是一个比较好的方法.用这种方法可以保证依据你的操作一同来添加正确的分隔符.在POSIX兼容的操作系统上,斜杠用来分割目录,而在Windows系统里用的是反斜杠.如果你手动为路径添加分隔符,那么你可能会在别的操作系统上操作时印发路径错误.

### 5.1.3 添加模板

在我们创建模板目录和设置好路径以后,我们需要在`template/rango/`目录里建立一个叫做`index.html`的文件,在新文件里加入下面代码:

```
<!DOCTYPE html>
<html>

    <head>
        <title>Rango</title>
    </head>

    <body>
        <h1>Rango says...</h1>
        hello world! <strong>{{ boldmessage }}</strong><br />
        <a href="/rango/about/">About</a><br />
    </body>

</html>
```

这个HTML代码创建一个问候用户的简单HTML页面.你可能注意到了在上面有一段非HTML代码`{{boldmessage}}`.这是Django模板的变量,他可以在输出时为这个变量赋值.一会我们就会用它.

为了使用这个模板,我们需要重新修改一下我们先前创建的`index()`视图.这次我们不让他传递简单的信息,而是让他返回我们的模板.

在`rango/views.py`里,确定文件头部有如下代码.

```
from django.shortcuts import render
```

修改`index()`视图函数如下,注释解释每行作用.

```
def index(request):

    # Construct a dictionary to pass to the template engine as its context.
    # Note the key boldmessage is the same as {{ boldmessage }} in the template!
    context_dict = {'boldmessage': "I am bold font from the context"}

    # Return a rendered response to send to the client.
    # We make use of the shortcut function to make our lives easier.
    # Note that the first parameter is the template we wish to use.

    return render(request, 'rango/index.html', context_dict)
```

首先我们建立一个在模板中使用的字典,然后我们调取`render()`函数.这个函数接受用户的`request`,模板名称和内容字典作为参数.这个`render()`函数将会把这些参数聚合到一起生成一个完整的HTML页面.然后返回给用户的浏览器.

当模板文件被加载到Django模板系统里时,它模板内容也会被创建.在简单的例子里模板的内容是字典里的模板变量对应的Python变量.在我们早先创建的模板文件,我们创建了一个叫做`boldmessage`的模板变量.在`index(request)`视图例子中,字符串`I am bold font from the context`映射到模板变量`boldmessage`.所以字符串`I am bold font from the context`将会替换模板里所有的`{{ boldmessage }}`.

现在你已经更新了视图,运行Django服务并访问 http://127.0.0.1:8000/rango/ .你讲会看到如下图所示.

![](img/rango-hello-world-template.png)

如果你没有看到上图,试着读读错误日志重新检查一下刚才修改的文件.确定所有所有的更改都正确.通常都是在`settings.py`文件中设置模板路径产生的错误.有时候需要添加`print`语句来输出`BASE_DIR`和`TEMPLATE_PATH`,确保它们俩设置的正确性.

这个例子展示了在视图里如何使用模板.然而我们仅仅接触了一些Django模板方面的功能.在我们的教程里将会接触到更复杂的模板应用.同时你可以参考templates from the official Django documentation.

## 5.2 提供静态媒体

现在Rango网站确实比较原始,没有样式也没有图片.为了增加样式和引入动态行为我们可以在我们的网站里加入CSS,Javascript和图像这些静态媒体.这些文件和网页有一些不同.这是因为它们不想HTML页面是生成出来的.这章节将会教你如何在Django项目里设置静态媒体.我们将会为我们的模板添加一些静态媒体.

### 5.2.1 设置静态媒体目录

为了设置静态媒体,你需要设立存储它们的目录.在你的项目目录(例如`<workspace>/tango_with_django_project/`),创建叫做`static`的目录.在`static`里再创建一个`images`目录.

现在在`static/images`目录里放置一个图片.如下图所示我们选择一张变色龙的图案来做我们项目的吉祥物.

![](img/rango-picture.png)

就像先前``templates`目录一样我们需要告诉Django我们创建了`static`目录.

在`settings.py`文件,我们需要更新两个变量`STATIC_URL`和`STATICFILES_DIRS`元组,像下面一样创建一个储存静态目录(`STATIC_PATH`)的变量.

```
STATIC_PATH = os.path.join(BASE_DIR,'static')

STATIC_URL = '/static/' # You may find this is already defined as such.

STATICFILES_DIRS = (
    STATIC_PATH,
)
```

第一个变量`STATIC_URL`定义了当Django运行时Django应用寻找静态媒体的地址.例如,像我们上面的代码一样吧`STATIC_URL`设置成`/static/`,我们就可以通过`http://127.0.0.1:8000/static/`来访问它了. official documentation on serving up static media 警告我们要注意斜杠的书写.如果不这么设置将会引起一大堆麻烦.

`STATIC_URL`定义了web服务链接媒体的URL地址,`STATICFILES_DIRS`允许你定义新的`static`目录.像`TEMPLATE_DIRS`元组一样.`STATICFILES_DIRS`需要`static`目录的绝对路径.这里,我们重新用5.1章的`BASE_DIR`变量来创建`STATIC_PATH`.

完成了这两个设置后,再一次运行你的Django服务.如果我们想要查看我们的Rango图片,访问`http://127.0.0.1:8000/static/images/rango.jpg`.如果没有出现请查看`setings.py`文件是否设置正确,并重启服务.如果出现了,试着加入其他类型的文件到`static`目录并在浏览器上访问他们.

>小心:在开发环境中可以你可以用这种方法来提供静态媒体文件,但是在生产环境就不太适合了. official Django documentation on Deploymen提供了在生产环境如何部署静态文件.

## 5.3 静态媒体文件和模板

现在你已经为你的Django项目设置了静态媒体,你可以在你的模板里加入这些媒体.

下面将展示hi如何加入静态媒体,打开位于`<workspace>/templates/rango`目录的`index.html`文件.像下面一样修改HTML源代码.新加入两行用注释标示.

```
<!DOCTYPE html>

{% load staticfiles %} <!-- New line -->

<html>

    <head>
        <title>Rango</title>
    </head>

    <body>
        <h1>Rango says...</h1>
        hello world! <strong>{{ boldmessage }}</strong><br />
        <a href="/rango/about/">About</a><br />
        <img src="{% static "images/rango.jpg" %}" alt="Picture of Rango" /> <!-- New line -->
    </body>

</html>
```

首先,我们需要使用`{% load static %}`标签来使用静态媒体.所以我们才可以用`{% static "rango.jpg" %`在模板里调用`static`文件.Django模板标签用`{ }`来表示.在这个例子里我们用`static`标签,它将会把`STATIC_URL`和`rango.jpg`连接起来,如下所示.

```
<img src="/static/images/rango.jpg" alt="Picture of Rango" /> <!-- New line -->
```

如果因为什么原因图片不能加载我们可以用一些文本来代替.这就是`alt`属性的作用 - 如果图片加载失败就显示`alt`属性中的文本.

好了,让我们再次运行Django服务访问`http://127.0.0.1:8000/rango`.幸运的话可以看到下图.

![](img/rango-site-with-pic.png)

当你希望在模板里使用静态媒体你需要调用`{% static %}`函数.下面的代码将展示给你如何在你的模板里添加Javascript,CSS和图片.

```
<!DOCTYPE html>

{% load static %}

<html>

    <head>
        <title>Rango</title>
        <link rel="stylesheet" href="{% static "css/base.css" %}" /> <!-- CSS -->
        <script src="{% static "js/jquery.js" %}"></script> <!-- JavaScript -->
    </head>

    <body>
        <h1>Including Static Media</h1>
        <img src="{% static "images/rango.jpg" %}" alt="Picture of Rango" /> <!-- Images -->
    </body>

</html>
```

很显然你需要引入的静态文件需要在你的`static`目录里.如果文件不存在或者引用错误的话,控制台将会输出错误.试试看引入一个不存在的文件将会发生什么.

为了进一步了解静态媒体可查看Django documentation on working with static files in templates.

>小心:在你模板的第一行确保文档类型声明(例如`<!DOCTYPE html>`).这就是为什么我们需要把`{% load static %}`放在文档类型声明后面而不是前面.根据HTML/XHTML不同文档类型对象的声明也有稍许不同.很显然Django命令会在模板输出的时候删除,但是删除之后的命令会留下一些空白意味着你的输出将得不到W3C认证服务的验证.

TODO(leifos):注意当你部署项目的时候最好不要这样做,你可以查询https://docs.djangoproject.com/en/1.7/howto/static-files/deployment/ 

TODO(leifos):DEBUG变量在`settings.py`中,设置它可以让我们控制是否输出错误信息.当把`DEBUG`设置成True时对于应用的部署并不安全.当你把`DEBUG`设置成False,你得需要把`settings.py`中的`ALLOWED_HOSTS`变量设置成`127.0.0.1`以便可以在本地主机上运行.你需要修改项目里的`urls.py`文件:

```
from django.conf import settings # New Import
from django.conf.urls.static import static # New Import


if not settings.DEBUG:
        urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

TODO(leifos):我们将在部署章节详细的介绍.

## 5.4 静态媒体服务

现在你可以使用静态文件,让我们看看如何上传媒体.许多网站提供了上传图像的功能.这章我们将展示如何为你的Django项目开发一个简单的媒体服务.在开发媒体服务中需要用到的文件上传表格我们将会在第9章里接触到.

那么,我们怎么样才能开发媒体服务呢?第一步是需要在我们Django项目根目录里(比如` <workspace>/tango_with_django_project/`)创建一个新的目录,名字叫`media`.这个目录就在`templates`和`static`同级目录里.在你创建目录后,需要要修改位于设置目录(例如` <workspace>/tango_with_django_project/tango_with_django_project/`)里的`urls.py`文件.修改如下.

```
# At the top of your urls.py file, add the following line:
from django.conf import settings

# UNDERNEATH your urlpatterns definition, add the following two lines:
if settings.DEBUG:
    urlpatterns += patterns(
        'django.views.static',
        (r'^media/(?P<path>.*)',
        'serve',
        {'document_root': settings.MEDIA_ROOT}), )
```

通过导入`django.conf`的`settings`模块我们可以得到我们项目里`settings.py`文件的变量.接下来的条件判断语句判断Django是否处在DEBUG模式.如果`DEBUG`被设定为`True`,那么就会在`urlpatterns`加入URL匹配模式.所有以`media/`开头的青豆都会传递给`django.views.static`视图.这个视图将会把上传的媒体传给你.

在修改了`urls.py`文件后,我们需要修改`settings.py`文件.我们需要创建`MEDIA_URL`和`MEDIA_ROOT`两个变量.

```
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media') # Absolute path to the media directory
```

第一个变量`MEDIA_URL`定义了基地址.如果把`MEDIA_URL`设置为`/media/`意味着上传URL为`http://127.0.0.1:8000/media/`.`MEDIA_ROOT`用来告诉Django你的上传文件保存在电脑的哪个位置.在上边的例子中,我们用5.1章节设置的`PROJECT_PATH`变量和`/media/`连接.就变成了绝对路径`<workspace>/tango_with_django_project/media/`.

>小心:和前面提到的一样,Django提供的开发媒体服务对debug非常有帮助.但是它不能用在生产环境中. Django documentation on static files警告这样做"低效率且不安全".如果你需要部署你的Django项目,你需要阅读文档选择更安全的方法解决上传文件的问题.

你可以在`media`目录里放入一个图片来检查它是否工作.启动Django服务,在浏览器里访问图片.例如如果你在`media`中加入`rango.jpg`文件,你需要访问的URL看起来是这样的`http://127.0.0.1:8000/media/rango.jpg`.你会看到你的图片.如果你没看到,你需要回去检查设置是否错误.

## 5.5 基本流程

学完这章,你应当学会如何设置和创建模板,在你的视图里使用模板,设置和使用Django来发送静态媒体文件,包括模板里的图片和为了文件上传设置Django静态媒体服务.我们已经学了很多了!

创建模板并在视图里使用是这章的关键.它需要一些步骤,但是当你尝试几次后就非常容易掌握了.

1. 首先,创建你希望使用的模板并把它保存在`templates`目录里,这个目录需要你写入`settings.py`文件.你可以在模板里使用Django模板变量(例如`{{ bariable_name }}`).你可以在视图里更换这些变量.
2. 在应用的`views.py`文件里查找或者创建一个新的视图.
3. 增加视图逻辑.例如你可以从数据库里获得数据.
4. 在视图里,创建一个字典对象可以吧模板内容传递给模板引擎.
5. 使用`render()`函数来生成返回.确保引用request,然后是模板文件,最后是内容字典!
6. 如果你还没有修改`urls.py`文件或者应用中的`urls.py`中的映射,你需要修改一下.

在你的页面上获取一个静态媒体文件也是一个你需要掌握的很重要的步骤.

1. 把你要添加的静态文件放入`static`目录.这个目录是你在`settings.py`中设置的`STATICFILES_DIRS`元组.
2. 在模板中添加静态媒体引用.例如一个HTML网页的图片用`<img />`标签.
3. 记得用`{% load staticfiles %}`和`{% static "filename" %}`命令在模板中设置静态文件.

下一章我们将会学习数据库.我们将学习如何用Django出色的数据库层来帮助我们摆脱繁琐!

## 5.6 练习

下面两个练习来巩固本章.

* 把about页面也用一个`about.html`模板来设置.
* 在`about.html`模板里,在你的静态媒体里增加图片.

