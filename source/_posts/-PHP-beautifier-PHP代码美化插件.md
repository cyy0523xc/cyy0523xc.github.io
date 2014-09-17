title: "【PHP beautifier】PHP代码美化插件"
date: 2014-09-15 12:04:45
tags:
- PHP 
- 工具

---

### 使用方式

测试

```
cat test.php|php_beautifier --filters "Pear()"
```

- vim中使用快捷键： **CTRL + B**

### vimrc配置

vim .vimrc ， 在最后增加一行：

```
map <C-b> :% ! php_beautifier --filters "Pear() ArrayNested()"<CR>
```

### 主要文件

- /usr/bin/php\_beautifier：入口文件 
- /usr/share/php/PHP/: 源码目录

### 缺点

- 无法根据等号等对齐（原来根据等号对齐的代码有问题）


