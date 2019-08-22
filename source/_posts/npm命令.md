---
title: npm命令
date: 2019-08-22 19:04:01
tags: [npm]
---

#### npm命令选项

- `search`：在存储库中查找模块包，如：`npm search express`

- `install`：使用在存储库或本地位置上的一个`package.json`文件来安装包

  ```cmd
  npm install 
  npm install express 
  npm install express@0.1.1 
  npm install express@latest 
  npm install ../tModule.tgz
  ```

- `install -g`：在全局可访问的位置安装一个包，如：`npm install express -g`

- `uninstall`：卸载一个模块，如：`npm uninstall express`

- `remove`：删除一个模块	

- `pack`：把在一个`package.json`文件中定义的模块封装成`.tgz`文件，如：`npm pack`

- `view`：显示模块的详细信息，如：`npm view express`

- `publish`：把在一个`package.json`文件中定义的模块发布到注册表，如：`npm publish`

- `unpublish`：取消发布您已发布到注册表的一个模块（在某些情况下，还需使用 `--force` 选项），如：`npm unpublish myModule`

- owner：允许您在存储库中添加、删除包和列出包的所有者

  ```cmd
  npm add <username> myModule 
  npm rm <username> myModule 
  npm ls myModule
  ```

- `whoami`:（根据指定注册表模块）打印用户名

  ```cmd
  npm whoami
  npm whoami --registry https://registry.npm.taobao.org
  ```

- `adduser`：将用户信息添加到当前的开发环境

  ```cmd
  npm adduser
  
  # 默认是登录到 npm 上，如要登录到其他源上，需要使用 registry 选项
  # 要想登录到某个 scope 中需要使用 scope 选项
  npm adduser --registry=http://myregistry.example.com --scope=@myco
  ```

- `login`：等同于adduser，如：`npm login`

- `logout`：将用户信息从当前的开发环境中清除，如：`npm logout`

- `init`：初始化Node包的信息，会创建`package.json`文件，如：`npm init`

