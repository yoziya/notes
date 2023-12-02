---
title: Typecho
date: 2023-12-2
tags: [Blog]
categories: [Utility]
---

# Typecho

记录一下使用 typecho 建站的经过。

## Local

### Typora

1.0 之后的 Typora 不免费了！莫关系，网上搜个 [typora-0-11-18](https://github.com/Pure-Happiness/Typora-0.11.18/releases) 下载，然后修改注册表使其无法检测更新（多金土豪请使用最新正版 [Typora](https://typoraio.cn/)）(≧ω≦)

### PicGo

由于博客要发到远端，搞个「图床」还是有必要的（你也不想因为图片资源而把租的服务器嗖的一下撑满吧~）。使用 [GitHub](https://github.com/) 作为图床，[PicGo](https://molunerfinn.com/PicGo/) 作为上传工具。

先进入 GitHub 建个仓库用来放图片，然后进入令牌设置页 [Tokens](https://github.com/settings/tokens) new 一个令牌。注意这里一定要勾上：

![image-20231202203223475](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231202203223475.png)

不然的话这个令牌权限只有只读，那就没办法上传图片做图床了。**令牌生成后只显示一次**，记得复制一下。

安装了 PicGo 后进入「图床设置」：

![image-20231202203155835](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231202203155835.png)

分支名要改成 **main**，也是用 GitHub 经常踩坑的地方，后两个不是必填，就无所谓了。确定和设为默认图床都要点。

在 Typora 设置中直接选择 PicGo，我们就可以直接粘贴图片时上传了！ヾ(≧▽≦*)o

![image-20231202202800836](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231202202800836.png)

之后就是在本地愉快的编写和审查文章，等写的内容完善后再发到远端。

## Website

### domain name

域名……哪家便宜买哪个吧。（com！我要.com！）

### 星辰云

在 [星辰云](https://starxn.com/) 买个廉价主机，主要还是它附带的类宝塔面板服务，我购买的配置如下

![image-20231202203804318](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231202203804318.png)

对博客来说应该差不多是够的（wordpress表示小伙子还是太年轻）。

首先是在控制台添加域名，告诉服务商这个域名和现在的面板所在的虚拟主机是有关联的，并复制一下服务商的解析地址：

![image-20231202204058695](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231202204058695.png)

然后在买了域名的地方解析服务商地址，解析完之后每次进入这个域名后续的一切就会交给解析的服务商地址去处理，那其实刚刚我们在星辰云的面板里已经做了对应的域名管理，这个域名会跳到主机的根目录 `wwwroot`。

![image-20231202204449367](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231202204449367.png)

至此，网站的配置就做好了，DNS系统将域名定向到根目录，并寻找目录下的index文件（什么是DNS系统？什么是定向、重定向，别、别问了，头已经大了）。

## typecho

不想从 `index.html` 开始手撸网站的话，框架就派上用场了，typecho 就是这种东西。

### typecho

下载 [Typecho](https://typecho.org/) 压缩包。然后进去控制台的根目录，上传整个压缩包文件。然后在控制台里解压。

![image-20231202221545570](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231202221545570.png)

这时再进入买的域名，就会进入 Typecho 配置界面。

![image-20231202184449397](https://raw.githubusercontent.com/yoziya/images/main/image-20231202184449397.png)

配置信息在星辰云购买的产品信息里能看到。

![image-20231202221922162](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231202221922162.png)

数据库名和数据库用户名都填面板账号就行了。

![image-20231202222204723](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231202222204723.png)

后面是设置能管理 typecho 后台的账号和密码，填写完成后进入下面的页面。

![image-20231202222229852](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231202222229852.png)

然后进入控制面板，也就是网站的后台了。第二项是网站主页，接下来是安装主题时间。

### Handsom

我使用的主题为 [handsome](https://www.ihewro.com/archives/489/)，啊？你问为啥，没得选啊，别的都不支持 LaTex 公式，需要自己改 PHP 去适配（应该是来写博客的吧(⊙_⊙)）。

主题目录 `/wwwroot/usr/themes/`。和安装 typecho 差不多，上传压缩包，然后在线解压。就不多说了。

![image-20231202185527952](https://raw.githubusercontent.com/yoziya/images/main/image-20231202185527952.png)

对于 Handsome 的一些功能，可以在控制台 → 外观 → 设置外观中配置。

![image-20231202185849063](https://raw.githubusercontent.com/yoziya/images/main/image-20231202185849063.png)

按个人想法配置即可。[《handsome-typecho 主题文档帮助手册教程》](https://geekdaxue.co/books/typecho-theme-handsome-docs)

## item

### 浏览器动态标题

外观 → 设置外观 → 开发者设置 → 自定义输出body尾部的HTML代码

```html
<!--浏览器动态标题开始-->
<script>
 var OriginTitle = document.title;
 var titleTime;
 document.addEventListener('visibilitychange', function () {
     if (document.hidden) {
         $('[rel="icon"]').attr('href', "//file.kaygb.top/static_image/tx.png");
         document.title = 'ヽ(●-`Д´-)ノ我藏好了哦！';
         clearTimeout(titleTime);
     }
     else {
         $('[rel="icon"]').attr('href', "//file.kaygb.top/static_image/tx.png");
         document.title = 'ヾ(Ő∀Ő3)ノ被你发现啦~！' + OriginTitle;
         titleTime = setTimeout(function () {
             document.title = OriginTitle;
         }, 4000);
     }
 });
</script>
<!--浏览器动态标题结束-->
```

### 头像呼吸光环和鼠标悬停旋转放大

外观 → 设置外观 → 开发者设置 → 自定义CSS

```css
.img-full {
    width: 100px;
    border-radius: 50%;
    animation: light 4s ease-in-out infinite;
    transition: 0.5s;
}

.img-full:hover {
transform: scale(1.15) rotate(720deg);
}

@keyframes light {
    0% {
        box-shadow: 0 0 4px #f00;
    }

    25% {
    box-shadow: 0 0 16px #0f0;
    }

    50% {
    box-shadow: 0 0 4px #00f;
    }

    75% {
    box-shadow: 0 0 16px #0f0;
    }

    100% {
    box-shadow: 0 0 4px #f00;
    }
}
```

### 文章内打赏图标跳动

外观 → 设置外观 → 开发者设置 → 自定义CSS

```css
.btn-pay {
    animation: star 0.5s ease-in-out infinite alternate;
}

@keyframes star {
    from {
        transform: scale(1);
    }

    to {
        transform: scale(1.1);
    }
}
```

### P站每日热门

外观 → 设置外观 → 开发者设置 → 全局右侧边栏广告位

````html
<iframe src="https://cloud.mokeyjay.com/pixiv" frameborder="0" style="width:240px; height:380px;"></iframe>
````

`/wwwroot/usr/themes/handsome/component/sidebar.php` 文件中搜「广告」并替换为「P站每日热门」

### 访客总数统计

`/wwwroot/usr/themes/handsome/functions.php`文件中添加以下统计代码

```php
//总访问量
function theAllViews()
{
    $db = Typecho_Db::get();
    $row = $db->fetchAll('SELECT SUM(VIEWS) FROM `typecho_contents`');
    echo number_format($row[0]['SUM(VIEWS)']);
}
```

`/wwwroot/usr/themes/handsome/component/sidebar.php` 文件中「博客信息」插入以下调用代码

```html
<li class="list-group-item"> <i class="glyphicon glyphicon-user text-muted"></i> <span class="badge
pull-right"><?php echo theAllViews();?></span><?php _me("访客总数") ?></li>
```

### 网站加载耗时

`/wwwroot/usr/themes/handsome/functions.php`文件中添加以下代码

```php
//加载耗时
function timer_start() {
    global $timestart;
    $mtime     = explode( ' ', microtime() );
    $timestart = $mtime[1] + $mtime[0];
    return true;
}
timer_start();
function timer_stop( $display = 0, $precision = 3 ) {
    global $timestart, $timeend;
    $mtime     = explode( ' ', microtime() );
    $timeend   = $mtime[1] + $mtime[0];
    $timetotal = number_format( $timeend - $timestart, $precision );
    $r         = $timetotal < 1 ? $timetotal * 1000 . " ms" : $timetotal . " s";
    if ( $display ) {
        echo $r;
    }
    return $r;
}
```

`/wwwroot/usr/themes/handsome/component/sidebar.php` 文件中「博客信息」插入以下调用代码

```html
<li class="list-group-item"> <i class="glyphicon glyphicon-time text-muted"></i> <span class="badge pull-right"><?php echo timer_stop();?></span><?php _me("加载耗时") ?></li>
```

### 左侧下拉框

`/wwwroot/usr/themes/handsome/component/aside.php` 文件友链列表下插入新列表框

```php
            <!--左侧下拉框-->
            <li>
                <a class="auto">
                <span class="pull-right text-muted">
                <i class="fontello icon-fw fontello-angle-right text"></i>
                <i class="fontello icon-fw fontello-angle-down text-active"></i>
                </span>
                <i class="glyphicon glyphicon-star"></i>
                <span>收藏页</span></a>
                <ul class="nav nav-sub dk">
                <!--网站-->
                    <li>
                        <a href="https://www.bilibili.com" class="auto" target="_blank">
                        <i class="nav-sub-header"></i>
                        <span>哔哩哔哩</span></a>
                    </li>
                    <li>
                        <a href="http://www.mxdm9.com/" class="auto" target="_blank">
                        <i class="nav-sub-header"></i>
                        <span>MX动漫</span></a>
                    </li>
                </ul>
            </li>
            <!--下拉框结束-->
```



---

## issues



## reference

- [Typora](https://typoraio.cn/)
- [PicGo](https://molunerfinn.com/PicGo/)
- [星辰云官网](https://starxn.com/)
- [Typecho](https://typecho.org/)
- [handsome](https://www.ihewro.com/archives/489/)
- [《handsome-typecho 主题文档帮助手册教程》](https://geekdaxue.co/books/typecho-theme-handsome-docs)

## appendix



---