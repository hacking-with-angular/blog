title: Step 4 - Forms
date: 2016-8-7 12:00:00
tags: ["Angular2", "Travel"]
cover: /images/angular2-travel/basics-demo-form.jpg
---

**系统提示: 阅读此文大概需要我也不知道几分钟...**

今天我们要带领大家走进一个新天地,那就是Angular2的**表单**;众所周知基本上所有的web系统都会涉及到关于表单的操作,
在那些电商类的网站中,表单更是占据着重要的地位;而且表单的**验证**,友好的**信息提示**,表单的**提交**等等都是web前端
开发者常常需要花费大量的时间和精力去完成的任务.但是现在,通过使用Angular2与表单相关的指令和特性我们可以很方便的解决上面的问题,Oh,耶!

当然,说得再多不如我们亲自来实践一下,毕竟**实践出真知**;当然搞不好如果你看了这篇文章,说不定也就喜欢上使用**Angular2**了呢?(我才不信呢...)

好,让我们开始这愉快的旅行;首先我们这个小的练习还是在[**QuickStart**][1]的基础上进行的,所以你还是要先完成这一步,才可以进行下面的练习.

首先我们写一个简单的表单模板:
```html
<div class="DEMO-form-container">
    <h1>This is A Awesome Form!</h1>
    <form class="DEMO-form">
        <div class="form-group">
            <label for="username">Username</label>
            <input type="text" class="form-control" id="username" placeholder="Username">
        </div>
        <div class="form-group">
            <label for="email">Email</label>
            <input type="email" class="form-control" id="email" placeholder="Email">
        </div>
        <div class="form-group">
            <label for="motto">Password</label>
            <input type="text" class="form-control" id="motto" placeholder="Motto">
        </div>
        <div class="form-group">
            <label for="favorite">Favorite fruit</label>
            <select id="favorite" class="form-control">
                <option value="apple">apple</option>
                <option value="banana">banana</option>
                <option value="pear">pear</option>
                <option value="orange">orange</option>
            </select>
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
    </form>
</div>
```
它的路径是`app/templates/form.html`,然后我们修改一下`app.component.ts`文件中我们对这个组件的配置:
```typescript
@Component({
    selector: 'my-app',
    //template: '<h1>My First Angular2 Travel</h1>'
    templateUrl: 'app/templates/form.html'
})
```
为了样式好看一点,我们直接使用`bootstrap`的样式,并且新增了一个样式文件`form.css`用来控制表单的样式,方便我们后边的使用;
在`index.html`中我们的修改如下:
```html
<!-- Bootstrap-->
<link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.6/css/bootstrap.min.css">
<!-- Form.css -->
<link rel="stylesheet" href="app/style/form.css">
<!-- Style.css -->
<link rel="stylesheet" href="styles.css">
```
在根目录下我们运行命令`npm run start`我们就看到了一个简单到不能再简单的表单了;不要小看这个表单哦,在接下来的十几分钟内,
它在我们`Angular2`的帮助下将会变成一个**超级英雄**;就像是电影中的老桥段,一个失魂落魄的少年受到大师的指点,突然间就变成了叱咤江湖的英雄.

我们首先要做的第一件事情就是将表单的数据展示到表单中,在进行这一步骤之前我们需要知道在`Angular`中,我们有两种方式来构建表单,
其中一种就是我们今天要给大家讲解的**`模板驱动`**的表单,还有另一种创建表单的方式那就是**`模型驱动`**的表单;关于模型驱动的表单,
我们会在后面的章节中给大家讲解,今天我们先来研究一下模板驱动的表单.

**系好安全带,老司机要开车了**;在展示数据之前,我们需要一个创建一个`User`类,这个类用来创建我们的用户数据:
```typescript
export class User {
    constructor(
        public username: string,
        public email: string,
        public motto: string,
        public favorite: string
    ){}
}
```
一个很简单的类,用来创建我们的用户实例.然后我们需要在`AppComponent`中导入这个类,并且初始化一个`user`;
```typescript
// 使用User类
import {User} from './classes/User';
// ...
export class AppComponent {
    user = new User('dreamapple', '2451123321@qq.com', 'Nothing is impossible!', 'apple');
}
```
就像上面这样,我们已经初始化好一个`user`实例了;可是,我们怎么将数据展示在表单中呢?如果你学习过`ng1`,
那么你会想到使用`ng-model`这个内置的指令;但是小伙子,我们学的是`ng2`,当然不可以使用`ng-model`了;
那么,我们应该使用什么呢?好啦,不兜圈子了,在`ng2`中我们使用**[(ngModel)]**来进行数据的双向绑定.

