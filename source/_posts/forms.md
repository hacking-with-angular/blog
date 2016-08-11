title: Step 4 - Forms
date: 2016-8-7 12:00:00
tags: ["Angular2", "Travel"]
cover: /images/angular2-travel/basics-demo-form.jpg
author: dreamapplehappy
---

**系统提示: 阅读此文大概需要我也不知道几分钟...**

今天我们要带领大家走进一个新天地,那就是Angular2的**表单**;众所周知基本上所有的web系统都会涉及到关于表单的操作,在那些电商类的网站中,表单更是占据着重要的地位;而且表单的**验证**,友好的**信息提示**,表单的**提交**等等都是web前端开发者常常需要花费大量的时间和精力去完成的任务.但是现在,通过使用Angular2与表单相关的指令和特性我们可以很方便的解决上面的问题,Oh,耶!

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
为了样式好看一点,我们直接使用`bootstrap`的样式,并且新增了一个样式文件`form.css`用来控制表单的样式,方便我们后边的使用;在`index.html`中我们的修改如下:
```html
<!-- Bootstrap-->
<link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.6/css/bootstrap.min.css">
<!-- Form.css -->
<link rel="stylesheet" href="app/style/form.css">
<!-- Style.css -->
<link rel="stylesheet" href="styles.css">
```
在根目录下我们运行命令`npm run start`我们就看到了一个简单到不能再简单的表单了;不要小看这个表单哦,在接下来的十几分钟内,它在我们`Angular2`的帮助下将会变成一个**超级英雄**;就像是电影中的老桥段,一个失魂落魄的少年受到大师的指点,突然间就变成了叱咤江湖的英雄.

我们首先要做的第一件事情就是将表单的数据展示到表单中,在进行这一步骤之前我们需要知道在`Angular`中,我们有两种方式来构建表单,其中一种就是我们今天要给大家讲解的**`模板驱动`**的表单,还有另一种创建表单的方式那就是**`模型驱动`**的表单;关于模型驱动的表单,我们会在后面的章节中给大家讲解,今天我们先来研究一下模板驱动的表单.

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
就像上面这样,我们已经初始化好一个`user`实例了;可是,我们怎么将数据展示在表单中呢?如果你学习过`ng1`,那么你会想到使用`ng-model`这个内置的指令;但是小伙子,我们学的是`ng2`,当然不可以使用`ng-model`了;那么,我们应该使用什么呢?好啦,不兜圈子了,在`ng2`中我们使用**[(ngModel)]**来进行数据的双向绑定.

**在这里我们要另起一行,以唤起大家对这个指令的足够重视;**上面几节中我们已经掌握了一些基本的招式,那就是**使用`[]`表示数据是从模型到模板的单项绑定;使用`()`表示数据是从模板到模型的单项绑定;**非常自然而然地,如果我们使用`[(ngModel)]`那应该表示模板和模型之间数据的双向绑定咯;Bingo,确实如此,我们先来尝试使用以下再说吧.
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
上面的代码就是我们使用`[(ngModel)]`的一个例子,这样我们就把用户的数据展示到表单里面了;先庆祝一下,至少到现在为止我们已经知道如何在表单中展示数据了.为了在我们对表单的数据做出改动的时候可以直观的看到它们的变化,我们在组件`AppComponent`中添加一个属性,我们使用`Getter`来获取这个属性.如果对`getter/setter`不是很熟悉的话,可以狠狠的点击这里[TypeScript存取器][2].
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

在前面几章节的学习中中,我们知道可以使用模板变量来对一个模板中的`input`模板进行赋值和取值;但是还是有点麻烦,当我们遇到`[(ngModel)]`后,一切都变得那么简单;我们接着来讲解`[(ngModel)]`这种双向绑定数据的方式;

