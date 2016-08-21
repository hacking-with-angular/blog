title: Step 5 - Dependency Injection
date: 2016-8-11 12:00:00
tags: ["Angular2", "Travel"] 
author: dreamapplehappy
---
在读这篇文章之前,你要先了解一下什么是[依赖注入][11],网上关于这个的解释很多,大家可以自行[**Google**][12].

![article-cover][1]

我们这一篇文章还是以[QuickStart][2]项目为基础,从头开始讲解怎么在Angular2中使用**依赖注入**,如果你按照本篇文章中讲解的示例亲自走一遍的话,你一定能够掌握如何在Angular2中使用**依赖注入**.好,废话不多说,开始我们今天的旅行吧!

我们首先将项目中的内联模板替换为一个模板文件,使用`templateUrl`替换`template`:

```html
@Component({
    selector: 'my-app',
    //template: '<h1>My First Angular2 Travel</h1>',
    templateUrl: 'app/templates/app.html'
})
```
接下来我们给自己的页面添加一些展示的数据,我们首先新建一个文件`app/classes/User.ts`,用来创建我们的`User`实例:

```typescript
export class User{
    constructor(
        private name:string,
        private age:number,
        private email:string
    ){}
}
```
然后我们在组件中引入这个类,然后创建我们的显示数据:

```typescript
import {User} from "./classes/User";
// ...
export class AppComponent {
    users: User[] = [
        new User('dreamapple', 22, '2312674832@qq.com'),
        new User('dreamapplehappy', 18, '2313334832@qq.com')
    ]
}
```
别忘了在模板中添加一些展示数据使用的html代码:

```html
<h1>依赖注入</h1>
<ul>
    <li *ngFor="let user of users">
        用户的姓名: {{user.name}}; 用户的年龄: {{user.age}}; 用户的邮箱: {{user.email}}
    </li>
</ul>
```
然后我们就会看到,在页面中显示出来了我们想要的那些数据:
![ng2-di-1][3]

**Angular2的依赖注入**

一般情况下在Web应用中,我们要展示的数据都是从后台服务器动态获取的,所以我们来模拟一下这个过程;我们在这里就要使用**服务的依赖注入**了,我们首先创建文件`user-data.mock.ts`,路径是`app/mock/user-data.mock.ts`;

```typescript
import {User} from "../classes/User";

export var Users:User[] = [
    new User('dreamapple1', 21, '2451731631@qq.com'),
    new User('dreamapple2', 22, '2451731632@qq.com'),
    new User('dreamapple3', 23, '2451731633@qq.com'),
    new User('dreamapple4', 24, '2451731634@qq.com'),
    new User('dreamapple5', 25, '2451731635@qq.com'),
    new User('dreamapple6', 26, '2451731636@qq.com')
]
```
我们使用了`User`类来创建我们的数据,然后把创建的数据导出.

接下来我们要创建一个获取用户数据的**服务**,我们创建一个新的文件`user.service.ts`,路径`app/services/user.service.ts`:

```typescript
import {Injectable} from '@angular/core';
import {Users} from "../mock/user-data.mock";


@Injectable()
export class UserService {
    getUsers() {
        return Users;
    }
}
```

大家关于上面的代码部分会有一些疑问,我们来给大家解释一下:首先我们使用了刚才我们创造的模拟数据`Users`;然后我们从`@angular/core`中导出了`Injectable`,就像我们从中导出`Component`一样;**`@Injectable()`标志着一个类可以被一个注入器实例化;通常来讲,在试图`实例化`一个没有被标识为`@Injectable()`的类时候,注入器将会报告错误.**上面的解释现在不明白不要紧,我们先学会如何使用;**就像你不懂计算机原理一样可以把计算机玩得很溜一样.**

我们接下来要在`AppComponent`组件中使用`UserService`了,需要注意的地方是:我们要在`@Component`的元数据中使用`providers`声明我们所需要的依赖,还要引入`User`类来帮助我们声明数据的类型.