**在这里我们要另起一行,以唤起大家对这个指令的足够重视;**上面几节中我们已经掌握了一些基本的招式,
那就是**使用`[]`表示数据是从模型到模板的单项绑定;使用`()`表示数据是从模板到模型的单项绑定;**
非常自然而然地,如果我们使用`[(ngModel)]`那应该表示模板和模型之间数据的双向绑定咯;
Bingo,确实如此,我们先来尝试使用以下再说吧.
```html
<div class="DEMO-form-container">
    <h1>This is A Awesome Form!</h1>
    <form class="DEMO-form">
        <div class="form-group">
            <label for="username">Username</label>
            <input  [(ngModel)]="user.username"
                    type="text" class="form-control" id="username" placeholder="Username">
        </div>
        <div class="form-group">
            <label for="email">Email</label>
            <input  [(ngModel)]="user.email"
                    type="email" class="form-control" id="email" placeholder="Email">
        </div>
        <div class="form-group">
            <label for="motto">Motto</label>
            <input  [(ngModel)]="user.motto"
                    type="text" class="form-control" id="motto" placeholder="Motto">
        </div>
        <div class="form-group">
            <label for="favorite">Favorite fruit</label>
            <select [(ngModel)]="user.favorite"
                    id="favorite" class="form-control">
                <option value="apple">apple</option>
                <option value="banana">banana</option>
                <option value="pear">pear</option>
                <option value="orange">orange</option>
            </select>
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
    </form>
</div>
```
上面的代码就是我们使用`[(ngModel)]`的一个例子,这样我们就把用户的数据展示到表单里面了;
先庆祝一下,至少到现在为止我们已经知道如何在表单中展示数据了.
为了在我们对表单的数据做出改动的时候可以直观的看到它们的变化,我们在组件`AppComponent`中添加一个属性,我们使用`Getter`来获取这个属性.
如果对`getter/setter`不是很熟悉的话,可以狠狠的点击这里[TypeScript存取器][2].
```typescript
export class AppComponent {
    user = new User('dreamapple', '2451123321@qq.com', 'Nothing is impossible!', 'apple');

    get userInfo() {
        return JSON.stringify(this.user);
    }
}
```
我们在模板中添加一些HTML片段来展示我们的`user`数据:
```html
<!-- ... -->
<h1>This is A Awesome Form!</h1>
<p class="user-msg">UserInfo: {{userInfo}}</p>
<form class="DEMO-form">
<!-- ... -->
```
这样我们改变表单中的数据,就可以在上面看到实时的变化,如下图所示;这就说明,使用`[()]`确实是能够起到双向绑定的作用的.
![表单图片][3]

在前面几章节的学习中中,我们知道可以使用模板变量来对一个模板中的`input`模板进行赋值和取值;
但是还是有点麻烦,当我们遇到`[(ngModel)]`后,
一切都变得那么简单;我们接着来讲解`[(ngModel)]`这种双向绑定数据的方式;

**既然我们已经知道可以使用`[]`和`()`来进行数据的单项并且不同方向的绑定,我们何不尝试着把`[(ngModel)]`进行拆分呢?**
看到这里,大家的思路应该是这样子的:
```typescript
<input  #username
        [ngModel]="user.username"
        (ngModel)="user.username = username.value"
        type="text" class="form-control" id="username" placeholder="Username">
```
然而事情并不像我们预期的那样,因为`ng2`中不是通过`ngModel`事件来触发表单中数据变化的操作,而是通过`ngModelChange`;
所以我们将`(ngModel)="user.username = username.value"`改为`(ngModelChange)="user.username = username.value"`
就可以实现我们想要的那个结果啦.

当然既然我们使用了`[(ngModel)]`就不需要使用模板变量了,所以我们改一下上面的代码:
```typescript
<input  [ngModel]="user.username"
        (ngModelChange)="user.username = $event"
        type="text" class="form-control" id="username" placeholder="Username">
```
不要说话,我知道你想问什么;上面代码中的`$event`是什么鬼?为什么可以直接使用,难道`$event`不代表DOM事件吗?别着急,待我细细说来,
这个`$event`是`ngModelChange`属性返回的输入框的值,它是一个Angular`EventEmitter`类型的属性,这个值就是我们想要的输入框的值.

在有些情况下,我们需要在数据从模板流向模型的时候做一些特殊的处理;比如合并或者限制按键频率,这个时候我们就需要使用`(ngModelChange)`了;
当然大部分情况下`[(ngModel)]`已经足够满足我们使用了,如果你还想深入的了解一下`ngModel`可以看看这里[模板语法][4].

// todo 下拉框.../老的form表单/表单验证/组件








[1]:https://github.com/hacking-with-angular/angular2-travel/tree/quickstart
[2]:http://dreamapple.leanapp.cn/gitbook/typescript/doc/handbook/Classes.html
[3]:/images/angular2-travel/form-demo-1.jpg
[4]:https://angular.cn/docs/ts/latest/guide/template-syntax.html