**既然我们已经知道可以使用`[]`和`()`来进行数据的单项并且不同方向的绑定,我们何不尝试着把`[(ngModel)]`进行拆分呢?**看到这里,大家的思路应该是这样子的:
```typescript
<input  #username
        [ngModel]="user.username"
        (ngModel)="user.username = username.value"
        type="text" class="form-control" id="username" placeholder="Username">
```
然而事情并不像我们预期的那样,因为`ng2`中不是通过`ngModel`事件来触发表单中数据变化的操作,而是通过`ngModelChange`;所以我们将`(ngModel)="user.username = username.value"`改为`(ngModelChange)="user.username = username.value"`就可以实现我们想要的那个结果啦.

当然既然我们使用了`[(ngModel)]`就不需要使用模板变量了,所以我们改一下上面的代码:
```typescript
<input  [ngModel]="user.username"
        (ngModelChange)="user.username = $event"
        type="text" class="form-control" id="username" placeholder="Username">
```
我知道你想问什么;上面代码中的`$event`是什么鬼?为什么可以直接使用,难道`$event`不代表DOM事件吗?别着急,待我细细说来,这个`$event`是`ngModelChange`属性返回的输入框的值,它是一个Angular`EventEmitter`类型的属性,这个值就是我们想要的输入框的值.

在有些情况下,我们需要在数据从模板流向模型的时候做一些特殊的处理;比如合并或者限制按键频率,这个时候我们就需要使用`(ngModelChange)`了;当然大部分情况下`[(ngModel)]`已经足够满足我们使用了,如果你还想深入的了解一下`ngModel`可以看看这里[模板语法][4].

忽然发现一个问题,我们的下拉选择框还是静态的内容;我们可不希望是这样子的,所以我们来动手修改一下这个下拉框吧;使用的也还都是前面几章中提到的知识点:
```typescript
//...
favoriteFruitList = ['apple', 'pear', 'banana', 'orange'];
user = new User('dreamapple', '2451123321@qq.com', 'Nothing is impossible!', this.favoriteFruitList[0]);
//...
```
我们给这个组件新增加了一个`favoriteFruitList`属性,它是一个数组,包含了我们的水果选项;然后我们使用`this.favoriteFruitList[0]`来初始化我们用户喜欢的水果.然后我们还要修改一下我们的模板:
```html
<!--<select [(ngModel)]="user.favorite"-->
        <!--id="favorite" class="form-control">-->
    <!--<option value="apple">apple</option>-->
    <!--<option value="banana">banana</option>-->
    <!--<option value="pear">pear</option>-->
    <!--<option value="orange">orange</option>-->
<!--</select>-->
<select [(ngModel)]="user.favorite" id="favorite" class="form-control">
    <option *ngFor="let fruit of favoriteFruitList"
            [value]="fruit">{{fruit}}</option>
</select>
```

至此,我们已经学习了不少东西了;但是最酷炫的部分还没有学习呢?我们接下来要学习的部分就是关于表单的验证,我们在设计表单的时候,常常会在用户输入错误信息的时候或者忘记输入一些信息的时候给用户一些温馨的提示;在`ng2`中我们通过`ngModel`来跟踪用户的输入状态并且根据输入状态来判断表单中某些部分的有效性,进而决定要不要给用户一些提示等等.

**ngModel**这个指令不仅仅跟踪表单的状态,它还会告诉我们一些关于表单控件的额外信息;那就是**用户碰过这个控件了吗?这个控件的值发生了变化吗?数据变得无效了吗?**

**ngModel**还会使用一些特殊的`CSS`类名来更新我们控件的`className`列表,我们可以通过定制这些CSS类的样式来更改控件的外观,或者让一些消息显示或者隐藏.

要想使用上面所说的那些特性,我们的表单控件需要具备两个特点:(1)存在`name`属性;(2)使用了`[(ngModel)]`指令;我们为什么一定要使用`name`属性呢?那是因为在`ng2`的内部,每一个`<form>`表单实际上都附加了一个`NgForm`指令,`ng2`会在每一个表单上产生一个`FormControls(表单控制)`并且把这个`FormControls`注册在表单中,每一个`FormControl`都是以`name`属性作为唯一的依据来进行注册的;如果没有看懂,不要紧我们会在后面的章节中更加深入的讲解.

