# 主题

## 主题概览

Hugo提供了一种健壮且易于实现、功能完整的主题系统.你可以在[HugoThemesWebsite](http://themes.gohugo.io)这里查看并下载使用在Hugo社区中展示的主题.

Hugo主题库由强大的Go模板库支持.设计目的是避免重复造轮子.里面的主题都可以重新自定义并和作者反馈同步.

## 安装和使用主题

使用命令行界面轻松通过Hugo主题展示页面安装并使用相应主题.

> Hugo现在并没有一个“默认”的主题，这是我们故意为之的，我们将主题的选择决定权交给您自己决定

### 假设（前提准备）

1. 您已经[在您的设备上安装了Hugo](https://gohugo.io/getting-started/installing/)
2. 您已经安装了git并且熟悉git的简单操作

### 安装主题

这个是[Youtube的介绍视频](https://gohugo.io/themes/installing-and-using-themes/#assumptions)

所有在[Hugo社区](https://themes.gohugo.io/)展示的主题都被集中在[这个GitHub的repo](https://github.com/gohugoio/hugoThemes)，这里的主题是包含着各种<u>**提醒和注释**</u>(这里翻译的有点问题)

> 如果您的电脑中没有安装[Git](https://git-scm.com),之后左右的主题工作都无法进行，Git的教程不在这个主题文档的讨论范围之内，但是在[GitHub](https://try.github.io/)和[Codecademy](https://www.codecademy.com/learn/learn-git)中都提供了针对初学者的免费的,互动性的教程.
>
> **译者注:这个GitHub出的教程非常非常棒,我花几天时间总共4\5个小时回顾了一下,懂得了好多原来没有理解的东西,察觉到了这种可视化的互动教程的前途,我觉得以后一定要做这种互动式的教程,这tm才是教育的未来,这tm才能让人有收获,有快乐,有动力去学习!**

#### 安装所有主题

你可以通过`git clone` [这个HugoTheme的Repo](https://github.com/gohugoio/hugoThemes)来下载所有当前可用的Hugo主题,当让下载速度和等待时间取决于你的网络连接状况(以及是否有代理).

```bash
git clone --depth 1 --recursive https://github.com/gohugoio/hugoThemes.git themes
```

在您使用主题之前,请将.git文件从您的主题根目录中删除,否则这将对您的Git使用产生一定的影响.

#### 安装一个主题

通过命令行,将执行目录改为`theme`目录,将所要下载的主题的URL替换掉`URL_TO_THEME`

```bash
cd themes
git clone URL_TO_THEME
```

这里有个使用主题`Hyde`的案例,在[这个Repo][https://github.com/spf13/hyde]可见这个主题的源代码.

```bash
cd themes
git clone https://github.com/spf13/hyde
```

或者,您可以将主题下载为.zip的压缩文件,自行解压安装在`themes`文件夹下

> 通常您需要仔细查看各个主题repo下`README.md`的介绍文件,这些文件往往包含很多重要参数修改\配置设置信息

### 主题位置

请确保您安装的主题在`/themes`文件夹下,这是Hugo的默认主题文件夹.您可以通过更改[网站变量配置](https://gohugo.io/getting-started/configuration/)中的`themesDir`变量来改变这个路径,不过我们并不推荐您这样做.

### 使用主题

Hugo首先将应用选定的主题,然后把所有本地配置信息配置好.这使得您可以和上流主题版本同步中维持兼容的同时自定义这些主题.详情请查看[自定义主题](https://gohugo.io/themes/customizing/).

### 命令行

有两种不同方式可以是您的Hugo网页应用主题:使用Hugo的命令行或一部分使用[网页配置文件](https://gohugo.io/getting-started/configuration/).

使用命令行去改变主题,您可以在编辑网站时使用`-t`标志:

```bash
hugo -t themename
```

相似地,若您想在本地以某种主题运行您的Hugo网站,特别是您打算[自定义主题](https://gohugo.io/themes/customizing/)时:

```bash
hugo server -t thememname
```

### `配置`文件

如果您已经决定好您所要使用的主题并不打算摆弄命令行,您可以将您的主题名称写进您的[网站配置文件](https://gohugo.io/getting-started/configuration/).

```bash
theme:themename
```

> 上文的`themename`案例必须和您在`/themes`中的主题名称匹配(是小写且url化的名称而不一定是theme的名称).

## 主题构成

**Hugo提供一套了使用主体组件的高级主题支持.**

从Hugo`0.42`版本开始一个Hugo项目可以使一个主题作为许多主题组件的集合来配置.

> 译者注:这里可以用多种标记语言如yaml\toml\json来作为配置文件

Yaml:

```yaml
 theme: 
 - my-shortcodes 
 - base-theme 
 - hyde
```

Toml:

```toml
theme = ["my-shortcodes", "base-theme", "hyde"]
```

Json:

```json
{
   "theme": [
      "my-shortcodes",
      "base-theme",
      "hyde"
   ]
}
```

你甚至可以嵌套这个config配置文件,给这个config再来一个config.toml(主题继承[^1]).

上面的主题配置案例的`config.toml`用三个主题组件以从左到右的主次顺序创建了一个主题.

对于任何给定的文件\数据等等,Hugo会首先关注`my-shortcodes`,然后是`base-theme`,最后是`hyde`.

Hugo使用了两种不同的算法来组合文件系统,使用哪种算法取决于文件类型:

- 对于`i18n`[^2]和`数据`文件,Hugo使用翻译id(原文:translation id)和文件中的数据key(原文:data key)深度整合它们.
- 对于`静态数据`(原文:static),`样式(模板)`(原文:layouts and templates)和`原型`(原文archetypes)文件,它们在文件层被整合.所以在最左端的文件会被(**优先?**)选择.

在以上`theme`描述中的主题名字必须和`您的网站/themes`文件夹里的名字一致,例如:`您的网站/themes/my-shortcodes`.

<u>我们有一个在这之后使得整个主题配置过程自动化进行的这么一个流程.[能力有限,这句话完全XJB翻译,我也不清楚是不是这个意思]</u>

还有值得注意的一点,每个主题的组件可以有自己的配置文件,例如自己的`config.toml`[^3].对于一个主题组件来说,它可以被配置的方面是有限的,如下:

- `参数`(全局的和每种语言的)
- `菜单`(全局的和每种语言的)
- `输出格式`和`媒体类型`(全局的和每种语言的)

这里有同样的应用顺序左侧优先规则:同样的ID的最左侧的 参数/菜单 会成为最终的选项.

在上面隐藏的和实验性质的命名空间支持(当编写theme时,我觉得是这个意思),有几之后我们会改进它们,不过我们鼓励主题的作者创作自己的命名空间以防止命名的冲突.

## 创作主题

使用`Hugo new theme`命令可以按照您的方式开搞一个新的主题.

> 如果您正在创作并计划在Hugo社区中分享一个主题,考虑到您将来主题的用户们可能不会在他们的网站根目录公布(<u>您制作的主题的链接?我猜是这么翻译</u>)我们建议使用相对URLs.
>
> 详情见[relURL](https://gohugo.io/functions/relurl)和[absURL](https://gohugo.io/functions/absurl).

Hugo可以通过使用`hugo new`命令在您现存的`themes`目录中初始化一个空白的主题菜单:

```bash
hugo new theme [name]
```

### 主题文件夹

一个主题组件可以提供一种或多种以下的标注Hugo文件夹里的文件:

**layouts**

渲染Hugo内容的模板,详见[Templates Lookup Order](https://gohugo.io/templates/lookup-order/).

**Static**

静态的文件,例如Logo\CSS\JS.

**i18n**

语言包(世界标准文件???这应该是字面的翻译).

**data**

数据文件.

**archetypes**

使用`hugo new`创造的文件模板.

### 主题配置文件

主题组件也可以提供其自己的配置文件，例如config.toml。可以在主题组件中配置的内容有一些限制，但是无法覆盖项目中的设置。

可以做以下设置：

* params （全球和每种语言）
* menu （全球和每种语言）
* outputformats 和 mediatypes

### 主题描述文件

另外介绍一下配置文件的描述文件,一个主题也可以通过一个`theme.toml`来描述这个主题,这个作者或者这个主题的作者等等等等,详见[在展示栏添加主题](https://gohugo.io/contribute/themes/).

“创建主题”最后更新时间：2018年6月12日：


[^1]: 现在在[Hugo主题页](https://themes.gohugo.io/)展示的主题都目前不支持,不过这个主题继承真的很实用,特比我是你需要在那些主题基础上建立自己的特有风格主题的时候.
[^2]: 是internationalization的缩写,详见[百度百科](https://baike.baidu.com/item/I18N/6771940?fr=aladdin).
[^3]: https://gohugo.io/getting-started/configuration/

