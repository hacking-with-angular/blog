title: Step 3 - User Input
date: 2016-8-4 12:00:00
tags: ["Angular2", "Travel"]
---
在这篇文章中,我们将要讲解如何处理用户的操作,最常见的就是点击和输入了;本篇文章着重讲解关于用户的点击和输入的处理.

我们这次的初始项目还是以我们的[**QuickStart**][1]小项目为基础,然后在那个基础上进行我们今天的操作;在我们这个项目中因为要使用到大量的HTML片段,所以我们决定使用模板文件而不是使用内敛的模板.

首先修改我们装饰器函数中配置对象的`templateUrl`选项,让它的值改为`app/templates/app-template.html`;然后我们在`app`文件夹下新建一个文件夹`templates`,在`templates`文件夹中新建文件`app-template.html`.

在装饰器函数中应该是这样的:
```typescript
templateUrl: 'app/templates/app-template.html'
```

好了,接下来就该我们大展身手了;首先我们先来介绍一下在`ng2`中我们使用`()`(英语的小括号)来绑定事件,通过括号后面的表达式来响应这个事件;现在我们来尝试使用这种语法,在导出的`AppComponent`类中我们给类添加一个属性`msg`以及一个方法`showMsg()`:
```typescript
export class AppComponent {
    msg = 'dreamapple';

    showMsg() {
        alert(this.msg);
    }
}
```
然后是我们的HTML模板:
```html
<h1>User Input</h1>
<div>
    用户的点击事件
    <br>
    <button (click)="showMsg()" >show msg</button>
</div>
```
这样,当我们点击按钮**show msg**的时候就会弹出`dreamapple`.这里你可能会好奇,为什么不是使用`*ngClick`,因为`ng1`中我们会使用`ng-click`来进行点击事件的绑定;
首先要注意的是,在`ng2`中并没有`*ngClick`这个指令;`ng2`之所以这样设计是因为它要让整个数据的流动方式变得清楚,通过使用`(event)="handler()"`我们可以清楚直观地看到数据是从视图流转到模型的;**这是视图到模型的一种单向的数据绑定**

好了,上面的那一点还是很重要的,需要我们牢记在心里;我们继续往下进行,那么我们如何获取一个用户的输入呢?在`ng2`中我们可以使用`keyup`事件来监听用户的输入,所以我们可以给`keyup`事件绑定一个事件处理函数,我们暂时叫它`keyupHandler1`吧:
```typescript
export class AppComponent {
    msg = 'dreamapple';
    input1 = '';

    showMsg() {
        alert(this.msg);
    }

    keyupHandler1(event:any) {
        this.input1 = event.target.value;
    }
}
```
HTML模板中添加下面这些:
```html
<div>
    keyup事件,传递原生的event对象
    <br>
    <input (keyup)="keyupHandler1($event)" type="text">
    <br>
    输入框中的值为: {{input1}}
</div>
```
我们首先来看一下HTML模板中的一些代码,我们使用`(keyup)`绑定了`input`的`keyup`事件,然后这个事件的处理函数是`keyupHandler1`,我们还给这个函数传递了一个事件对象`$event`;这个`$event`就是我们在触发`input`的`keyup`事件后产生的一个事件对象.在`AppComponent`中,我们使用`keyupHandler1`来处理上面那个事件,我们的这个方法传递进来了一个参数,然后我们将这个参数的也就是那个事件对象的`target.value`赋值给我们的`input1`属性.

还需要注意的是,我们在方法`keyupHandler1`中定义`event`的类型为`any`类型;我们并没有使用**强类型**,下面我们使用强类型来改写上面的例子:
```typescript
export class AppComponent {
    msg = 'dreamapple';
    input1 = '';
    input2 = '';

    showMsg() {
        alert(this.msg);
    }

    keyupHandler1(event:any) {
        this.input1 = event.target.value;
    }

    keyupHandler2(event: KeyboardEvent) {
        this.input2 = (<HTMLInputElement>event.target).value;
    }
}
```
HTML模板中添加下面的部分:
```html
<div>
    keyup事件,传递原生的event对象,使用强类型
    <br>
    <input (keyup)="keyupHandler2($event)" type="text">
    <br>
    输入框中的值为: {{input2}}
</div>
```
改变的部分主要是两个部分,首相我们把`keyupHandler2`中的参数声明为`KeyboardEvent`的类型,然后我们使用类型转换将`event.target`转换为`HTMLInputElement`类型,然后我们再取它的值.

