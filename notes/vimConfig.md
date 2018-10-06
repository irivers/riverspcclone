# VimConfig--Vim配置记录

> 写文档的的初衷是为了记录下配置Vim的过程
>
> @RiversYung 2018106 享受假期，享受生活



## Vim配置原理

#### Vim配置文件位置

在`/usr/share/vim/vimrc`, 将文件拷贝到个人目录下即可`~/.vimtc`

```shell
cp /usr/share/vim/vimrc ~/.vimrc
```



## Vim常用配置

### 1. 代码缩进

将如下的配置添加到`~/.vimrc`中

```shell
set sw=4
set ts=4

filetype indent on
autocmd FileType python setlocal et sta sw=4 sts=4

// tabstop(ts):编辑一个tab字符占多少个空格位置
// shiftwidth(sw):使用每层缩进的空格数
// 代码第三行开启自动的缩进检测
// 最后一行，根据Python语言的简建议（tab展成4个空格），进行专门设置
```

### 2. 语法高亮

```
syntax on
```



## Vim常用快捷键

> 已经是最最常用的了，总是记不住

| 功能描述           | 快捷键 |
| ------------------ | ------ |
| 跳到行末尾         | $      |
| 跳到行开始处       | b      |
| 跳到最后一行       | G      |
| 跳到第一行         | gg     |
| 在光标下方新开一行 | o      |
| 在光标上方新开一行 | O      |
| 在光标前插入文本   | i      |
| 在光标后插入文本   | a      |
| 在行末插入文本     | A      |

