---
title: 字节跳动游戏客户端实习
date: 2020-12-10 14:40:13
tags:
categories: 面试
---

也算是第三次面试字节了，第一次的时候面暑期实习调剂到了测开啥都不会尬聊了三十分钟，第二次夏令营回来hr放错了把我放进秋招笔试了，改成实习之后发挥的太烂直接GG了，这次实际也是没有投简历，7号在从济南回来的火车上接到HR的电话，考不考虑过去实习，想着面就面吧，就有了这几次面试。



## 一面

一面面试官人蛮好的，我好久没面试了一上来有点紧张，然后面试管跟我说感觉我有点紧张，不需要紧张放松就好了。

+ 首先问了我项目，让我介绍一下渲染器的项目，然后问我超分采样是什么，其实就是SSAA做抗锯齿。
+ 面试官说比较关注我的C#和Unity使用方面的能力，首先从Unity开始，用了那些Unity的常用函数，比如在一个Monobehaviour里面都会有哪些常见的函数；我说了`start(), Onawake(), Update(), Fixupdate()`，然后想不起来了，其实交互也算事件函数，网上[博客很多](https://www.cnblogs.com/zhxmdefj/p/9966556.html)。
+ 在我们项目中使用了哪一些Unity的器件，列举了Canvas，Layer，VideoClip等等
+ 说一下canvas的作用，*Canvas是所有UI组件的父物体，也就是说每一个UI组件都必须在Canvas下，作为Canvas的子物体*
+ 说一下canvas的渲染顺序，根据渲染方式不同，渲染顺序也不同
+ 然后开始问C#问题，说一下静态函数有什么作用
+ class与struct有什么区别？ 一开始没想到，因为没怎么用过struct，想到值类型和引用类型，然后猜对了。
+ 说起来值类型和引用类型，就接着让我介绍一下有什么区别，如果引用作为参数传入函数值会改变吗？值类型呢？
+ 学习过Lua语言吗？

<!-- more -->

+ C++中指针和引用有什么区别？ **有什么时候只能用引用不能用指针？有什么时候只能用指针不能用引用？** 这个问题上一次面试就问过，结果忘记了，整理下

**只能使用指针不能使用引用的情况：**

1. 如果一个指针所指向的对象，需要用分支语句加以确定，或者在中途需要改变他所指的对象，那么在它初始化之后需要为他赋值，而引用只能在初始化时指定被引用的对象，所以不能胜任。

2. 有时一个指针的值可能是空指针，例如当把指针作为函数的参数类型或返回类型是，有时会用空指针表达特定的含义，而没用空引用之说。

3. 使用函数指针，由于没有函数引用，所以函数指针无法被引用替代。

4. 使用new创建的对象或数组，需要用指针来存储它的地址。

5. 以数组形式传递大批量数据时，需要用指针类型接受参数。

+ Vector和list有什么区别？Vector在物理上是连续的吗？
+ map是用什么数据结构实现的

+ const在函数的前面和后面有什么区别，当const在前面时

  ```cpp
  如果给以“指针传递”方式的函数返回值加const修饰，那么函数返回值（即指针）的内容不能被修改，该返回值只能被赋给加const修饰的同类型指针。例如如下函数：
  const char *GetString(void);
  
  ```

   如下语句将出现编译错误：

  ```cpp
  char *str = GetString();
  ```

   正确的用法是：

  ```cpp
  const char *str = GetString();
  ```

   如果函数返回值采用"值传递方式”，由于函数会把返回值复制到外部临时的存储单元中，加const修饰没有任何价值。

+ `const char *a`,  `char* const a`和`char const *`的区别 `const char*` 与 `char const *`相同, `char *const`定义一个指向字符的指针常数，即const指针，不能修改指针指向的内容，但是可以修改该指针指向的内容。
+ 假设一个空间中有许多点，如何判断一个球与多少点相交？可以考虑kd-tree
+ 说一下kd-tree的基本原理
+ 喜欢什么样的游戏，想做什么样的游戏？答现在对技术比较感兴趣，被告知项目组做的是老虎机，实际工作中写逻辑比较多还是会比较枯燥的。
+ 手撕代码出了道BFS。

面完之后当场和我说还有下一轮面试，然后他去叫第二个面试官了。

## 二面

等了十多分钟主程来了，然后重新自我介绍，有很多问题记不清了，印象比较深的几个

