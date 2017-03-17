##$resource与$http 
- 本质上功能都是一样的，都是基于XMLHttpRequest和服务器交互的服务。  
- 不同点
    - 不同的是$resource是对$http更高层次的抽象，**$resource依赖于$http**，也就是说$resource是在$http的基础上开发的。
    - $resource需要安装angular-resource模块，$http是继承在angular里的。
- 基本上你可以把$http等同于jQuery的$.ajax，$http除了有$http方法为，也有快捷方法$http.get、$http.post、$http.put等快捷方法，还有$http(url)返回的是一个Promise。
##$resource用法
 1. 首先自定义和手动安装的模块在module中使用时都需要先写入依赖。
 ```
  var app = angular.module('appModule',['ngResource']);
 ```
  2. $resource对常见的5中类型请求进行封装,
  ```
  //以下是源码的封装的一部分，
    'get': {method: 'GET'},//取一条数据，要求服务端返回一个对象
    'save': {method: 'POST'},//添加一条数据，要求服务端返回一个对象
    'query': {method: 'GET', isArray: true},//获取全部数据，要求服务端返回一个数组
    'remove': {method: 'DELETE'},//删除一条数据，要求服务端返回一个空对象
    'delete': {method: 'DELETE'}//同上
  ```

          
 3. 格式：$resource(url,[paramDefaults],[actions],options);
- url:一个参数化的url模板，带有前缀参数(如：/user/:username)，模板中的冒号表示这项可有可无
- （可选）paramDefaults:url参数的默认值 
- （可选）actions：
- Options：扩展$resourceProvider行为的自定义设置，
 以下是使用$resource定制关于一个书籍的服务。

 
 ```
 //定义一个ajax服务，访问的路径是/book/或者/book/1,并扩展了一个使用put方式向服务端发请求的update方法。
     app.factory('Books',function ($resource) {
         return $resource('/book/:id',null,{
             update:{
                 method:'PUT'
             }
         });  // /book/:id
     });
 ```
 ```
 //修改一条id=456的数据，把这条数据改成$scope.tem。修改成功之后前端又执行了一些操作
         Books.update({id:456},$scope.temp).$promise.then(function () {
             $scope.flag = true;
             $scope.book = $scope.temp;
         })
 ```
 ##jsonp
 - $http.jsonp