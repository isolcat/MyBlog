---
title: 试试将VScode作为自己的RSS阅读器，开发自己的VScode插件
author: isolcat
datetime: 2023-10-5T20:27:11Z
featured: true
draft: false
tags:
  - 插件
description: `RSS(RDF Site Summary)`作为一种消息来源格式规范，用以聚合多个网站更新的内容并自动通知网站订阅者，距今已经24年了的历史了，RSS也用于各个博客上，方便用于的订阅，但是在选择RSS阅读器的时候往往会因此犯愁，不知道选择什么好，在尝试了几个知名的RSS阅读器后，我总觉得仍有些许的不满意，望着任务栏里躺着的VScode，一个自认为好玩的想法诞生了————将VScode打造成自己的RSS阅读器
---

# 试试将VScode作为自己的RSS阅读器，开发自己的VScode插件

## 前言

`RSS(RDF Site Summary)`作为一种消息来源格式规范，用以聚合多个网站更新的内容并自动通知网站订阅者，距今已经24年了的历史了，RSS也用于各个博客上，方便用于的订阅，但是在选择RSS阅读器的时候往往会因此犯愁，不知道选择什么好，在尝试了几个知名的RSS阅读器后，我总觉得仍有些许的不满意，望着任务栏里躺着的VScode，一个自认为好玩的想法诞生了————将VScode打造成自己的RSS阅读器

## 💡需求分析

既然有了这个需求，那就要对这个需求进行分析：

> 1. **添加RSS链接**: 用户输入或选择RSS链接，并将其保存在插件的配置中
> 2. **解析RSS**: 使用RSS解析库解析保存的RSS链接，提取博客信息
> 3. **显示博客列表**: 在插件中展示博客列表，允许用户选择要查看的博客
> 4. **选择文章**: 允许用户选择博客中的一篇文章
> 5. **显示文章内容**: 将选定的文章内容展示出来

## 🛠️开发过程

### 项目初始化

