# AngularJS

## （一）MVC  和 MVVM 
+ MVC : 
    - 随着代码规模越来越大，切分职责是大势所趋，还有为了后期维护方便，修改一块功能不影响其他功能。还有为了复用，因为很多逻辑是一样的。而 MVC 只是手段，终极目标是模块化和复用。
+ MVVM :
    - 在 angular 中 MVVM 模式主要分为四部分：
        * View ：它专注于界面的显示和渲染，在 angular 中则是包含一堆声明式 Directive 的视图模板；
        
        * ViewModel ：它是 View 和 Model 的粘合体，负责 View 和 Model 的交互和协作，它负责给 View 提供显示的数据，以及提供了 View 中 Command 事件操作 Model 的途径；在 angular 中 $scope 对象充当了这个 ViewModel 的角色；
        
        * Model ：它是与应用程序的业务逻辑相关的封装载体，它是业务领域的对象，Model 并不关心会被如何显示或操作，所以模型也不会包含任何界面显示相关的逻辑。在 web 页面中，大部分 Model 都是来自 Ajax 的服务端返回数据或者是全局的配置对象的；而 angular 中的 service 则是封装和处理这些与 Model 相关的业务逻辑的场所，这类的业务服务是可以被多个 Controller 或者其他 service 复用的领域服务；
        
        * Controller ：这并不是 MVVM 模式的核心元素，但它负责 ViewModel 对象的初始化，它将组合一个或者多个 service 来获取业务领域 Model 放在 ViewModel 对象上，使得应用界面在启动加载的时候达到一种可用的状态。

        ![mvvm](mvvm.png)

### *mvc 的界面和逻辑关联紧密，数据直接从数据库读取，mvvm 的界面与 viewmodel 是松耦合，界面数据从 viewmodel 中获取。所以 angularjs 更倾向于 mvvm。*

## （二）MVVM 的优点
+ 低耦合 ：View 可以独立于 Model 变化和修改，同一个 ViewModel 可以被多个 View 复用；并且可以做到 View 和 Model 的变化互不影响；

+ 可重用性 ：可以把一些视图的逻辑放在 ViewModel ，让多个 View 复用；

+ 独立开发 ：开发人员可以专注与业务逻辑和数据的开发；

+ 可测试性 ：清晰的 View 分层，使得针对表现层业务逻辑的测试更容易，更简单；

## （三）MVC 和 MVVM 的区别
+ 在 MVC 中，View 是可以直接访问 Model 的，从而 View 里面也会包含 Model 信息，不可避免的还要包括一些业务逻辑；MVC 模型关注的是 Model 的不变，所以，在 MVC 模型里，Model不依赖与 View，而 View 是依赖于 Model 的；不仅如此，因为有一些业务逻辑在 View 里实现了，导致要更改 View 也是比较困难的，至少那些业务逻辑是无法重用的；

+ MVVM 在概念上是真正的将页面与数据逻辑分离的模式，它把数据绑定工作放在一个 JS 文件里去实现，而这个 JS 文件的主要功能是完成数据绑定，即把 Model 绑定到 UI 的元素上；

+ 有人做过测试，使用 Angular (MVVM) 代替 Backbone (MVC) 开发，代码可以减少一半；

+ 此外，MVVM 另一个重要特性，双向绑定，它更方便同时维护页面上都依赖于某个字符的N个区域，而不用手动去更新它们；

## （四）AngularJS 的数据双向绑定的实现
1. 每个双向绑定的元素都有一个 watcher
2. 在某些事件发生的时候，调用 digest 脏数据检测
    + 这些事件有：表单元素内容变化，Ajax 请求响应，点击按钮执行的函数等
3. 脏数据检测会检测 rootscope 下所有被 watcher 的元素
    + $digest 函数就是脏数据检测

## （五）controller 之间怎么通讯
1. event
+ 这里可以有两种方式，一种是 $scope.$emit ，然后通过监听 $rootScope 的事件获取参数；
+ 另一种是 $rootScope.$broadcast ，通过监听 $scope 的事件获取参数；

2. service
+ 可以创建一个专用的事件 Service ，也可以按照业务逻辑切分，将数据存储在相应的 Service 中；

3. $rootScope
+ 这个方法可能会比较dirty一点，胜在方便，也就是把数据存在 $rootScope 中，这样各个子 $scope 都可以调用，不过需要注意一下生命周期；

4. 直接使用 $scope.$$nextSibling 及类似的属性
+ 类似的还有 $scope.$parent 这个方法的缺点就更多了，官方不推荐使用任何 $$ 开头的属性，既增加了耦合，又需要处理异步的问题，而且 scope 的顺序也不是固定的，不推荐。

5. 另外就是通过本地存储、全局变量或者现代浏览器的 postMessage 来传递参数了，除非特殊情况，请避免这类方式。

