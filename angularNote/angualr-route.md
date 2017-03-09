##AngularJS 路由
- 通过 AngularJS 可以实现多视图的单页Web应用（single page web application，SPA）
- AngularJS 路由 就通过 # + 标记 帮助我们区分不同的逻辑页面并将不同的页面绑定到对应的控制器上,
    - 这样页面虽然发生了跳转，但是页面不会刷新。
    - 只要页面发生了，浏览器就会记录，前进和后退按钮就有效。  
 

## 路由的使用    
 - 需要安装 angular-route模块（是route不是router）
    - 1.6.2 版的路由，是 #! + 标记,有利于seo收录url。
 - 如果要实现多级的路由嵌套，需要安装angular-ui-router 模块
 - 首先，要在引入angular.js之后引入angular-route.js文件
 - 其次，在模块中是用route服务时，需要加入route模块依赖，采用使用
 ```
 //angular的内置模块都是ng开头的
 var app = angular.module('appModule',['ngRoute']);
 ```
 - 然后，配置route服务的规则
 ```
 //$routeProvider是服务中this
    app.config(function ($routeProvider) {
        $routeProvider.when('/',{
            templateUrl:'tmpl/home.html',
            controller:'homeCtrl'//声明模板对应的控制器，相当于
        }).when('/add',{
            templateUrl:'tmpl/add.html',
            controller:'addCtrl'
        }).when('/list',{
            templateUrl:'tmpl/list.html',
            controller:'listCtrl'
        }).when('/detail/:id',{//表示id必须要有，但是随机的
            templateUrl:'tmpl/detail.html',
            controller:'detailCtrl'
        }).otherwise('/');//表示其他访问跳转的路由
    });
 ```
 - 另外，提供了$routeParams解析路由中参数
 ```
 //详情页规则   '/detail/:id' -->  /detail/id
 app.controller('detailCtrl', function ($scope,$routeParams,$location) {
    //$routeParams将路由中的参数转变成对象格式
    var id = $routeParams.id;
    ...
     $location.path('/list'); //页面跳转
 
 }
 ```