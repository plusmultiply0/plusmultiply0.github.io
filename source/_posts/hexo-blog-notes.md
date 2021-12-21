---
title: hexo 博客搭建&升级记录
date: 2020-05-28 16:22:38
tags:
    - blog
    - hexo
category:
    - notes
comments: true
toc: true
# layout: timeline
---
> 对自己搭建博客以及后续升级/添加功能的记录，仅供参考:
<!-- 
- [博客部署小记](#blogstart)
- [规避部署重复输入账号密码](#avoidpwd)
- [统计博客运行时间](#runtime)
- [添加Google Analytics](#ga)
- [更换博客主题](#changetheme)
- [为博客添加评论](#addcomment) -->

## <a id="blogstart"></a>博客部署小记
**情景**：hexo博客+Github pages
搭建博客不难--可参考[官方文档](https://hexo.io/zh-cn/docs/),以下命令仅供参考。
```bash
npm install -g hexo-cli
hexo init <folder>
cd <folder>
hexo g     //生成静态文件
```
参照官方文档，部署的时候会有些问题：
默认不调整分支的情况，[一键部署](https://hexo.io/zh-cn/docs/one-command-deployment)会把先前上传的站点文件覆盖，[部署](https://hexo.io/zh-cn/docs/github-pages)无法生成博客页面，因为Github强制要求User page必须在master分支上，而文档默认生成在gh-pages分支上。

根据个人需求，我需要将站点文件用远程分支管理，所以我选择将站点文件放在别的分支上，master分支用来存放生成的博客文件(User Page)。

部署基本步骤同[这里](https://hexo.io/zh-cn/docs/github-pages)，因为是利用 travis CI 自动构建博客，我们需要在这里修改 .travis.yml 配置文件--参考[这里](https://docs.travis-ci.com/user/deployment/pages/)
以下为参考代码
```yml
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - hexo # build master branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: hexo        # 设置站点文件所在分支
  local-dir: public
  target_branch: master # 这里设置public文件/博客文件生成的分支
```
之后的步骤比较简单，将站点文件从本地仓库推到远程设好的分支上(比如：hexo分支)。
然后，travis CI 就会自动构建博客文件。在博客网址查看即可。
注：Github可能会提示“minimist依赖版本”风险，根据提示修改下依赖的版本号就可以了。
**参考链接**：
- [GitHub Pages 站点的发布来源](https://help.github.com/cn/github/working-with-github-pages/about-github-pages)
- [Github pages Deployment](https://docs.travis-ci.com/user/deployment/pages/)
- [git本地分支与远程分支关联与解除关联](https://www.jianshu.com/p/526eb3eec83e)
- [Why call git branch --unset-upstream to fixup?](https://stackoverflow.com/questions/21609781/why-call-git-branch-unset-upstream-to-fixup)


## <a id="avoidpwd"></a>规避部署重复输入账号密码
**情景**：每次部署重复输入账号密码
**方法**：博客目录命令行内 输入如下命令，
```git
git config --global credential.helper wincred
```
之后，再进行一次部署，按系统提示输入账号密码即可。
此后，部署无需再输入账号密码
**参考链接：**
 - [git config --global credential.helper wincred](https://stackoverflow.com/questions/38333752/trying-to-understand-wincred-with-git-for-windows-confused)

## <a id="runtime"></a>统计博客运行时间
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

## <a id="ga"></a>添加Google Analytics
首先注册Google Analytics,然后在全局_config.yml文件内，添加：
```yml
google_analytics: UA-[TrackingID]-1
```
**参考链接:**
- [配置Google Analytics](http://wanderyt.github.io/2015/07/13/Apply-theme-and-other-features-in-Hexo/)

## <a id="#changetheme"></a>更换博客主题
由于默认主题不够美观，所以更换了 [card](https://github.com/ChrAlpha/hexo-theme-cards) 主题。
具体更换方法可以参考[文档](https://theme-cards.ichr.me/start/)。

更换了 [maupassant](https://github.com/tufu9441/maupassant-hexo) 主题，感觉十分简约和美观。
简述一下更换步骤：
- 下载主题文件的 zip 压缩包，并解压至 hexo 博客的 themes 目录下（修改文件名为 maupassant）
- 在 hexo 的配置文件里，修改 theme 为 maupassant
- 后续，可参考 maupassant 主题文档进行设置

PS：给文章添加多个分类时，默认是没有间隔的。可以将分类名用引号引起来并添上分隔符即可分隔显示。（这样会造成分类名有重复的，所以要修改相关所有分类名，就能保持一致了）
```
category:
    - [skills]
    - [' notes']
```

## <a id="#addcomment"></a>为博客添加评论
根据[主题文档](https://theme-cards.ichr.me/third-party/#Gitalk)，选择Gitalk作为博客评论。
首先是申请 **GitHub Application** ，
Authorization callback URL这一项填写博客主页地址即可，评论成功会自动返回评论所在页面。
然后，复制粘贴 clientID 和 clientSecret 填写主题配置文件，repo 这一项填写存放评论的仓库名称即可。
最后，生成静态文件，检查评论即可。
**参考链接：**
- [Register a new OAuth application](https://auth0.com/docs/connections/social/github)
- [Error: Not Found](https://github.com/gitalk/gitalk/issues/98)

## 简化--推送博客内容至远程
由于博客内容放在hexo分支，每次推送到远程都需要输如下命令：
```
git push origin HEAD:hexo
```
有时会忘记，直接输入<code>git push</code>，CLI 会提示输入上面的命令，于是复制粘贴实现推送到远程。
为了避免这些麻烦，使用 alias，用简写来代替长的命令
比如：
```
git config --global alias.pushoh "push origin HEAD:hexo"
```
这样就可以用如下简写替代每次要输的命令了
```
git pushoh
```
**参考链接：**
- [Git Basics - Git Aliases](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)
- [how-do-i-alias-commands-in-git](https://stackoverflow.com/questions/2553786/how-do-i-alias-commands-in-git)

## 小修博客样式
有时博客样式不是太完美，自己想调整一下，怎么把？
我的做法是，浏览器打开F12，定位到指定元素的类名/属性名，打开vscode进行搜索
然后，修改成自己想要的样式就行了
比如：
- **博客（白色背景）右上角的togglebutton有点不完美**
右键检查，复制类名，在vscode中搜索
其实，这样搜不到...注意到，注释掉box-shadow并微调height后，会比较美观。
于是改为搜索box-shadow/height，然后找到了，进行了修改！
- **修改字体**
基本同上，vscode全局搜索font-family属性，找到font.styl文件后将想要的字体设置为第一个即可
我使用的字体是[Lato](https://fonts.google.com/specimen/Lato?preview.text_type=custom&sidebar.open=true&selection.family=Lato:ital,wght@1,300;1,400;1,700#standard-styles)，最初是在 https://shuxiao.wang/ 上看到的。
个人倾心于英文normal斜体，感觉特别美丽和优雅~~
