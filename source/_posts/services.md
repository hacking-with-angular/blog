title: Step 6 - Services
date: 2016-9-18 12:00:00
tags: ["Angular2", "Travel"]
cover: /images/angular2-travel/services.jpg
author: dreamapplehappy
---

这篇文章我们来讲解如何使用`service(服务)`,谈及服务我们就要了解什么是服务;在`Angular`中,我们所说的服务是指那些能够被其它的组件或者指令调用的**单一的**,**可共享的**代码块.服务能够使我们提高代码的利用率,方便组件之间共享数据和方法,方便测试和维护.

如果你看了上一篇文章[Step 5 - Dependency Injection][1],你就会发现我们这一部分讲解的内容和上一篇有很多的相似之处;当然也有一些新的知识点,温故而知新嘛;好了让我们来开始今天的旅行吧.

首先我们还是切换回之前的[quickstart][2]版本,然后运行:
```bash
npm run start
```
可以看到`My First Angular2 Travel`,然后继续我们的铺垫工作;主要是三个部分:
1.将我们的组件模板使用单一的`html`模板来替代;
2.构建`User`类方便我们后面的使用;
3.构建我们的模拟数据,方便我们使用服务来获取这些数据;

首先我们来完成第一部分,修改`app.component.ts`中`@Component`的元数据:~~`template: '<h1>My First Angular2 Travel</h1>'`~~改为:`templateUrl: 'app/templates/main.html'`,然后我们在`main.html`中书写我们的模板代码:
```html
<h1>My First Angular2 Travel</h1>
```

然后我们来进行第二个工作,创建我们的`User`类;为了展示方便,我们就给`User`两个属性吧,一个是`id(number)`,一个是`name(string)`;文件的路径是:`app/classes/User.ts`,具体的代码如下:
```typescript
export class User {
    //id: number;
    //name: string;

    constructor(
        private id: number,
        private name: string
    ){}
}
```

上面注释的部分是这个`User`类的简写,这个根据个人的喜好;你喜欢写就怎么写.

最后一项工作就是来创造我们的模拟数据,我们现在还没有学习如何在`Angular2`中使用`HTTP`,所以我们暂时将这些数据记录在一个文件中,然后导出这些数据,供我们接下来使用.我们的模拟数据文件路径是:`app/mock/user.data.ts`;下面是代码部分:
```typescript
import {User} from "../classes/User";

export const USERS: User[] = [
    {id: 1, name: 'dreamapple1'},
    {id: 2, name: 'dreamapple2'},
    {id: 3, name: 'dreamapple3'},
    {id: 4, name: 'dreamapple4'},
    {id: 5, name: 'dreamapple5'},
    {id: 6, name: 'dreamapple6'},
    {id: 7, name: 'dreamapple7'},
    {id: 8, name: 'dreamapple8'}
];
```

可以看到,我们导出了一个数组,这个数组的每一个元素都是一个`User`类的实例.

接下来,我们就要步入今天的主题了;构建一个服务,这个服务能够获取我们刚刚书写的模拟数据;首先不要忘记的是,`service`是一个类,然后这个类可以注入到别的组件
或者指令中去,还有一点就是我们知道,获取数据的服务往往都是异步的,所以我们使用了`Promise`去封装我们获取到的数据,来模拟异步请求;文件的路径是:`app/service/user.service.ts`,我们也遵循一个约定,服务的文件后缀是`*.service.ts`,前面的单词如果是多个的话就使用短横线来连接,比如`SpecialUserService`我们就写成`special-user.service.ts`,详细的部分请看代码:
```typescript
import {User} from "../classes/User";
import {USERS} from "../mock/user.data";

export class UserService {
    getUsers(): User[] {
        return Promise.resolve(USERS);
    }
}
```
关于上面代码的一些解释:因为我们的`getUsers`函数是有返回值的,它的返回值是一个数组,数组的每一个元素都是`User`类的实例,所以我们使用了`getUsers(): User[]`,还有因为我们要模拟异步请求获取数据,所以我们使用了`Promise`,如果你对`Promise`有什么不懂的地方,可以看看[这里][3].

接下来我们就要使用这个服务了,如何使用这个服务呢?在上一章节中我们已经讲解了许多种使用服务的方法;现在我们使用最简单的一种方式,直接使用`providers`来注入我们的服务,然后我们还要把我们获取到的数据展示到我们的模板中,具体的代码如下所示:
```typescript
import {Component} from '@angular/core';
import {User} from "./classes/User";
import {UserService} from "./services/user.service";

/*
 * 别忘记了使用@前缀
 * 这里相当于组件视图
 */
@Component({
    selector: 'my-app',
    //template: '<h1>My First Angular2 Travel</h1>',
    templateUrl: 'app/templates/main.html',
    providers: [UserService]
})

/*
 * 导出这个组件,也就是一个类
 * 这里相当于组件控制器
 */
export class AppComponent {
    users: User[];

    constructor(private userService: UserService){
        //noinspection TypeScriptUnresolvedFunction
        this.userService.getUsers().then(
            users => this.users = users
        )
    }
}
```