```typescript
import {UserService} from "./services/user.service";
import {User} from "./classes/User";
//...
@Component({
    selector: 'my-app',
    //template: '<h1>My First Angular2 Travel</h1>',
    templateUrl: 'app/templates/app.html',
    providers: [
        UserService
    ]
})
export class AppComponent {
    users: User[];

    constructor(private userService: UserService) {
        this.users = userService.getUsers();
    }
}
```

对上面代码的一些解释:**我们使用`providers: [UserService]`来声明我们这个组件的依赖,如果没有这个选项,我们的程序会报错;然后我们给这个类添加一个属性`users`,同时声明这个属性的类型是一个含有`User`类实例的数组;最后我们在构造函数中又声明了一个私有的属性`userService`,它是`UserService`服务类的一个实例,我们可以用这个实例来获取`users`数据.**

运行一下,然后我们就会看到下面的页面,表示一切成功.
![ng2-di-2][4]

如果这个时候你试图把`user.service.ts`的`@Injectable`注释掉的话,整个程序是没有报错的,但是我们建议为每个服务类都添加`@Injectable()`,包括那些没有依赖所以技术上不需要它的.因为:**(1)面向未来,没有必要记得在后来添加了一个依赖的时候添加`@Injectable()`.(2)一致性,所有的服务都遵循同样的规则,并且我们不需要考虑为什么少一个装饰器.**

这是因为,我们的`UserService`服务现在还没有什么依赖,如果我们给`UserService`添加一个依赖的话,如果这时候把`@Injectable()`注释掉的话,程序就会报错;我们来试试看吧.

很多Web程序都会需要一个日志服务,所以我们来新建一个服务`Logger`,路径如下:`app/services/logger.service.ts`:

```typescript
import {Injectable} from '@angular/core';

@Injectable()
export class Logger{
    logs: string[] = [];

    log(msg) {
        this.logs.push(msg);
        console.warn('From logger class: ' + msg);
    }
}
```
然后我们在`UserService`服务中使用这个服务:

```typescript
import {Injectable} from '@angular/core';
import {Users} from "../mock/user-data.mock";
import {Logger} from "./logger.service";


@Injectable()
export class UserService {
    constructor(private logger: Logger) {

    }

    getUsers() {
        this.logger.log('get users');
        return Users;
    }
}
```

可以看到,我们把`Logger`当做`UserService`服务的一个依赖,因为我们在`UserService`类的构造函数中声明了一个`logger`属性,它是`Logger`类的一个实例;还有别忘了在`AppComponent`中添加这个`Logger`依赖:

```typescript
@Component({
    selector: 'my-app',
    //template: '<h1>My First Angular2 Travel</h1>',
    templateUrl: 'app/templates/app.html',
    providers: [
        Logger, // 添加Logger依赖
        UserService
    ]
})
```
然后我们可以在页面中看到:
![ng2-di-3][5]

如果这个时候,我们注释掉`UserService`的`@Injectable()`的话,程序就会报错:
![ng2-di-4][6]

所以,就像上面所说的;我们还是给每一个服务类添加`@Injectable()`,以免出现不必要的麻烦.

接下来我们来讨论一下在`Angular2`中服务的**提供商们**,如果你对所谓的**提供商**不理解的话,没关系;可以这样理解,每当我们使用一个服务的时候,**Angular2**都会通过**提供商**来创建或者获取我们想要的服务的实例.

我们上面所说的那种提供服务的方式其实是最简单的一种方式,接下来我们讨论注册不同的**服务提供商**的方法;首先第一种就是我们上面所说的那种了,其实它是一种简写的方式;详细的方式应该是这样的:

```typescript
[{ provide: Logger, useClass: Logger }]
```
其中`provide`作为键值`key`使用,用于定位依赖,用于注册这个提供商;这个其实就是在后面的程序中使用的服务的名字;`useClass`表示我们使用哪一个服务类去创建实例,当然我们可以使用不同的服务类,只要这些服务的类满足我们相应的需求就行.

