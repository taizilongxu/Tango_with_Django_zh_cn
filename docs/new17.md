
>警告:这个版本仍在草稿阶段.尽管它们应当还管用(但是一些链接和截图需要更新).如果发现bug,问题或者其他请求请通过github来提交: https://github.com/leifos/tango_with_django_book/tree/master/17

在这个版本中,我们加入了以下特性:

* 代码更新适配Django1.7版本
    * 数据库交互从`syncdb`更新为`migratesql`和`migrate`
    * Render响应从`render_to_response`更新为`render`,所以现在不用为每个view请求内容.
    * `url`模板标签现在用模板代替,它提供了相对引用而不是绝对引用.
    * 在模板中载入静态文件现在可以用{% load staticfiles %}
    * 用`slugify`来创建组织好的URL字符串
* 增加验证一章
    * 用Django-Registration-Redux来处理登陆和注册(参见第12章)
* Bootstrap章节更新为Bootstrap 3.2.0(参见第13章)
    * 同时讲解一些如何使用Django-Bootstrap-Toolkit的技巧
* 增加使用模板标签一章(参见第14章)
* 增加在Django里使用JQuery一章(参见第18章)
* 关于测试的一章扩展了一些内容-但是仍在修改中(参见第20章)
    * 包括如何用`coverage`包去检测测试覆盖(参见 http://nedbatchelder.com/code/coverage/ )
