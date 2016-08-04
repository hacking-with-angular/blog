title: Step 2 - Displaying Data
date: 2016-8-3 12:00:00
tags: ["Angular2"]
---
在这一章节中,我们来学习如何使用Angular2来展示数据,以及如何使用它的内置指令`NgFor`和`NgIf`

首先要确保你有一个可以运行起来的Angular2的样例程序,最好就是我们上一章节中完成的那个[QuickStart][1]小项目或者你自己根据[官网][2]上面的步骤完成的QuickStart小项目,因为我们的讲解都是在那个基础上来进行的;然后让我们开始下面的快乐旅程吧.

因为我们的这一系列的文章都是使用的`TypeScript`所以在看下面的内容之前你最好可以看一下TypeScript或者ES6的类,它们的网址分别是[TypeScript][3],[ES6][4];如果以前你学过`Java`或者`C++`这种类似的面向对象的语言的话,那么学习这里的类就很轻松了;这里面的类基本上和那些语言中的类是相似的.

上一节中我们在`app.component.ts`中导出了一个空的类,那么下面我们就要开始填充它,让它变得丰满起来.首先我们给这个`AppComponent`类(组件)添加三个属性,分别是`name`,`age`,`fruit`;就像下面这样:

```javascript
export class AppComponent {
    username = 'dreamapple';
    age = 22;
    fruit = 'apple'
}
```
上面的写法其实只是一种简写的形式,实际上完整的写法应该是这样的:
```javascript
export class AppComponent {
    //username = 'dreamapple';
    //age = 22;
    //fruit = 'apple'
    username: string;
    age: number;
    fruit: string;

    constructor() {
        this.username = 'dreamapple';
        this.age = 22;
        this.fruit = 'apple';
    }
}
```
然后我们要修改我们的模板了,因为我们在模板中要使用较多的HTML,所以我们要使用反引号来包裹住我们的HTML片段;我们的装饰器函数如下所示:
```javascript
@Component({
    selector: 'my-app',
    template:`
        <p>My name is: {{username}}</p>
        <p>My age is: {{age}}</p>
        <p>My favorite fruit is: {{fruit}}</p>
    `
})
```
当然我们也可以使用装饰器函数中元数据对象的`templateUrl`配置选项,如下所示:
```javascript
@Component({
    selector: 'my-app',
    //template:`
    //    <p>My name is: {{username}}</p>
    //    <p>My age is: {{age}}</p>
    //    <p>My favorite fruit is: {{fruit}}</p>
    //`,
    templateUrl: 'app/templates/app-template.html'
})
```
其中`app/templates/app-template.html`表示的是程序的根目录`app`(而不是`angular2-travel`)下的`templates`文件夹下的`app-template.html`文件,然后将我们之前写的HTML片段复制过去就好了.

从上面的过程中,我们可以看到`Angular2`显示数据依然使用的是`Angular1`惯用的双大括号;它作为一个插值符号,在这个插值符号出现的地方就是我们要显示的数据的地方.接下来我们要学习使用`NgFor`这个内置指令了,熟悉`Angular1`的同学应该很容易就上手这个指令,因为`NgFor`和`ng-repeat`基本是一样的;它用来循环一个可迭代的对象,一般情况下会是一个数组.

接下来我们给`AppComponent`再添加一个属性`fruitsList`,如下所示:
```javascript
export class AppComponent {
    username = 'dreamapple';
    age = 22;
    fruitsList = ['apple', 'orange', 'pear', 'banana'];
    fruit = this.fruitsList[0]; // 这里要使用 this.fruitsList[0]
}
```
我们需要注意的地方是那个有注释的地方,我们必须使用`this.fruitsList`来指代上面我们已经定义好的属性,如果使用`fruitsList`的话,`Angular`就不知道它表示是什么.
接下来我们要循环这个水果数组了,看看如何在模板中表示吧.
```javascript
@Component({
    selector: 'my-app',
    template:`
        <p>My name is: {{username}}</p>
        <p>My age is: {{age}}</p>
        <ul>
            <li *ngFor="let fruit of fruitsList">{{fruit}}</li>
        </ul>
        <p>My favorite fruit is: {{fruit}}</p>
    `
})
```
上面的代码中有几个地方是需要注意的地方,首先我们使用了`*ngFor`而不是`ngFor`,这里的`*`是不能够去掉的;如果去掉的话那么我们的程序不会报错,但是我们只循环出来了
数组的第一项.关于为何要加`*`,你可以详细的看看这里[模板语法][5];当然我们可以在`*ngFor`表达式的后面写一些能够帮助我们展示每一项索引的操作,可以像下面这样:
```html
<li *ngFor="let fruit of fruitsList; let i = index;">{{i}}-{{fruit}}</li>
```
上面的模板代码有一些需要注意的地方,首先要知道`*ngFor`后面跟的是表达式,所以我们可以写一些简单的表达式,帮助我们更好地进行循环;还有我们使用了`let`关键词,增
加了块级作用域,使我们的这些变量都限定在`*ngFor`这个循环块中.好啦,如果你想更多地了解`*ngFor`,你可以看一下官方关于[NgFor][6]的API.

接下来要介绍的这个指令是`NgIf`,当然这个指令和`Angular1`的`ng-if`指令基本上是相似的,根据后面表达式的值决定要不要在DOM上添加或者移除一个元素.
看看我们是如何在模板使用这个指令的:
```html
<p *ngIf="fruitsList.length > 3">fruitsList's length is bigger than 3</p>
<p *ngIf="fruitsList.length <= 3">fruitsList's length is not bigger than 3</p>
```
首先要指出的是,和使用`*ngFor`一样,我们使用了`*ngIf`;然后我们可以在`*ngIf`后面写上一个表达式,这个表达式被期望的返回结果是`Boolean`类型的值;然后`*ngIf`指令会根据这个表达式的值决定要不要在DOM上添加或者移除它掌控的这个元素.

既然我们使用了`TypeScript`,我们就可以使用很多新的特性,比如说使用类去构建实例;接下来我们来使用`Fruit`类来构建我们的水果实例.首先我们在`app`文件夹下创建一个新的文件夹,就叫它`classes`吧,然后我们创建一个`Fruit.ts`文件,在这里面我们书写`Fruit`类的代码:
```typescript
export class Fruit{
    constructor(
        public name:string,
        public price: number
    ){}
}
```
解释一下上面的代码,我们使用了构造函数,然后在构造函数里面声明了这个对象的两个属性;一个是`name`,另一个是`price`,我们使用构造函数里面的`name:string,`和`price: number`参数来创建和初始化这个对象的属性.接下来我们就可以在我们的程序中使用这个类了;

首先在`app.component.ts`中引入这个类:
```typescript
import {Fruit} from './classes/Fruits';
```
然后在`AppComponent`中使用`Fruit`类来初始化我们的水果列表:
```typescript
export class AppComponent {
    username = 'dreamapple';
    age = 22;
    fruitsList = [
        new Fruit('apple', 20),
        new Fruit('orange', 30),
        new Fruit('pear', 40),
        new Fruit('banana', 50)
    ];
    //noinspection TypeScriptUnresolvedVariable
    fruit = this.fruitsList[0].name; // 这里要使用 this.fruitsList[0]
}
```
然后也要改一下我们的模板:
```html
<li *ngFor="let fruit of fruitsList; let i = index;">{{i}}-{{fruit.name}}-{{fruit.price}}</li>
```
最后的结果应该是下面这个样子:
![displaying-data][7]

最后不得不说,无论是**ES6**,还是**TypeScript**都让我感觉到了有种写**Java**感觉;这也算有利有弊吧,好处肯定是增加了代码的可读性,使程序更加容易维护,也更容易写出
大型的项目,让团队协作也非常的便利;当然也有它的一些不足,不足主要是增加了大家的学习成本;当然一切向前看呀.

如果本篇文章你有哪里不明白,或者是那里有错误你都可以在[这里][8]指出来,当然更欢迎热爱**Angular2**的童鞋来加入我们这个组织哈.




[1]:https://github.com/hacking-with-angular/angular2-travel/tree/quickstart
[2]:https://angular.cn/docs/ts/latest/quickstart.html
[3]:http://dreamapple.leanapp.cn/gitbook/typescript/doc/handbook/Classes.html
[4]:http://es6.ruanyifeng.com/#docs/class
[5]:https://angular.cn/docs/ts/latest/guide/template-syntax.html#!#ngIf
[6]:https://angular.io/docs/ts/latest/api/common/index/NgFor-directive.html
[7]:http://angular.angular-china.org/152cf77b-f2bf-49b4-933c-a5b0709104e5.jpg
[8]:https://github.com/hacking-with-angular/hacking-with-angular.github.io/issues