我们也要改动`main.html`的内容:
```html
<h1>My First Angular2 Travel</h1>
<ul>
    <li *ngFor="let user of users">{{user.name}}</li>
</ul>
```

这时我们打开浏览器,就会看到我们想要的结果:
![service][4]

上面的写法是有一些问题的,构造函数是为了简单的初始化工作而设计的,比如把构造函数的参数赋值给属性.它的负担不应该过于沉重.所以我们把数据的获取放在了组件的生命周期的钩子函数中去,如果你不了解组件的生命周期的话,那么你可以[看看这里][5],在这里我们使用了`ngOnInit`;我们修改一下上面的代码:
```typescript
import {Component, OnInit} from '@angular/core';
import {User} from "./classes/User";
import {UserService} from "./services/user.service";

/*
 * 别忘记了使用@前缀
 * 这里相当于组件视图
 */
@Component({
    selector: 'my-app',
    //template: '<h1>My First Angular2 Travel</h1>',
    templateUrl: 'app/templates/main.html',
    providers: [UserService]
})

/*
 * 导出这个组件,也就是一个类
 * 这里相当于组件控制器
 */
export class AppComponent implements OnInit{
    users: User[];

    constructor(
        private userService: UserService
    ){}

    getUsersData() {
        this.userService.getUsers()
            .then(users => this.users = users);
    }

    ngOnInit() {
        this.getUsersData();
    }

}
```
上面代码的一些解释,首先我们在`@angular/core`中导出了`OnInit`这个接口,然后我们又通过组件中的`ngOnInit`方法实现了这个接口;将构造函数中的获取数据的业务提取了出来,这种做法是组件初始化的时候获取数据比较好的一种方案.我们在后面的文章中也会讲解关于组件或者指令生命周期的文章.

最后,我们还可以更真实的的去模拟从服务器读取数据的操作;我们可以通过使用`setTimeout`来延时获取我们的数据,这就很好地模拟了我们从服务器获取数据的操作;具体的代码部分看下面:
```typescript
import {User} from "../classes/User";
import {USERS} from "../mock/user.data";

export class UserService {
    getUsers(): Promise<User[]> {
        return Promise.resolve(USERS);
    }
    getMockUsers(): Promise<User[]> {
        return new Promise(resolve => setTimeout(resolve(USERS), 2000))
            .then(() => this.getUsers());
    }
}
```
**首先需要注意的一点是,我们之前写的代码把`getUsers()`函数的返回值定义为`User[]`,其实更准确的应该是`Promise<User[]>`;我们接下来写的函数`getMockUsers()`利用`setTimeout`延时返回了我们的模拟数据.**

其实我们的程序里还有一个小错误,不容易被发现;当我在构建的时候我发现了下面的错误:
![services][8]

它提醒我们说,`Property 'id' is private in type 'User' but not in type '{ id: number; name: string; }'.`这是因为我们把`id`作为`User`的私有属性了,但是在`{ id: number; name: string; }`对象中,`id`不是私有的属性;要解决这个问题有多种思路,你可以将我们的模拟数据使用`User`类来创建,或者将`id`和`name`作为`public`属性.那我们就取一个简单的方法,将属性定义为`public`.`User`类的代码修改如下:
```typescript
export class User {
    id: number;
    name: string;

    //constructor(
    //    private id: number,
    //    private name: string
    //){}
}
```

在TypeScript里，每个成员默认为是public的.所以上面的写法是很简便的.

到这里我们要说的内容已经说完了,源代码可以参考这里[angular2-travel][6],当然欢迎[批评指正][7].


[1]:https://hacking-with-angular.github.io/2016/08/11/dependency-injection/
[2]:https://github.com/hacking-with-angular/angular2-travel/tree/quickstart
[3]:http://es6.ruanyifeng.com/#docs/promise
[4]:http://angular.angular-china.org/250af0ce-0cb7-48e2-82ed-30ec2e5ef92b.png
[5]:https://angular.io/docs/ts/latest/guide/lifecycle-hooks.html
[6]:https://github.com/hacking-with-angular/angular2-travel/tree/basics-demo-services-v1
[7]:https://github.com/hacking-with-angular/hacking-with-angular.github.io/issues
[8]:http://angular.angular-china.org/532055d9-8315-459a-be44-25ac65570ce4.png