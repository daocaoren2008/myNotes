###数据存储
####客户端存储
> 由客户端浏览器来完成存储的操作
- 记住用户名，密码(或者自动登录)
- 购物车(用户未登录状态)
- 当登录成功后，在本地存储一些用户的基本信息。以后我们可以查看本地是否有这些信息来验证是够登录---"验证登录态"
-...
> 技术方案
- cookie
- localStorage
- sessionStorage
- indexDB (本地浏览器数据库)
- web SQL 本地浏览器数据库
- cacheStorage 本地缓存存储
- Application cache 应用程序缓存存储

####服务器端存储
> 由服务器端程序完成存储的操作，把数据存储在服务器上了。
- 购物车(用户已经登录)
- 正常的信息存储基本上都是存储在服务器上的。
> 技术方案
- session
- 文件存储(例如我们做的CRM)
- 数据库存储
+ mongodb （node）
+ Access (微软办公软件自带的)--非常轻量级的
+ mySql
+ SQL server
+ Oracle

###cookie 和 localStorage
> cookie 和 localStorage都是本地存储经常用的方式，他们之间的区别如下：
####cookie
- cookie兼容所有浏览器
- cookie存储的大小是有限制的，一般情况同源下只能存储4kb大小
- 它有过期时间，当然我们可以自己设置，一般最长超不过一个月。到大过期时间存储的信息会自动清除。
- 用户可能会处于安全的考虑禁用cookie(无痕浏览，隐私模式)
- 360安全卫士等在清理垃圾的时候会清除掉cookie,浏览器自带的清理机制也会清除掉cookie
- cookie 不是严格的本地存储，我们在获取cookie的时候，需要保证客户端和服务器端保持连接的，读取cookie也是要经过HTTP处理的。
####localStorage
- 它是HTML5提供的，不兼容IE低版本浏览器
- 存储的大小也有限制，一般同源下只能存储5MB大小
- 永久存储到本地，只要不手动清除，或者浏览器不卸载等。
- 不收无痕浏览、或者隐私模式的影响，依然可以正常存储。
- 360安全卫士等目前对localStorage没有任何影响
- localStorage是严格的本地存储，和服务器之间没有半毛钱关系。

> 不管那一种，都是明文存储吗，在本地的控制台中都可以查看的到，所以为了保证信息的安全，我们需要对重要的信息进行加密
- 可逆转加密：按照规则加密完成后，还可以按照规则解密还原，例如手机号等信息需要使用可逆转加密方式处理
- 不可逆转加密：类似于密码之类的信息需要使用不可逆转的加密方式，也就是加密后谁都不知道原始密码是什么(MD5)
####localStorage的使用
- localStorage.setItem([key],[value])
- localStorage.getItem([key])
- localStorage.removeItem([key])
- localStorage.key([index])通过索引获取指定位置存储信息对应的key
-localStorage.clear();清空本源下的所有存储信息
> 使用localStorage存储和获取到的结果都是字符串格式的。
> sessionStorage和它一样，但是属于临时的会话存储，页面刷下不收影响，但是页面关闭信息自动清空了。
####cookie的使用



> 所谓的本地存储（cookie/localStorage）都是把信息存储到当前电脑的指定浏览器的指定位置下了，而且还记录了存储在那个域名下。然后我们在其他的任何地方都无法获取这些信息。



