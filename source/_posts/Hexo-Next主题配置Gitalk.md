---
title: Hexo Next主题配置Gitalk
date: 2020-03-17 11:19:16
categories:
- Hexo配置
tags:
- Hexo
mathjax: true
---
## Gitalk
在Hexo下的next主题中配置gitalk评论插件
gitalk是一个基于 Github Issue 和 Preact 开发的评论插件
项目地址是 `https://gitalk.github.io`
<!-- more --> 
## 准备工作
### 新建一个github仓库
在github上新建一个仓库用来存储博客的所有评论 一定要打开Issue 因为最后所有的评论都会被存储在这个仓库的Issue中
### Register a new OAuth application
在github中注册新应用 地址为 `https://github.com/settings/applications/new`
<div style="width: 537px;height: 550px;">
    ![OAuth](OAuth.png)
</div>
```bash
#参数说明
Application name #应用名随便填写
Homepage URL #填写博客地址 例如:https://shaoyuanhangyes.github.io
Authorization callback URL #填写博客地址
```
注册成功后 会跳转到github/setting/Developer setting的OAuth Apps页面
页面中显示了Client ID和Client Secret 这两项字段需要记忆保存下来

## 文件配置
### md5.min,js
部分文章评论区报错`Error:Validation Failed`
原因是gitalk的label标题长度要求不超过50 解决方案为通过MD5加密来缩短label长度
在文件目录`themes/next/source/js/src/`下创建`md5.min.js`文件 内容为
```bash
! function(n) {
    "use strict";
    function t(n, t) {
        var r = (65535 & n) + (65535 & t);
        return (n >> 16) + (t >> 16) + (r >> 16) << 16 | 65535 & r
    }
    function r(n, t) {
        return n << t | n >>> 32 - t
    }
    function e(n, e, o, u, c, f) {
        return t(r(t(t(e, n), t(u, f)), c), o)
    }
    function o(n, t, r, o, u, c, f) {
        return e(t & r | ~t & o, n, t, u, c, f)
    }
    function u(n, t, r, o, u, c, f) {
        return e(t & o | r & ~o, n, t, u, c, f)
    }
    function c(n, t, r, o, u, c, f) {
        return e(t ^ r ^ o, n, t, u, c, f)
    }
    function f(n, t, r, o, u, c, f) {
        return e(r ^ (t | ~o), n, t, u, c, f)
    }
    function i(n, r) {
        n[r >> 5] |= 128 << r % 32, n[14 + (r + 64 >>> 9 << 4)] = r;
        var e, i, a, d, h, l = 1732584193,
            g = -271733879,
            v = -1732584194,
            m = 271733878;
        for (e = 0; e < n.length; e += 16) i = l, a = g, d = v, h = m, g = f(g = f(g = f(g = f(g = c(g = c(g = c(g = c(g = u(g = u(g = u(g = u(g = o(g = o(g = o(g = o(g, v = o(v, m = o(m, l = o(l, g, v, m, n[e], 7, -680876936), g, v, n[e + 1], 12, -389564586), l, g, n[e + 2], 17, 606105819), m, l, n[e + 3], 22, -1044525330), v = o(v, m = o(m, l = o(l, g, v, m, n[e + 4], 7, -176418897), g, v, n[e + 5], 12, 1200080426), l, g, n[e + 6], 17, -1473231341), m, l, n[e + 7], 22, -45705983), v = o(v, m = o(m, l = o(l, g, v, m, n[e + 8], 7, 1770035416), g, v, n[e + 9], 12, -1958414417), l, g, n[e + 10], 17, -42063), m, l, n[e + 11], 22, -1990404162), v = o(v, m = o(m, l = o(l, g, v, m, n[e + 12], 7, 1804603682), g, v, n[e + 13], 12, -40341101), l, g, n[e + 14], 17, -1502002290), m, l, n[e + 15], 22, 1236535329), v = u(v, m = u(m, l = u(l, g, v, m, n[e + 1], 5, -165796510), g, v, n[e + 6], 9, -1069501632), l, g, n[e + 11], 14, 643717713), m, l, n[e], 20, -373897302), v = u(v, m = u(m, l = u(l, g, v, m, n[e + 5], 5, -701558691), g, v, n[e + 10], 9, 38016083), l, g, n[e + 15], 14, -660478335), m, l, n[e + 4], 20, -405537848), v = u(v, m = u(m, l = u(l, g, v, m, n[e + 9], 5, 568446438), g, v, n[e + 14], 9, -1019803690), l, g, n[e + 3], 14, -187363961), m, l, n[e + 8], 20, 1163531501), v = u(v, m = u(m, l = u(l, g, v, m, n[e + 13], 5, -1444681467), g, v, n[e + 2], 9, -51403784), l, g, n[e + 7], 14, 1735328473), m, l, n[e + 12], 20, -1926607734), v = c(v, m = c(m, l = c(l, g, v, m, n[e + 5], 4, -378558), g, v, n[e + 8], 11, -2022574463), l, g, n[e + 11], 16, 1839030562), m, l, n[e + 14], 23, -35309556), v = c(v, m = c(m, l = c(l, g, v, m, n[e + 1], 4, -1530992060), g, v, n[e + 4], 11, 1272893353), l, g, n[e + 7], 16, -155497632), m, l, n[e + 10], 23, -1094730640), v = c(v, m = c(m, l = c(l, g, v, m, n[e + 13], 4, 681279174), g, v, n[e], 11, -358537222), l, g, n[e + 3], 16, -722521979), m, l, n[e + 6], 23, 76029189), v = c(v, m = c(m, l = c(l, g, v, m, n[e + 9], 4, -640364487), g, v, n[e + 12], 11, -421815835), l, g, n[e + 15], 16, 530742520), m, l, n[e + 2], 23, -995338651), v = f(v, m = f(m, l = f(l, g, v, m, n[e], 6, -198630844), g, v, n[e + 7], 10, 1126891415), l, g, n[e + 14], 15, -1416354905), m, l, n[e + 5], 21, -57434055), v = f(v, m = f(m, l = f(l, g, v, m, n[e + 12], 6, 1700485571), g, v, n[e + 3], 10, -1894986606), l, g, n[e + 10], 15, -1051523), m, l, n[e + 1], 21, -2054922799), v = f(v, m = f(m, l = f(l, g, v, m, n[e + 8], 6, 1873313359), g, v, n[e + 15], 10, -30611744), l, g, n[e + 6], 15, -1560198380), m, l, n[e + 13], 21, 1309151649), v = f(v, m = f(m, l = f(l, g, v, m, n[e + 4], 6, -145523070), g, v, n[e + 11], 10, -1120210379), l, g, n[e + 2], 15, 718787259), m, l, n[e + 9], 21, -343485551), l = t(l, i), g = t(g, a), v = t(v, d), m = t(m, h);
        return [l, g, v, m]
    }

    function a(n) {
        var t, r = "",
            e = 32 * n.length;
        for (t = 0; t < e; t += 8) r += String.fromCharCode(n[t >> 5] >>> t % 32 & 255);
        return r
    }

    function d(n) {
        var t, r = [];
        for (r[(n.length >> 2) - 1] = void 0, t = 0; t < r.length; t += 1) r[t] = 0;
        var e = 8 * n.length;
        for (t = 0; t < e; t += 8) r[t >> 5] |= (255 & n.charCodeAt(t / 8)) << t % 32;
        return r
    }

    function h(n) {
        return a(i(d(n), 8 * n.length))
    }

    function l(n, t) {
        var r, e, o = d(n),
            u = [],
            c = [];
        for (u[15] = c[15] = void 0, o.length > 16 && (o = i(o, 8 * n.length)), r = 0; r < 16; r += 1) u[r] = 909522486 ^ o[r], c[r] = 1549556828 ^ o[r];
        return e = i(u.concat(d(t)), 512 + 8 * t.length), a(i(c.concat(e), 640))
    }

    function g(n) {
        var t, r, e = "";
        for (r = 0; r < n.length; r += 1) t = n.charCodeAt(r), e += "0123456789abcdef".charAt(t >>> 4 & 15) + "0123456789abcdef".charAt(15 & t);
        return e
    }
    function v(n) {
        return unescape(encodeURIComponent(n))
    }
    function m(n) {
        return h(v(n))
    }
    function p(n) {
        return g(m(n))
    }
    function s(n, t) {
        return l(v(n), v(t))
    }
    function C(n, t) {
        return g(s(n, t))
    }
    function A(n, t, r) {
        return t ? r ? s(t, n) : C(t, n) : r ? m(n) : p(n)
    }
    "function" == typeof define && define.amd ? define(function() {
        return A
    }) : "object" == typeof module && module.exports ? module.exports = A : n.md5 = A
}(this);
//# sourceMappingURL=md5.min.js.map
```
并且要在下一项即将新建的`gital.swig`文件中引入这个js文件 同时要对id进行md5加密
### gitalk.swig
在文件目录`themes/next/layout/_third-party/comments/`下 新建`gitalk.swig`文件
添加代码:
```bash
{% if page.comments && theme.gitalk.enable %}
  <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
  <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
  <script src="/js/src/md5.min.js"></script> #引入md5.js
   <script type="text/javascript">
        var gitalk = new Gitalk({
          clientID: '{{ theme.gitalk.ClientID }}',
          clientSecret: '{{ theme.gitalk.ClientSecret }}',
          repo: '{{ theme.gitalk.repo }}',
          owner: '{{ theme.gitalk.owner }}',
          admin: ['{{ theme.gitalk.adminUser }}'],
          id: md5({{ theme.gitalk.ID }}), #对id进行md5加密
          labels: {{ theme.gitalk.labels }},
          perPage: {{ theme.gitalk.perPage }},
          pagerDirection: '{{ theme.gitalk.pagerDirection }}',
          createIssueManually: {{ theme.gitalk.createIssueManually }},
          distractionFreeMode: {{ theme.gitalk.distractionFreeMode }}
        })
        gitalk.render('gitalk-container')           
    </script>
{% endif %}
```
### index.swig
修改`themes/next/layout/_third-party/comments/index.swig`文件 在最后一行添加内容
```bash
{% include 'gitalk.swig' %}
```
### comments.swig
修改`themes/next/layout/_partials/comments.swig`文件 找到以下内容
```bash
{% elseif theme.valine.appid and theme.valine.appkey %}
    <div class="comments" id="comments"></div>
    
  {% endif %}
```
在空白处添加内容
```bash
{% elseif theme.gitalk.enable %}
    <div id="gitalk-container"></div>
```
### gitalk.styl
在文件目录下`themes/next/source/css/_common/components/third-party/`新建`gitalk.styl`
添加代码:
```bash
.gt-header a, .gt-comments a, .gt-popup a
  border-bottom: none;
.gt-container .gt-popup .gt-action.is--active:before
  top: 0.7em;
```
### third-party.styl
修改`themes/next/source/css/_common/components/third-party/third-party.styl`文件 在最后一行添加内容
```bash
@import "gitalk";
```
### _config.yml
在next主题下的`themes/next/_config.yml`中添加gitalk的配置
```bash
gitalk:
  enable: true
  githubID: shaoyuanhangyes  # account name
  repo: gitalk # GitHub repository.准备工作中新建的仓库
  ClientID: fe67**************c438e # GitHub Application Client ID.
  ClientSecret: 62b0************86ab67bc57b11 # GitHub Application Client Secret.
  owner: shaoyuanhangyes # GitHub repository owner. Can be personal user or organization.
  adminUser: shaoyuanhangyes
  distractionFreeMode: true # Facebook-like distraction free mode.
  ID: location.pathname
  labels: "['Gitalk']"
  perPage: 15 # Pagination size, with maximum 100.
  pagerDirection: last # Comment sorting direction, available values are last and first. 
  createIssueManually: true # By default, Gitalk will create a corresponding github issue for your every single page automatically when the logined user is belong to the admin users. You can create it manually by setting this option to true.
```
## 调试收集错误
配置好后`hexo clean` `hexo g` `hexo d`部署完毕后即可
然后在某一文章下发现存在gitalk的评论框 先登录github账户 然后点击init按钮 自己发表一个评论 其他人就可以在这篇文章下评论了
### Error Not Found
错误原因是主题配置文件`_config.yml`中gitalk字段配置错误 往往错误集中在`repo`中 这个仓库的名字要填写的是新申请要保存issues的仓库名
### Error:Validation Failed
错误原因 标题长度超过50 处理办法为使用md5加密id来缩短label长度 具体操作在`md5.min.js`栏目中