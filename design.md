<a name="top"></a>
## 🔥 Effective Dart: Design 🔥 
> WIP (work in progress) 

- 命名
    - [DO] [使用统一的命名策略](#1-1)
    - [AVOID] [避免使用缩写](#1-2)
    - [PREFER] [将最有意义的描述名词放在最后](#1-3)
    - [CONSIDER] [增加代码可读性，使之语义化](#1-4)
    - [PREFER] [非布尔值的属性或者变量命名使用名词短语](#1-5)
    - [PREFER] [布尔值属性或者变量命名使用非命令性动词](#1-6)
    - [CONSIDER] [布尔值命名参数请省略动词](#1-7)
    - [PREFER] [布尔值属性或者变量命名时使用正面词汇命名](#1-8)
    - [PREFER] [当函数或者方法会产生副作用时用动词短语命名](#1-9)
    - [PREFER] [当函数或者方法有返回值时用名词短语命名](#1-10)
    - [CONSIDER] [如果你想强调函数工作内容请用动词短语命名](#1-11)
    - [AVOID] [避免使用get作为函数方法名字的开头](#1-12)
    - [PREFER] [如果是将一个对象状态复制到另一个对象的方法请使用`to_()`格式命名](#1-13)
    - [PREFER] [改变对象类型使用`as_()`格式命名](#1-14)
    - [AVOID] [不要在函数方法名中出现参数名](#1-15)
    - [DO] [当给类型参数命名时请遵循如下的助记符约定](#1-16)
- 库
    - [PREFER] [声明为私有](#2-1)
    - [CONSIDER] [可以在同一个库里声明多个类](#2-2)
- 类
    - [AVOID] [避免定义单成员抽象类使用函数替代](#3-1)
    - [AVOID] [避免定义只含一个静态成员的类](#3-2)
    - [AVOID] [避免继承不想拥有子类的类](#3-3)
    - [DO] [如果你的类支持继承请在文档里说明](#3-4)
    - [AVOID] [避免实现不支持作为接口的类](#3-5)
    - [DO] [如果你的类支持作为借口请在文档里说明](#3-6)
    - [AVOID] [避免混合`mixin`不支持作为`mixin`的类](#3-7)
    - [DO] [如果你的类支持作为`mixin`请在文档里说明](#3-8)
- 构造函数
    - [PREFER] [建议定义构造函数而不是静态方法去生成实例](#4-1)
    - [CONSIDER] [如果类支持请将构造函数声明为`const`](#4-2)
- 类成员
    - [PREFER] [将顶级作用域的变量声明为`final`](#4-1)
    - [DO] [为可获得的属性添加`getters`](#4-2)
    - [DO] [为可设置的属性添加`setters`](#4-3)
    - [DON'T] [不要定义`setters`时不定义对应的`getters`](#4-4)  
    - [AVOID] [避免返回值返回`null`](#4-5)
    - [AVOID] [避免直接返回`this`,链式调用请用cascade也就是`..`运算符](#4-6)
- 类型
    - [PREFER] [声明顶级作用域变量类型](#5-1)
    - [CONSIDER]  [声明私有作用域变量类型](#5-2)
    - [AVOID] [避免声明初始化本地变量类型](#5-3)
    - [AVOID] [避免在闭包中声明推断参数的类型](#5-4)
    - [AVOID] [避免在使用泛型时重复冗余的声明类型](#5-5)
    - [DO] [当推断类型时确定类型时请声明类型](#5-6)
    - [PREFER] [使用`dynamic`声明类型而不是让推断直接fail掉](#5-7) 
    - [PREFER] [当函数作为参数时请声明函数类型](#5-8)
    - [DON'T] [不要为`setter`声明类型](#5-9)
    - [DON'T] [不要使用遗留版本的`typedef`语法](#5-10)
    - [PREFER] [使用函数类型声明而不是`typedef`](#5-11) 
    - [CONSIDER] [函数作为参数时使用函数类型语法](#5-12) 
    - [DO] [当参数可以是任意类型时用`Object`声明而不是`dynamic`](#5-13)
    - [DO] [当异步函数不返回值时使用`Future<void>`声明](#5-14)
    - [AVOID] [避免使用`FutureOr<T>`作为返回类型](#5-15)
- 参数
    - [AVOID] [避免布尔值类型位置参数](#6-1)
    - [AVOID] [如果你想省略一些参数请请避免使用位置参数](#6-2)
    - [AVOID] [避免强制性参数当参数可以省略时](#6-3)
    - [DO] [获取范围时将参数设置为左闭右开](#6-4)
- 相等符
    - [DO] [如果你重载`==`请一并重载`hashcode`](#7-1)
    - [DO] [请让你的`==`运算符遵守数学上的相等](#7-2)
    - [AVOID] [避免为可变类定义常规意义上的相等](#7-3)
    - [DON'T] [不需要在重载`==`运算符时判断类型是否为`null`](#7-4)
```dart
    // good
    pageCount         // A field.
    updatePageCount() // Consistent with pageCount.
    toSomething()     // Consistent with Iterable's toList().
    asSomething()     // Consistent with List's asMap().
    Point             // A familiar concept.

    // bad
    renumberPages()      // Confusingly different from pageCount.
    convertToSomething() // Inconsistent with toX() precedent.
    wrappedAsSomething() // Inconsistent with asX() precedent.
    Cartesian            // Unfamiliar to most users.
```
```dart
    // good
    pageCount
    buildRectangles
    IOStream
    HttpRequest

    // bad
    numPages    // "num" is an abbreviation of number(of)
    buildRects
    InputOutputStream
    HypertextTransferProtocolRequest
```
```dart
    // good
    pageCount             // A count (of pages).
    ConversionSink        // A sink for doing conversions.
    ChunkedConversionSink // A ConversionSink that's chunked.
    CssFontFaceRule       // A rule for font faces in CSS.

    // bad
    numPages                  // Not a collection of pages.
    CanvasRenderingContext2D  // Not a "2D".
    RuleFontFaceCss           // Not a CSS.
```
```dart
    // good
    // "If errors is empty..."
    if (errors.isEmpty) ...

    // "Hey, subscription, cancel!"
    subscription.cancel();

    // "Get the monsters where the monster has claws."
    monsters.where((monster) => monster.hasClaws);

    // bad
    // Telling errors to empty itself, or asking if it is?
    if (errors.empty) ...

    // Toggle what? To what?
    subscription.toggle();

    // Filter the monsters with claws *out* or include *only* those?
    monsters.filter((monster) => monster.hasClaws);
```
```dart
    // bad
    if (theCollectionOfErrors.isEmpty) ...

    monsters.producesANewSequenceWhereEach((monster) => monster.hasClaws);
```
```dart
    // good
    list.length
    context.lineWidth
    quest.rampagingSwampBeast
    
    // bad
    list.deleteItems

```
```dart
    if (window.closeable) ...  // Adjective.
    if (window.canClose) ...   // Verb.
```
```dart
    // good
    isEmpty
    hasElements
    canClose
    closesWindow
    canShowPopup
    hasShownPopup

    // bad
    empty         // Adjective or verb?
    withElements  // Sounds like it might hold elements.
    closeable     // Sounds like an interface.
                // "canClose" reads better as a sentence.
    closingWindow // Returns a bool or a window?
    showPopup     // Sounds like it shows the popup.
```
```dart
    Isolate.spawn(entryPoint, message, paused: false);
    var copy = List.from(elements, growable: true);
    var regExp = RegExp(pattern, caseSensitive: false);
```

```dart
    // good
    if (socket.isConnected && database.hasData) {
        socket.write(database.read());
    }

    // bad
    if (!socket.isDisconnected && !database.isEmpty) {
        socket.write(database.read());
    }
```

```dart
    list.add("element");
    queue.removeFirst();
    window.refresh();
```

```dart
    var element = list.elementAt(3);
    var first = list.firstWhere(test);
    var char = string.codeUnitAt(4);
```

```dart
    // good
    var table = database.downloadData();
    var packageVersions = packageGraph.solveConstraints();
```

```dart
    // good
    list.toSet();
    stackTrace.toString();
    dateTime.toLocal();
```

```dart
    // good
    var map = table.asMap();
    var list = bytes.asFloat32List();
    var future = subscription.asFuture();
```

```dart
    // good
    list.add(element);
    map.remove(key);

    // bad
    list.addElement(element)
    map.removeKey(key)
```

```dart
    // good
    map.containsKey(key);
    map.containsValue(value);
```

```dart
    // good
    class IterableBase<E> {}
    class List<E> {}
    class HashSet<E> {}
    class RedBlackTree<E> {}
```

```dart
    // good
    class Map<K, V> {}
    class Multimap<K, V> {}
    class MapEntry<K, V> {}
```

```dart
    // good
    abstract class ExpressionVisitor<R> {
        R visitBinary(BinaryExpression node);
        R visitLiteral(LiteralExpression node);
        R visitUnary(UnaryExpression node);
    }
```

```dart
    // good
    class Future<T> {
        Future<S> then<S>(FutureOr<S> onValue(T value)) => ...
    }
```

```dart
    // good
    class Graph<N, E> {
    final List<N> nodes = [];
    final List<E> edges = [];
    }

    class Graph<Node, Edge> {
    final List<Node> nodes = [];
    final List<Edge> edges = [];
    }    
```

```dart
    // good
    typedef Predicate<E> = bool Function(E element);
    
    // bad
    abstract class Predicate<E> {
        bool test(E element);
    }
```

```dart
    // good
    DateTime mostRecent(List<DateTime> dates) {
        return dates.reduce((a, b) => a.isAfter(b) ? a : b);
    }

    const _favoriteMammal = 'weasel';

    // bad
    class DateUtils {
        static DateTime mostRecent(List<DateTime> dates) {
            return dates.reduce((a, b) => a.isAfter(b) ? a : b);
        }
    }

    class _Favorites {
    static const mammal = 'weasel';
    }
```

```dart
    // good
    class Color {
        static const red = '#f00';
        static const green = '#0f0';
        static const blue = '#00f';
        static const black = '#000';
        static const white = '#fff';
    }
```

```dart
    // good
    class Point {
        num x, y;
        Point(this.x, this.y);
        Point.polar(num theta, num radius)
            : x = radius * cos(theta),
                y = radius * sin(theta);
    }

    // bad
    class Point {
        num x, y;
        Point(this.x, this.y);
        static Point polar(num theta, num radius) =>
            Point(radius * cos(theta), radius * sin(theta));
    }
```

```dart
    // bad
    connection.nextIncomingMessage; // Does network I/O.
    expression.normalForm; // Could be exponential to calculate.    
```

```dart
    // bad
    stdout.newline; // Produces output.
    list.clear; // Modifies object.
```

```dart
    // bad
    DateTime.now; // New result each time.
```

```dart
    // good
    rectangle.area;
    collection.isEmpty;
    button.canShow;
    dataSet.minimumValue;
```

```dart
    // good
    rectangle.width = 3;
    button.visible = false;
```

```dart
    // good
    var buffer = StringBuffer()
        ..write('one')
        ..write('two')
        ..write('three');

    // bad
    var buffer = StringBuffer()
        .write('one')
        .write('two')
        .write('three');
```

```dart
    bool isEmpty(String parameter) {
        bool result = parameter.length == 0;
        return result;
    }
```

```dart
    var lists = <num>[1, 2];
    lists.addAll(List<num>.filled(3, 4));
    lists.cast<int>();
```

```dart
    List<int> ints = [1, 2];
```

```dart
    install(id, destination) => ...
```
```dart
   Future<bool> install(PackageId id, String destination) => ...
```
```dart
    const screenWidth = 640; // Inferred as int.
```

```dart
    // good
    List<List<Ingredient>> possibleDesserts(Set<Ingredient> pantry) {
        var desserts = <List<Ingredient>>[];
        for (var recipe in cookbook) {
            if (pantry.containsAll(recipe)) {
            desserts.add(recipe);
            }
        }
        return desserts;
    }

    // bad
    List<List<Ingredient>> possibleDesserts(Set<Ingredient> pantry) {
        List<List<Ingredient>> desserts = <List<Ingredient>>[];
        for (List<Ingredient> recipe in cookbook) {
            if (pantry.containsAll(recipe)) {
            desserts.add(recipe);
            }
        }
        return desserts;
    }
```
```dart
    // good
    List<AstNode> parameters;
    if (node is Constructor) {
        parameters = node.signature;
    } else if (node is Method) {
        parameters = node.parameters;
    }
```
```dart
    // good
    var names = people.map((person) => person.name);

    // bad
    var names = people.map((Person person) => person.name);
```
```dart
    // good
    Set<String> things = Set();

    // bad
    Set<String> things = Set<String>();
```
```dart
    // good
    var things = Set<String>();

    // bad
    var things = Set();
```
```dart
    // good
    num highScore(List<num> scores) {
        num highest = 0;
        for (var score in scores) {
            if (score > highest) highest = score;
        }
        return highest;
    }

    // bad
    num highScore(List<num> scores) {
        var highest = 0;
        for (var score in scores) {
            if (score > highest) highest = score;
        }
        return highest;
    }
```
```dart
    // good
    dynamic mergeJson(dynamic original, dynamic changes) => ...
    
    // bad
    mergeJson(original, changes) => ...
```

```dart
    // good
    bool isValid(String value, bool Function(String) test) => ...

    // bad
    bool isValid(String value, Function test) => ...
```

```dart
    // good
    void handleError(void Function() operation, Function errorHandler) {
        try {
            operation();
        } catch (err, stack) {
            if (errorHandler is Function(Object)) {
            errorHandler(err);
            } else if (errorHandler is Function(Object, StackTrace)) {
            errorHandler(err, stack);
            } else {
            throw ArgumentError("errorHandler has wrong signature.");
            }
        }
    }
```

```dart
    // good
    set foo(Foo value) { ... }

    // bad
    void set foo(Foo value) { ... }
```

```dart
    // bad
    typedef int Comparison<T>(T a, T b);
```

```dart
    // bad
    typedef bool TestNumber(num);
```

```dart
    // good
    typedef Comparison<T> = int Function(T, T);
```

```dart
    // good
    typedef Comparison<T> = int Function(T a, T b);
```

```dart
    // good
    class FilteredObservable {
        final bool Function(Event) _predicate;
        final List<void Function(Event)> _observers;

        FilteredObservable(this._predicate, this._observers);

        void Function(Event) notify(Event event) {
            if (!_predicate(event)) return null;

            void Function(Event) last;
            for (var observer in _observers) {
            observer(event);
            last = observer;
            }

            return last;
        }
    }
```

```dart
    Iterable<T> where(bool predicate(T element)) => ...
```

```dart
    // good
    Iterable<T> where(bool Function(T) predicate) => ...
```

```dart
    // good
    void log(Object object) {
        print(object.toString());
    }

    /// Returns a Boolean representation for [arg], which must
    /// be a String or bool.
    bool convertToBool(dynamic arg) {
        if (arg is bool) return arg;
        if (arg is String) return arg == 'true';
        throw ArgumentError('Cannot convert $arg to a bool.');
    }
```

```dart
    // good
    Future<int> triple(FutureOr<int> value) async => (await value) * 3;
    
    // bad
    FutureOr<int> triple(FutureOr<int> value) {
        if (value is int) return value * 3;
        return (value as Future<int>).then((v) => v * 3);
    }
```

```dart
    // good
    Stream<S> asyncMap<T, S>(
        Iterable<T> iterable, FutureOr<S> Function(T) callback) async* {
    for (var element in iterable) {
            yield await callback(element);
        }
    }
```

```dart
    // bad
    new Task(true);
    new Task(false);
    new ListBox(false, true, true);
    new Button(false);
```

```dart
    // good
    Task.oneShot();
    Task.repeating();
    ListBox(scroll: true, showScrollbars: true);
    Button(ButtonState.enabled);
```

```dart
    // good
    listBox.canScroll = true;
    button.isEnabled = false;
```

```dart
    // good
    String.fromCharCodes(Iterable<int> charCodes, [int start = 0, int end]);

    DateTime(int year,
        [int month = 1,
        int day = 1,
        int hour = 0,
        int minute = 0,
        int second = 0,
        int millisecond = 0,
        int microsecond = 0]);

    Duration(
        {int days = 0,
        int hours = 0,
        int minutes = 0,
        int seconds = 0,
        int milliseconds = 0,
        int microseconds = 0});
```

```dart
    // good
    var rest = string.substring(start);

    // bad
    var rest = string.substring(start, null);
```

```dart
    // good
    [0, 1, 2, 3].sublist(1, 3) // [1, 2]
    'abcd'.substring(1, 3) // 'bc'
```

```dart
    // good
    class Person {
        final String name;
        // ···
        operator ==(other) => other is Person && name == other.name;

        int get hashCode => name.hashCode;
    }

    // bad
    class Person {
        final String name;
        // ···
        operator ==(other) => other != null && ...
    }
```
**[⬆ back to top](#top)**