+ 还是先从项目问起，SSAA是怎么做的，了解过MSAA吗，不了解
+ 纹理映射是如何做的，如果提取纹理坐标中的值，一开始把mipmap说成tilemap了，感觉不太对，问了一句是叫tilemap吗？面试官有点无奈的说了一句mipmap，mipmap的用法，作用，怎么要提取所要确定的精度
+ 说一下phong光照模型
+ C++，多态是怎么实现的
+ 静态函数有什么作用
+ 有考虑过free是怎么确定释放的内存大小的吗？没想过，其实malloc分配内存的前几个字节储存了开辟空间的大小
+ 如果在异常中抛出一个异常会发生什么？[异常抛出机制是什么？](https://blog.csdn.net/pzp201833/article/details/80996133)懵逼三联，人已经傻了
+ 手撕代码，写了一个层次遍历的函数，被检查出了拉了一句话，尴尬。
+ 反问环节，您是具体的项目组吗？是的，面试官是主程，如果进组大概率是到这个组里，明天会有总监面，过了会有HR面。

感觉确实主程的思路很快，提问的问题也很刁钻，很多问题想不起来了，大概一共也是四十多分钟的样子，整个过程还是比较愉快的。面试快结束的时候主程就比较肯定的说你大概率会来我们组。

面完之后大概五六分钟，HR打电话约下一轮面试。

## 三面

总监面约在第二天下午，当天中午和cly聊了聊天问了一下实习的情况，cly说fy就在他后面吃饭，我就忽然想到技术总监会不会是fy。

一进面试间果然是fy，没叫我自我介绍直接开面了，猝不及防。

+ 一般用C++哪个版本比较多，C\++11和C\++14
+ 有什么经常用到的特性，一时间就想起来lambda表达式和auto，然后就后悔了，其实还有特别多，比如`std::unordered_map`,  `std::unordered_set`, `std::thread`等等，网上有很多[资料](https://www.cnblogs.com/feng-sc/p/5710724.html)。
+ 然后fy就开始问lambda表达式，好久不用了内心相当绝望，lambda表达式如何捕获一个值，还让我设计一个lambda表达式来处理一个HTTP请求，我哭了，然后发现我不会，只问我，这个需要请求内存吗，我说需要。
+ 分析一下vector, list, queue的时间复杂度，并说一下区别
+ 刚才说到vector的插入的实现方式，如果自己定义了copy函数，需要调用自己的copy函数吗？ 这个说了不需要，然后fy举了一个例子

```cpp
class A{
	int a;
	int *b;
	A() {b = *a;}
}
```

这种情况下如果直接进行copy的话，一定会造成指针指向的地址错误，所以需要调用自己写的copy函数。

+ 刚才说到list的插入是 $O(1)$ 的，如果直接遍历过去遍历是 $O(N)$ 的，那总复杂度不应该是 $O(N)$ 的吗？为什么是 $O(1)$ 的？可以直接存储指针或者迭代器进行删除。
+ 平时玩游戏多吗？都喜欢玩哪些游戏？
+ FPS游戏的战斗逻辑应该放在服务器端还是放在客户端？为什么？回答了放在客户端，因为为了低延迟然后服务器进行同步计算校对，举了CF中当网络卡的时候，射击但不造成伤害的例子。
+ 那这样如何处理网络延迟？如果一个人网络延迟为100ms，到达服务器已经有一个身位的延迟了，那他岂不是永远无法击中对方，两方都有延迟，送至服务器的时候是相同的时间。其实就是考帧同步和状态同步，正好前两天看的时候刚好想过这些问题。
+ 在MOBA游戏中一个AOE技能如何判断哪些目标被击中了？回答直接遍历，举了英雄联盟中全图AOE的BUG的例子来说明。
+ 这样会不会造成效率特别低？不会，因为在MOBA游戏中一般单位并不会很多
+ 那如果在一个MMO中，地图特别大怎么办？可以考虑地图分片或者kd-tree
+ 那如果一个地图中的人数特别的多怎么办？kd-tree不会效率特别低吗，如果一片人从地图的一端到另外一端？ 确实，还是地图分片吧
+ 每个地图的人用什么数据结构来维护？如果一个人要从一个地图到另外一一个地图，用链表维护，那这样怎么设计数据结构？每次都要遍历寻找到玩家吗，每次复杂度都是 $O(N)$ 吗？
+ 可以开一个map、unordered_map，来存储在地图链表中的指针，然后 $O(1)$ 删除？
+ 可以用vector来实现  $O(1)$ 的插入和删除吗？可以，可以开一个stack记录一下空的区域。
+ 还有什么其他办法吗？没想到，fy提示可以和最后一个元素swap一下，我附和对对对
+ 为什么可以这么做呢？因为数据并没有顺序性
+ 在开发游戏的过程中有遇到什么问题或者困难，最终试着去解决了？说了在夏令营时候等待的时候都是用协程来解决的，但是感觉代码写的丑，不知道在大型工程中应该怎样去解决。
+ 反问环节，说了与前面两个面试官都聊了很多了，问了一下如果实习不够三个月怎么办？没有太大关系，但是更希望实习的久一点，以后暑假也可以来实习。

## HR面

三面完五分钟直接就HR面了

面试的是当初夏令营负责会议的zzk，面试十分钟想起来了名字，声音太有辨识度了。

+ 自我介绍
+ 介绍一下自己为什么要进入游戏行业
+ 我是如何提升自己的水平的？有哪些学习的方法和途径
+ 喜欢什么类型的游戏
+ 如何看待原神这款游戏
+ 自己未来的规划，会去读研吗？
+ 实习的时间
+ 自己有什么优势
+ 反问环节？问了一下实习生薪资

总体来说感觉从面试官就能感觉到差距，fy真的思路太清晰了，问的问题信手拈来，又有一定的难度，希望以后能够达到fy一样的水平。

