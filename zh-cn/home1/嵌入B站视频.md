# Markdown插入B站视频

## 代码示例

```html
<iframe 
        src="//player.bilibili.com/player.html?bvid=BV1br4y1j7Mi&page=1&high_quality=1&danmaku=1" 
        allowfullscreen="allowfullscreen" 
        width="100%" height="500" 
        scrolling="no" 
        frameborder="0" 
        sandbox="allow-top-navigation allow-same-origin allow-forms allow-scripts">
</iframe>
```

## 使用说明

1. 打开源视频网址的网址，复制链接参数`bvid`
2. 替换示例代码中参数`bvid`

## 效果展示

<iframe 
        src="//player.bilibili.com/player.html?bvid=BV1br4y1j7Mi&page=1&high_quality=1&danmaku=1" 
        allowfullscreen="allowfullscreen" 
        width="100%" height="500" 
        scrolling="no" 
        frameborder="0" 
        sandbox="allow-top-navigation allow-same-origin allow-forms allow-scripts">
</iframe>

## 参数拓展

> **src 参数：(写在 src 引号中链接最后，各项之间用「&」连接)**
>
>     「bvid」bilibili video id，即视频的 bv 号。
>
>     「cid」chat id，即视频对应弹幕池的编号，如果填了 aid / bvid，那么不填也可。
>
>     「page」视频集数（默认为1），即分集视频中的第几集（起始下标为1）。
>
>     「autoplay」设定视频是否自动播放，1为自动播放、0为禁止自动播放，默认为1；注意：开启自动播放会默认伴随静音开播。
>
>     「danmaku」是否开启弹幕，1为开启、0为关闭，默认为1。
>
>     「as_wide」是否宽屏，1为宽屏、0为小屏，默认为1。
>
>     「t」iframe加载完毕时自动跳转至该秒数并自动播放，单位s，默认为0。
>
>     「high_quality」视频清晰度，1为高清、0为最低清晰度，默认为0；注意：由于iframe默认处于未登陆状态，且B站播放器在未登录状态下只允许视频 360p 播放，所以该参数约等于没用。
>
> **iframe 参数：（写在 iframe 标签中，各项之间用空格隔开）**
>
>     「src」规定 iframe 中显示文档的URL。
>
>     「height」规定 iframe 宽度，单位px。
>
>     「width」规定 iframe>高度，单位px。
>
>     「name」规定 iframe>名称。
>
>     「allowfullscreen」规定 iframe>是否允许全屏播放。
>
>     「seamless」令 iframe>看起来就像是父文档的一部分，是一个 Boolean 值；注意：只有 Chrome 和 Safari 6 支持 iframe>标签的 seamless 属性。
>
>     「sandbox」对 iframe>的额外限制，用以提升iFrame的安全性；注意：sandbox 值既可以是用空格分隔的一系列指定的字符串，也可以是一个空字符串（将会启用所有的限制）。
>
>     其他属性，如「align」、「scrolling」等，HTML5 不支持，建议使用 CSS 代替（建议所有样式有关的属性都尽量使用 CSS 代替）。
>
>     **sandbox 参数：（写在 sandbox 属性的引号中，各项之间用空格隔开）**
>
>     「allow-same-origin」允许将内容作为普通源来对待，否则嵌入的内容将被视为一个独立的源（跨域）。
>
>     「allow-top-navigation」允许嵌入的网页对顶级窗口进行导航。
>
>     「allow-forms」允许提交表单。
>
>     「allow-scripts」允许脚本执行。
>
>     「allow-modals」允许弹出提示框。
>
>     「allow-popups」允许嵌入的网页使用 `window.open();` 方法弹出窗口。
>
>     「allow-popups-to-escape-sandbox」允许嵌入的网页使用 `window.open();` 方法弹出窗口，且弹出的窗口不受沙箱的限制。
>
>     「allow-downloads-without-user-activation」允许在没有用户激活的情况下，嵌入的网页启动下载。
>
>     「allow-orientation-lock」允许嵌入的网页用脚本锁定屏幕的方向。  

