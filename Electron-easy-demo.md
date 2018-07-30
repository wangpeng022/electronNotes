### Electron


#### 简介

Electron是由Github开发 （曾用名 Atom Shell），它将Chromium和Node.js合并到同一个运行环境，使前端开发者能用HTML，CSS和JavaScript来构建跨平台桌面应用程序。大大缩减了开发者打包构建等过程，以至于把精力集中到业务代码上。

#### 初步上手

1. 初始化一个项目

```
npm init
```
2. 目录结构

```
your-app/
  ├── package.json
  ├── main.js
  └── index.html
```
3. `package.json`

```
{
    "name": "your-app",
    "version": "0.1.0",
    "main": "main.js",
    "scripts": {
      "start": "electron ."
    }
  }
```
4. 安装 Electron

```
npm install --save-dev electron
```

这个官方推荐还是本地安装，就像安装vue一样，更能控制electron的版本，因为更新频繁，并且可能不会向下兼容，这个版本问题会是个问题。
Tips：当然这里推荐使用cnpm、taobao.org 安装，原因你懂的。

5. 创建窗口，在main.js中require各种内置模块:

```
const {app, BrowserWindow} = require('electron')

  function createWindow () {
    // 创建浏览器窗口
    win = new BrowserWindow({width: 800, height: 600})

    // 然后加载应用的 index.html。
    win.loadFile('index.html')
  }

  app.on('ready', createWindow)
```
	1) electron.app  负责管理Electron 应用程序的生命周期；
	2) electron.BrowserWindow  类负责创建窗口。
6. 启动程序
q
```
npm start
```
是不是自动启动了窗口？什么，你说出窗口了但是没内容？那就对了，因为页面渲染的是index.html ，现在你可以尽情的用html+css开始绘画了。
7. 最后加入一些内容：
	1）打开开发者工具
   2）增加对macOS的优化

```
const {app, BrowserWindow} = require('electron')

  // Keep a global reference of the window object, if you don't, the window will
  // be closed automatically when the JavaScript object is garbage collected.
  let win

  function createWindow () {
    // 创建浏览器窗口。
    win = new BrowserWindow({width: 800, height: 600})

    // 然后加载应用的 index.html。
    win.loadFile('index.html')

    // 打开开发者工具
    win.webContents.openDevTools()

    // 当 window 被关闭，这个事件会被触发。
    win.on('closed', () => {
      // 取消引用 window 对象，如果你的应用支持多窗口的话，
      // 通常会把多个 window 对象存放在一个数组里面，
      // 与此同时，你应该删除相应的元素。
      win = null
    })
  }

  // Electron 会在初始化后并准备
  // 创建浏览器窗口时，调用这个函数。
  // 部分 API 在 ready 事件触发后才能使用。
  app.on('ready', createWindow)

  // 当全部窗口关闭时退出。
  app.on('window-all-closed', () => {
    // 在 macOS 上，除非用户用 Cmd + Q 确定地退出，
    // 否则绝大部分应用及其菜单栏会保持激活。
    if (process.platform !== 'darwin') {
      app.quit()
    }
  })

  app.on('activate', () => {
    // 在macOS上，当单击dock图标并且没有其他窗口打开时，
    // 通常在应用程序中重新创建一个窗口。
    if (win === null) {
      createWindow()
    }
  })
```

8.如果实在还是有问题下载这个[github上的Demo](https://github.com/wangpeng022/first-easy-electron.git)吧。

 Now，愉快的开始electron之旅。