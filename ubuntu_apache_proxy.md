title: ubuntu apache反向代理配置   
date: 2015-11-11 09:12:30   
tags:

---

最近因为配置一个nodejs的博客，然而我的vps上面的80端口已经被占用了，所以就打算使用反向代理技术。

反向代理维基上的解释是这样的：它根据客户端的请求，从后端的服务器上获取资源，然后再将这些资源返回给客户端。与前向代理不同，前向代理作为一个媒介将互联网上获取的资源返回给相关联的客户端，而反向代理是在服务器端作为代理使用，而不是客户端。

[反向代理维基地址](https://zh.wikipedia.org/wiki/反向代理)

博客的端口使用4000，使用反向代理后，用户返回80端口的网站，apache会将博客的内容显示给用户，就像用户直接访问4000端口一样。

以下是配置apache的步骤：

1.加载apache模块，使用a2enmod命令加载模块

```

a2enmod proxy proxy_balancer proxy_http 

```

加载完成后需要使用命令 `/etc/init.d/apache2 restart` 重启服务器

2.配置反向代理功能，进入sites_available，创建一个新的站点配置文件，然后编辑内容如下：

```

<VirtualHost *:80>
		#配置站点的域名
        ServerName xxx.com
        #配置站点的管理员信息
        ServerAdmin xxx@gmail.com

		#off表示开启反向代理，on表示开启正向代理
        ProxyRequests Off
        ProxyMaxForwards 100
        ProxyPreserveHost On
        #这里表示要将现在这个虚拟主机跳转到本机的4000端口
        ProxyPass / http://127.0.0.1:4000/
        ProxyPassReverse / http://127.0.0.1:4000/

        <Proxy *>
            Order Deny,Allow
            Allow from all
        </Proxy>
</VirtualHost>

```

然后通过a2ensite命令加载当前配置

最后重启apache，当你重新打开网页的时候就会跳转到4000端口的博客了


