---
title: 用Puppeteer留下掘金热榜
datetime: 2023-7-2T11:21:11Z
featured: true
draft: false
tags:
  - javascript
  - 前端优化
description: 成为打工人后，没有那么多的时间去刷掘金了，很多时候只能在周末才有时间去优哉游哉的躺在床上刷着热榜.....


# 用puppeteer留下历史掘金热榜

## 前言

成为打工人后，没有那么多的时间去刷掘金了，很多时候只能在周末才有时间去优哉游哉的躺在床上刷着热榜，但这个时候我们往往会错过掘金热榜的历史记录(热榜的内容一直在变化)，这种情况或许会导致我们错过一些很不错的掘金文章，那么有没有什么办法可以让我们只需要一行命令就可以记录下掘金的历史热榜呢？这个时候就需要介绍一下Puppeteer了

## 简单介绍

Puppeteer 是一个由 Google 开发和维护的 Node.js 库，它提供了一个高级的 API，用于通过控制一个 **Headless Chrome**（无界面的 Chrome 浏览器）实例来进行网页的自动化操作。它可以用于执行各种任务，包括网页截图、生成 PDF、爬取数据、自动化表单填写和交互等。

这里再重点说一下Headless Chrome，也就是无头浏览器(之后的代码会涉及这个概念)，无头浏览器是指**没有可见用户界面的网络浏览器。它可以在后台运行，执行网页操作和浏览行为，但没有图形界面显示**，相较于传统的网络浏览器，通常会给我们一个用户可以看见的界面，用户通过这些界面进行交互，实现前后端的逻辑交互，如输入URL进行链接跳转，又或者是在登录注册的时候点击提交。但无头浏览器就可以让这些操作在后台自动化运行，并不会一次次的弹窗，方便开发人员可以通过编写脚本的方式来自动化的执行各种操作，如：

> - Web应用程序中的测试自动化
> - 拍摄网页截图
> - 对JavaScript库运行自动化测试
> - 收集网站数据
> - 自动化网页交互

接下来我就来演示一下如何快速的上手Puppeteer并且能够仅仅依靠一行命令就可以获取到掘金的热榜文章信息吧

## 配置Puppeteer

这里的配置实际上只是跟着官方文档的快速上手走一下罢了，我这里就简单的过一下

这里我们可以选择直接安装`Puppeteer`

```shell
npm i puppeteer
```

这里要注意一点，这里我们可以通过创建一个`puppeteer.config.cjs`文件来对puppeteer进行配置：

```js
const {join} = require('path');

/**
 * @type {import("puppeteer").Configuration}
 */
module.exports = {
  // Changes the cache location for Puppeteer.
  cacheDirectory: join(__dirname, '.cache', 'puppeteer'),
};
```

我们先创建完这个配置文件后运行安装的命令的话，我们可以看见多了一个`.cache`文件夹，我们点开查看会发现里面**存储了很多二进制**文件，这也就涉及到一个**提高启动速度**的优化方案了。`.cache`文件会在我们第一次使用`Puppeteer`的时候自动下载适合我们当前操作系统的Chrome浏览器二进制文件，这样一来就避免了后续启动的时候Puppeteer还需要重新下载所需文件，提高了启动速度

## 初上手Puppeteer

这时候我们创建一个`test.js`文件，输入以下内容，我将逐行进行解释：

```js
//引入 Puppeteer 库，使我们可以使用其中的功能，这里使用ESM的语法也是可以的
//import puppeteer from 'puppeteer';
const puppeteer = require('puppeteer');

(async () => {
  //启动Chrome浏览器实例，目前Puppeteer默认为无头浏览器的启动模式
  const browser = await puppeteer.launch();//const browser = await puppeteer.launch({headless: true});
  //创建一个新的页面对象
  const page = await browser.newPage();
  //代码导航到指定的 URL，模拟我们在输入URl进行跳转这一操作
  await page.goto('https://example.com');
  //对当前页面进行截图操作，注意：Chrome团队为了让不同的设备上显示的内容一致，默认的浏览器窗口呈现大小为800x600
  await page.screenshot({path: 'example.png'});

  //关掉Chrome
  await browser.close();
})();
```

