# 关于 Gitbook

GitBook 是一个供开发人员构建文档的开源工具。[GitBook网站](https://www.gitbook.com/)是一个创建和托管书籍的在线平台（需要翻墙）。
本文主要讲解如何使用gitbook提供的工具，在本地开发一个书籍文档，并部署到自己的网站上。

## 一、简介

GitBook是一个基于Node.js的格式转换的命令行工具 ，可以转换的格式有：PDF、ePub、mobi、HTML（静态网页）：

- HTML（静态网页）：构建个人静态网站。GitBook默认的输出格式。
- e-Book（电子书）：构建精美的电子书
  - 前提：使用ebook-convert插件（需要安装[calibre](https://calibre-ebook.com/download)软件）
  - 输出格式有： epub、mobi、pdf

GitBook常用的命令有：

| 命令 | 功能 |
| --- | --- |
| gitbook build | 生成静态网页 |
| gitbook build –gitbook=2.0.1 | 生成时指定gitbook的版本, 本地没有会先下载 |
| gitbook build –log=debug | 指定log的级别 |
| gitbook builid –debug | 输出更详细的错误消息（堆栈跟踪） |
| gitbook epub | 生成电子书：epub |
| gitbook fetch 标签/版本号 | 安装对应的gitbook版本 |
| gitbook help | 列出gitbook所有的命令 |
| gitbook –help | 输出gitbook-cli的帮助信息 |
| gitbook init | 初始化目录文件 |
| gitbook install | 安装插件 |
| gitbook ls | 列出本地所有的gitbook版本 |
| gitbook ls-remote | 列出远程可用的gitbook版本 |
| gitbook mobi | 生成电子书：mobi |
| gitbook parse | 解析和打印书籍的调试信息 |
| gitbook pdf | 生成电子书：pdf |
| gitbook serve | 生成静态网页并运行服务器 |
| gitbook uninstall 2.0.1 | 卸载对应的gitbook版本 |
| gitbook update | 更新到gitbook的最新版本 |
GitBook的命令应该在根目录（SUMMARY.md文件所在目录）内运行。

## 二、安装gitbook

GitBook安装组件和功能示意图：

![GitBook流程](https://kuang.netlify.app/blog/gitbook/GitBook%E6%B5%81%E7%A8%8B.png)

`gitbook`命令行工具首先需要本地电脑有`node.js`和终端。  
一般linux系统和Mac系统自带`node.js`和终端，Windows系统需要安装`node.js`和终端。  
windows系统推荐[cmder](https://cmder.app)终端。  

### 1、安装Cmder

进入[cmder](https://cmder.app)官网以后，有mini版和完整版，下载好解压文件包以后，配置好环境变量，右击鼠标就可以调用。

详细介绍及配置可参见[[Cmder]]。

### 2、安装node.js

对Gitbook 3.2.3支持最好的 #nodejs 是版本v10.21.0。

下载[Node.js v10.21.0 x64](https://10.46.1.218/sites/eti/DocLib5/node-v10.21.0-x64.msi)到本地，双击安装。

![[Pasted image 20220916090453.png]]

安装完成后，可使用如下命令在cmder终端查看系统是否有nodejs：

```bash
node -v
v10.21.0

```

如果出现版本号，那么说明已经安装好了。

### 3、安装gitbook-cli工具

gitbook-cli是一个在同一系统上安装和使用多个版本的GitBook的实用程序。它将自动安装所需版本的GitBook来构建一本书。
打开终端输入`npm install gitbook-cli -g`命令进行全局安装：

```awk
npm install gitbook-cli -g

npm http fetch GET 304 https://registry.npm.taobao.org/os-tmpdir 100ms (from cache)
npm http fetch GET 304 https://registry.npm.taobao.org/os-homedir 113ms (from cache)
/usr/local/bin/gitbook -> /usr/local/lib/node_modules/gitbook-cli/bin/gitbook.js
+ gitbook-cli@2.3.2
added 578 packages from 672 contributors in 17.806s
```

![[Pasted image 20220916092156.png]]
安装成功后可使用`gitbook --version`来查看是否安装成功：

```apache
gitbook --version
CLI version: 2.3.2
GitBook version: 3.2.3
```

***注意：***终端第一次运行`gitbook`命令，可能会自动安装`gitbook`，因为刚才安装的是CLI，此时CLI会自动安装`gitbook`。

如果想卸载CLI，可使用`npm uninstall gitbook-cli -g`来删除。

### 4、Gitbook 初始化

使用`gitbook init`来初始化一本书：

```pgsql
~ gitbook init

```

#### 4.1、初始化长时间卡壳报错

查两点：
1）、检查Nodejs版本是否为v10.21.0，最新的高版本不完美支持Gitbook3.2.3；
2）、由于开源服务器在国外，网路链接不好，需要使用国内淘宝镜像加速。

#### 4.2、使用淘宝镜像下载

npm下载路径，检查是不是淘宝镜像：

```bash
npm config get registry

npm config set registry https://registry.npm.taobao.org

```

切换成淘宝镜像，再检查是不是淘宝镜像：

```bash
npm config get registry
```

## 三、构建一本书

初始化一本书的命令是`gitbook init`

首先在终端创建一个项目目录，并进入这个目录：

```bash
mkdir book
cd book
```

然后使用`gitbook init`来初始化一本书：

```pgsql
~ gitbook init

warn: no summary file in this book 
info: create README.md 
info: create SUMMARY.md 
info: initialization is finished

```

`gitbook init`会在空项目中创建`README.md`和`SUMMARY.md`两个文件：
`README.md`文件是项目的介绍文件。
`SUMMARY.md`是gitbook书籍的目录。

![[Pasted image 20220916095757.png]]

### 1、重要文件`SUMMARY.md`和`README.md`

在新创建的项目根目录下，`SUMMARY.md`和`README.md`是必须的两个文件，如果没有的话，在`gitbook init`初始化时会自动创建这两个文件。这两个文件是`markdown`格式文件，其内容可以使用`markdown`编辑器软件来编辑。
推荐使用`Obsidian`和`vs code`。

如果`SUMMARY.md`文件里面有如下内容：

```markdown
* [项目介绍](README.md)
* http
    * [http说明](doc/http/http解析.md)
        * [tcp说明](doc/http/tcp/tcp说明.md)
            * [udp说明](doc/http/tcp/udp/udp说明.md)
* HTML
    * [HTML5-特性说明](doc/html/HTML5-特性说明.md)
```

那么在使用`gitbook init`时，如果项目目录里面的文件不存在，则会创建目录中的文件：

```
~ gitbook init

info: create doc/http/http解析.md 
info: create doc/http/tcp/tcp说明.md 
info: create doc/http/tcp/udp/udp说明.md 
info: create doc/html/HTML5-特性说明.md 
info: create SUMMARY.md 
info: initialization is finished 
```

![[Pasted image 20220916100942.png]]

### 2、编辑GLOSSARY.md

GLOSSARY.md（词汇表文件），主要存储词汇信息，如果在其他页面中出现了该文件中的词汇，鼠标放到词汇上会给出词汇提示。存放在书籍（MyBook）的根目录内。

GLOSSARY.md文件的主要语法是：

- 术语：使用`H2标题`定义一个名称
- 解释：新起一行，创建一个描述术语的提示信息的段落。

示例：

```markdown
## Git
分散式版本控制软件

## Markdown
Aaron Swartz 跟John Gruber共同设计的排版语言
```

### 3、编辑YAML

使用YAML格式的风格，在三条虚线之间。 文档中也可以不写顶部描述。顶部描述的内容可以定义自己的变量，可以参考页面变量，以便您可以在模板中使用它们。

备注：在没有安装支持插件前，不要在文件中使用，否则编译或者运行会失败。即编辑器需要支持YAML。

### 4、编辑LANGS.md

GitBook支持多种语言编写的书籍或者文档。 存放在书籍（MyBook）的根目录内。首先需要在根目录创建一个名为LANGS.md的文件，然后按照语言创建子目录：

```
# Languages

* [中文](zh/)
* [English](en/)
* [French](fr/)
* [Español](es/)
```

每种语言的配置：每个语言(例如：en)目录中都可以有一个book.json文件来定义自己的配置，它将作为主配置的扩展。

唯一的例外是插件，插件是全局指定的，语言环境配置不能指定特定的插件。

### 5、本地启动服务编写书籍

终端打开项目目录，使用`gitbook serve`启动服务：

```bash
~ gitbook serve

Live reload server started on port: 35729
Press CTRL+C to quit ...

info: 7 plugins are installed 
info: loading plugin "livereload"... OK 
……
info: loading plugin "theme-default"... OK 
info: found 5 pages 
info: found 0 asset files 
info: >> generation finished with success in 2.1s ! 

Starting server ...
Serving book on http://localhost:4000
```

然后根据终端的提示，在浏览器中打开`http://localhost:4000`查看书籍，效果如下图所示：

![[Pasted image 20220916101116.png]]

**_注意：_**`gitbook serve`命令会在项目中生成一个`_book`的文件夹，此文件夹就是最终生成的项目。

## 四、gitbook的配置文件

如果想对你的网站有更详细的个性化配置或使用插件，那么需要使用配置文件。  
配置文件写完后，需要重启服务或者重新打包才能应用配置。  
gitbook的配置文件名是`book.json`，首先在项目的根目录中创建`book.json`文件。  

关于`book.json`的详细介绍及配置可参见[[GitBook插件整理-明月]]

## 五、使用Sharepoint Designer在OneLink网站中发布项目

在使用`gitbook serve`命令生成的`_book`文件夹里有一个`index.html`文件，这个文件就是文档网站的HTM入口，把`_book`文件夹复制到服务器，然后把web服务的入口引向`index.html`即可完成文档网站的部署。

下载[Sharepoint Designer](https://10.46.1.218/sites/eti/DocLib5/spd/cn_sharepoint_designer_2013_with_sp1_x64_3948125.exe)
下载[kb3114721](https://10.46.1.218/sites/eti/DocLib5/spd/spd2013-kb3114721-fullfile-x64-glb.exe)
下载[kb2817441](https://10.46.1.218/sites/eti/DocLib5/spd/spdsp2013-kb2817441-fullfile-x64-zh-cn.exe)

## 六、版本管理（本地仓库）：Git

在工具链中，最强大的功能是GitBook，可以生成静态网站或创建电子书，极大地扩展了Markdown的用途，用来写书、API文档、公共文档、企业手册、论文、研究报告等。最核心的功能是Git，实现多人协作和跟踪管理文档的版本历史。

可使用Git命令跟踪管理文档（源文档和发布文档）。Git是分布式的版本管理工具，方便对项目文件和开发进度进行管理，支持版本历史管理和多人协作管理等功能。

本地仓库：**.git**目录

```
.                             # 项目的根目录
├── .git                      # 项目的Git本地版本库。运行git init命令自动创建
├── MyBook                    # 子项目目录。项目可以有多个子项目
│   ├── docs                  # 子项目的源代码目录：使用Markdown编写的源文档
│   │   ├── 分类目录1          # 源文档的分类（章节）目录
│   │   │   ├── assets        # 图片目录：存放源文档的图片
│   │   │   ├── README.md     # 章节的简介
│   │   │   └── xxxxxx.md     # 源文档
│   │   └── 分类目录2          # 源文档的分类（章节）目录
│   ├── _book                 # 发布文档目录。gitbook serve命令自动生成
│   ├── node_modules          # npm包（GitBook的JavaScript插件）的安装目录
│   ├── css                   # CSS样式表目录：根据输出格式自定义CSS
│   ├── book.json             # GitBook的配置文件
│   ├── GLOSSARY.md           # 词汇表文件。专业术语（词汇）的解释
│   ├── LANGS.md              # 多语言配置文件。使用多种语言编写书籍时配置
│   ├── .bookignore           # GitBook的忽略文件
│   ├── README.md             # 整本书的简介
│   └── SUMMARY.md            # 整本书的目录索引
└── .gitignore                # Git的忽略文件
```

 运行`git init`命令自动创建的`.git`隐藏子目录。

可通过设置2个分支分别对源文档和发布文档进行跟踪管理。

- master分支（默认）管理书籍的源文档
- pages分支（手动创建）管理书籍的发布文档

## 七、制作书籍封面

### 1、封面

为了书籍显示得更加优雅，可以指定一个封面。

封面由 **cover.jpg** （大封面）文件指定，**cover\_small.jpg** （小封面）作为小版本封面存在。封面应该是 **JPEG** 格式的文件。将 cover.jpg / cover\_small.jpg文件放在书本的根目录下。

好的封面应该遵守以下准则：

- cover.jpg 的尺寸为 1800x2360 像素，cover\_small.jpg 为 200x262 像素
- 没有边界
- 清晰可见的书名
- 任何重要的文字应该在小版本中可见

### 2、自动封面

使用 [autocover plugin](https://plugins.gitbook.com/plugin/autocover) 自动生成一个封面。

## 八、将项目部署到GitHub Pages

这部分需要使用git和github网站，如果你不会，请自行在网上搜索文档查看。

由于gitbook生成的项目跟文档的源码是两个部分，所以可以把文档放到master分支上，部署的网站放到gh-pages 分支。

### 8.1 在github上创建一个仓库

这个仓库用于存放你编写的项目，和部署项目，如何创建请自行查找。

### 8.2 本地项目提交到github仓库

在项目中创建一个`.gitignore`文件，内容如下：

```
# 忽略gitbook生成的项目目录
_book
```

然后终端打开项目，输入如下命令,来提交文档项目到github上：

```bash
~ git init
~ git add .
~ git commit -m '初始化gitbook本地项目'
~ git remote add origin https://github.com/yulilong/book.git 
~ git push -u origin master
```

上面命令执行结束后，就会把代码提交到github上的仓库。  
**_注意仓库地址要替换成你自己的链接。_**

### 8.3 生成项目并上传到github仓库的gh-pages分支

由于打包命令太多，为了简单化，现在写一个脚本命令来自动执行。当然你也可以终端自己执行这些命令。

为了部署方便，可以创建一个脚本文件`deploy.sh`,内容如下：

```bash
#!/usr/bin/env sh

echo '开始执行命令'
# 生成静态文件
echo '执行命令：gitbook build .'
gitbook build .

# 进入生成的文件夹
echo "执行命令：cd ./_book\n"
cd ./_book

# 初始化一个仓库，仅仅是做了一个初始化的操作，项目里的文件还没有被跟踪
echo "执行命令：git init\n"
git init

# 保存所有的修改
echo "执行命令：git add -A"
git add -A

# 把修改的文件提交
echo "执行命令：commit -m 'deploy'"
git commit -m 'deploy'

# 如果发布到 https://<USERNAME>.github.io/<REPO>
echo "执行命令：git push -f https://github.com/yulilong/book.git master:gh-pages"
git push -f https://github.com/yulilong/book.git master:gh-pages

# 返回到上一次的工作目录
echo "回到刚才工作目录"
cd -
```

注意脚本文件代码中仓库地址要替换成你自己的地址。

文件保存后，在终端执行如下命令，把生成的项目推送到github仓库上的`gh-pages`分支：

```bash
bash deploy.sh
```

执行成功后，打开你的github仓库，然后选择`branch`分支，会发现多了一个`gh-pages`分支，打开这个分之后，里面会有一个`index.html`文件。说明部署的代码上传成功了。  
**注意：如果没有`gh-pages`分支说明没有部署成功请查看刚才执行的终端看哪里报错了，解决报错直到成功部署。**

### 8.4 配置GitHub Pages显示网站

在github网站上的仓库里面点击`Settings` -> `GitHub Pages`选项中 -> `Source`里面选择`gh-pages branch` 然后点击`Save`按钮，然后在`GitHub Pages`下面就会看见一个网址，这个网址就是最终的网站。  
最终效果如下图所示：

![clipboard.png](https://segmentfault.com/img/bVbnQS1 "clipboard.png")

### 8.5 一些部署到GitHub Pages的例子

[https://github.com/yodaos-pro...](https://link.segmentfault.com/?enc=ZjIW7y6bed49SpE%2FuFN1jg%3D%3D.rQB4ikgY7kHO7QQW8n3ahLQj84nENSba1uCaP1hrFGyfsAOUYsrrAY5l7rBBTr3O)

## 九、常见问题

这里向大家介绍一些关于在`gitbook`安装使用过程中遇到的一些问题/坑及其解决方案。

### 1、链接跳转

`gitbook`在serve生成html并部署在网站后，左侧菜单的首页超链接不能点击跳转进入, 主要是`gitbook`不在支持本地模式了。默认README.md的连接为“./”，但有的时候不能指向默认的index.html文件。

修改文件：C:\Users\{用户名}\.gitbook\versions\{版本号}\lib\output\helper\fileToURL.js

修改成下面内容（注释掉三行）

```js
function fileToURL(output, filePath) {
    var options = output.getOptions();
    var directoryIndex = options.get('directoryIndex');

    filePath = fileToOutput(output, filePath);

    /*if (directoryIndex && path.basename(filePath) == 'index.html') {
        filePath = path.dirname(filePath) + '/';
    }*/

    return LocationUtils.normalize(filePath);
}
```

修改后，再使用`gitbook serve`后的首页连接就是“./index.html”

### 2、图片文件名

`markdown`格式文档中，图片文件名不能带有中文字符

后果：图片不能正常显示。

### 3、空行

建议图片、视频上下文各保留一行空行

后果：图片或视频与文字之间没有换行，格式错乱。

### 4、页脚信息定制

如果想在页脚加入一个URL，自己可以去`index.js`（"{项目目录}\node_modules\gitbook-plugin-tbfed-pagefooter\index.js"）里，把`powered by gitbook`，改成  
`powered by <a href="你的URL" target="_blank">你的名字</a>`。

---
\_\_EOF\_\_
