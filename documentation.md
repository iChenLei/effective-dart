<a name="top"></a>
## 🔥 Effective Dart: Documentation 🔥 

大多数程序员讨厌两件事：1.写注释 2.别人不写注释。这很滑稽，但也说明了注释的重要性，特别是自己在接手别人的代码时。我们需要注释来保证代码的可维护性。

- 注释
    - [DO] [用描述性的语句写注释](#1-1)
    - [DON'T] [使用块注释作为文档](#1-2)
- 文档注释
    - [DO] [使用`\\\`文档注释去注释成员和类型](#2-1)
    - [PREFER] [为公共API写文档注释](#2-2)
    - [CONSIDER] [编写库级别的文档注释](#2-3)
    - [CONSIDER] [为私有API编写文档注释](#2-4)
    - [DO] [使用一句话完成文档总结](#2-5)
    - [DO] [将文档注释段落的第一句话分离出来](#2-6)
    - [AVOID] [注释避免冗余](#2-7)
    - [PREFER] [注释函数或者方法时使用第三人称动词](#2-8)
    - [PREFER] [注释变量，getter/setter时使用名词短语](#2-9)
    - [PREFER] [注释库或者类型时使用名词短语](#2-10)
    - [CONSIDER] [在注释中提供代码例子](#2-11)
    - [DO] [在文档注释中使用方括号突出作用域內标识符](#2-12)
    - [DO] [使用简介的描述来注释参数，返回值和抛出的异常](#2-13)
    - [DO] [将文档注释放在元数据注解`@`之前](#2-14)
- Markdown
    - [AVOID] [避免过度使用Markdown](#3-1)
    - [AVOID] [避免使用HTML作为注释](#3-2)
    - [PREFER] [使用反引号来区分代码块](#3-3)
- 书写
    - [PREFER] [尽量简短](#4-1)
    - [AVOID] [尽量避免使用缩略词除非它们很常见](#4-2)
    - [PREFER] [使用`this`而不是`the`去指代实例本身](#4-3)

### 注释
> 下面的注释不会出现在自动生成的文档中

<a name="1-1"></a>
#### [DO] 用描述性的语句写注释
```dart
    // Not if there is nothing befort it.
    if (_chunks.isEmpty) return false;
```
> 将第一个单词首字母大写除非是大小写敏感的标识符，这对文档注释，TODO等都是通用的

<a name="1-2"></a>
#### [DON'T] 不要使用块注释作为文档
``` dart
    // good
    greet(name) {
        // Assume we have a valid name
        print('Hi $name !');
    }

    // bad
    greet(name) {
        /* Assume we have a valid name */
        print('Hi $name');
    }
```
*你可以使用在代码体外使用块注释（/\* ... \*/）作为临时注释，其他情况使用\/\/*

**[⬆ back to top](#top)**

### 文档注释
> 文档注释是很重要的，因为dartdoc可以解析注释来生成文档。dartdoc工具会根据`///`标识符去寻找解析文档注释相关内容。

<a name="2-1"></a>
#### [DO] 使用`///`也就是文档注释来方便生成文档
> 使用文档注释代替普通注释可以方便dartdoc工具去解析注释以生成文档
```dart
    // good
    /// The number of characters in this chunk when unsplit.
    int get length => ...

    // bad
    // The number of characters in this chunk when unsplit
    int get length => ...
```
> 因为历史原因dartdoc支持两种风格的文档注释( `///` C# style)和( `/** ... */` JavaDoc Style),我们更建议使用///这种风格，因为相对于JavaDoc风格更加紧凑特别是多行注释的时候。

<a name="2-2"></a>
#### [PREFER] 为公共API写文档注释
> 你不需要为每一个顶级变量，库，类型和成员写上注释，但是大多数还是需要文档注释的

<a name="2-3"></a>
#### [CONSIDER] 编写库级别的文档注释 
> 不同于其他语言比如Java类是一个单独的程序组织(比如JAR包)，在Dart语言中库是可以直接被用户使用的(有点类似Nodejs库)。这导致了`library`指令所在的位置成了一个比较好的位置，可以用来写文档来注释说明库的主要用途已经提供了哪些函数供使用。可以包括以下内容：

- 一句话说明总结库的作用
- 解释库中出现的一些术语
- 一些完整的代码例子
- 指向最重要或者最常用的类和函数的链接
- 指向库中使用的外部引用的链接

> TIPS: 如果使用的库没有`library`指令你可以自己添加一个

<a name="2-4"></a>
#### [CONSIDER] 为私有API编写文档注释
> 文档注释不仅仅用于面向用户的公共API，私有API的文档注释可以帮助用户理解库的功能实现

<a name="2-5"></a>
#### [DO] 使用一句话总结开始你的文档注释
> 文档注释使用一句总结性的描述开头，帮助用户快速了解用户最关注的功能

```dart
    // goods
    /// Deletes the file at [path] from the file system.
    void delete(String path) {
        ...
    }

    // bad
    /// Depending on the state of the file system and the user's permissions,
    /// certain operations may or may not be possible. If there is no file at
    /// [path] or it can't be accessed, this function throws either [IOError]
    /// or [PermissionError], respectively. Otherwise, this deletes the file.
    void delete(String path) {
        ...
    }
```
<a name="2-6"></a>
#### [DO] 将文档注释段落的第一句话分离出来
> 通过增加一行空白行将第一句总结从注释段落中分离出来，如果不止一句解释是有用的,那就将剩余的注释分离出来。这可以帮助你写出简短的句子来总结文档，像`dartdoc`这种工具也会将第一个段落作为一个简短的总结。

```dart
    // good
    /// Deletes the file at [path].
    ///
    /// Throws an [IOError] if the file could not be found. Throws a
    /// [PermissionError] if the file is present but could not be deleted.
    void delete(String path) {
        ...
    }

    // bad
    /// Deletes the file at [path]. Throws an [IOError] if the file could not
    /// be found. Throws a [PermissionError] if the file is present but could
    /// not be deleted.
    void delete(String path) {
        ...
    }
```
**[⬆ back to top](#top)**

<a name="2-7"></a>
#### [AVOID] 避免信息冗余
> 类注释文档的阅读者可以很清楚的看到类名，以及实现的接口。注释文档应该专注于用户不知道的东西而不是将显而易见的东西也罗列出来。

```dart
    // good
    class RadioButtonWidget extends Widget {
        /// Sets the tooltip to [lines], which should have been word wrapped using
        /// the current font.
        void tooltip(List<String> lines) {
            ...
        }
    }

    // bad
    class RadioButtonWidget extends Widget {
        /// Sets the tooltip for this radio button widget to the list of strings in
        /// [lines].
        void tooltip(List<String> lines) {
            ...
        }
    }
```

<a name="2-8"></a>
#### [PREFER] 注释函数或者方法时使用第三人称动词
> 文档注释应该专注于代码能做什么

```dart
    /// Returns `true` if every element satisfies the [predicate].
    bool all(bool predicate(T element)) => ...

    /// Starts the stopwatch if not already running.
    void start() {
        ...
    }
```

<a name="2-9"></a>
#### [PREFER] 注释变量，getter/setter时使用名词短语
> 文档属性应当强调属性是什么，比如对于getter来说调用者关心的是处理结果而不是如何处理

```dart
/// The current day of the week, where `0` is Sunday.
int weekday;

/// The number of checked buttons on the page.
int get checkedCount => ...
```
*避免同时为getter和setter编写文档注释，dartdoc只会解析并展示一个*

<a name="2-10"></a>
#### [PREFER] 注释库或者类型时使用名词短语
> 文档注释对于类来说是非常重要的
```dart
    /// A chunk of non-breaking output text terminated by a hard or soft newline.
    ///
    /// ...
    class Chunk { ... }
```
<a name="2-11"></a>
#### [CONSIDER] 在注释中提供代码例子
```dart
    /// Returns the lesser of two numbers.
    ///
    /// ```dart
    /// min(5, 3) == 3
    /// ```
    num min(num a, num b) => ...
```
*代码例子使API的学习更加高效*

<a name="2-12"></a>
#### [DO] 在文档注释中使用方括号突出作用域內标识符
> 如果你将变量，方法或者类名使用方括号包裹，dartdoc可以查询这些名字并生成指向相关API的链接，尽管这是可选的但这让你的文档变得更加清晰

```dart
    /// Throws a [StateError] if ...
    /// similar to [anotherMethod()], but ...
```
>为了链接类的特定成员变量或者函数，可以用点(dot)号将类和成员连接起来

```dart
    /// Similar to [Duration.inDays], but handles fractional days.
```
> dot语法也可以用于命名构造函数，对于默认未命名构造函数可以直接在类名后加上括号
```dart
    /// To create a point, call [Point()] or use [Point.polar()] to ...
```
<a name="2-13"></a>
#### [DO] 使用简介的描述来注释参数，返回值和抛出的异常
> 其他语言用很冗长的tag和段落来描述参数和返回值

```dart
    // bad
    /// Defines a flag with the given name and abbreviation.
    ///
    /// @param name The name of the flag.
    /// @param abbr The abbreviation for the flag.
    /// @returns The new flag.
    /// @throws ArgumentError If there is already an option with
    ///     the given name or abbreviation.
    Flag addFlag(String name, String abbr) => ...
```
> 在dart中我们通过使用方括号高亮我们的参数返回值等，这让注释变得更加简洁

```dart
    // good
    /// Defines a flag.
    ///
    /// Throws an [ArgumentError] if there is already an option named [name] or
    /// there is already an option using abbreviation [abbr]. Returns the new flag.
    Flag addFlag(String name, String abbr) => ...
```

<a name="2-14"></a>
#### [DO] 将文档注释放在元数据注解`@`之前
```dart
    // good
    /// A button that can be flipped on and off.
    @Component(selector: 'toggle')
    class ToggleComponent {}

    // bad
    @Component(selector: 'toggle')
    /// A button that can be flipped on and off.
    class ToggleComponent {}
```
**[⬆ back to top](#top)**

## Markdown
> 你可以通过使用Markdown来注释你的文档，dartdoc通过markdown包来解析Markdown的内容。
```dart
/// This is a paragraph of regular text.
///
/// This sentence has *two* _emphasized_ words (italics) and **two**
/// __strong__ ones (bold).
///
/// A blank line creates a separate paragraph. It has some `inline code`
/// delimited using backticks.
///
/// * Unordered lists.
/// * Look like ASCII bullet lists.
/// * You can also use `-` or `+`.
///
/// 1. Numbered lists.
/// 2. Are, well, numbered.
/// 1. But the values don't matter.
///
///     * You can nest lists too.
///     * They must be indented at least 4 spaces.
///     * (Well, 5 including the space after `///`.)
///
/// Code blocks are fenced in triple backticks:
///
/// ```
/// this.code
///     .will
///     .retain(its, formatting);
/// ```
///
/// The code language (for syntax highlighting) defaults to Dart. You can
/// specify it by putting the name of the language after the opening backticks:
///
/// ```html
/// <h1>HTML is magical!</h1>
/// ```
///
/// Links can be:
///
/// * http://www.just-a-bare-url.com
/// * [with the URL inline](http://google.com)
/// * [or separated out][ref link]
///
/// [ref link]: http://google.com
///
/// # A Header
///
/// ## A subheader
///
/// ### A subsubheader
///
/// #### If you need this many levels of headers, you're doing it wrong
```

<a name="3-1"></a>
#### [AVOID] 避免过度使用Markdown
> 简言之，文字更重要

<a name="3-2"></a>
#### [AVOID] 避免使用HTML作为注释
> 维护麻烦，不建议使用

<a name="3-3"></a>
#### [PREFER] 使用反引号来区分代码块
> Markdown可以通过两种方式展示代码块，一种是空行一种是反引号，推荐第二种

```dart
    // good
    /// You can use [CodeBlockExample] like this:
    ///
    /// ```
    /// var example = CodeBlockExample();
    /// print(example.isItGreat); // "Yes."
    /// ```

    // bad
    /// You can use [CodeBlockExample] like this:
    ///
    ///     var example = CodeBlockExample();
    ///     print(example.isItGreat); // "Yes."
```
**[⬆ back to top](#top)**

## 写作
> 我们总是把自己当做编程人员，但是源文件中大多数文字是给人阅读的，所以写作能力很重要，这有一篇指导科技写作的文章[Technical writing style](https://en.wikiversity.org/wiki/Technical_writing_style).

<a name="4-1"></a>
#### [PREFER] 尽量简短
>简洁不简单

<a name="4-2"></a>
#### [AVOID] 尽量避免使用缩略词除非它们很常见
> 不是所有人都懂`i.e.`，`e.g.`等等类似的缩写

<a name="4-3"></a>
#### [PREFER] 使用`this`而不是`the`去指代实例本身
> 写类的相关文档时经常会描述类本身，使用`this`代指`class`,使用`the`会造成困惑。
```dart
    class Box {
        /// The value this wraps.
        var _value;

        /// True if this box contains a value.
        bool get hasValue => _value != null;
    }
```

**[⬆ back to top](#top)**