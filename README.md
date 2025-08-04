## 说明

此处作为 eide 的公共远程模板仓库，使用 eide 的 `从远程模板创建项目` 功能时，将在此仓库中获取可用的项目模板

![image](https://user-images.githubusercontent.com/43022536/165442017-dc7f8377-f533-422d-b56a-bef5c5ef2cd9.png)

## 模板浏览器

由于国内连接 Github 不太稳定，因此我们正在制作一个更加易于使用的模板浏览页面，站点如下：

https://templates.em-ide.com/

大家可以在此处浏览、下载模板，同时我们也在增加更多的功能，以便大家能够更方便地分享自己的模板。

## 如何贡献项目模板？

- **克隆仓库**：在 github 上 fork 仓库到自己的账号中，并在本地使用 `git clone` 克隆 fork 后的仓库

- **生成模板**：在 vscode 上打开你想要分享的 eide 项目，使用 [导出 eide 项目模板](https://docs.em-ide.com/#/zh-cn/export_project?id=%e5%af%bc%e5%87%ba-eide-%e6%a8%a1%e6%9d%bf) 功能，将在项目根目录下生成一个 `.ept` 文件

- **检查模板**：使用 eide 的 `从本地模板创建项目` 功能，选择上一步生成的模板文件，新建一个工程，检查工程是否可用

- **添加文件**：将 `ept` 文件复制到本地仓库中

- **添加索引**：修改索引文件：`index.json`，将模板的信息（文件名，分类，版本号，可读名称，作者名称...）添加到其中；可参考该 [提交记录](https://github.com/github0null/eide-templates/pull/1/commits/8994e21c7b0898f228b45649859b6a54eef1566e#diff-7aebb122a6ea8a2749d60cb05b7e103c9eae6e2e85e48d2d6cd9e20b63013975)

- **推送仓库**：将更改后的本地仓库推送至 github 远程仓库中，然后创建 `pull request`，请求合并至主仓库，合并完成之后即可在 vscode 上直接使用

## 注意事项

- 不清楚如何修改/添加模板的，可以参考该 [提交记录](https://github.com/github0null/eide-templates/pull/1/commits/8994e21c7b0898f228b45649859b6a54eef1566e#diff-7aebb122a6ea8a2749d60cb05b7e103c9eae6e2e85e48d2d6cd9e20b63013975)

- 设置模板分类时，尽量为模板选择**已存在**的分类，若要新建分类，应该使用**更具代表性的名称**，而不是只是用 `芯片系列名`，因为插件会根据这个分类生成树状列表方便查看和检索，如果你使用的分类粒度过细，反而不会便于快速选择

- 在提交 `pull-request` 之前，可以修改插件设置：`EIDE.Repository.Template.Url` 将模板仓库临时修改为你自己的仓库分支，然后新建项目进行测试，一切 ok 后，再 `pull-request`

- 提交 commit 内容，使用 markdown 格式，语言使用中文。比如：`增加 STM32F407xxx 项目模板`。便于后续自动化工具生成 ChangeLog。

## 模板格式要求

> 新增加的模板可以参考；旧的模板暂时保持原样吧

模板的文件夹结构要足够简单清晰，格式统一

对于源文件的存放，可以参考如下文件夹结构：

```
---
  |
  +--- examples       用于存放例程源码
       |
       +--- gpio
            |
            +--- main.c
       |
       +--- adc
       |
       +--- timer
       |
       +--- ...
  |
  +--- library        用于存放源码库，比如 外设库，dsp算法库，github上的开源库 等等
       |
       +--- STM32F10x_StdPeriph_Driver
            |
            +--- inc
            |
            +--- ...
       |
       +--- lwrb-v3.2.0
       |
       +--- ...
  |
  +--- include        用于存放用户的 标头文件（.h, .hpp）等
  |
  +--- source         用于存放用户的 源文件
       |
       +--- main.c
       |
       +--- ...
  |
  +--- tools          用于存放一些工具类的程序，配置文件等，比如自定义的 烧录脚本，命令行烧录软件 等
  |                   这个文件夹也是EIDE在搜索一些配置文件时的默认搜索目录，比如：如果你有一些手写的 openocd cfg 文件，你可以存放在这个目录下
.....
  |
  +--- ...其他文件夹1...
  |
  +--- ...其他文件夹2...
  

```

### 模板文档

> 虽然项目模板使得创建项目更加方便，但是由于目前仓库中的模板缺乏文档，如果不是模板作者自己，其他人使用起来可能会有一些疑惑的地方。

新增加的模板根目录下应当附带一个 `README.md`，以及使用 `README.md` 生成的同名 pdf 文件 `README.pdf`（markdown 转 pdf 可以使用相关插件完成）

增加一定的文档，便于模板的维护，以及向用户更加详细的介绍模板，确保不会选择错误的模板。

这里有一个示例，这将指引你如何放置模板的文档：

![](https://em-ide.com/resource/template_docs.png)

同时我们计划在 模板浏览器 中提供模板文档的预览，因此向模板添加文档是有必要的。

## 模板索引格式

模板索引文件 index.json 存放了所有的模板的信息，该文件是一个 `TemplateIndexDef` 类型的 json 对象，定义如下：

```typescript

// 模板索引
export interface TemplateIndexDef {
    category_map: { [name: string]: CategoryInfo };
    template_list: TemplateInfo[];
}

// 模板分类
export interface CategoryInfo {
    display_name: string;   // 用于显示的名称
    description: string;    // 简短的描述
}

// 模板信息
export interface TemplateInfo {

    file_name: string;          // 模板文件名

    display_name: string;       // 用于显示的名称

    category: string[];         // 模板所属的分类

    version: string;            // 版本

    author: string | undefined; // 作者名称
}

```
