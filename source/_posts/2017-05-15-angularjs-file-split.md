---
layout: single
classes: wide
title: 关于angularjs的controller依赖和文件分离
date: 2016-07-07 22:56:57 +0800
categories:
- FE
tags:
- javascript
- angularjs
---

# 关于angular
关于angular我想不用做太多介绍了，google出的超级框架，主要nb点是双向数据绑定和指令系统，angular2目前还在rc4阶段，虽然做过一些研究，无奈相关的UI组件实在有限（尤其对xx管理系统需要的treeview、grid等），所以目前还是以angular1作为主力。
关于angularjs的更多介绍，可以参见
- [angularjs官方网站](https://angularjs.org)
- [大漠穷秋在慕课网上的教程](http://www.imooc.com/learn/156)

---
<!--more-->
# 一般anguarjs的加载方式
最为常见的angularjs的加载方式采用在html标签加ng-app="my-app"，下面是一个典型的angular应用，带ui-router：
```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
    <script src="../../node_modules/angular/angular.js"></script>
    <script src="../../node_modules/angular-ui-router/release/angular-ui-router.js"></script>
    <script src="App.js"></script>

</head>
<body data-ng-app="myApp">
<h1>AngularJS Home Page (Ui-router Demonstration)</h1>
<div data-ui-view="" ng-controller="mianController"></div>
</body>
<html>
```

在这种情况下，一般通过引用不同controller的js来加载不同的页面，当controller变多以后，是下面一种情况：

```html
 <!-- 外部 JS 包引用 -->
    <script src="bowers/jquery/dist/jquery-1.9.1.js"></script>
    <script src="bowers/bootstrap/dist/js/bootstrap.js"></script>
    <script src="scripts/libs/ace.min.js"></script>
    <script src="bowers/angular/angular.js"></script>
    <script src="scripts/libs/angular-locale_zh-cn.js"></script>
    <script src="bowers/angular-bootstrap/ui-bootstrap-tpls.js"></script>
    <script src="bowers/angular-resource/angular-resource.js"></script>
    <script src="bowers/angular-cookies/angular-cookies.js"></script>
    <script src="bowers/angular-sanitize/angular-sanitize.js"></script>
    <script src="bowers/angular-route/angular-route.js"></script>
    <script src="bowers/ng-grid/ng-grid-2.0.13.min.js"></script>
    <script src="bowers/sweetalert/angular-sweetalert.min.js"></script>
    <script src="bowers/sweetalert/sweetalert.min.js"></script>

    <!-- 公用功能  -->
    <script src="scripts/libs/ocLazyLoad.js"></script>
    <script src="scripts/application.js"></script>
    <script src="scripts/libs/Dialog/dialogs.js"></script>

    <script src="scripts/services/alertMsg.js"></script>
    <script src="scripts/services/angularLocalStorage.js"></script>
    <script src="scripts/libs/angular-file-upload.js"></script>
    <script src="scripts/services/pmglobal.js"></script>
    <script src="scripts/directives/PageChange.js"></script>
    <script src="scripts/directives/PageFooter.js"></script>
    <script src="scripts/services/auth.js"></script>
    <script src="scripts/directives/chosen.js"></script>
    <script src="scripts/directives/chosen.jquery.js"></script>
    <!--这儿添加自己的JS引用-->
    <script src="scripts/controllers/HomeCtr/HomeCtr.js"></script>
    <script src="scripts/controllers/Modules/Modules.js"></script>
    <script src="scripts/controllers/WUBC/WUBC.js"></script>
    <script src="scripts/controllers/BasicData/BasicData.js"></script>
    <script src="scripts/controllers/Organizations/Organizations.js"></script>
    <script src="scripts/controllers/ReceiveNotice/ReceiveNotice.js"></script>
    <!--<script src="scripts/controllers/Activist/ActivistCtr.js"></script>-->
    <!--<script src="scripts/controllers/KeyObject/KeyObjectCtr.js"></script>-->
    <script src="scripts/controllers/History/HistroyCtr.js"></script>
    <script src="scripts/controllers/HistoryUser/HistoryUserCtr.js"></script>
    <script src="scripts/controllers/Praises/Praises.js"></script>
    <script src="scripts/controllers/Trains/Trains.js"></script>
    <script src="scripts/controllers/Corps/Corps.js"></script>
    <script src="scripts/controllers/OutOrJoin/OutOrJoin.js"></script>
    <script src="scripts/controllers/UserGroup/UserGroup.js"></script>
    <script src="scripts/controllers/ProveLetter/ProveLetter.js"></script>
    <script src="scripts/controllers/JoinPartyNotice/JoinPartyNoticeCtr.js"></script>
    <script src="scripts/controllers/PartyUser/PartyUserCtr.js"></script>
    <script src="scripts/controllers/RecommendExcellent/RecommendExcellentCtl.js"></script>
    <script src="scripts/controllers/PersonReviews/PersonReviews.js"></script>

```

是不是很爽:P，js管理会带来相当大的复杂性，这对稍微大型的应用来说是不可接受的，所以我们需要按需加载。

---

# 按需加载
js按需加载的工具很多，常见的有requirejs、seajs、systemjs（angular2的例子里面使用的是这个），还有目前比较火的webpack，这里我们使用webpack作为加载工具。
关于webpack，可以访问webpack的官方网站获取更多信息：
- [webpack官网](webpack.github.io)

首先需要配置webpack的entry
```javascript
    entry: {
        index: './src/startup/main'
    }
```

而main.js使用AMD方式：
```javascript
define('main',
    [
        './app',
        './router'
    ],
    function () {

    }
);
```
从上面的代码就可以看出，我们已经将初始文件分成了app和router。

实际上这个时候的目录结构如下：
- src
  - assets ///resource files like images, fonts
  - components ///controllers and components including html css and js
  - mocks ///mock data, like json
  - services ///services js
  - startup
    - app.js ///entry of app
    - main.js ///entry of webpack
    - router.js ///front router, by angular-ui-router
  - index.html ///entry of web
  - bundle.js ///packed js, generated by webpack
  - bundle.css ///packed css, generated by webpack

此时的app.js如下，这里没有使用手动加载，个人觉得单页应用手动加载意义不大啊...
```javascript
'use strict';
require('../../node_modules/normalize.css/normalize.css');
require('../../node_modules/bootstrap/dist/css/bootstrap.css');
require('../../library/kendo/src/styles/web/kendo.material.css');
require('../../library/kendo/src/styles/web/kendo.common.css');
require('../components/main.scss');

define('app', ['jquery','angular','angular-ui-router','kendo.angular'], function (jQuery,angular) {
    // 定义 angular 模块
    var app = angular.module('app', ['ui.router','kendo.directives']);
    app.constant('config',{
        'SERVICE_URL': 'http://localhost:8000/mocks/'
    });
    return app;
});
```

router.js中使用angular-ui-router来定义路由：
```javascript
'use strict';
define('router',
    [
        './app',
        '../components/manage/manageComponent'
    ],
    function (app) {
        app.config(function ($stateProvider, $urlRouterProvider, $locationProvider) {
            // $locationProvider.html5Mode({
            //     enabled: true,
            //     requireBase: false,
            //     rewriteLinks:false
            // });
            $urlRouterProvider.otherwise('/home');
            $stateProvider
                .state('help', {
                    url: '/help',
                    views: {
                        'content': {
                            templateUrl: './components/help/help.html'
                        }
                    }
                })
                .state('home', {
                    url: '/home',
                    views: {
                        'content': {
                            templateUrl: './components/manage/manage.html',
                            controller: 'manageComponent'
                        }
                    }
                })
                .state('home.index',{
                    url:'/index',
                    views: {
                        'board': {
                            templateUrl: './components/manage/manage.board.html'
                        }
                    }
                });
        });
    });
```
在router中，使用ui-router的controller赋值，指定哪个ui-view使用哪个controller，这样实现了html中ng-controller的完全分离，同时路由在统一的文件中配置路由，也更加清晰。

---
# 总结
实现参考了[张志敏的技术专栏：使用 RequireJS 加载AngularJS](http://beginor.github.io/2014/11/17/load-angularjs-with-requirejs.html)，但是requirejs实在是麻烦，由于webpack的出现以及ui-router，同时鉴于现在客户端计算机都很强大了，这里没有考虑promise对象的问题，采用的是直接加载所有的js文件，只是重点解决controller依赖的问题。