我们可以试着替换`Logger`类为`BetterLogger`类,我们首先创建`BetterLogger`类:

```typescript
import {Injectable} from '@angular/core';

@Injectable()
export class BetterLogger{
    logs: string[] = [];

    log(msg) {
        this.logs.push(msg);
        console.warn('From better logger class: ' + msg);
    }
}
```
然后在`AppComponent`中使用这个`BetterLogger`类:

```typescript
@Component({
    selector: 'my-app',
    //template: '<h1>My First Angular2 Travel</h1>',
    templateUrl: 'app/templates/app.html',
    providers: [
        //Logger,
        [{provide: Logger, useClass: BetterLogger}],
        UserService
    ]
})
```
我们可以看到,控制台的输出是:
![ng2-di-5][7]

从中可以看到,我们使用了`BetterLogger`类替换了`Logger`类.如果我们的提供商需要一些依赖,我们应该怎么办呢?不用怕,我们可以使用下面这种形式:

```typescript
[ LoggerHelper,{ provide: Logger, useClass: Logger }]
```
接下来我们来创建一个`LoggerHelper`类,它的路径是`app/services/logger-helper.service.ts`:

```typescript
import {Injectable} from '@angular/core';

@Injectable()
export class LoggerHelper {
    constructor() {
        console.warn('Just a logger helper!');
    }
}
```
我们在`AppComponent`中注册提供商:

```typescript
@Component({
    selector: 'my-app',
    //template: '<h1>My First Angular2 Travel</h1>',
    templateUrl: 'app/templates/app.html',
    providers: [
        //Logger,
        //[{provide: Logger, useClass: BetterLogger}],
        [LoggerHelper, {provide: Logger, useClass: BetterLogger}], // 带有依赖的注册商
        UserService
    ]
})
```
然后我们在`BetterLogger`服务中使用这个依赖:

```typescript
import {Injectable} from '@angular/core';
import {LoggerHelper} from "./logger-helper.service";

@Injectable()
export class BetterLogger{
    logs: string[] = [];

    constructor(private loggerHelper: LoggerHelper) {
    }

    log(msg) {
        this.logs.push(msg);
        console.warn('From better logger class: ' + msg);
    }
}
```

然后可以看到我们的控制台的输出结果是:
![ng-di-6][8]

说明我们正确的使用了依赖;还有我们可以使用别名来使用相同的提供商,这种方式可以解决一些问题;尤其是当我们想让某个老的组件使用一个新的服务,就好比我们想让`AppComponent`使用`BetterLogger`类来打印日志,而不是使用`Logger`类,假如我们不能够改变`AppComponent`类,并且我们还想让其他的组件也是用新的`BetterLogger`类的话,那么我们就可以像下面这样注册这个提供商

```typescript
[{ provide: BetterLogger, useClass: BetterLogger}],
[{ provide: Logger, useExisting: BetterLogger}]
```
看到了吗,我们使用`useExisting`而不是`useClass`;因为使用`useClass`或导致我们的应用中出现两个`BetterLogger`类的实例.我们可以试验一下,在`AppComponent`中:

```typescript
@Component({
    selector: 'my-app',
    //template: '<h1>My First Angular2 Travel</h1>',
    templateUrl: 'app/templates/app.html',
    providers: [
        //Logger,
        //[{provide: Logger, useClass: BetterLogger}],
        [LoggerHelper, {provide: BetterLogger, useClass: BetterLogger}],
        [LoggerHelper, {provide: Logger, useExisting: BetterLogger}],
        UserService
    ]
})
```
然后我们在`BetterLogger`类的构造函数中添加一个打印语句:

```typescript
console.warn('BetterLogger Constructor');
```
我们还要在`UserService`类的构造函数中声明一个属性`betterLogger`,它是`BetterLogger`类的一个实例:

```typescript
constructor(private logger: Logger, private betterLogger: BetterLogger) {

    }
```
最后我们可以看到控制台的打印结果是:

```
Just a logger helper!
BetterLogger Constructor
From better logger class: get users 
```
但是一旦我们使用了`useClass`而不是`useExisting`,那么控制台的打印结果就变成了:

```typescript
Just a logger helper!
BetterLogger Constructor
BetterLogger Constructor
From better logger class: get users
```
说明我们创建了两个`BetterLogger`的实例.所以当我们的多个服务想使用同一个提供商的时候,我们应该使用`useExisting`,而不是`useClass`.

[**2016-8-20:续写**]

**值提供商**:我们可以使用更简便的方法来注册一个提供商,那就是使用`值`,所谓的值可以是任何一种有效的`TypeScript`的基本的数据类型.我们来首先使用一个`对象`吧.首先我们新创建一个文件`logger.value.ts`,路径是`app/values/logger.value.ts`;我们写一个基本的`loggerValue`对象如下:
```typescript
let loggerValue = {
    logs: ['Hello', 'World'],
    log: (msg) => {
        console.warn('From values: ' + msg);
    },
    hello: () => {
        console.log('Just say hello!');
    }
};

export {loggerValue};
```

那我们如何注册这个提供商呢?我们使用`useValue`选项来注册我们这种提供商;如下所示:
```typescript
// ...
providers: [
        //Logger,
        //[{provide: Logger, useClass: BetterLogger}],
        [LoggerHelper, {provide: BetterLogger, useClass: BetterLogger}],
        //[LoggerHelper, {provide: Logger, useClass: BetterLogger}],
        {provide: Logger, useValue: loggerValue},
        //{provide: Logger, useValue: loggerValue1}, // 我们使用了useValue选项
        UserService
    ]
// ...    
```
还要记住把`loggerValue`导入进来;然后我们稍微修改一下`user.service.ts`的代码:
```typescript
// ...
getUsers() {
        this.logger.log('get users');
        //noinspection TypeScriptUnresolvedFunction
        this.logger.hello();
        return Users;
    }
// ...
```
然后我们会看到控制台的输出是:
```
// ...
From values: get users
Just say hello!
// ...
```
表明我们这种方式注册提供商成功了. 当然我们也可以使用一个字符串了,这些读者可以自行尝试;或者观看[这个][9]示例.

**工厂提供商**:有时我们需要动态创建这个依赖值,因为它所需要的信息我们直到最后一刻才能确定;我们如何注册一个工厂提供商呢?不着急,我们一步一步来:我们首先来创建一个验证权限的文件,`authorize.ts`,路径是:`app/services/authorize.ts`,我们暂且在里面放置一些简单的逻辑,来判定当前用户有没有获取`Users`的权限:
```typescript
import {Injectable} from '@angular/core';

@Injectable()
export class Authorize {
    isAuthorized: boolean;
    constructor(){
        this.isAuthorized = Math.random() > 0.5 ? true: false;
    }
    getIsAuthorized() {
        return this.isAuthorized;
    }
}
```

好吧,我承认这样写有点随意,暂时先这样吧;我们的目的是为了告诉大家如何使用工厂提供商,暂时简化权限验证这一块;从上面的代码我们可以大概了解到,这个服务就是为了获取当前用户的权限情况;然后我们来配置我们的`UserService2Provider`,为了方便我们暂时直接在`app.component.ts`中书写我们的配置:
```typescript
// ...
let UserService2Provider = (logger: Logger, authorize: Authorize) => {
    return new UserService2(logger, authorize.getIsAuthorized());
};
// ...
```
可以看到,我们的`UserService2Provider`其实就是一个返回了类的实例的一个函数;我们给这个函数传递了两个参数,分别是`Logger`和`Authorize`类的实例,然后我们根据这两个实例,创建出了我们新的服务实例;奥,忘了告诉大家,我们还要创建一个新的`UserService2`类,路径:`app/services/user-service2.ts`:
```typescript
import {Injectable} from '@angular/core';
import {Users} from "../mock/user-data.mock";
import {Logger} from "./logger.service";


@Injectable()
export class UserService2 {
    isAuthorized: boolean;
    constructor(private logger: Logger, isAuthorized: boolean) {
        this.isAuthorized = isAuthorized;
    }

    getUsers() {
        if(this.isAuthorized){
            this.logger.log('get users');
            return Users;
        }
        else {
            this.logger.log('not isAuthorized!');
            return [];
        }
    }
}
```