**好了,能动手解决的,我们绝不瞎比比;**我们首先让表单的`username`控件作为我们的实验对象吧:
```typescript
<input  [(ngModel)]="user.username" name="username" required #spy
        type="text" class="form-control" id="username" placeholder="Username">
<p>className: {{spy.className}}</p>
```
忘了说了,这个`name`属性还必须是唯一的,不然会出错的;说明一下上面的代码,我们首先给`input`这个控件添加了`name`属性,还添加了一个原生的`required`属性表明这个控件必须是有值的;然后我们给这个控件绑定了一个模板变量`spy`,然后我们在下面引用这个模板变量展示出这个控件的`className`列表.好了,下面我们要操作这个控件了;

当我们刚打开页面的时候,`className`的值为:
```
 form-control ng-untouched ng-pristine ng-valid
```
然后我们点击这个输入框然后点击页面空白部分,会看到`className`的值变为:
```
form-control ng-pristine ng-valid ng-touched
```
我们在修改一下这个控件的值,这时`className`的值变为:
```
form-control ng-valid ng-touched ng-dirty
```
我们完全删除这个输入框中的值,`className`的值变为:
```
 form-control ng-dirty ng-invalid ng-touched
```

我们可以清楚地看到,用户的操作会引起控件的`className`的变化,我们也就可以根据这些变化的类名,来对这些控件做出一些样式上的改变,以便给用户提供一些有效的信息.

好,下面我们就要添加一些`CSS`的样式,来改变我们的表单了;是时候让我们的表单变得的强大一点了:
```css
.ng-valid[required] {
    border-left: 5px solid #42A948; /* green */
}

.ng-invalid {
    border-left: 5px solid #a94442; /* red */
}
```

上面的样式用来添加到`form.css`里面,当用户的输入是正确的时候我们会在输入框前面加上一个绿颜色的竖条,当用户输入错误的时候,我们会在输入框的前面加上一个暗红颜色的竖条.就像下面这样:
![输入正确][5]
输入错误的时候应该是下面这样:
![输入错误][6]

上面的修改已经很好了,能够提示用户一些有用的信息了;但是用户可能无法根据一个暗红色的竖条知道他(她)哪里出错了,所以我们还需要给用户一些提示的信息才可以.我们可以再次使用模板变量来达到这个效果.
```html
<input  [(ngModel)]="user.username" name="username" required #spy #username="ngModel"
        type="text" class="form-control" id="username" placeholder="Username">
<p [hidden]="username.pristine || username.valid" class="alert alert-danger">用户名不可以为空!</p>
```
我当然知道你这时候又有一些问题了;待我给你慢慢解释,首先我们使用了`#username="ngModel"`,我们又新增了一个模板变量`username`然后给它赋值为`ngModel`,我们为什么要这么做呢?首先我们想让`username`链接到这个控件的`[(ngModel)]`指令中,之所以使用`ngModel`是因为这个指令的[exportAs][7]属性值为`ngModel`.

但是,当我们运行的时候;却出现了错误...这是为什么呢?
![exception-1][8]

**因为我们一直没有引入表单需要的依赖`provideForms`,所以就出现了上面的错误.**那我们现在把这个依赖加进去,下面的代码添加到`main.ts`里面:
```typescript
import {bootstrap} from '@angular/platform-browser-dynamic';
import {disableDeprecatedForms, provideForms} from '@angular/forms';

import {AppComponent} from './app.component';
//noinspection TypeScriptValidateTypes
//bootstrap(AppComponent);

import {AppComponent} from './app.component';
//noinspection TypeScriptValidateTypes
bootstrap(AppComponent, [
    disableDeprecatedForms(),
    provideForms()
])
.catch((err: any) => console.error(err));
```

