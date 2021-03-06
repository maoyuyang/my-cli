<p>
  <a href="https://www.npmjs.com/package/myy-cli"><img src="https://img.shields.io/npm/v/myy-cli.svg" alt="version"></a>
  <a href="https://www.npmjs.com/package/myy-cli"><img src="https://img.shields.io/npm/dm/myy-cli.svg" alt="downloads"></a>
  <a href="https://www.npmjs.com/package/myy-cli"><img src="https://img.shields.io/github/license/maoyuyang/mcli.svg" alt="license"></a>
  <img src="https://img.shields.io/david/maoyuyang/mcli.svg">
  <img src="https://img.shields.io/david/dev/maoyuyang/mcli.svg">
</p>

## mcli脚手架工具 

做这个的原因，就是写一些小项目的时候，老是要重新配置，所以写一个可以快速生成项目的

## usage
```
  npm install -g myy-cli
  mcli init hello
```

## 总结

涉及到库：commander, chalk, metalsmith, ora, rimraf, download-git-repo, handlebars, inquirer

1. 利用package.json的bin选项指定命令名和可执行文件的位置

2. commander.parse之后，会自动执行执行子命令。比如在命令行输入`mcli init hello`, commander在解析完参数之后，获取第一个参数`init`，自动拼接成一个新文件名`mcli-init`，然后尝试寻找`mcli-init.js`文件来执行。第二个参数`hello`从`commander.args[0]`可获取到。所以分成了两个文件

3. chalk，一个颜色插件，可以改变命令行文字颜色

4. ora, 显示等待图标的插件

5. rimraf, 用于删除文件的插件

6. download-git-repo, 用于从github拷贝模板的插件

7. inquirer, 提供交互式命令的插件，基本用法可见mcli-init.js文件，每一个问答都会被封装到then的answer里，answer是一个对象，它的key是之前的name字段，value是回答的答案

8. metalsmith，静态网站生成器，这里可以简单理解为用于批量处理模板

9. handlebars，模板引擎，具体用法见`generator.js`，就一行`Handlebars.compile(t)(meta)`，第一个参数是模板字符串，meta是要替换的键值对

8,9主要用于模板插值，为什么要这个，因为想实现init不同的项目名称, 相关文件里的文件名也要对应替换的效果

metalsmith有一个metadata的东西，可以理解为全局的变量对象。把需要替换的键值放进去。

基本流程：`mcli init xxx`获取xxx为项目名称。检测同名文件夹是否已存在等等，然后交互式命令，确定名称，版本号，简介。这些为要替换的东西，也就是metadata。然后从github拉取模板，下载完成后，用metalsmith和handlebars替换模板内容，结束

## UPDATE

1. 使用cac替代commander，原因是看到了vuepress用了，尝试一下，感觉api更简洁。另外使用高阶函数可以非常有效的解耦代码