## （六）自定义指令的几个参数
1. restrict ：指令在 dom 中的声明形式 E （元素）A （属性）C （类名）M （注释）；
2. template ：两种形式，一种 HTML 文本；一个可以接受两个参数的函数，tElement 和 tAttrs ，并返回一个代表模板的字符串。模板字符串必须存在一个根 DOM 元素；
3. templateUrl ：两种形式，一种代表外部 HTML 文件路径的字符串；一个可以接受两个参数的函数，参数为 tElement 和 tAttrs ，并返回一个外部 HTML 文件路径的字符串；
4. compile （对象或函数）：compile 选项可以返回一个对象或函数。如果设置了 compile 函数，说明我们希望在指令和实时数据被放到 DOM 中之前进行 DOM 操作，在这个函数中进行诸如添加和删除节点等 DOM 操作是安全的。本质上，当我们设置了 link 选项，实际上是创建了一个 postLink() 链接函数，以便 compile() 函数可以定义链接函数。

### *compile 和 link 的区别：*
+ 编译的时候，compile 转换 dom ，碰到绑定监听器的地方就先存着，有几个存几个，到最后汇总成一个 link 函数，一并执行，提升了性能。

## （七）angular 中的 $http
+ $http 是 AngularJS 中的一个核心服务，用于读取远程服务器的数据。
+ 我们可以使用内置的 $http 服务直接同外部进行通信。$http 服务只是简单的封装了浏览器原生的 XMLHttpRequest 对象。

1. 链式调用
    + $http 服务是只能接受一个参数的函数，这个参数是一个对象，包含了用来生成 HTTP 请求的配置内容。这个函数返回一个 promise 对象，具有 success 和 error 两个方法。

2. 返回一个 promise 对象

3. 快捷的 get 请求

4. 响应对象

## （八）angualr 和 jquery 的区别
+ angular 是基于数据驱动，所以 angular 适合做数据操作比较繁琐的项目；
+ jquery 是基于 dom 驱动，jquery 适合做 dom 操作多的项目；

## （九）angular 中的 form 表单
+ Angualr 对 input 元素的 type 进行了扩展，一共提供了以下10种类型：
    - text
    - number
    - url
    - email
    - radio
    - checkbox
    - hidden
    - button 
    - submit
    - reset
+ Angualr 为表单内置了4中 CSS 样式
    - ng-valid ：校验合法状态；
    - ng-invalid ：校验非法状态；
    - ng-pristine ：如果要使用原生的 form ，需要设置这个值；
    - ng-dirty ：表单处于脏数据状态；
    - ng-maxlength ：最大长度；
    - ng-minlength ：最小长度；
    - required ：表示是否输入内容；
+ Angular 在对表单进行自动校验的时候回校验 Model 上的属性，如果不设置 ng-model ，则 Angualr 无法知道 myForm.$invalid 这个值是否为真。

## （十）ng-show/ng-hide 与 ng-if 的区别
+ ng-show/ng-hide 实际上是通过 display 来进行隐藏和显示的。
+ 而 ng-if 实际上控制 dom 节点的增删除来实现的，因此如果是根据不同的条件来进行 dom 节点的加载确认的话，那么 ng-if 的性能好过 ng-show ；

## （十一）什么是 $rootScope
+ $rootScope 是所有 $scope 的父对象；

## （十二）表达式 { { yourModel } } 是如何工作的
+ 它依赖于 $interpolation 服务，在初始化页面 html 后，它会找到这些表达式，并且进行标记，于是每遇见一个 { { } } ，则会设置一个 $watch 。
+ 而 $interpolation 会返回一个带有上下文参数的函数，最后该函数执行，则算是表达式 $parse 到那个作用域上。

## （十三）filter 的了解
+ ng 内置的 filter 有九种：
    - date （日期）；
    - currency （货币）；
    - limitTo （限制数组或字符串长度）；
    - orderBy （排序）；
    - lowercase （小写）；
    - uppercase （大写）；
    - number （格式化数字，加上千位分隔符，并接收参数限定小数点位数）；
    - filter （处理一个数组，过滤出含有某个子串的元素）；
    - json （格式化 json 对象）；

+ filter 有两种使用方法：
    - 一种是直接在页面里：
    ```html
    <p>{{now | date : 'yyyy-MM-dd'}}</p>
    ```
    - 另一种是在 js 里面：
    ```javascript
    $filter('date')(now,'yyyy-MM-dd');
    //$filter('过滤器名称')(需要过滤的对象，参数1，参数2，……);
    ```

**自定义一个去重数组的**
```javascript
app.filter('unique',function(){
    return function(arr){
        var n = [];
        var obj = {};

        for(var i = 0;i<arr.length;i++){
            if(!obj[arr[i]]){
                n.push(arr[i])
                obj[arr[i]] = 1;
            }
        }
        return n;
    }
})
```