我们也顺便把老的表单API给禁用了,因为我们添加了`disableDeprecatedForms`依赖.但是当我们再次运行的时候,还是报了错:
![exception-2][9]
这次给我们的提示是,我们没有给每一个使用了`ngModel`指令的控件添加一个`name`属性;所以赶紧添加`name`属性吧.添加完成之后,我们的表单终于正常了,刚才一定是走火入魔了.

现在当我们把`username`表单中的内容删除的时候,就会看到有提示消息显示`用户名不可以为空!`,并且输入框的左侧有一个暗红色的小竖条.是不是感觉很不错.我们继续为剩下的(除去下拉框)那些表单控件添加`required`属性,还有一些提示的消息吧.

好啦,关于表单的验证上面的讲解已经可以在大部分情况下满足我们的使用了;下面我们来给这个表单添加一些其他的功能吧;我们给组件`AppComponent`新增加一个方法`addUser`,每当我们点击表单上的哪个新增用户的按钮的时候,我们就要清空一下表单;,然后重新进行输入:
```typescript
addUser() {
    this.user = new User('', '', '', '');
}
```
然后我们修改模板:
```html
<!--...-->
<button class="btn btn-primary" (click)="addUser()" type="button" class="btn btn-default">addUser</button>
<!--...-->
```
然后当我们点击`addUser`按钮的时候,会发现哪个`Username`的输入框左边会有一个暗红色的小竖条;这些都是在预料之中的,但是当我们在`Username`输入框中输入一些内容,然后再次点击`addUser`按钮的时候,意外却发生了:
![表单错误1][10]

这是为什么,我们的逻辑应该没有错误呀?确实不是我们的错误,是`Angular2`没有明白我们的意思...因为在这种实现方式下,`Angular`没有办法区分是替换了整个用户的数据还是用程序单独清除了`username`属性.当然`Angular`也不能作出假设,所以它只好保存了当前的状态,就是`被污染`的状态.

所以我们只好再次讨好`Angular`,被迫做出一些小花招;我们给组件添加一个`active`标记,把它初始化为`true`.当我们新增加一个用户的时候,就把它标记为`false`;然后通过`setTimeout`迅速的把它重新设置回`true`:
```typescript
export class AppComponent {
    active: boolean = true;
    favoriteFruitList = ['apple', 'pear', 'banana', 'orange'];
    user = new User('dreamapple', '2451123321@qq.com', 'Nothing is impossible!', this.favoriteFruitList[0]);

    get userInfo() {
        return JSON.stringify(this.user);
    }

    addUser() {
        this.active = false;
        this.user = new User('', '', '', '');
        setTimeout(() => this.active = true, 0);
    }
}
```

在模板中我们使用`NgIf`指令来控制整个表单:
```html
<!--...-->
<form class="DEMO-form" *ngIf="active">
<!--...-->
```
我们这样做的目的,就是为了每次新添加用户的时候,整个表单的状态都是全新的.当然这只是一个小小的`hack`写法,后面我们会学习如何使用更标准的方法达到我们上面的目的.

如果看到这里你还没有睡着的话,那么,**小伙子我觉得你骨骼惊奇,很适合敲代码,来跟我们一起学做菜吧!**我们说了这么多,却迟迟没有看到表单的提交?好啦,下面我们来讲解一下关于表单的提交;我们可以通过使用`ngSubmit`事件来达到提交表单的功能;我们先写一个提交表单的方法吧:
```typescript
submitForm() {
    alert(this.userInfo + '已经被提交!');
}
```
然后再次修改模板:
```html
<!--...-->
<form class="DEMO-form" *ngIf="active" (ngSubmit)="submitForm()">
<!--...-->
```
这样当我们点击`Submit`按钮的时候,我们就可以进行表单的提交啦.虽然上面只是一个简单的弹出框,但是已经达到了我们想要的效果.

