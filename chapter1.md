优先测试
========
在写任何真正的生产代码前,先写一些测试！通过编写一些浏览器测试开始（称之为功能测试）,模拟真实的用户将会看到什么，做社么。我们将使用一个Python的测试工具Selenium,它会打开了一个真正的web浏览器,然后它会模拟一个真正的用户,点击链接和按钮,检查屏幕上显示的是什么。这些测试会告诉我们，是否我们的应用程序按照我们想要的来执行。

我们写我们的功能测试我们就要去思考我们的应用程序将展现给用户什么)我们可以开始思考我们如何从技术上实现想要的功能。

幸运的是我们不需要做太多困难的思维,因为功能测试是我们的指导——我们要做什么，才能让功能测试一步步实现?

我们选定了这个函数或类,将解决我们的第一个问题,我们可以为它编写一个单元测试。或者,我们从外面去思考想要什么功能写在单元测试上。

第一部分我们要实现的
====================
我怎么知道我做得是对的？答案是通过测试！这就是我们的目标。

部署开发环境
============
主要信息：Ubuntu 12.04   Python 2.7.3  Django 1.6

首先，需要安装python、pip和virtualenv
或者编译安装

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

对Django不熟的同学，可以去Django官方文档学习：https://www.djangoproject.com/en/1.6/
    