可以看到这个服务类和`UserService`差不多,就是多了一个条件验证,如果当前用户有获取`Users`的权限,我们就会返回这些`Users`;如果没有,我们就返回一个空数组.接下来就是很重要的一步了,我们需要在`app.component.ts`的`providers`中使用`UserService2Provider`:
```typescript
// ...
providers: [
        //Logger,
        //[{provide: Logger, useClass: BetterLogger}],
        [LoggerHelper, {provide: BetterLogger, useClass: BetterLogger}],
        //[LoggerHelper, {provide: Logger, useClass: BetterLogger}],
        {provide: Logger, useValue: loggerValue},
        //{provide: Logger, useValue: loggerValue1},
        UserService,
        Authorize, // 不可缺少
        {
            provide: UserService2,
            useFactory: UserService2Provider,
            deps: [Logger, Authorize]
        }
    ]
// ...    
```

还要记住,要添加`Authorize`依赖;我们在`app.component.ts`中使用新的服务:
```typescript
// ...
export class AppComponent {
    users: User[];

    constructor(private userService: UserService, private userService2: UserService2) {
        this.users = userService.getUsers();

        console.log(this.userService2.getUsers());
    }
}
```

刷新浏览器,你会看到有时它会输出:
```
From values: not isAuthorized!
[]
```

有时它会输出:
```
From values: get users
[User, User, User, User, User, User]
```
那么说明,我们这种方式注册`工厂提供商`的方式也成功了.

也许大家会有一些疑问,我们在类的构造函数中使用`private userService2: UserService2`怎么就获取了这个服务的一个示例,`Angular2`是怎么知道我们要的是`UserService2`类,它又是如何获取这个类的实例的呢?在`Angular2`中我们通过注射器来进行依赖注入,其实上面的形式只是一种简写;详细一点的写法是这样的:
```typescript
import {Component, Injector} from '@angular/core';
```
我们先从`Ng2`中获取`Injector`注射器,然后使用这个注射器来进行我们所需服务的实例化:
```typescript
// ...
export class AppComponent {
    users: User[];
    private userService2: UserService2;
    
    //constructor(private userService: UserService, private userService2: UserService2) {
    //    this.users = userService.getUsers();
    //
    //    console.log(this.userService2.getUsers());
    //}

    constructor(private userService: UserService, private injector: Injector) {
        this.users = userService.getUsers();
        this.userService2 = this.injector.get(UserService2);
        console.log(this.userService2.getUsers());
    }
}
```

所以可以看出来,这些繁琐的活我们全部都让`injector`去做了,只需要我们提供一些简单的说明,聪明的`Ng2`就知道如何进行依赖注入.

#### 非类依赖

我们上面的讲解全部都是把一个类作为一个依赖来进行服务的依赖注入,但是假如我们想要的不是一个类,而是一些值,或者对象;我们应该怎么办?我们先来写出这么一个文件`app-config.ts`,路径是:`app/config/app-config.ts`:
```typescript
export interface AppConfig {
    title: string,
    apiEndPoint: string
}

export const AppConf: AppConfig = {
    title: 'Dreamapple',
    apiEndPoint: 'https://hacking-with-angular.github.io/'
};
```

