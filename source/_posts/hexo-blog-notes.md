---
title: hexo-blog-notes
date: 2020-05-28 16:22:38
tags:
    - blog
    - hexo
category:
    - [skills]
    - [notes]
---
## 博客搭建记录
对自己搭建博客功能的记录，仅供参考:

- [规避部署重复输入账号密码](#avoidpwd)
- [统计博客运行时间](#runtime)

#### <a id="avoidpwd"></a>规避部署重复输入账号密码
**情景**：每次部署重复输入账号密码
**方法**：博客目录命令行内 输入如下命令，
```git
git config --global credential.helper wincred
```
之后，再进行一次部署，按系统提示输入账号密码即可。
此后，部署无需再输入账号密码
**参考链接：**
 - [git config --global credential.helper wincred](https://stackoverflow.com/questions/38333752/trying-to-understand-wincred-with-git-for-windows-confused)

#### <a id="runtime"></a>统计博客运行时间
找到 /themes/xxx/layout/_partial/ 目录下的footer命名的文件，打开后加入如下统计代码
```html
<span>本站已运行 <span id="runningtime"></span></span>
<script>
let birthd = new Date(2020,4,28);//这里设置建站时间
setInterval(()=>{
    let nowtime = Date.now();
    let mtimeInterval = nowtime-birthd;
    let thisyear = Math.floor(mtimeInterval/(365*24*60*60*1000));
    let today = Math.floor(((mtimeInterval/(24*60*60*1000))-thisyear*365));
    let thishour = Math.floor((mtimeInterval/(60*60*1000))-today*24-thisyear*365*24);
    let thisminute = Math.floor((mtimeInterval/(60*1000))-thishour*60-today*24*60-thisyear*365*24*60);
    let thissecond = Math.floor((mtimeInterval / (1000)) - thisminute * 60 - thishour * 60*60 - today * 24 * 60*60 - thisyear * 365 * 24 * 60*60);
    document.getElementById('runningtime').innerHTML=`${thisyear?thisyear+'年':''}${today}天${thishour}小时${thisminute}分钟${thissecond}秒`;
},1000);
</script>
```
代码最好放在以下代码内部
```html
<div class="outer"></div>
```
具体位置自定，以上代码实现仅供参考

**参考链接：**
- [统计博客运行时间](https://www.bingyublog.com/2019/02/20/hexo%E5%A2%9E%E5%8A%A0%E7%BD%91%E7%AB%99%E8%BF%90%E8%A1%8C%E6%97%B6%E9%97%B4%E7%BB%9F%E8%AE%A1/)