一般情况下,如果用户填写的表单不合法,我们是不允许表单的提交的;那么我们应该怎么做呢?相信你已经有了答案.对就是使用模板变量,我们可以在表单和提交按钮上做一些小文章:
```html
<!--...-->
<form class="DEMO-form" *ngIf="active" (ngSubmit)="submitForm()" #userForm="ngForm">
<!--...-->
<button [disabled]="!userForm.form.valid" type="submit" class="btn btn-default">Submit</button>
<!--...-->
```
就像我们上面是使用`#username="ngModel"`一样,这次我们使用的是`#userForm="ngForm"`前面已经说过了,`Angular`会为每一个表单附加一个`ngForm`指令;每当表单不合法的时候我们就禁止表单的提交,就是让`Submit`按钮禁用.到此为止我们要讲解的内容就差不多讲完了.

**但是**,我们今天还要额外的多讲解一点知识;那就是关于组件的使用,我们把上面的表单封装成一个组件吧.我们新建一个文件`form.component.ts`,然后把所有的逻辑都放到这里面,我们再新建一个模板文件`app.html`,用来放置`AppComponent`组件的模板.首先是`form.component.ts`:
```typescript
import {Component} from '@angular/core';

// 使用User类
import {User} from './classes/User';

/*
 * 别忘记了使用@前缀
 * 这里相当于组件视图
 */
@Component({
    selector: 'my-form',
    //template: '<h1>My First Angular2 Travel</h1>'
    templateUrl: 'app/templates/form.html'
})

/*
 * 导出这个组件,也就是一个类
 * 这里相当于组件控制器
 */
export class FormComponent {
    active: boolean = true;
    favoriteFruitList = ['apple', 'pear', 'banana', 'orange'];
    user = new User('dreamapple', '2451123321@qq.com', 'Nothing is impossible!', this.favoriteFruitList[0]);

    get userInfo() {
        return JSON.stringify(this.user);
    }

    addUser() {
        this.active = false;
        this.user = new User('', '', '', '');
        setTimeout(() => this.active = true, 0);
    }

    submitForm() {
        alert(this.userInfo + '已经被提交!');
    }
}
```

注意`selector`选项我们修改了它的值为`my-form`,然后我们修改`app.component.ts`:
```typescript
import {Component} from '@angular/core';
import {FormComponent} from './form.component';

/*
 * 别忘记了使用@前缀
 * 这里相当于组件视图
 */
@Component({
    selector: 'my-app',
    templateUrl: 'app/templates/app.html',
    directives: [
        FormComponent
    ]
})

/*
 * 导出这个组件,也就是一个类
 * 这里相当于组件控制器
 */
export class AppComponent {}
```

我们导入了`FormComponent`组件,并且在`directives`选项中添加了它;关于为什么要这样使用,我们后面的文章中会再次介绍的,现在大家先知道是这样使用就行啦.然后是`app.html`:
```html
<h1>Hello, World!</h1>
<my-form></my-form>
```
我们添加了一个新的标签`my-form`,这就是我们在`form.component.ts`中使用`selector: 'my-form'`的原因.

终于结束啦,希望大家可以学的开心,然后能更好的使用`Angular2`.

欢迎提[issue][11],这篇文章的[项目地址][12].








[1]:https://github.com/hacking-with-angular/angular2-travel/tree/quickstart
[2]:http://dreamapple.leanapp.cn/gitbook/typescript/doc/handbook/Classes.html
[3]:/images/angular2-travel/form-demo-1.jpg
[4]:https://angular.cn/docs/ts/latest/guide/template-syntax.html
[5]:/images/angular2-travel/form-correct.jpg
[6]:/images/angular2-travel/form-error.jpg
[7]:https://angular.cn/docs/ts/latest/api/core/index/DirectiveMetadata-class.html#!#exportAs
[8]:/images/angular2-travel/exception-1.jpg
[9]:/images/angular2-travel/exception-2.jpg
[10]:/images/angular2-travel/form-error-1.jpg
[11]:https://github.com/hacking-with-angular/hacking-with-angular.github.io/issues
[12]:https://github.com/hacking-with-angular/angular2-travel/tree/basics-demo-forms-v1