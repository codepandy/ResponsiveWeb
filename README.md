# ResponsiveWeb

一个响应式网站

## 目录结构

```
.
├── README.md
├── doc                    # 项目文档
└── src                    # 源代码
    ├── css                # 样式文件
    │   ├── main.css       # 公用样式
    │   └── normalize.css  # reset的样式
    ├── img                # 存放图片的文件夹
    ├── js                 # 存放js的文件夹
    │   ├── main.js        # main页面js
    │   └── ventor         # 第三方库文件
    ├── index.html         # 首页
    └── login.html         # 登录页

```

## 一些有用的文件

### 404 页面

当放生 404 时，可以以更友好的方式展现，而不是使用系统给的错误页面。

### robots.txt

这个是搜索引擎在访问网站时第一个要查看的文件。`robots.txt` 告诉搜索引擎的爬虫程序，在服务器上什么文件可以被查看，什么文件不可用被查看。

比如下面就是指定除了`admin`文件夹都可以访问。

```json
User-agent: *
Disallow: /admin/
```

### favicon.ico

这个是网站默认的图标，

### humans.txt

上面的 robots.txt 是给机器人看的，这个是给人类看的，包含了创建网站人员的信息等。
[humanstxt 的介绍信息](http://humanstxt.org/ZH)

### .editorconfig

这个是配置 IDE 的参数，创建这个文件后，IDE 工具会使用这里面的配置，这样即使更换工具，配置还是一样的。
[官网地址](https://editorconfig.org/)

### .gitignore

配置一些 git 需要忽略的文件

## html 头部元素的讲解

```html
<!DOCTYPE html>
<!--lang规定了页面内容的语言，默认是en，这里指定简体中文，也可以使用zh，zh-CN是简体中文，zh支持更广泛的中文，比如繁体、方言等-->
<html lang="zh-CN">
  <head>
    <!--设置网页编码，这个尽可能早的声明，比如放在了title下面，title的可能无法应用这个编码，导致乱码-->
    <meta charset="UTF-8" />
    <!--设置视口，这里设置了可视视口和设备视口相等，并且默认不放大缩小-->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <!--设置兼容性，指定浏览器以哪种版本来渲染，比如下面就是ie以edge的方式来渲染-->
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <!--[if lte IE8]>
<p>你的浏览器版本过低，请到 <a href="https://browsehappy.com/">这里</a> 进行升级</p>
    <[endif]-->
  </body>
</html>
```

> 这里只考虑 IE8 以上版本，上面 body 中的写法是 IE 浏览器中认识的注释提示，如果版本低于 IE8,就会看到提示内容。

## cssreset.css vs Normalize.css

这两个都是格式化 html 标签样式的库，第一个是把所有 html 的默认样式都去掉了，主要是去掉一些 margin、padding，并且统一了各个浏览器的显示样式。建议使用 Normalize.css

[cssreset.css](https://meyerweb.com/eric/tools/css/reset/)
[Normalize.css 地址](http://necolas.github.io/normalize.css/)

## px、em、rem

[区别请看这]
响应式布局建议使用 rem 或者 em。

使用 rem 时需要注意一点，浏览器对每种字体的大小有个最小下限，比如汉字是 12px，英文是 10px，一次如果 rem 换算后低于这个下限，可能和预期会不一样。

### 高度塌陷

高度塌陷就是因为使用了 float 浮动布局，导致父元素的高度预和预想的不一致，因为在计算高度时，浮动的子元素是不包含在内的。

解决方法：

1.增加一个空的子元素，设置`clear:both`样式

```html
<div>
    ....其他内容
    <div style="clear:both"></div>
</>
```

但这种方式需要无意义的空标签，违背了结构和表现分离的精髓，所以这种方式现在已经不用了。

2.给父元素增加一个`overflow:auto`，或者也变成浮动。(就是把父元素变成`bfc`了)

3.使用 after 伪元素，在父元素上增加下面的样式

```css
.clearfix::before,
.clearfix::after {
  content: " "; //使用一个空格或者.
  display: table;
}
.clearfix::after {
  clear: both;
}
.clearfix {
  zoom: 1;
}
```

4.使用 bfc

把容器变成 bfc 就可以了。

### html 中的空白字符

在对`li`设置成`inline-block`时，各个元素之间会有 3 像素的空隙，这个就是 html 中的空白字符，就是在编写 html 文件时，各个元素之间的换行符引起的。

解决方案：

1. 把所有的`li`元素都写在一行，这可读性太差
2. 把父元素的字体大小设置成 0，因为空白字符也属于字符
3. 把元素的关闭标签和下一个的开始标签写到一块

```
<ul>
    <li>aaaa
    </li><li>bbb
    </li><li>ccc</li>
</ul>
```

但是很多格式化工具都给格式化回去了。

4. 不闭合

```
<ul>
  <li><a>aaa</a>
  <li><a>bbb</a>
</ul>
```

5. 设置间隙为负值

```css
li {
  margin-left: -3px;
}
```

> 设置 inline-block 的容器，font-size 设置成 0，不然有间隙。

## 文字不换行

```css
.notice {
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}
```

## 滤镜

下面是让图片显示成灰色

```css
filter: grayscale(100%);
```

## 更改浏览器分辨率插件

`Viewport Resizer`

## 使用媒体查询设置不同大小屏幕的样式

```css
@media only screen and (min-width: 800px) {
  .header .top {
    background-color: red;
  }
}
@media only screen and (min-width: 400px) and (max-width: 800px) {
  .header .top {
    background-color: red;
  }
}
```

### 媒体查询使用相对单位时的坑

上面已经把 html 的字体大小设置成了 1rem=10px 的关系

```css
@media only screen and (min-width: 80rem) {
  .header .top {
    background-color: red;
  }
}
```

通过上面的设置会发现，80rem 转换后并不是 800px，而是一千多，这是为什么呢？
这是因为媒体查询的级别很高，它不是 html 下面的一个子元素，所以设置 html 的字体大小对它没影响，他使用的是浏览器的字体大小。chrome 默认是 16px；

```css
@media only screen and (min-width: 50em) {
  .header .top {
    background-color: red;
  }
}
```

细心的同学发现了，上面单位改成了 em，而不是 rem，因为这个不需要针对 html，所以使用 em 也一样，并且 rem 的兼容性没有 em 好。

## 使用媒体查询设置打印样式

可以通过媒体查询设置打印显示的样式。

## 根据不同分辨率加载不同大小的图片

因为移动端屏幕小，不需要 pc 那样大的图片，并且大图片会导致加载慢，还费流量；所以需要根据不同的分辨率，加载不同尺寸的图片。如何解决呢？

### 使用 js 或者服务端返回

```js
const windowWidth = $(window).width();
if(windowWidth<=480px){
    $('imgId').attr('src','480.png');
}
```

或者把尺寸信息放到 cookie 中传给服务端，服务端返回对应图片的地址。

### 使用新标记 srcset

```html
img { display: block; width: 100%; }
<img
  alt="测试不同分辨率显示图片"
  src="/img/ad001.png"
  srcset="./img/ad001.png 480w, ./img/ad001-m.png 800w, ./img/ad001-l.png 1600w"
/>
```

当屏幕分辨率小于等于 480px 时，使用 `ad001.png`，当大于 480px 小于等于 800px 时使用 `ad001-m.png`;
当分辨率被调整到大一级时再缩小回来，也会使用加载后最大的图片来显示，并不会再替换到对应小分辨率设置的图片。因为当加载过大的图片后，就被认为缓存了，再次加载不会造成影响。

> 注意：`img` 需要设置成块级元素，不然不管用。

这样有个问题，比如 img 图片是放在一个容器中，img 只是容器的 50%宽，这时图片的加载判断依据还是屏幕的大小，这其实不是很合适，当屏幕是 700px 时，按上面的条件，这时加载的是 800px 的图片，但是图片实际显示大小确实 350px，小于 480px，所以应该加载 480px 的图片。那么该如何设置图片判断的实际尺寸呢？这就需要用到另一个属性`sizes`

### 使用新标记 srcset 配合 sizes

```html
<img
  alt="测试不同分辨率显示图片"
  src="/img/ad001.png"
  srcset="./img/ad001.png 480w, ./img/ad001-m.png 800w, ./img/ad001-l.png 1600w"
  sizes="50vw"
/>
```

上面 sizes 设置成`50vw`，就是说图片按照视频宽度的 50%显示。这样上面的情况图片大小改变就符合实际了。

`sizes`的值是媒体查询和宽度组成，媒体查询可省略，默认就是所有的分辨率都适应，可以针对不同的分辨率设置不同的值。

```html
<img
  alt="测试不同分辨率显示图片"
  src="/img/ad001.png"
  srcset="./img/ad001.png 480w, ./img/ad001-m.png 800w, ./img/ad001-l.png 1600w"
  sizes="(min-width:800px) 800px, 100vw"
/>
```

上面的配置就是当屏幕分辨率大于等于 800px 时，图片都是 800px，否则 100%视图宽度显示。

```
sizes="(min-width:800px) calc(100vw - 30em), 100vw"
```

计算大小，注意：减号两边带上空格

### 使用新标记 picture

HTML `picture` 元素通过包含零或多个 `source` 元素和一个 `img` 元素来为不同的显示/设备场景提供图像版本。浏览器会选择最匹配的子 `source` 元素，如果没有匹配的，就选择 `img` 元素的 `src` 属性中的 `URL` 。然后，所选图像呈现在`img`元素占据的空间中。

```html
<section class="content">
  <picture>
    <source media="(max-width:30em)" srcset="./img/ad002.png 480w" />
    <source media="(max-width:50em)" srcset="./img/ad002-m.png 800w" />
    <source media="(max-width:87em)" srcset="./img/ad002-l.png 1600w" />
    <source media="(orientation:landscape)" srcset="./img/ad002-m.png 800w" />
    <source type="image/svg+xml" srcset="logo.svg" />
    <source type="image/webp" srcset="logo.webp 400w, logo-m.webp 800w" />
    <img src="./img/ad002.png" />
  </picture>
</section>
```

根据不同的媒体条件加载不同的图片。
上面第四个是横屏的设置。
注意几点：

1. media 的值必须被小括号括起来，不然不起作用、
2. 必须有 img 标签

`picture` 比 `img` 使用 `srcset` 要智能，即使放到最大再缩小回来，还是会加载对应条件的图片。

使用 svg 的图片

### 使用 svg 格式的图片

矢量图，它是根据一定的规则来绘制图片，而不是像素。在不同的分辨率下不会失真，且图片尺寸小。

在线绘制网站

[第一个](https://editor.method.ac/)
[第二个](https://icomoon.io/)

### 兼容性对比

IE 浏览器完全不支持 srcset 和 picture，安卓浏览器支持也不好，而 svg 是支持比较好的。但是有些图片色彩比较丰富，必须使用位图，使用位图，我们就是用 pictrue 或者 srcset 来做，然后通过 polyfill 来解决兼容性的问题。图片的 polyfill 我们使用 [Pictruefill](https://github.com/scottjehl/picturefill)的工具库来解决，下载并引用这个 js 库即可。=

> polyfill 就是泥瓦匠的那个腻子，往墙上抹水泥的那个。

### 压缩图片网站

[压缩 svg 网站](https://iconizr.com/)

[压缩 png 网站](https://tinypng.com/)

## 什么是 DPR

dpr 是设备像素比，就是物理像素和显示像素的比。

## 搭建服务

使用 http-server，也可以 serve 这些线程的 node.js 服务包

## 云测试网站

https://testin.cn/

[移动端虚拟机](http://genymotion.net/)

## 使用 hacks 来解决兼容性

[查看各浏览器或设备的 hacks 信息](http://browserhacks.com/)

使用 pollyfill 或 shiv 来解决兼容性

- [html5shiv](https://github.com/aFarkas/html5shiv)
- [一个有名的 pollyfile-Respond](https://github.com/scottjehl/Respond)

手动来检测是否支持一些特性
[https://modernizr.com/](https://modernizr.com/)

[省时的浏览器同步测试工具](http://browsersync.cn/)
就是启动一个会自动刷新的服务，但是多个设备上的网页操作是同步的，比如开了三个浏览器，滚动一个，其他两个也会滚动。包括更改内容，等等都是同步的。

## 部署

### 代码压缩

[代码压缩网站](https://javascript-minifier.com/)

使用一些第三方构建工具

- [gulp]
- [webpack]

## 响应式框架

- [Bootstrap](https://www.bootcss.com/)
- [Foundation]()
- [Semantic UI](https://semantic-ui.com/)
- [PureCSS](http://www.purecss.cn/)

## 原型设计的工具

- Balsamlq Mockups 3
- axure
- sketch (mac)

## 交互工具

- [flinto](http://www.flinto.com.cn/)
- [principleformac](https://principleformac.com/)

让图片动起来，用UI的设计图把交互工程连起来，还能导出 gif 动画。