练习到这里我们可能会感觉到通过使用`keyup`事件来获得用户的输入是很不方便的,因为这种方式过多的关注了模板的细节,使我们的代码变得臃肿和难以理解;
我们希望有更简便的方法可以更好地获取用户的输入,当然`ng2`还给我们提供了一个叫做模板引用变量的东西,接下来我们就来好好研究一下这个**模板引用变量**.

还是和上面一样,我们先来把实例代码写出来,然后慢慢和大家来探讨怎么使用这个**模板引用变量**

```html
<div>
    使用模板引用变量
    <br>
    <input #name1 (keyup)="0" type="text">
    <br>
    输入框中的值为: {{name1.value}}
</div>
```
这次我们没有在`AppComponent`中添加代码,所有的代码都是写在HTML模板中的;我们使用`#name1`来表示一个模板引用变量,其中`#`表示后面的变量是一个模板引用变量,
而这个模板引用变量是可以在这个模板的同级元素或者子元素中直接使用的.还有我们很奇怪的使用了`(keyup)="0"`,这很让人费解;其实我们这么做只是为了'讨好'`ng2`,
当这个元素的`keyup`事件触发的时候,`ng2`会更新`name1.value`的值,所以我们也就看到了相应的变化.

当然上面那样做,只是能够在页面中显示输入框中的值;如果想要获取到这个值,我们就要做一点点小的改变:
```typescript
export class AppComponent {
    msg = 'dreamapple';
    input1 = '';
    input2 = '';
    input3 = '';

    showMsg() {
        alert(this.msg);
    }

    keyupHandler1(event:any) {
        this.input1 = event.target.value;
    }

    keyupHandler2(event: KeyboardEvent) {
        this.input2 = (<HTMLInputElement>event.target).value;
    }

    keyupHandler3(value: string) {
        this.input3 = value;
    }
}
```
HTML模板中我们添加下面的代码:
```html
<div>
    使用模板变量,获取模板变量的值
    <br>
    <input #name2 (keyup)="keyupHandler3(name2.value)" type="text">
    <br>
    输入框中的值为: {{input3}}
</div>
```
上面的代码应该就比较清楚明了了,下面我们来讲一下事件过滤;`ng2`还提供了事件的过滤,就好比上面的`keyup`事件,如果我们只想`enter`键触发这个`keyup`事件的话,我们应该怎么做呢?

我们可以使用`keyup.enter`事件,这样的话就只有`enter`键的`keyup`事件触发的时候才会执行一些方法;我们可以看下面这个例子:
```html
<div>
    使用模板引用变量
    <br>
    <input #name3 (keyup.enter)="0" type="text">
    <br>
    输入框中的值为: {{name3.value}}
</div>
```
只有当我们按下`enter`然后又松开的时候,才会正确的显示`name3.value`的值,也就是输入框的值.

还有另一个事件`blur`,也就是在当前元素失去焦点的时候,才会触发一些方法;我们可以改写上面的列子,将`keyup.enter`换成`blur`就可以了,是不是很简单呢?
```html
<div>
    使用模板引用变量
    <br>
    <input #name4 (blur)="0" type="text">
    <br>
    输入框中的值为: {{name4.value}}
</div>
```

学习到了这里,我们可以试着做一个**TODO list**小应用啦;用到的基本上都是上面所学的知识,还有一些JavaScript的基本知识,这一部分就不多讲啦,先来看看代码部分吧:
```typescript
    item = '';
    itemList = [];

    addItem(name) {
        this.itemList.push({name: name, status: 'not complete'});
    }
    removeItem(id) {
        this.itemList.splice(id, 1);
    }
    doneItem(id) {
        this.itemList[id].status = 'done!';
    }
```
上面的部分是我们添加到`AppComponent`中的代码,下面的是添加到模板中的代码:
```html
<div>
    我们运用上面学的知识,来写一个TODO List小程序;效果如下:
    <br>
    <input #item
           (keyup.enter)="addItem(item.value);item.value=''"
           type="text">
    <button (click)="addItem(item.value)">add</button>
    <ul>
        <li *ngFor="let item of itemList; let i= index">
            {{item.name}} - 完成度:{{item.status}}
            <button (click)="removeItem(i)">delete</button>
            <button (click)="doneItem(i)">done</button>
        </li>
    </ul>
</div>
```
运行一下,感觉是不是很酷呢?好啦这一篇文章就先到这里啦.








[1]:https://github.com/hacking-with-angular/angular2-travel/tree/quickstart