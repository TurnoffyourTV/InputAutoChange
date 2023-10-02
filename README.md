# windows下neovim切换中英输入法方案
# InputAutoChange
a solution to Input auto change on neovim &amp; windows

笔者之前在windows下一直使用gvim作为日常编辑器使用,总所周知,vim下的中英文切换一直是难题,尤其是在需要频繁切换normal/insert模式的场景,每次切换模式前都得先确认自己是英文模式还是中文模式,沉浸体验感很差,其实不止在vim下,日常使用中,用户通常不会去记忆输入法的所处模式,而像笔者这样的强迫症用户又喜欢隐藏任务栏(哪怕不隐藏,时不时往屏幕右下角瞥也不是一种很好的解决方案)

在使用gvim的时候,vim自带了一个选项

set iminsert =2 

这个配置可以让我们在进入vim normal模式时自动变为输入法的英文模式,用户不需要关心在切换前输入法是中文还是英文模式,在进入normal模式后系统都会自动切为英文模式,实际用起来操作流畅了许多,
而就是这么一个在gvim中只需要一行代码就能实现的功能,在neovim中实现起来,却复杂了许多


在vim的发展上,除去gvim,在vim中优雅的切换输入法也算是一个痛点了,三种操作系统(mac,windows,linux)有三种解决方案,笔者下面着重介绍一下windows下的解决方案

[im-select](https://github.com/daipeihust/im-select)

这是一款windows获取输入法ID的程序,配合[vim-im-select](https://github.com/brglng/vim-im-select)使用

[neuims](https://github.com/neur1n/neuims)

im-select 与vim-im-select 的整合版

在介绍之前,先区分一下输入法切换的方式,以微软拼音输入法举例

平时我们摁一下shift切换就能从英文切换到中文,这种叫做输入法内切换

而还有一种切换方法是我们下载两个输入法,一个是微软拼音(双语),一个是ENS美国英语键盘(仅有英语),这种为切换输入法,在windows下的操作方式是win+space

gvim 中设置set  iminsert=2 的方法,相当于是在第一种方案中,gvim自动帮我们摁了shift,笔者认为,这也是最流畅的方案,在同一个输入法内切换不同语言才是最方便的方法

而可惜的是,除了gvim提供的官方解决方案,目前主流的切换方案,都是第二种,在两种输入法之间进行切换,,上面im-select和neovim两种解决方案都是用的这种方案,笔者在使用了一段时间后体验不佳,遂放弃,放弃的原因如下

1.不稳定性:频繁的切换输入法会时不时的出现bug,比如图标显示不正确,在normal模式可以打出字来等等,极大的影响了使用体验

2.繁琐:需要理解一点,方案2的本质是摁下esc键之后进入了ENS英文键盘,也就是说normal模式是在纯英文模式下进行的,在这种模式下没有什么,但是当你进入insert模式时,你还是ENS,此时如果你相打中文,就不得不先切换成微软拼音输入法,而进入拼音输入法时,会继承上次切换前的状态,例如你是从英文模式转入ENS话,那切换回来还是英文,这就又陷入到记忆当前输入法的窘境里面去了,得不偿失
这说明从vim插件角度是无法解决这件事的,本质是由于历史开发的插件只能获取当前的输入法ID,而不倒得到输入法内部的状态ID,程序并不知道当前的双语输入法用的是英文还是中文,这才是根源所致


那么想要实现,从insert模式退出时自动进入英语,从n/c模式进入i时自动变为英语,还是要在一个输入法内切换才能满足稳定,下意识的需求,在google了几天后,笔者意识到想要解决这个问题,必须从输入法的角度来解决,这就引入了一个神器
[KBLAutoSwitch](https://github.com/flyinclouds/KBLAutoSwitch) [AHK中英文输入法自动切换]

一个基于autohotkey的输入法切换程序,也是笔者现在找到的唯一一个可以在单个输入法内切换指定语言的解决方案,对该部分原理感兴趣的朋友可以阅读相关源码
接下来是使用说明

利用左shift键位绑定切换中文,右shift键位绑定切换英文
![image](https://github.com/TurnoffyourTV/InputAutoChange/assets/54170984/356dcd0f-2460-484c-aa63-a9b36e8d2514)

使用vim的话esc键位不能少

![image](https://github.com/TurnoffyourTV/InputAutoChange/assets/54170984/c10f61e1-2136-4204-b9a1-8581f43a672a)

![image](https://github.com/TurnoffyourTV/InputAutoChange/assets/54170984/474cda02-e9bc-45da-91f8-4b9c16c85ca6)


启动程序,这样子我们就实现了输入法层面的绑定,左shfit键位单击切换至中文输入,右shift切换至英文输入,esc同理,不需要记忆输入法的状态了
(使用该程序时候,要在windows设置中关闭使用shift切换中英输入法的功能)

目前笔者测试了微软拼音输入法以及小狼毫输入法,都可以稳定可用,kblautoswitch还有其他功能,欢迎大家挖掘
