---
{"publish":true,"created":"2025-06-01T00:26:57.000+08:00","modified":"2025-12-27T14:34:31.907+08:00","tags":["我的电子资产观察"],"cssclasses":""}
---


## 烦人的安卓相册问题

obsidian 的图片同步到安卓设备后会被相册索引，包括微信等 app 发送图片的时候都会被这些 ob 图片污染
加个 .nomedia 试试，但看网上说澎湃 OS2 后不行了
![[Internal/attachments/20251227/Observation - Obsidian-1766817179113.webp]]!

## 自定义样式探究

this is **BOLD** ...
this is **<u>underline</u>** ... (**好像没有**, 看起来得用 markup 实现)
this is ~~Delete~~ ....
this is ==highlight== ...


2<sup>100</sup> =  ?
## A Iconic Bug

第一次安装 Iconic 怎么都没反应，突然意识到这应该是个 chromium 套壳应用，随即 cmd + I 激活了 devtools, 顺着报错信息找到出错的代码行

看起来报错是在 filename.lastIndexOf 报了读 undefined 的错误，也就是 filename 没有定义, 上面这个正则有问题

断点调试发现我的本地目录里有个叫 `Icon?` 的文件，但后面这个问号应该不是寻常问号，可能是某个不可见字符，所以导致正则出错

尝试移除 ==Icon?== 文件后一切恢复正常
> 但遗憾的是忘了备份一下当时的那个文件，不太确定具体出问题的是哪个字符了

``` typescript

/**

* Split a filepath into its hierarchical components.

*/

splitFilePath(fileId = ''){

const subpathExts = ['md', 'pdf']; // Extensions with linkable subpaths
const subpathStart = Math.max(...subpathExts.map(ext => {
const index = fileId.lastIndexOf(`.${ext}#`);
return index > -1 ? (index + ext.length + 1) : -1;
}));
const subpath = subpathStart > -1 ? fileId.substring(subpathStart, fileId.length) : '';

const path = subpathStart > -1 ? fileId.substring(0, subpathStart) : fileId;


const [, tree = '', filename] = path.match(/^(.*\/)?(.*)$/) ?? [];

const extensionStart = filename.lastIndexOf('.');
const extension = filename.substring(extensionStart > -1 ? extensionStart + 1 : filename.length) || '';
const basename = filename.substring(0, extensionStart > -1 ? extensionStart : filename.length) || '';

  

return { path, tree, filename, basename, extension, subpath };

}


```



## Footnotes

突然发现脚注 [^1] 是很好用的一个特性，可以比较规整地区分正文和 comment[^2]，在==阅读模式==下可以快速跳转到脚注说明，以及快速跳转回来


> [!NOTE|aside-l] 左侧注释
> 注释内容



## Image Management

> TBD [^3]

## 文件合并
this snippet is for testing

[^1]: 就像这样
[^2]: 包括一些之前需要通过括号来补充说明的内容
[^3]: 文件目录管理和 size 管理也是一门学问，写了一些但又删掉了，总觉得说的不是那么贴切
