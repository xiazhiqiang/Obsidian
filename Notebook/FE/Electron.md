## Electron-Vite

> https://cn.electron-vite.org/

### 预加载基本原理

- 预加载脚本会在渲染器的网页加载之前注入。
- 预加载脚本运行在具有 HTML DOM APIs 和 Node.js、Electron APIs 的有限子集访问权限的环境中。
- - **在主进程和渲染进程之间通信**：使用 Electron 的 `ipcMain` 和 `ipcRenderer` 模块进行进程间通信（IPC）。

![[Pasted image 20250505172451.png]]
[@electron-toolkit/preload 使用API](https://github.com/alex8088/electron-toolkit/tree/master/packages/preload)

### worker

[electron worker demo](https://github.com/alex8088/electron-vite-worker-example/tree/master)

## 参考资料
