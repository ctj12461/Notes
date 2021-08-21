---
title: Sublime Text 4 配置笔记
date: 2021-08-15 13:38:27
tags:
 - Sublime Text
 - 配置
 - 编辑器
 - 插件
categories: Sublime Text
---

`Sublime Text` 是一个优秀的跨平台编辑器，它不仅 `UI` 华丽，高速，又高度可配置。通过一系列配置，
可以让你体验到 `IDE` 般的强大和记事本的速度。接下来是基于 `Windows 10` 的 `Sublime Text 4` 
的配置笔记。
<!-- more -->

### 配置前的准备
首先肯定是要有一个 `Sublime Text` 编辑器，版本影响不大，一般都是通用的，不同系统间也可以照搬。

如果对便携版有需求，安装后先不要打开编辑器，在安装目录下创建一个 `Data` 目录，这样 `Sublime Text`
就会把所有的数据都储存在其中，包括所有的插件和配置文件。如果已经打开了，可以通过 `首选项-浏览插件目录`
查看插件目录，进而找到存放所有的数据的目录。比如 `Windows` 都是在 `%AppData%/Roaming/Sublime Text` 
中，移动这个目录到安装目录下再重命名即可。这样方便了备份，重装系统时就很方便了。

### 插件安装
#### `Package Control`
`Sublime Text` 的可扩展能力主要体现在它的插件上，而 `Package Control` 就是它的插件管理器。我们可
以用 `Package Control` 安装插件，管理插件。

首先访问 <http://packagecontrol.io/installation> 下载 `Package Control.sublime-package`，
再把它放到数据目录里的 `Installed Packages` 中，重启 `Sublime Text`。

接下来按 `Ctrl + Shift + P` 打开 `Commmand Palette`, 输入 `Package Control:` 前缀，即可找到
所有 `Package Control` 命令，注意，`Sublime Text` 支持模糊搜索，可以只输入 `pc`。

#### 修改语言
找到 `Package Control: Install Package`，按 `Enter`，等待其加载完成，然后安装 `ChineseLocalization`。
随后可以在 `帮助-Language` 中切换为其他语言。

如果出现 `There are no packages aliveable for installation`，可以多试几次。

#### `Colorsublime`
这是一个管理配色方案的插件，支持先在编辑器中预览后安装。

安装后可以在 `Commmand Palette` 中找到 `Colorsublime: Install Theme`。`Colorsublime` 会先下载所有的
配色方案以提供预览，下载后可以在它的配置目录 `[Your Data Directory]/Packages/Colorsublime - Themes` 
中找到缓存目录 `cache`，可以删除它以节省空间。

我个人比较喜欢 `Into The Dark` 和 `Mariana` 这两种。

`Into The Dark`:  
{% asset_img into-the-dark-1.png %}  
{% asset_img into-the-dark-2.png %}  

`Mariana`:
{% asset_img mariana-1.png %}  
{% asset_img mariana-1.png %}  

#### `EditorConfig`
如果见过一些大型项目，应该对 `.editorconfig` 不会陌生，它规定了编辑器应该如何处理文件，如
 - 是否要把 `Tab` 转换为 `Space`
 - `Tab` 的宽度
 - 使用 `LF` 还是 `CRLF`
 - 是否要删除每行结尾的空格
 - 文件使用的编码

但是 `Sublime Text` 必须要先安装 `EditorConfig`。之后只要目录下有 `.editorconfig`，
`Sublime Text` 就会根据其中的内容进行处理。

#### `SideBarEnhancements`
`Sublime Text` 原来的侧边栏功能极少，安装这个插件后就可以当一个小型的文件管理器了。

#### `SublimeAStyleFormatter`
`SublimeAStyleFormatter` 为 `C/C++`、`Java`、`C#` 提供了代码格式化功能。

在 `首选项-Package Settings-SublimeAStyleFormatter` 中可以找到配置文件，同时打开 `Default` 和 `User`，
可以参考着 `Default` 在 `User` 中进行配置，如果想要修改一个配置，直接在 `User` 中进行覆盖即可。

`Setting` 中是格式化选项，比如代码风格和括号的选项。
以下是我自己的配置：
```json
{
    "options_default": {
        // Default bracket style
        // Can be one of "allman", "bsd", "break", "java", "attach", "kr", "k&r",
        // "k/r" "stroustrup", "whitesmith", "banner", "gnu", "linux", "horstmann",
        // "1tbs", "otbs ", "google", "pico", "lisp", "python", "vtk", or null
        // for default.
        "style": "java",
    }
}
```

`Key Bindings` 则是快捷键的配置，这个配置文件是所有的快捷键配置公用的，你可以看到其他的快捷键配置。
同样是推荐在 `User` 里配置。

