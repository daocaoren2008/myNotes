title: 搭建博客
data: 2015-12-22
type: "tags"
---

## 安装hexo形成静态博客
```
1.安装hexo,安装后会提供一个命令 hexo
npm install hexo-cli -g
2.本地新建博客文件，进入目录命令行初始化文件
hexo init
3.启动服务 
 hexo serve 
>如果端口被占用：
hexo serve -p 3333(更改的端口号)

```

## 下载主题并让博客变成可以互联网访问
（进入到themes文件下下载）

```
1.主题是找的git上别人博客的主题，把别人的博客地址复制下来，在本地进行克隆
git clone https://github.com/iissnan/hexo-theme-next.git

2.hexo g    生成发布的内容

3.安装hexo关于git管理组件：
npm install hexo-deploy-git --save
  
4.安装组件之后需要进入_config.yml配置用户路径：
deploy:
  type: git
  repo: https://git用户名(不能是邮箱):git密码@github.com/fandonglai/fandonglai.github.io.git,master（@后面的地址是git博客仓库地址）
 
5.hexo deploy  发布博客

6.新建博客git线上仓库时注意：
名字要求： github账号名.github.io
例如：liuyumei111.github.io

```

>>操作可参考：http://theme-next.iissnan.com/getting-started.html