按照上面的使用`值`的方法,我们应该可以这样做:
```typescript
// ...
{provide: AppConfig, useValue: AppConf}
// ...
constructor(private userService: UserService, private userService2: UserService2, private appConf: AppConfig) {
        this.users = userService.getUsers();

        console.log(this.userService2.getUsers());

        console.log(this.appConf);
    }
// ...    
```
但是我们这样做却没有达到我们想要的效果;控制台报错:
```
Error: ReferenceError: AppConfig is not defined(…)
```

因为接口`interface`不能够被当做一个类`class`来处理,所以我们需要换一种新的方式,使用`OpaqueToken`(不透明的令牌):
```typescript
// 首先导入 OpaqueToken和Inject
import {Component, Injector, OpaqueToken, Inject} from '@angular/core';
// 引入AppConf,并且使用OpaqueToken
import {AppConf} from "./config/app-config";
let APP_CONFIG = new OpaqueToken('./config/app-config');
// 在providers中进行配置
{provide: APP_CONFIG, useValue: AppConf}
// 在类中使用
constructor(private userService: UserService, private userService2: UserService2, @Inject(APP_CONFIG) appConf: AppConfig) {
        this.users = userService.getUsers();

        console.log(this.userService2.getUsers());

        console.log(appConf);
    }
```

对上面的代码的一些解释首先我们使用`OpaqueTokean`对象注册依赖的提供商,然后我们在`@Inject`的帮助下,我们把这个配置对象注入到需要它的构造函数中,最后我们就可以使用最初的那个对象了.虽然`AppConfig`接口在依赖注入过程中没有任何作用,但它为该类中的配置对象提供了强类型信息.

最后我们来讲解一下`可选依赖`(Optional),有些服务的依赖也许不是必须的;我们就可以使用`@Optional()`来对这些参数做标记;记住,当使用`@Optional()`时,如果我们想标记`logger`这个服务是可选的;那么如果我们不在组件或父级组件中注册一个`logger`的话,注入器会设置该`logger`的值为空`null`;我们的代码必须要为一个空值做准备.来看一下例子吧,我们在`user.service.ts`中做一些改动:
```typescript
// 导入 Optonal
import {Injectable, Optional} from '@angular/core';
import {Users} from "../mock/user-data.mock";
import {Logger} from "./logger.service";
import {BetterLogger} from "./better-logger.service";


@Injectable()
export class UserService {
    constructor(private logger: Logger,
                // 使用@Optional标记
                @Optional()private betterLogger: BetterLogger) {
    }

    getUsers() {
        this.logger.log('get users');
        //noinspection TypeScriptUnresolvedFunction
        this.logger.hello();

        // 存在betterLogger时的处理
        if(this.betterLogger) {
            this.betterLogger.log('optional');
        }

        //console.log(this.logger);
        return Users;
    }
}
```

至此,整篇文章已经结束了;如果你坚持读到了这里,那说明你也是一个很有耐心的人;如果你有什么问题可以在[这里][10]提出.


[1]:https://segmentfault.com/img/bVAsmD
[2]:https://github.com/hacking-with-angular/angular2-travel/tree/quickstart
[3]:https://segmentfault.com/img/bVAn59
[4]:https://segmentfault.com/img/bVAsbH
[5]:https://segmentfault.com/img/bVAse6
[6]:https://segmentfault.com/img/bVAsfs
[7]:https://segmentfault.com/img/bVAsiJ
[8]:https://segmentfault.com/img/bVAskd
[9]:https://github.com/hacking-with-angular/angular2-travel/tree/basics-demo-di-test-1
[10]:https://github.com/hacking-with-angular/hacking-with-angular.github.io/issues
[11]:https://en.wikipedia.org/wiki/Dependency_injection
[12]:https://www.google.com.hk/search?rlz=1C5CHFA_enKR651TW659&q=Dependency+Injection&spell=1&sa=X&ved=0ahUKEwjpm6vovdLOAhXBqo8KHVcMAZMQvwUIGSgA