以下是我自己的 `SublimeAStyleFormatter` 键位配置：
```json
[
    // ...
    // 对这个文件格式化 Shift + Alt + F
    {
        "keys": ["shift+alt+f"], "command": "astyleformat",
        "context": [{"key": "astyleformat_is_enabled", "operator": "equal", "operand": ""}]
    },
    // 对选中区域格式化
    {
        "keys": ["ctrl+alt+f"], "command": "astyleformat", "args": {"selection_only": true},
        "context": [{"key": "astyleformat_is_enabled", "operator": "equal", "operand": ""}]
    }
    // ...
]
```
把最大的 `[]` 里的部分它粘贴进去就就可以了。

#### `Transparency`
窗口透明插件，安装后按 `Ctrl + Shift + 1 ~ 6` 调整不透明度。  
{% asset_img transparency-addon.png %}

#### `DocBlockr`
快速生成注释的插件，如果你想要为函数或变量生成一个详细的注释而又不想写文档，可以用它。
```cpp
#include <iostream>

using namespace std;

class Writer {
public:
    Writer();
    ~Writer();

    /**
     * [write description]
     * @param count      [description]
     * @date  2021-08-15
     */
    void write(int count) {
        for (int i = 1; i <= count; ++i)
            cout << word << endl;
    }
private:
    string word = "Hello World";
};

/** [main description] */
int main() {
    Writer().write(1);
    return 0;
}

```

第一处的注释可以通过输入 `/**` 然后再按 `Enter` 或 `Tab` 来完成，`DocBlockr` 会默认选中第一个
括号，还可以通过 `Tab` 跳转到下一个括号，其中换行会自动在前面补全 `*`。第二个可以输入 `/**` 然后再按 
`Space`。比较奇怪的是这个插件不支持为构造函数和析构函数创建注释。

`DocBlockr` 同样支持配置：
```json
{
    // 额外添加的信息，如 @author
    // 可以到 Default 配置文件里面查找，里面写很清楚
    "jsdocs_extra_tags":[
        "@date {{date}}",
    ],
    // 为注释启用简单的描述
    "jsdocs_function_description": true,
    // @date 这类额外的信息是否要放在最后
    "jsdocs_extra_tags_go_after": true
}
```

#### `CTags`
`Sublime Text` 虽然拥有 `Go to Definition` 的功能，但不够完善，可以使用 `CTags` 进行补充。

`CTags` 是一个生成 `C/C++` 代码的 `tags` 文件的程序，`tags` 文件可以用于跳转和补全。`CTags` 的优
势在于它非常轻量级，而且十分高效，一些大型项目如果使用 `VS Intellisense` 这样的工具，卡顿会很严重，
并且占用大量系统资源，`CTags` 虽不是很智能，但也可以满足大多数需求。

首先要安装 `CTags` 程序，注意是 `Exuberant CTags`，可以在 <http://ctags.sourceforge.net/> 
上安装，然后在 `Sublime Text` 中安装 `CTags` 插件，以调用 `CTags`。

在需要跳转的目录下打开一个文件，在 `Command Palette` 中运行 `CTags: Rebuild Tags`，`CTags` 就会
在目录下递归地生成 `tags`，最后生成 `.tags` 和 `.tags_sorted_by_file` 两个文件。然后就可以使用跳转
功能了。

默认快捷键：
 - `Ctrl + T, Ctrl + T`：跳转到光标所指向的符号
 - `Ctrl + T, Ctrl + Y`：搜索指定的符号
 - `Ctrl + T, Ctrl + B`：跳回

当然，这个快捷键还是不够方便，我有进行了进一步配置：
```json
[
    // 跳转到光标所指向的符号
    {
        "command": "navigate_to_definition",
        "keys": ["ctrl+]"]
    },
    // 跳回
    {
        "command": "jump_prev",
        "keys": ["ctrl+["]
    },
    // 搜索指定的符号
    {
        "command": "search_for_definition",
        "keys": ["ctrl+\\"]
    }
    // ...
]
```
这个配置有参考 `Vim` 的快捷键，但又不完全相同，我个人挺喜欢使用。要记得及时更新 `tags`。

### 键位配置
高效的操作离不开快捷键，前面的插件已经配置了一部分键位，接下来是针对 `Sublime Text` 本身
的键位配置。
#### 编译系统
`Sublime Text` 可以自定义编译系统进行构建，接下来会讲到，如果有需要可以配置一下这一部分的
快捷键。

```json
[
    // 使用上一次的选项编译
    {
        "keys": ["f5"],
        "command": "build"
    },
    // 重新选择一个选项并编译
    {
        "keys": ["ctrl+f5"],
        "command": "build",
        "args": {"select": true}
    }
]
```

