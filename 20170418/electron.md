### Electron 的学习
#### quick start
- 与普通浏览器不同点，普通浏览器的页面，是在一个沙盒中运行，而Electron是在她的平台上运行
- Main process and renderer process  
  - Electron 有两种不同进程，一个是主进程（main process），一种是渲染进程（renderer process）,其中主进程是用于启动app，渲染进程是用于渲染web page 
  - 主进程产生于browserWindow(浏览器)，并且管理器相应的renderer process,当一个browserWindow 关闭，其相应的renderer process 也相应的被destroyed
- 基本目录结构
  - package.json 里面需要设置main入口，如果没有设置，则默认去寻找index.js作为入口
  - main.js 需要创建一个window，并且处理相关的的系统事件
- 兼容性
  - macos 64位 10.9 win7 以上   