正如[VScode插件开发文档](https://code.visualstudio.com/api/get-started/your-first-extension)所言，要开发我们的第一个VScode插件，要先安装好安装[Yeoman](https://yeoman.io/)和[VS Code Extension Generator](https://www.npmjs.com/package/generator-code)：

```bash
npm install -g yo generator-code
```

运行命令：`yo code`

![](https://isolcat.xlog.app/_next/image?url=https%3A%2F%2Fipfs.xlog.app%2Fipfs%2Fbafkreideiyxnp5cwcerpo6vlrx52uxpoelq53n6axhpec5wrmt53hjtkje&w=828&q=75)

这里选择新插件(JavaScript)即可，然后选择扩展名称(后续可以更改)，在跑完这个命令后，我们的插件也便初始化完成了

项目结构如下：

```
.
├── .vscode
│   ├── launch.json     // 用于启动和调试扩展的配置
│   └── tasks.json      // 用于编译 JavaScript 的构建任务配置
├── .gitignore          // 忽略构建输出和 node_modules
├── README.md           // 插件功能的可读描述
├── src
│   └── extension.js    // 插件源代码
├── package.json        // 插件清单
├── jsconfig.json       // JavaScript 配置
```

### 运行项目

我们点开`extension.js`，可以看到充满注释的代码，这里我们只需要注意两个函数：

> 1. activate
>
> 这是插件被激活时执行的函数
>
> 1. deactivate
>
> 这是插件被销毁时调用的方法，比如释放内存等

我们这次主要开发均在`activate`函数中执行，现在让我们来运行这个项目，在`extension.js`页面，我们按下F5，选择VScode插件开发，这时候便会打开一个新的VScode窗口，这里新开的窗口便可以运行我们的开发中的插件了，按下`ctrl+Shift+P`，输入插件默认的执行命令`Hello world`，便可以看见右下角的小弹窗![image-20231004114114185](https://isolcat.xlog.app/_next/image?url=https%3A%2F%2Fipfs.xlog.app%2Fipfs%2Fbafkreigxbevctnqydxnb5vbmscbbydpyxywzbo3mokpg43nifvcrm2n6iq&w=256&q=75)这便是默认的运行插件的命令

为了更好的运行我们的插件，我们进入`package.json`，修改配置：

```json
  "activationEvents": [
    "onCommand:allblog.searchBlog"
  ],
  "main": "./extension.js",
  "contributes": {
    "commands": [
      {
        "command": "allblog.searchBlog",
        "title": "searchBlog"
      }
    ]
  },
```

注意，修改这里后我们应同步修改执行插件的命令：`vscode.commands.registerCommand`将命令 ID 绑定到扩展中的处理函数

![image-20231004114509410](https://isolcat.xlog.app/_next/image?url=https%3A%2F%2Fipfs.xlog.app%2Fipfs%2Fbafkreietmfodmsgxyz7d77r6vsbptusl7kxfjyjlz2bgwmgyft3ht2bp4y&w=1080&q=75)

现在我们在新开的VScode窗口运行命令`Reload window`，重新加载窗口，再次运行插件的时候，就可以使用我们自己定义的命令：`searchBlog`了

### 基础功能实现

平常我们订阅RSS的时候不难发现，基本上都是XML(可扩展标记语言)格式，为了实现我们的功能，就必须将其解析成可用Js代码调用的格式，我们引入工具库：`fast-xml-parser`，这里用antfu老师的博客作为测试例子：

```js
const vscode = require("vscode");
const xmlParser = require("fast-xml-parser");

async function activate(context) {
  // 获取RSS feed并解析
  const response = await axios.get("https://antfu.me/feed.xml");
  const articles = xmlParser.parse(response.data).rss.channel.item;
  
  // 保存文章列表
  const articleList = articles.map((item, index) => ({
    label: item.title,
    description: item.description,
    index
  }));
...
```

为了实现用户可以自由选择文章的列表，我们要将文章的列表展现出来，此时已经实现了基础的展示：

![image-20231004115550486](https://isolcat.xlog.app/_next/image?url=https%3A%2F%2Fipfs.xlog.app%2Fipfs%2Fbafkreichc2myd3ilpu6victdqo4j2zggqpqpd343w33uv6aq45jlbo54dq&w=1200&q=75)

但此时我们点击文章，是没有任何的反应的，毕竟我们还没有写好展示的方式嘛😂

这里我们查看VScode提供的API，发现[Webview API](https://code.visualstudio.com/api/extension-guides/webview)很适合我们这个功能的实现，将`webview`视为`iframe`您的扩展控制的VSCode内的内容,于是我们修改如下代码：

```js
    if (selectedArticle) {
      const { link } = selectedArticle;
      const webViewPanel = vscode.window.createWebviewPanel(
        'blogWebView',
        'Blog Article',
        vscode.ViewColumn.One,
        {
          enableScripts: true,
          retainContextWhenHidden: true
        }
      );

      // 设置Webview的HTML内容
      webViewPanel.webview.html = `
        <!DOCTYPE html>
        <html>
          <head>
            <style>
              body {
                font-family: Arial, sans-serif;
              }
            </style>
          </head>
          <body>
            <iframe src="${link}" width="100%" height="100%"></iframe>
          </body>
        </html>
      `;
    }
  }
```

成功打开！![image-20231004160439369](https://isolcat.xlog.app/_next/image?url=https%3A%2F%2Fipfs.xlog.app%2Fipfs%2Fbafkreiar7ys5pomhce7bz6hzcn53kdsmpdkme4rnuq3j6kme4phwnl7b3m&w=1920&q=75)

再稍微优化一下？让他能够自由的设置窗口的高度:

```js
if (selectedArticle) {
			const { link } = selectedArticle;

			const heightInput = await vscode.window.showInputBox({
				prompt: 'Enter the height of the window (in px)',
				placeHolder: '500'
			});

			const webViewPanel = vscode.window.createWebviewPanel(
				'blogWebView',
				customName, // Use the custom name as the title
				vscode.ViewColumn.One,
				{
					enableScripts: true,
					retainContextWhenHidden: true
				}
			);

			let height = parseInt(heightInput) || 500;
			webViewPanel.webview.html = `
        <!DOCTYPE html>
        <html>
          <head>
            <style>
              body {
                font-family: Arial, sans-serif;
              }
            </style>
          </head>
          <body>
            <iframe src="${link}" width="100%" height="${height}px"></iframe>
          </body>
        </html>
      `;
		}
```

到此为止，我们基本的功能就做好了

### 优化一下？

这时候我们再次运行命令的时候，会发现已经订阅的RSS展示出来的是之前的链接，这可很伤脑筋，想想一下订阅的RSS多了之后，只看链接或许不能记忆的很清楚，那这时候我们不如试试给他新增个**自定义名称**，让我们订阅的RSS这些可以用我们习惯的方式记忆，来修改一下代码：

```js
...
if (customName) {
            // Check for duplicate custom names
            const duplicateCustomName = Object.values(rssLinks).includes(customName);
            if (duplicateCustomName) {
              vscode.window.showErrorMessage('This custom name is already in use. Please choose a different name.');
              return;
            }
            // 添加新的RSS链接和对应的自定义名字
            rssLinks[newLink] = customName;
            vscode.workspace.getConfiguration().update(RSS_LINKS_CONFIG_KEY, rssLinks, vscode.ConfigurationTarget.Global);
            await getArticleContent(newLink, customName);
          }
```

ok，现在便是自定义的名称了![image-20231004162410356](https://isolcat.xlog.app/_next/image?url=https%3A%2F%2Fipfs.xlog.app%2Fipfs%2Fbafkreigrvwxbgtunhnv5l5jzxpvedfy3hcob4q6zatflunlhsjspaqpidu&w=750&q=75)

## 🧐发现问题并解决

接着便是选择发布插件了，这里可以自己去查找相关文章，本文就不过多解释了，当插件发布成功后，我们下载完插件，添加了自己感兴趣的RSS博客，当关掉后再次打开查看，完蛋！之前已经订阅的RSS没有了！😭
问题不大，发现了问题就去解决它，我们查阅先前的代码可以发现之前的存在一个巨大的问题就是忘记了将已经订阅好的RSS进行一个**数据持久化**的处理，我们查阅VScode文档发现，它一共提供了四种数据存储的方案：
![image-20231004163241706](C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20231004163241706.png)

这里我们使用`ExtensionContext.globalState`进行全局的存储，将数据存储在存储在用户的本地配置文件中，这样即使关掉VScode再重新打开，也不会存在数据丢失的情况了，修改代码：

```js
...
// 添加新的RSS链接和对应的自定义名字
      rssLinks[newLink] = customName;
      // 保存订阅信息到用户的本地
      context.globalState.update(RSS_LINKS_CONFIG_KEY, rssLinks);
      await getArticleContent(newLink, customName);
...
```

再重新发布插件，搞定！现在一个简陋的RSS阅读器插件便完成了！

 ![example](https://isolcat.xlog.app/_next/image?url=https%3A%2F%2Fraw.githubusercontent.com%2Fisolcat%2FRSS-SearchBlog%2Fmain%2Fdocs%2Fexample.gif&w=750&q=75)

## 🐱总结

目前插件已经上线了(虽然还不是正式版)，可以在VScode上进行下载：[下载链接](https://marketplace.visualstudio.com/items?itemName=isolcat.isolcat)，或者直接在VScode扩展里搜索**RSS-searchBlog**，目前功能还不够完善，比如没法提供订阅后有更新的情况会进行小弹窗提示之类的，后续会通过更新来完善这个插件，之前看过一个有意思的开源项目是**[RSSHub](https://docs.rsshub.app/zh/)**，可以给任何奇奇怪怪的内容生成 RSS 订阅源，打算之后将插件接入RSSHub，并不断完善这个插件的功能，相信正式版插件会让大家满意的，感谢你的阅读，该项目源码：[RSS-searchBlog](https://github.com/isolcat/RSS-SearchBlog)