#### 文件切换
标签页可以通过 `Ctrl + Tab` 切换，但是往后切换要按 `Ctrl + Shift + Tab`，可以改成
`Shift + Tab`。

同时原来的切换焦点到侧边栏的快捷键要按两次，也可以改。
```json
[
    // 上一个文件 
    {
        "keys": ["shift+tab"],
        "command": "prev_view_in_stack"
    },
    // 切换到侧边栏，如果前面编译系统的键位没改，这里会冲突
    {
        "keys": ["ctrl+b"],
        "command": "focus_side_bar"
    }
]
```

#### 项目切换
`Sublime Text` 可以管理项目，如果需要开始在多个项目中切换，就可以设置 `Ctrl + Alt + P` 
实现该功能。

```json
[
    {
        "command": "prompt_select_workspace",
        "keys": [
            "ctrl+alt+p"
        ]
    }
]
```

#### 行交换
```json
[
    // 向上交换
    {
        "keys": ["alt+up"], 
        "command": "swap_line_up"
    },
    // 向下交换 
    {
        "keys": ["alt+down"], 
        "command": "swap_line_down"
    }
]
```

#### 分组操作
`Sublime Text` 中的分组快捷键一般都要两步，且按起来都要用到方向键，对于笔记本电脑很麻烦，
而 `Alt` 基本没用，可以利用 `Alt` 优化成一步并把方向键的操作移动到 `J K L I` 或者模仿
`Vim` 改成 `H J K L`。

```json
[
    // 新建一个分组并移动当前文件到其中
    {
        "keys": ["alt+i"],
        "command": "new_pane"
    },
    // 新建一个分组
    {
        "keys": ["ctrl+alt+i"],
        "command": "new_pane",
        "args": {
            "move": false
        }
    },
    // 关闭当前分组
    {
        "keys": ["alt+k"],
        "command": "close_pane"
    },
    // 移动焦点到下一个分组
    {
        "keys": ["alt+j"],
        "command": "focus_neighboring_group",
        "args": {
            "forward": false
        }
    },
    // 移动焦点到上一个分组
    {
        "keys": ["alt+l"],
        "command": "focus_neighboring_group"
    },
    // 移动当前文件到下一个分组
    {
        "keys": ["ctrl+alt+j"],
        "command": "move_to_neighboring_group",
        "args": {
            "forward": false
        }
    },
    // 移动当前文件到上一个分组
    {
        "keys": ["ctrl+alt+l"],
        "command": "move_to_neighboring_group"
    }
]
```

### 编译系统
#### 配置
`Sublime Text` 对每一种常见的语音都提供了默认的编译系统，但是这些编译系统不一定适合我们。
我们可以通过一些简单的配置完成一个自定义编译系统的创建。接下来以配置 `C++` 单文件运行环境为例。

选择菜单上的 `工具-编译系统-新建编译系统`，`Sublime Text` 会新建一个配置文件，可以先参考
我的配置，再修改：
```json
{
    // 文件编码
    "encoding": "utf-8",
    // 工作目录
    "working_dir": "$file_path",
    // 正则表达式匹配文件
    "file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
    // 文件类型，用于选择指定的文件进行编译，后缀可以改成任意语言的文件的后缀
    "selector": "source.cpp",
    "variants": [
        // 仅编译
        {
            "name": "Build",
            "shell_cmd": "g++ -Wall -std=c++17 \"$file\" -o \"$file_base_name\"" 
        },
        // 编译运行，从 project.in 中获得输入，并输出到 project.out 中
        {
            "name": "Run",
            "shell_cmd": "g++ -Wall -std=c++17 \"$file\" -o \"$file_base_name\" && \"${file_path}/${file_base_name}\" < project.in > project.out" 
        },
        // 编译后在终端中运行
        {
            "name": "Run In Terminal",
            "shell_cmd": "g++ -Wall -std=c++17 \"$file\" -o \"$file_base_name\" && start cmd /c \"\"${file_path}/${file_base_name}\" & pause \""
        },
        // 在终端中调试
        {
            "name": "Debug",
            "shell_cmd": "g++ -Wall -g -std=c++17 \"$file\" -o \"$file_base_name\" && start cmd /c \"gdb \"${file_path}/${file_base_name}\" & pause \""
        }
    ] 
}
```

#### 运行
默认按 `Ctrl + B` 按照上一次的选项编译，按 `Ctrl + Shift + B` 选择一个选项编译。

注意 `Sublime Text` 的输出窗口是不能输入的，如果要输入数据，可以使用重定向或终端运行。