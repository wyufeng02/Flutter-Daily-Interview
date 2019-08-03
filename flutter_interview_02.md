# 一次Flutter面试经验，这些问题你一定要知道！必问！！

https://juejin.im/entry/5c7f34b3f265da2d864b610f

#### 一面问的Java 和Android基础

1.  Jvm虚拟机
1.  messageQueue会不会阻塞ui线程
1.  对象锁和类锁
1.  之字形打印树
1.  还有其他的记不清了，主要是我对二面印象太深刻了。

#### 二面问的Flutter和Dart

1.  dart是值传递还是引用传递
2.  Widget和element和RenderObject之间的关系
3.  widget的root节点
4.  mixin extends implement之间的关系（除了extends其他的没怎么用过。。）
5.  jvm内存模型（感觉这个是面试官可怜我，看我什么都不会才问的=。=）
6.  Future和microtask执行顺序
7.  dart中..的用法（基本没用过。。）
8.  await for（没用过。。）


说实话，第一个、第三个、第六个我准备的话应该能答出来的，但是一个多月没碰Flutter了，忘了都差不多。。。 等下把二面的答案写出来，希望能帮助后来人。
此外GitHub和博客维护好很重要，像我这种demo随手写，随手删的人直接GG。。

#### 1. dart是值传递还是引用传递

首先给个结论，dart是**值传递**。

**之前把引用传递理解错了，给各位读者报个歉，同时也感谢评论区的指正**

先来看段代码


```
main(){
  Test a = new Test(5);
  print("a的初始值为：${a.value}");
  setValue(a);
  print("修改后a的值为: ${a.value}");
}

class Test{
  int value = 1;
  Test(int newValue){
    this.value = newValue;
  }
}

setValue(Test s){
  print("修改value为100");
  s.value = 100;
}
```

输出结果为：


```
a的初始值为：5
修改value为100
修改后a的值为:100

```
从这里可以看出是值传递，如果只是复制了一个对象的话，`main`函数中的`a`值是不会发生变化的。 有些人可能会以以下代码反驳我：


你看，这输出的不是6吗，在`dart`中一切皆为对象，如果是值传递，那为什么是6啊。

答案是这样的，在`setValue()`方法中，参数`s`实际上和我们初始化`int s =
6`的`s`不是一个对象，只是他们现在指的是同一块内存区域，然后在`setValue()`中调用`s +=
1`的时候，这块内存区域的对象执行`+1`操作，然后在堆(类比java)中产生了一个新的对象，`s`再指向这个对象。所以`s`参数只是把`main`函数中的`s`的内存地址复制过去了，就比如java中的：


我们只要记住一点，参数是把内存地址传过去了，如果对这个内存地址上的对象修改，那么其他位置的引用该内存地址的变量值也会修改。千万要记住dart中一切都是对象。

偷偷说一句，我觉得面试官这个地方面试的不好，这种细节问题，如果不是遇到什么bug，业务忙的时候是没时间注意这个的，面试官可以把这两种情况展示下，然后问面试者原因是什么。。然后我就能回答出来了。。哭唧唧。。

#### 2. Widget和element和RenderObject之间的关系

首先我详细说下当时的情景，面试官问我`Widget`和`Element`之间是不是一对多的关系，如果是增加一个`Widget`之后，这个关系又是什么。
这部分还是没有很好地答案，现在只是一个猜想，如果添加了一个`widget`，`Element`树遍历后面所有的`Element`看类型是否发生改变，有的话再重建`RenderObject`。`Element`和`Widget`之间应该还是一对一的关系，因为每个`Widget`的`context`都是独一无二的。等想好了再写上去吧。

#### 3. widget树的root节点

还是没能理解面试官的意思。。有能够理解的同学请评论告知我一下。
现在理解了，面试官的意思应该指是runApp()方法中的那个的Widget。我当时也想说的，不过忘记这个方法名是啥了。。。

#### 4. mixin extends implement之间的关系

这部分可以参考掘金的小德大佬的文章，高产似那啥。。

#### 6. Future和microtask执行顺序

这部分就不多做赘述了，大家可以自行搜索文章观摩参考。

#### 7. dart中..是什么

级联符号 ..
可以让你连续操作相同的对象，不单可以连续地调用函数，还可以连续地访问方法，这样做可以避免创建临时变量，从而写出更流畅的代码，流式编程更符合现代编程习惯和编程风格:


#### 8. await for使用

先来一段官方文档

> 

## await-for

As every Dart programmer knows, the **for-in** loop plays well with iterables.
Similarly, the **await-for** loop is designed to play well with streams. Given a
stream, one can loop over its values: Every time an element is added to the
stream, the loop body is run. After each iteration, the function enclosing the
loop suspends until the next element is available or the stream is done. Just
like **await** expressions, **await-for** loops can only appear inside
asynchronous functions.

大概意思就是`await for`是不断获取`stream`流中的数据，然后执行循环体中的操作。


输出为


`await for` 和 `listen`的作用很相似，都是获取流中数据然后输出，但是正如`await
for`中的`await`所示，如果`stream`没有传递完成，就会一直阻塞在这个位置，上面没吃饭是最后输出的，下面给个`listen`的实例，一看就懂。


输出为


所以`await for`一般用在直到`stream`什么时候完成，并且必须等待传递完成之后才能使用，不然就会一直阻塞，造成类似于`Android
ANR`的问题。

#### 总结

其实面试官还是很nice的，第一次见到活的大佬。。大佬对flutter和dart的研究真的很深入，远不是我这种只会调api的人可以比拟的。
主要还是我一个半月没使用过flutter了，然后之前问其他大佬要不要准备Flutter，大佬们说不用，以前看的很多东西都忘的差不多了。
哎，还是自己准备不充分，或者开始大佬问我的时候直接回答忘得差不多了，应该就能过了吧。所以自己还是要做好充分的准备。
  