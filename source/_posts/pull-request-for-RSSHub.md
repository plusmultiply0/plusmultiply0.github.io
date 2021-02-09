---
title: 为 RSSHub 贡献 pr
tags:
  - Github
  - RSS
  - attempt
  - NodeJs
  - JavaScript
comments: true
category:
  - notes
date: 2020-07-19 15:55:41
# toc: true
---

> 记一次为 RSSHub 提 pr 的经历

{% asset_img RSSHub.png %}

**内容索引**
- [开始](#start)
- [具体过程](#process)
- [一些感受](#feelings)


## <a id="start"></a>开始
最近为 [RSSHub](https://github.com/DIYgod/RSSHub) 提交了一个 pr，是关于学校图书馆新闻动态的 RSS 订阅。

[RSSHub](https://github.com/DIYgod/RSSHub) 可以为各种各样的网站生成 RSS，再用 RSS 阅读器订阅就可以很方便的浏览网站的更新信息。

简单的浏览了一下文档，issues 和 pull requests 里都没有发现我校的身影，
而且文档里有这么一句话
> 如果你会写 JavaScript，请按照规则提交 pull request

看上去提 pr 应该不会很难:)

于是浏览了一下我校的各个网站，决定为我校写一个图书馆新闻动态的 RSS 订阅规则。

那么，开始动手吧。

## <a id="process"></a>具体过程

先在 Github 上 fork 一下项目，然后 <code>git clone</code> 到本地。

然后，参照文档和别人已经写好的内容添加[脚本路由](https://docs.rsshub.app/joinus/#ti-jiao-xin-de-rsshub-gui-ze-tian-jia-jiao-ben-lu-you)。

现在，开始来写具体的脚本。

分析一下大致思路，
先使用 [got](https://github.com/sindresorhus/got) 获取源站的网页，然后用 [cheerio](https://github.com/cheeriojs/cheerio) 提取网页中需要的信息（如：新闻的链接、标题等）。又因为，我们需要用 RSS 获取到新闻内容，所以我们还需要根据新闻的链接再次使用 [got](https://github.com/sindresorhus/got) 请求具体的某个新闻的网页，然后使用 [cheerio](https://github.com/cheeriojs/cheerio) 按照 [生成 RSS 源的规则](https://docs.rsshub.app/joinus/#ti-jiao-xin-de-rsshub-gui-ze-bian-xie-jiao-ben-sheng-cheng-rss-yuan) 提取需要的信息编码即可。

貌似有 [got](https://github.com/sindresorhus/got) 和 [cheerio](https://github.com/cheeriojs/cheerio) 两个陌生面孔，不用慌，先扫一下技术文档并且参照一下别人写好的代码。我们就会发现，
[got](https://github.com/sindresorhus/got) 基本只用来请求源站的数据，而 [cheerio](https://github.com/cheeriojs/cheerio) 是从请求来的数据中提取生成 RSS源的信息，使用规则类似于 jQuery，应该不难上手。

具体代码就不详述了，因为编写的规则不难而且也可以参考先前的代码。

不过，有一点需要注意的是，在提取详细信息的时候，需要替换一下 description 里的链接地址。
```javascript
replace(/src="\//g, `src="${url.resolve(host, '.')}`)
replace(/href="\//g, `href="${url.resolve(host, '.')}`)
```
链接使用的是相对地址，而源站和 RSS阅读器站点显然不同，所以在 RSS 阅读器就看不到链接导向的内容了。
替换（添加上源站网址）之后，就可以在 RSS阅读器访问到相应内容啦。

其他的，考虑一下意外情况（如：获取信息有误），那么可以生成一个简单的提示让读者访问源站。

编写完成后，本地进行调试无误后，就可以添加上[脚本文档](https://docs.rsshub.app/joinus/#ti-jiao-xin-de-rsshub-gui-ze-tian-jia-jiao-ben-wen-dang)

之后，执行 <code>npm run format</code> 自动标准化代码格式，提交到 fork 的仓库后，然后在这个仓库内进行 pull request 就行了！

至此，为 RSSHub 的 pr 的经历就告一段落了。[DIYgod](https://github.com/DIYgod) 诚不欺我，确实只需要会一点 Javascript 就可以提一个简单的 pr 了:)

## <a id="feelings"></a>一些感受

除此之外，感觉看了下 [RSSHub](https://github.com/DIYgod/RSSHub) 的路由文档，能涨不少见识。
能认识到不同领域的不少网站，在需要了解某一领域/某方面知识时，可以做一个简单的参考，比如：设计、科学期刊、出行旅游等路由。可以通过这些分类的网站去了解信息。
固然，不能排除有为了水 pr 或者打广告等原因写的路由规则，这个在所难免，还是得靠自己分辨。

## 后记
pr 被合并啦 :)
{% asset_img merged.png %}