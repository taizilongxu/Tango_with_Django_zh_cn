# 12 使用Django-Registration-Redux进行用户验证

在Django里有许多的应用提供登录,注册和验证机制.许多的应用都提供这种功能而且几乎不重写url,视图和模板.在这章,我们需要使用`django-registration-redux`包来提供这些功能.这就意味着我们需要重构我们的代码 - 然而它将会教给我们如何使用外部应用以及如何轻松的在我们Django项目里加入应用.它也会使我们的应用更加简洁.

>注意:这章并不是必要的,你可以跳过,但是在随后的章节里我们会假设你已经更新了验证机制.

## 12.1 设置Django-Registration-Redux

开始我们先安装`django-registration-redux`:

```
$ pip install django-registration-redux
```

安装好后,我们需要告诉Django我们使用这个应用.打开`settings.py`文件,修改`INSTALLED_APPS`元组:

```
INSTALLED_APPS = (
'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
'rango',
'registration', # add in the registration package
)
```

同时在`settings.py`文件你可以加入:

```
REGISTRATION_OPEN = True                # If True, users can register
ACCOUNT_ACTIVATION_DAYS = 7     # One-week activation window; you may, of course, use a different value.
REGISTRATION_AUTO_LOGIN = True  # If True, the user will be automatically logged in.
LOGIN_REDIRECT_URL = '/rango/'  # The page you want users to arrive at after they successful log in
LOGIN_URL = '/accounts/login/'  # The page users are directed to if they are not logged in,
                                                                # and are trying to access pages requiring authentication
```

上面这些设置都带了注释.现在在`tango_with_django_project/urls.py`里修改`urlpatterns`以增加注册包的引用:

```
urlpatterns = patterns('',

url(r'^admin/', include(admin.site.urls)),
url(r'^rango/', include('rango.urls')),
(r'^accounts/', include('registration.backends.simple.urls')),
)
```

`django-registration-redux`包提供了许多不同的注册后台供你选择.在`registration.backend.simple.urls`,它提供了:

* registration -> /accounts/register/
* registration complete -> /accounts/register/complete
* login -> /accounts/login/
* logout -> /accounts/logout/
* password change -> /password/change/
* password reset -> /password/reset/

在`registration.backends.default.urls`里也提供了两步激活账户的功能:

* 全面激活(使用两步注册) -> `ctivate/complete/`
* 激活(如果账户激活失败) -> `ctivate/<activation_key>/`
* 激活邮件(通知用户激活邮件已经发送)
    * 激活邮件主体(文本文件,包含激活邮件的文本)
    * 激活邮件标题(文本文件,包含激活文件的标题)

注意Django Registration Redux只提供功能,没有提供模板.所以我们需要自己写模板来连接每个视图.

## 12.3 设置模板

在这篇  https://django-registration-redux.readthedocs.org/en/latest/quickstart.html 快速入门里只提供了一般的模板需求,没有清楚的说明每个模板都应当包含什么.

在这里我们可以下载Anders Hofstee的Github项目,https://github.com/macdhuibh/django-registration-templates ,你可以在这里找到详细的模板.我们将使用这个模板作为我们的指导.

首先,我们需要在`templates`目录新建一个`registration`目录.这里将存储所有关于 Django Registration Redux应用的页面.

### 12.3.1 登录模板

在`templates/registration`创建`login.htnl`文件,代码如下:

```
{% extends "rango/base.html" %}

{% block body_block %}
<h1>Login</h1>
        <form method="post" action=".">
                {% csrf_token %}
                {{ form.as_p }}

                <input type="submit" value="Log in" />
                <input type="hidden" name="next" value="{{ next }}" />
                </form>

        <p>Not  a member? <a href="{% url 'registration_register' %}">Register</a>!</p>
{% endblock %}
```

注意每个url引用都要用`url`模板标签来引用.如果你访问  http://127.0.0.1:8000/accounts/ 你就会看到绑定的url列表,每个列表后边都有它的名字.

### 12.3.2 注册模板

在`templates/registration`创建`registration_form.html`文件:

```
{% extends "rango/base.html" %}


{% block body_block %}
<h1>Register Here</h1>
        <form method="post" action=".">
                {% csrf_token %}
                {{ form.as_p }}

                <input type="submit" value="Submit" />
        </form>
{% endblock %}
```

### 12.3.3 注册完成模板

在`templates/registration`创建`registration_complete.html`文件:

```
{% extends "rango/base.html" %}


{% block body_block %}
<h1>Registration Complete</h1>
        <p>You are now registered</p>
{% endblock %}
```

### 12.3.4 注销模板

在`templates/registration`目录创建`logout.html`文件:

```
{% extends "rango/base.html" %}


{% block body_block %}
<h1>Logged Out</h1>
        <p>You are now logged out.</p>
{% endblock %}
```

### 12.3.5 尝试注册

运行Django服务器,访问:  http://127.0.0.1:8000/accounts/register/

注意到注册表单包含两个密码字段 - 它可以进行检车.尝试输入不同的密码.

它能够正常的运行,但并不是所有的东西都做好了,我们还需要一些逻辑代码.

### 12.3.6 重构项目

现在需要修改`bashe.html`来使用心得注册url/视图:

* 修改注册url为`<a href="{% url 'registration_register' %}">`
* 登录为`<a href="{% url 'auth_login' %}">`
* 注销为`<a href="{% url 'auth_logout' %}?next=/rango/">`
* 修改`settings.py`中的`LOGIN_URL`为`/accounts/login/`.

注意到注销页面我们包含了`?next=/rango/`.这就是为什么当我们注销时会重定向到rango主页.如果我们不加入它,我们将会重定向到注销页面(但是这样做不太友好).

下面就是解绑`rango`应用的`register`,`login`,`logout`的功能,例如删除urls,视图和模板(或者把它们注释掉)

### 12.3.7 修改注册流程

这时候如果用户进行注册,完成后它会重定向到注册完成页面.这有些笨重,我们可以直接重定向到主页.我们可以通过重写`registration.backends.simple.views`的`RegistrationView`来实现这个功能.好了,现在在`tango_with_django_project/urls.py`文件里引入`RegistrationView`,增加一个新的注册类,然后更新`urlpatterns`如下:

```
from registration.backends.simple.views import RegistrationView

# Create a new class that redirects the user to the index page, if successful at logging
class MyRegistrationView(RegistrationView):
    def get_success_url(self,request, user):
        return '/rango/'


urlpatterns = patterns('',
    url(r'^admin/', include(admin.site.urls)),
    url(r'^rango/', include('rango.urls')),
        #Add in this url pattern to override the default pattern in accounts.
    url(r'^accounts/register/$', MyRegistrationView.as_view(), name='registration_register'),
    (r'^accounts/', include('registration.backends.simple.urls')),
)
```

TODO(leifos):增加一个定制注册表单...

## 12.4 练习

* 为用户提供修改密码的功能.

