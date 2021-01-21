### node 命令

> - node 位置 where node
> - node wondows 平台下的版本管理 https://github.com/coreybutler/nvm-windows，需要把目前安装的所有node卸载； 可以参考 https://www.jianshu.com/p/7e573feed39e https://www.jianshu.com/p/0ffa636a6fe1
> - node 其他平台下的版本管理有 nvm ,n
> - 进入风险监控平台
> - 系统设置 > 用户管理 > 根据`员工号`或`机构号`查询出相应人员，编辑用户为`财务管理权限`. _分行对应分行财务管理权限、省行对应省行财务管理权限_

### node 注意事项

- n 包不支持 window 平台

---

---

### npm 命令

> - npm 全局安装包位置 where npm
> - 卸载全局包 npm uninstall -g <package> ；卸载后，你可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看：npm ls
> - 卸载/删除本地包 npm uninstall <package>
> - npm 全局安装过哪些模块 npm list -g --depth 0
> - 【npm install xxx】利用 npm 安装 xxx 模块到当前命令行所在目录；称为本地安装；本地安装还有以下规则

> > - 【npm install xxx】安装但不写入 package.json；
> > - 【npm install xxx --save】 安装并写入 package.json 的"dependencies"中；
> > - 【npm install xxx --save-dev】安装并写入 package.json 的"devDependencies"中。

> - 【npm install -g xxx】利用 npm 安装全局模块 xxx；

---
