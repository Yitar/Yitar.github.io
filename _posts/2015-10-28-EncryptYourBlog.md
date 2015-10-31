---
layout:     post
title:      "为你的博客启用 HTTPS"
subtitle:   ""
date:       2015-10-28
author:     "Yitar"
header-img: ""
tags:
    - HTTPS
    - SSL
    - Jekyll
---

>偶然发现自己的博客在使用手机流量访问的时候竟然被注入了联通的广告，于是研究一番，为自己的博客启用了 HTTPS 。本文适用于 Github Pages + Custom Domains + Jekyll 搭建而成的博客。



###准备工作

 1. 注册并登录你的 [Cloudflare](Cloudflare.com) 账户
 2. 添加你的自定义域名并检查生成的信息与你在域名注册商的信息是否相同
 3. 去你的域名注册商将 Domain Name Servers 更改为 CloudFlare 提供给你的值
 4. 设置完成之后选择 上方仪表盘中的 SSL,  选择 "Flexible SSL"

此时你发现已经可以使用 HTTPS 打开你的博客了，但这仅仅是开始，**搜索引擎与别的访问者依然不知道你使用 SSL 版本的博客。**

###搜索引擎篇
在你的 `_config.yml` 中，添加：


    url: https://www.yoursite.com   # with the https protocol
    enforce_ssl: www.yoursite.com   # without any protocol

    # For example, my configuration is this:
    url: https://yitar.me
    enforce_ssl: yitar.me

<p> 确保你的链接是 `canonical` 的在 `head&gt` 下面： </p>

    <link rel="canonical" href=" { { site.url } }{ { page.url } }" />

###访客篇
 <p>把这个加到你的 `&lt;head&gt;` 的最前面： </p>

    <script type="text/javascript">
        var host = "yoursite.com";
        if ((host == window.location.host) && (window.location.protocol != "https:"))
            window.location.protocol = "https";
    </script>


检查以确保你在 `stylesheet` 中的链接没有使用任何协议：


    <!-- Change this -->
    <link rel="stylesheet" href="http://www.somesite.com/path/to/styles.css">

    <!-- to this: -->
    <link rel="stylesheet" href="//www.somesite.com/path/to/styles.css">


你也可以在你的 Cloudflare 账户当前网站的仪表盘设置中找到 PageRules 添加强制 https 重定向，开启 Forwarding 并添加你的 https 地址即可。

###至此，你已经拥有了免费的安全的博客了。


###实用技巧
在你的博客使用 https 之后，你会发现类似于网易云音乐之类的外链 iframe 播放器会无法加载（ html 5的暂时都好丑），提示试图从未经验证的来源加载脚本。你可以把生成的外链也改为 https 类。在 Markdown 语法下正确的 https 网易云音乐外链示范如下：

    <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" _width_="330" _height_="86" src="https://music.163.com/outchain/player?type=2&id=2001325&auto=0&height=66"></iframe>

显示效果如下

----
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86" src="https://music.163.com/outchain/player?type=2&id=2001325&auto=0&height=66"></iframe>
