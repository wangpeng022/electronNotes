### Electron进阶——Electron-vue


#### 1.简介Electron-vue
> 该样板代码被构建为 vue-cli 的一个模板，并且包含多个选项，可以自定义你最终的脚手架程序。本项目需要使用 node@^7 或更高版本。electron-vue 官方推荐 yarn 作为软件包管理器，因为它可以更好地处理依赖关系，并可以使用 yarn clean 帮助减少最后构建文件的大小。

#### 2.快速上手 

##### 2.1 使用vue-cli快速构建electron应用

```
vue init simulatedgreg/electron-vue my-project
```

##### 2.2 生成目录结构
   文件夹 PATH 列表
	
	F:.
	│  .babelrc
	│  .gitignore
	│  package.json
	│  README.md
	│  tree.txt
	│  
	├─.electron-vue
	│      build.config.js
	│      build.js
	│      dev-client.js
	│      dev-runner.js
	│      webpack.main.config.js
	│      webpack.renderer.config.js
	│      webpack.web.config.js
	│      
	├─build
	│  └─icons
	│          256x256.png
	│          icon.icns
	│          icon.ico
	│          
	├─dist
	│  ├─electron
	│  │      .gitkeep
	│  │      
	│  └─web
	│          .gitkeep
	│          
	├─src
	│  │  index.ejs
	│  │  
	│  ├─main
	│  │      index.dev.js
	│  │      index.js
	│  │      
	│  └─renderer
	│      │  App.vue
	│      │  main.js
	│      │  
	│      ├─assets
	│      │      .gitkeep
	│      │      logo.png
	│      │      
	│      ├─components
	│      │  │  LandingPage.vue
	│      │  │  
	│      │  └─LandingPage
	│      │          SystemInformation.vue
	│      │          
	│      ├─router
	│      │      index.js
	│      │      
	│      └─store
	│          │  index.js
	│          │  
	│          └─modules
	│                  Counter.js
	│                  index.js
	│                  
	└─static
        
 有木有很眼熟，对，src里的文件和vue-cli的特别像，唯一不同的是多了个main和renderer 的区分。
 
##### 2.3 主进程和渲染进程
		1. 主进程：src/main/index.js
	 可以理解为electron的入口文件，只能有一个；
		2. 渲染进程：renderer/main.js
	 可以理解为是vue的入口文件，相对于主进程来说，可以有多个。
     

##### 2.4  主进程和渲染进程之间的通信  
1. 使用ipcMain(主进程)和ipcRenderer(渲染进程);
		对象app:
		```
			//引用方法：
			const {app} = require('electron')
		```
> app.quit()
		尝试关闭所有窗口 将首先发出 before-quit 事件。 如果所有窗口都已成功关闭, 则将发出 will-quit 事件, 并且默认情况下应用程序将终止。
		此方法会确保执行所有beforeunload 和 unload事件处理程序。 可以在退出窗口之前的beforeunload事件处理程序中返回false取消退出。
		
>app.exit([exitCode])
exitCode Integer (可选)
立即退出程序并返回 exitCode。exitCode 的默认值是 0。
所有窗口都将立即被关闭（不会弹出询问提示），而且 before-quit 和 will-quit 事件也不会被触发

>app.relaunch([options])
选项 Object (可选)
从当前实例退出，重启应用。
默认情况下，新的实例会和当前实例使用相同的工作目录以及命令行参数。 当设置了 args 参数时， args 将作为命令行参数传递。 当设置了 execPath ，execPath 将被执行以重新启动，而不是当前的应用程序。
请注意, 此方法在执行时不会退出当前的应用程序, 你需要在调用 app.relaunch 方法后再执行 app. quit 或者 app.exit 来让应用重启。

>app.isReady()
返回 Boolean 类型 - 如果 Electron 已经完成初始化，则返回 true, 其他情况为 false

>app.focus()
在 Linux 系统中, 使第一个可见窗口获取焦点。在 macOS 上, 让该应用成为活动应用程序。在 Windows 上, 使应用的第一个窗口获取焦点。

>app.getAppPath()
返回 String 类型 - 当前应用程序所在目录

>app.getPath(name)
name : String
返回 String -与 name 参数相关的特殊文件夹或文件路径。当失败时抛出 Error 。

>app.getVersion()
返回 String-加载的应用程序的版本。 如果应用程序的 package. json 文件中找不到版本号, 则返回当前包或者可执行文件的版本。

>app.getName()
返回 String-当前应用程序的名称, 它是应用程序的 package. json 文件中的名称。
根据 npm 的命名规则, 通常 package.json 中的 name 字段是一个短的小写字符串。 通常还应该指定一个 productName 字段, 是首字母大写的完整名称，用于表示应用程序的名称。Electron 会优先使用这个字段作为应用名。

>app.setName(name)
name String
设置当前应用程序的名字

>app.getLocale()
返回 string——当前应用程序的语言环境。可能的返回值被记录在这里。
要设置区域，则需要在应用启动时使用命令行时打开开关，你可以在这里找到。
注意: 分发打包的应用程序时, 你必须指定 locales 文件夹。
注意： 在 Windows 上，你必须得等 ready 事件触发之后，才能调用该方法

>app.addRecentDocument(path) macOS Windows
path String
将此 path 添加到最近打开的文件列表中
这个列表由操作系统进行管理。在 Windows 中从任务栏访问列表, 在 macOS 中通过 dock 菜单进行访问。

>app.clearRecentDocuments() macOS Windows
清空最近打开的文档列表

等等等等，具体查看官方文档。


2. remote模块 
作用：在渲染进程中使用主进程模块。

eg: 
	从渲染进程创建浏览器窗口：
```
const {BrowserWindow} = require('electron').remote
  let win = new BrowserWindow({width: 800, height: 600})
  win.loadURL('https://github.com')
```


#### 3.指令

默认指令：

```
"scripts": {
    "build": "node .electron-vue/build.js",
    "build:darwin": "cross-env BUILD_TARGET=darwin node .electron-vue/build.js",
    "build:linux": "cross-env BUILD_TARGET=linux node .electron-vue/build.js",
    "build:mas": "cross-env BUILD_TARGET=mas node .electron-vue/build.js",
    "build:win32": "cross-env BUILD_TARGET=win32 node .electron-vue/build.js",
    "build:clean": "cross-env BUILD_TARGET=clean node .electron-vue/build.js",
    "build:web": "cross-env BUILD_TARGET=web node .electron-vue/build.js",
    "dev": "node .electron-vue/dev-runner.js",
    "pack": "npm run pack:main && npm run pack:renderer",
    "pack:main": "cross-env NODE_ENV=production webpack --progress --colors --config .electron-vue/webpack.main.config.js",
    "pack:renderer": "cross-env NODE_ENV=production webpack --progress --colors --config .electron-vue/webpack.renderer.config.js",
    "postinstall": ""
  },
```

##### 3.3 开发环境

```
yarn run dev # 或者 npm run dev
```
启动electron，并且打包vue代码放到虚拟内存中，并在electron中调用。


```
yarn run build:win32 # 或者 npm run build:win32
```
打包代码为桌面应用程序（windows x86) 并保存到指定文件夹下（默认路径：build文件夹下），替换build/icons文件夹下的三个不同格式的图标，可以更改三个平台上的exe启动文件的图标。

#### 4 总结

留个位置，以后想起来什么再写。
