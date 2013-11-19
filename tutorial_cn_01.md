优先测试
--------
在写任何真正的生产代码前,先写一些测试！通过编写一些浏览器测试开始（称之为功能测试）,模拟真实的用户将会看到什么，做社么。我们将使用一个Python的测试工具Selenium,它会打开了一个真正的web浏览器,然后它会模拟一个真正的用户,点击链接和按钮,检查屏幕上显示的是什么。这些测试会告诉我们，是否我们的应用程序按照我们想要的来执行。

我们写我们的功能测试我们就要去思考我们的应用程序将展现给用户什么)我们可以开始思考我们如何从技术上实现想要的功能。

幸运的是我们不需要做太多困难的思维,因为功能测试是我们的指导——我们要做什么，才能让功能测试一步步实现?

我们选定了这个函数或类,将解决我们的第一个问题,我们可以为它编写一个单元测试。或者,我们从外面去思考想要什么功能写在单元测试上。

第一部分我们要实现的
------------------
我怎么知道我做得是对的？答案是通过测试！这就是我们的目标。

部署开发环境
-----------
主要信息：Ubuntu 12.04 Python 2.7.3 Django 1.6

首先，需要安装python、pip和virtualenv 或者编译安装

    sudo apt-get install python
    sudo apt-get install python-pip
    sudo pip install virtualenv

然后创建虚拟环境

    virtualenv --no-site-packages ttdenv
    source ttdenv/bin/activate

安装需要的包

    pip install django (default django1.6)
    pip install selenium
    pip install mock

开始第一个项目

    django-admin.py startproject mysite

关于Django，就不做过多介绍，对Django不熟的同学，可以去Django官方文档学习：https://docs.djangoproject.com/en/1.6/

现在，让我们来看一下"mysite"的目录结构：

    `-- mysite
        |-- manage.py
        `-- mysite
            |-- __init__.py
            |-- settings.py
            |-- urls.py
            `-- wsgi.py

如果你到Django是1.4以上版本，你会得到这样到目录结构。

确认有没有成功创建一个Django项目：把runserver跑起来！

    cd mysite
    python manage.py runserver

打开浏览器，输入：http://127.0.0.1:8000,你会看到一个"It worked!"的页面
[img_001_itworked.png]

如果没有成功，或者有什么问题，可以访问Django官方文档学习：https://docs.djangoproject.com/en/1.6/intro/tutorial01/

像现在这样，手动去测试也很好，使用runserver，在接下来到自动测试遇到问题到时候，我也是使用runserver查看问题原因。

使用TDD，是在努力避免手动测试！接下来到目标，是建立一个自动测试。

第一个功能测试：Django admin site
================================

在这部分，我们需要作下面几件事：

    *创建第一个测试FT
    
    *在``settings.py``配置数据库

    *打开站点管理：django admin

    *创建一个admin用户帐号

开始我们的FT。关于测试方法，我们倾向于为一个功能模块编写测试。可以称为“user stories”，每个都有一套与之相关的测试，跟踪用户的潜在行为。

在Django文档到第二部分，介绍Django的站点管理：https://docs.djangoproject.com/en/1.6/intro/tutorial02/

我们的第一个“user stories”就是用户登录到'django admin'创建一个新的'poll'。我们会在后续到测试中配置这些内容，现在我们想要测试的是打开'django admin'页面，我们怎么知道我们成功打开了'django admin'页面呢？对，就是检查一些关键词是不是在页面上出现，像*Django administration*

你是否把你的测试单独放在一个应用呢，还是和生产应用在一起？我的建议是单独创建一个测试应用fts，可以单独运行功能测试，测试多个生产应用，也方便单元测试，让我们创建一个功能测试吧！

    python manage.py startapp fts

创建了一个应用，目录结构就变了：

    mysite
    |-- fts
    |   |-- __init__.py
    |   |-- models.py
    |   |-- tests.py
    |   `-- views.py
    |-- manage.py
    `-- mysite
        |-- __init__.py
        |-- settings.py
        |-- urls.py
        `-- wsgi.py

现在打开文件``fts/tests.py``，写我们到第一个功能测试。您可以删掉Django自带到例子，把下面到代码写进去：

.. sourcecode:: python
    :filename: mysite/fts/tests.py

    from django.test import LiveServerTestCase
    from selenium import webdriver

    class PollsTest(LiveServerTestCase):

        def setUp(self):
            self.browser = webdriver.Firefox()
            self.browser.implicitly_wait(3)

        def tearDown(self):
            self.browser.quit()

        def test_can_create_new_poll_via_admin_site(self):
            # Gertrude opens her web browser, and goes to the admin page
            self.browser.get(self.live_server_url + '/admin/')

            # She sees the familiar 'Django administration' heading
            body = self.browser.find_element_by_tag_name('body')
            self.assertIn('Django administration', body.text)

            # TODO: use the admin site to create a Poll
            self.fail('finish this test')

功能测试是一个类，每个测试是这个类里的方法，只是每个方法都要用``test_``开始。

这样做还不错，告诉Django这是个测试的函数，开发人员看到也知道！

我们用了``LiveServerTestCase``，在Django1.4版本新加的模块，测试到时候，它会调用Djagno到web服务，也就是``runserver``。