运行`node .\test.js`命令后得到这张图便说明你已经成功了：

 ![image.png](https://pic1.58cdn.com.cn/nowater/webim/big/n_v2d929b03f32614c32918896483ce5efa1.png)

很好，现在你已经初上手了Puppeteer，并且掌握了最基本的操作，接下来便是实现我们上述所说的需求了

## 功能实现

首先我们要知道掘金热榜上，文章标题和文章的链接究竟是在哪，具体点说，不是让我们明白它们的位置，而是让`Puppeteer`知道他的位置，这里我们在热榜部分打开控制台，这里我们使用**选择器语法**：`Page.$$()`，该方法可以在浏览器里运行`document.querySelectorAll`方法，输入：`$$('a')`，得到的是一大堆a标签，但这很明显与我们预期的不符

这时候我们就需要缩小他的范围，将鼠标放在热榜上，单击右键“检查”![image.png](https://pic7.58cdn.com.cn/nowater/webim/big/n_v252192c45af574266983f7578ce59cfee.png)

这时候我们就可以在控制台上快速定位到这一部分的内容了，这时候我们修改选择器内容：`$$('.hot-list>a')`，这时候我们就获取到了链接内容，这时候我们想要获取到标题，原理也就一样了，再对其进行稍微的处理：`$$('.article-title').map(x=>x.innerText)`,便可以得到掘金热榜的标题了

## 坑点

如果此时我们直接运行代码，极大概率得到的是`[]`，这里就涉及很重要的一点了，**网页加载延迟**

这里我们取消掉默认的无头浏览器模式，修改代码为：

```js
const browser = await puppeteer.launch({ headless: false })
```

这时候我们再运行代码，就会发现网页在还没有加载完全的情况下就结束了我们的操作，这个时候我们就需要设置`waitUntil`或延迟加载该脚本了，修改代码：

```js
await page.goto("https://juejin.cn/hot/articles", {
        waitUntil: "domcontentloaded",
    });
await page.waitForTimeout(2000);
```

但这里我们翻看文档会发现，这里会提示我们`page.waitForTimeout`已过时，更推荐我们使用`Frame.waitForSelector`，它会**等待与给定选择器匹配的元素出现在帧中**才运行代码，相比较与直接延迟执行代码来讲更加的高效,这里先暂时这样，之后我附上完整的代码

## 补全并优化功能

当我们能够成功检测到内容后，我们需要的便是将其保存到本地，这时候便引入Node.js的文件系统模块，将检测到的文件内容进行写入：

```js
import puppeteer from "puppeteer";
import fs from "fs";

(async () => {
    const browser = await puppeteer.launch({ headless: false });
    const page = await browser.newPage();
    await page.goto('https://juejin.cn/hot/articles', {
        waitUntil: "domcontentloaded"
    });
    await page.waitForTimeout(2000);

    let hotList = await page.$$eval(".article-title[data-v-cfcb8fcc]", (title) => {
        return title.map((x) => x.innerText);
    });

    console.log(hotList);

    // 将文章标题保存到文本文件
    fs.writeFile('titles.txt', hotList.join('\n'), (err) => {
        if (err) throw err;
        console.log('文章标题已保存到titles.txt文件');
    });

    await browser.close();
})();
```

此时便获取到了文章的所有标题，但是光有标题可不够，周末的时候想刷点文章如果还需要手动的输入的话那多麻烦，这时候就需要将文章的标题和链接一起存入，调用`closest("a").href`来获取链接：

```js
const articleList = await page.$$eval(
    ".article-title[data-v-cfcb8fcc] a",
    (articles) => {
      return articles.map((article) => ({
        title: article.innerText,
        link: article.href,
      }));
    }
  );

  console.log(articleList);

  // 将文章标题和链接保存到文本文件
  const formattedData = articleList.map(
    (article) => `${article.title} - ${article.link}`
  );
  fs.writeFile("articles.txt", formattedData.join("\n"), (err) => {
    if (err) throw err;
    console.log("文章标题和链接已保存到articles.txt文件");
  });
```

大功告成！但这时候我们发现第二天我们重新运行这个脚本的时候，把前一天的文件给覆盖了，这可不行啊，那就按照**日期分类**，将文章的热榜按照不同的天数进行分类，这里我们再添加上先前所说的等待与给定选择器匹配的元素出现在帧中后的功能。最终得到：

```js
import puppeteer from "puppeteer";
import fs from "fs";

(async () => {
    const browser = await puppeteer.launch({ headless: false });
    const page = await browser.newPage();
    await page.goto("https://juejin.cn/hot/articles", {
        waitUntil: "domcontentloaded",
    });

    //处理文件夹名
    const currentDate = new Date().toLocaleDateString();
    const fileName = `${currentDate.replace(/\//g, "-")}.txt`;

    await page.waitForSelector(".article-title[data-v-cfcb8fcc]");

    const articleList = await page.$$eval(
        ".article-title[data-v-cfcb8fcc]",
        (articles) => {
            return articles.map((article) => ({
                title: article.innerText,
                link: article.closest("a").href,
            }));
        }
    );

    console.log(articleList);

    const formattedData = articleList.map(
        (article) => `${article.title} - ${article.link}`
    );
    fs.writeFile(fileName, formattedData.join("\n"), (err) => {
        if (err) throw err;
        console.log(`文章标题和链接已保存到文件: ${fileName}`);
    });

    await browser.close();
})();
```

运行代码后变得到了这样的内容：

![image.png](https://pic5.58cdn.com.cn/nowater/webim/big/n_v2bc5c152ed7b441268edf0712372f2758.png)

## 总结

`Puppeteer`作为一个由Google团队开发并维护的Node.js库，极大程度上方便了我们进行各种自动化的操作，想象一下，之后你只需要运行一行简单的node命令即可存储当前的热榜文章和信息，岂不美哉🐱

然而爬虫这一方案仅仅是他众多功能中最微不足道的一点，正如官方所说，它还可以进行自动化表单提交、UI测试、捕获站点的时间线、对SPA进行爬虫以达到**预呈现**的效果(*这点有机会再出一篇关于前端首屏优化的文章😽*)

本篇文章仅作为教学和学习记录，**使用爬虫获取私人敏感信息属于违法行为**，请使用爬虫前了解相关法律法规

> 扯远了，掘金目前还有很多功能尚未完善，也希望掘金官方之后能够出一个**回看历史热榜**的功能，让这个社区更加完善！